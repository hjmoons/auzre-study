# Application Gateway (AppGW)

> 참고: [Application Gateway 개요 - Microsoft Docs](https://learn.microsoft.com/ko-kr/azure/application-gateway/overview)

웹 애플리케이션에 대한 트래픽을 관리하는 웹 트래픽 Load Balancer

- 기존 Load Balancer: IP 주소 및 포트 기반 트래픽 라우팅
- AppGW: URL 경로 및 호스트 헤더와 같은 HTTP 요청 특성 기반으로 지능형 라우팅

예를 들어, URL에 /images이 포함된 요청은 이미지에 최적화된 서버로 라우팅하고, /video이 포함된 요청은 비디오 콘텐츠에 최적화된 서버로 라우팅 가능
![application-gateway-figure-1](./images/application-gateway-figure1-720.png)

## Azure Application Gateway v2

Application Gateway v2는 Application Gateway의 최신 버전. v1 대비 성능 향상, 자동 스케일링, Zone-Redundancy 및 정적 VIP 등의 이점 제공.

### 주요기능

- TCP/TLS 프록시: TCP 프로토콜 및 TLS 프록시 지원.
- 자동 스케일링: 트래픽 부하 패턴의 변화에 따라 스케일 아웃하거나 스케일 인 가능. 자동 스케일링을 사용하면 프로비전 시 배포 크기 또는 인스턴스 수를 선택할 필요가 없음.
- Zone-Redundancy: Application Gateway 또는 WAF 배포는 기본적으로 여러 AZ에 걸쳐 배포되므로 각 AZ에 별도의 Application Gateway 인스턴스를 프로비전할 필요가 없음. 인스턴스는 최소 두 개의 AZ에 배포되어 AZ 오류에 대한 복원력이 향상되며, 백엔드 풀도 AZ 전반에 분산 배포 가능.
- 정적 VIP: Application Gateway와 연결된 VIP가 재시작 후에도 배포 수명 주기 동안 변경되지 않음.
- Header Rewrite: v2 SKU를 사용하여 HTTP 요청 및 응답 헤더를 추가, 제거 또는 업데이트 가능.
- Key Vault 통합: HTTPS 수신기에 연결된 서버 인증서에 대한 Key Vault 통합을 지원.
- 상호 인증(mTLS): 클라이언트 요청 인증을 지원.
- AKS Ingress 컨트롤러: Application Gateway v2 Ingress 컨트롤러를 사용하면 Application Gateway를 AKS 클러스터의 Ingress로 사용 가능.
- Private Link: v2 SKU는 Private Endpoint를 사용하여 다른 Region 및 구독의 다른 VNet으로부터 Private 연결을 제공.
- 성능 향상: v2 SKU는 Standard/WAF SKU 대비 최대 5배 더 나은 TLS 오프로드 성능을 제공.
- 빠른 배포 및 업데이트: v2 SKU는 Standard/WAF SKU 대비 빠른 배포 및 업데이트 시간을 제공하며, WAF 구성 변경도 포함.

## Web Applcation Firewall (WAF)

