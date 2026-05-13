# Region

> 참고: [Azure 지역 개요 - Microsoft Docs](https://learn.microsoft.com/ko-kr/azure/reliability/regions-overview)

데이터 센터와 네트워크 인프라를 포함하는 실제 시설의 집합

Region(지역)은 특정 유형의 복원력 옵션을 제공. 다른 지역과 쌍을 이뤄 복원이 가능하도록 하지만, 일부 지역은 해당 기능을 제공하지 않아 복원이 불가능한 곳도 존재.

## Azure 지역선택 시 고려해야할 사항

- 대기 시간 : 사용자와 지리적으로 가까운 지역을 선택하여 대기 시간 감소. 예를 들어 사용자가 미국에 있는 경우, 미국 또는 캐나다에서 지역을 선택 가능
- 가용성 영역 : 가용성 영역을 지원하는 지역을 선택하여 중복성 및 오류 격리 제공. 리소스를 지역의 여러 가용성 영역에 분산 필요
- 데이터 상주 : 선택한 지역이 조직에 필요한 데이터 상주 경계 내에 있는지 확인. 예를 들어 개인정보보호법, 금융규제 등으로 데이터를 국내에만 저장해야 하는 경우, `koreacentral` 같은 국내 Region을 선택해야 하며 `japaneast`, `eastus` 등 해외 Region에 저장하면 규정 위반 가능성이 있음

## 쌍을 이루는 영역과 쌍을 이루지 않는 영역

일부 Azure 지역은 다른 Azure 지역과 짝지어져 지역 쌍을 형성. 지역 쌍은 Microsoft에서 선택하며 고객이 직접 선택 불가. 지역 쌍을 활용하는 일부 Azure 서비스는 지역 복제 및 지역 중복성을 지원하며, 치명적이고 복구 불가능한 오류 발생 시 재해 복구를 위해 사용.

많은 최신 지역은 쌍을 이루지 않고, 대신 가용성 영역을 주된 중복 수단으로 사용. 많은 Azure 서비스는 지역 쌍 여부와 관계없이 지역 중복성을 지원하므로, 쌍을 이루는 지역, 쌍을 이루지 않는 지역 또는 두 가지 조합을 사용하더라도 복원력 있는 솔루션 설계 가능.

## 아시아/태평양 Region 목록

> `*` 표시 지역은 특정 고객 시나리오(지역 내 재해 복구 등)에만 액세스가 제한됨

| 지역 | 가용성 영역 | 쌍을 이루는 지역 | 물리적 위치 | 지리 | 프로그래밍 방식 이름 |
|------|:---------:|----------------|------------|------|-------------------|
| 오스트레일리아 중부 | | 오스트레일리아 중부 2* | 캔버라 | 오스트레일리아 | australiacentral |
| 오스트레일리아 중부 2* | | 오스트레일리아 중부 | 캔버라 | 오스트레일리아 | australiacentral2 |
| Australia East | O | Australia Southeast | 뉴사우스웨일즈 | 오스트레일리아 | australiaeast |
| Australia Southeast | | Australia East | 빅토리아 | 오스트레일리아 | australiasoutheast |
| Central India | O | South India | 푸네 | 인도 | centralindia |
| East Asia | O | 동남아시아 | 홍콩 특별행정구 | 아시아 태평양 | eastasia |
| 인도네시아 중부 | O | N/A | 자카르타 | 인도네시아 | indonesiacentral |
| Japan East | O | 일본 서부 | 도쿄, 사이타마 | 일본 | japaneast |
| 일본 서부 | O | Japan East | 오사카 | 일본 | japanwest |
| Korea Central | O | 한국 남부 | 서울 | 대한민국 | koreacentral |
| 한국 남부 | | Korea Central | 부산 | 대한민국 | koreasouth |
| 말레이시아 서부 | O | N/A | 쿠알라룸푸르 | 말레이시아 | malaysiawest |
| 뉴질랜드 북부 | O | N/A | 오클랜드 | 뉴질랜드 | newzealandnorth |
| South India | | Central India | 첸나이 | 인도 | southindia |
| 동남아시아 | O | East Asia | 싱가포르 | 아시아 태평양 | southeastasia |
| West India | | South India | 뭄바이 | 인도 | westindia |
