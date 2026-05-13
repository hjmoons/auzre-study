# Azure Study

Azure 주요 리소스 학습 정리 문서입니다.

---

## 목차

### 01. Availability (가용성)

| 항목 | 설명 |
|------|------|
| [Region](azure-resource/01-Availability/Region.md) | Azure 데이터센터가 위치한 지리적 영역. 리소스 배포 위치 단위 |
| [Availability Zone](azure-resource/01-Availability/Available-Zone.md) | 단일 Region 내 물리적으로 분리된 데이터센터 묶음. 고가용성 보장을 위해 사용 |

---

### 02. Network (네트워크)

| 항목 | 설명 |
|------|------|
| [VNet](azure-resource/02-Network/VNet.md) | Azure 내 격리된 사설 네트워크. 리소스 간 통신의 기반 |
| [Subnet](azure-resource/02-Network/Subnet.md) | VNet을 더 작은 네트워크 범위로 분할한 단위 |
| [NSG](azure-resource/02-Network/NSG.md) | Network Security Group. 인바운드/아웃바운드 트래픽을 제어하는 방화벽 규칙 집합 |
| [UDR](azure-resource/02-Network/UDR.md) | User Defined Route. 사용자가 직접 정의하는 라우팅 테이블 |
| [Application Gateway](azure-resource/02-Network/AppGW.md) | L7 로드밸런서. SSL 종료, URL 기반 라우팅, WAF 기능 제공 |
| [ILB](azure-resource/02-Network/ILB.md) | Internal Load Balancer. VNet 내부에서만 접근 가능한 L4 로드밸런서 |
| [Private Endpoint](azure-resource/02-Network/PrivateEndpoint.md) | Azure PaaS 서비스를 VNet 내 사설 IP로 연결하는 네트워크 인터페이스 |

---

### 03. Storage (스토리지)

| 항목 | 설명 |
|------|------|
| [Storage Account](azure-resource/03-Storage/StorageAccount.md) | Blob, File, Queue, Table 등 Azure 스토리지 서비스의 기반 계정 |
| [Blob Storage](azure-resource/03-Storage/BlobStorage.md) | 비정형 데이터(이미지, 동영상, 로그 등)를 저장하는 오브젝트 스토리지 |
| [Azure Files](azure-resource/03-Storage/AzureFiles.md) | SMB/NFS 프로토콜을 지원하는 관리형 파일 공유 서비스 |

---

### 04. Security (보안)

| 항목 | 설명 |
|------|------|
| [Key Vault](azure-resource/04-Security/Key-Vault.md) | 시크릿, 키, 인증서를 안전하게 저장하고 관리하는 서비스 |

---

### 05. AKS (Azure Kubernetes Service)

| 항목 | 설명 |
|------|------|
| [Cluster](azure-resource/05-AKS/Cluster.md) | AKS 클러스터 구성 및 관리. Control Plane은 Azure가 관리 |
| [Node Pool](azure-resource/05-AKS/NodePool.md) | 동일한 VM 구성을 가진 노드 그룹. System/User Pool로 구분 |
| [CNI Overlay](azure-resource/05-AKS/CNI-Overlay.md) | Pod 네트워크를 VNet IP와 분리하여 대규모 클러스터에서 IP 절약 |
| [RBAC](azure-resource/05-AKS/RBAC.md) | Role Based Access Control. Kubernetes 및 Azure AD 기반 접근 제어 |
| [Managed Identity](azure-resource/05-AKS/Managed-Identity.md) | AKS가 다른 Azure 리소스에 접근할 때 사용하는 자격증명 관리 방식 |
