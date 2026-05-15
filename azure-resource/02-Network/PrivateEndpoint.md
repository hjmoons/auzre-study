# Private Endpoint

Azure PaaS 서비스(Storage, SQL Database, Key Vault 등)에 VNet 내부의 프라이빗 IP 주소를 부여하여, 공인 인터넷을 거치지 않고 프라이빗하게 접근할 수 있게 해주는 네트워크 인터페이스

Azure SQL Database, Storage Account 같은 PaaS 서비스는 Microsoft 관리 인프라에서 실행되어 서브넷에 직접 배포할 수 없으며, 기본적으로 Public Endpoint(공개 IP)를 통해서만 접근 가능. Private Endpoint는 VNet 안에 대리 NIC를 만들어 프라이빗하게 연결하는 방식으로 이 한계를 극복

> VNet에 직접 배포 가능한 예외: Azure SQL Managed Instance, App Service Environment(ASE), AKS, Databricks (VNet Injection 방식)

## 트래픽 방향

Private Endpoint는 **내 VNet → Azure PaaS 서비스** 방향이며, 외부에서 직접 접근하는 것은 불가

```
[인터넷 사용자]  ──X──→  Private Endpoint (불가, 의도적)

[VNet 내 VM/앱]  ──────→  Private Endpoint → Azure Storage/SQL/etc.
[온프레미스]  ──ExpressRoute/VPN──→  Private Endpoint → Azure Storage/SQL/etc.
```

| 리소스 | 방향 | 목적 |
|---|---|---|
| AppGW / LB | 외부 → 내 앱 | 내 애플리케이션을 외부에 노출 |
| Private Endpoint | 내 VNet → PaaS 서비스 | PaaS 서비스를 인터넷에서 숨기고 프라이빗하게 접근 |

일반적으로 AppGW와 함께 사용되며, 역할이 다름

```
인터넷 사용자
    ↓
AppGW (Public IP)        ← 외부 트래픽을 VNet 안으로 받아들임
    ↓
AKS / VM (VNet 내부)
    ↓
Private Endpoint         ← VNet 안에서 PaaS로 나가는 트래픽을 프라이빗하게 유지
    ↓
Azure SQL / Storage / Key Vault
```

## 기본값 및 생성 방식

PaaS 서비스는 기본적으로 Public Endpoint가 활성화된 상태로 생성되며, 생성 시 Networking 탭에서 접근 방식을 선택할 수 있음

**Storage Account**

| 옵션 | 설명 |
|---|---|
| Public endpoint (all networks) | 기본값. 인터넷 어디서든 접근 가능 |
| Public endpoint (selected networks) | 특정 VNet/IP만 허용 |
| Private endpoint | 프라이빗 전용 |

**Azure SQL Database**

| 옵션 | 설명 |
|---|---|
| Public endpoint | 기본값 |
| Private endpoint | 프라이빗 전용 |
| No access | 접근 전체 차단 |

Private Endpoint를 만들어도 Public Endpoint는 자동으로 비활성화되지 않으므로, 완전한 프라이빗 구성을 위해서는 Public Endpoint를 명시적으로 비활성화해야 함

```
[완전 프라이빗 구성]
1. Private Endpoint 생성
2. Public Endpoint 비활성화
   - Storage: "Allow public access" 비활성화
   - SQL: Firewall에서 public 차단
```

리소스 생성 후 Networking 설정에서 변경 가능하며, Private Endpoint는 독립 리소스로 나중에 따로 추가도 가능

## 구성 요소

| 구성 요소 | 설명 |
|---|---|
| Private Endpoint NIC | VNet 서브넷에 생성되는 프라이빗 IP를 가진 네트워크 인터페이스 |
| Private DNS Zone | FQDN을 프라이빗 IP로 해석하기 위한 DNS 영역 |
| Target Resource | 연결 대상 Azure 서비스 (Storage, SQL, Key Vault 등) |
| Approval | 리소스 소유자가 연결 요청을 승인/거부하는 프로세스 |

### DNS 동작 방식

```
myaccount.blob.core.windows.net
    → Private DNS Zone (privatelink.blob.core.windows.net)
    → 10.0.1.5 (프라이빗 IP)
```

- Private DNS Zone을 VNet에 연결해야 VNet 내 VM이 올바른 IP로 해석 가능
- 온프레미스에서 접근 시 DNS Forwarder 또는 Conditional Forwarder 설정 필요

## NSG와의 관계

Private Endpoint는 서브넷 주소 범위에서 IP를 할당받지만, 해당 서브넷에 NSG가 붙어있어도 **기본적으로 NSG 규칙이 적용되지 않음**

Private Endpoint는 설계 자체가 네트워크 정책(NSG, UDR)을 우회하도록 만들어졌기 때문에, 같은 서브넷의 VM NIC와 다르게 동작함

```
서브넷 (NSG 붙어있음)
├── VM NIC               → NSG 규칙 적용됨 ✓
└── Private Endpoint NIC → NSG 규칙 무시됨 ✗ (기본값)
```

NSG를 적용하려면 서브넷의 `PrivateEndpointNetworkPolicies` 속성을 `Enabled`로 명시적으로 활성화해야 함

### 서브넷 분리 권장

Private Endpoint와 VM을 같은 서브넷에 두면 NSG 동작이 섞여 관리가 복잡해지므로, 실무에서는 전용 서브넷을 분리하는 것이 일반적

```
myVNet (10.0.0.0/16)
├── subnet-app (10.0.1.0/24)   → VM, AKS 등 + NSG 적용
└── subnet-pe  (10.0.2.0/24)   → Private Endpoint 전용
```

### NSG 제한사항

- Private Endpoint NIC에서 유효 경로 및 보안 규칙이 **포털에 표시되지 않음** (적용은 되지만 확인 불가)
- **NSG 흐름 로그** 미지원
- ASG 사용 시 서브넷당 **50개 IP 구성** 제한

## Private Endpoint vs Service Endpoint

| 항목 | Private Endpoint | Service Endpoint |
|---|---|---|
| 프라이빗 IP 할당 | O | X (공개 IP로 라우팅) |
| 온프레미스 연결 | O (ExpressRoute/VPN 통해) | X |
| 비용 | 발생 (시간당 + 데이터) | 무료 |
| DNS 설정 필요 | O | X |

## Private Link와의 관계

- **Azure Private Link** — 서비스 측에서 프라이빗 접근을 허용하는 기술 (인프라)
- **Private Endpoint** — 클라이언트 VNet 측에서 만드는 실제 진입점 (인터페이스)
- Private Endpoint는 Private Link 위에서 동작
