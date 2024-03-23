## ⭐⭐ RAC 핵심5. RAC 환경에서 데이터 일관성을 유지하는 기술  ⭐⭐

Oracle Real Application Clusters(RAC) 환경에서는 어떤 노드로 접속하든 항상 일관된, 
최신 상태의 데이터를 조회할 수 있는 기술이 있습니다. 

이 기술은 Global Resource Directory (GRD)에 기반을 둡니다.

### 1️⃣ 데이터 일관성 보장

<img src="https://github.com/oracleyu01/rac_class/blob/main/rac%EA%B7%B8%EB%A6%BC.png" width="500" height="400">


위의 2개의 노드중 어느 노드로 접속하든 항상 일관된 데이터를 검색할 수 있습니다.

### 2️⃣ GRD와 데이터 일관성

RAC에서는 Global Resource Directory (GRD)를 통해 최신 데이터의 위치 정보를 관리합니다.

GRD는 클러스터 내의 모든 노드의 Shared Pool에 위치하며, 각 데이터 블록의 현재 위치와 상태 정보를 담고 있습니다.

**예시:**
<img src="https://github.com/oracleyu01/rac_class/blob/main/rac%EA%B7%B8%EB%A6%BC.png" width="500" height="400">

1. 'emp' 테이블의 최신 데이터 위치 정보가 1번 노드의 GRD에 등록되어 있습니다.  

2. 'dept' 테이블의 최신 데이터 위치 정보는 2번 노드의 GRD에 있습니다.

3. 사용자가 2번 노드에 접속하여 'emp' 테이블의 데이터를 조회하려 할 때,
   
   &nbsp;&nbsp;2번 노드는 1번 노드의 GRD를 참조하여 'emp' 테이블의 최신 데이터 위치를 파악합니다. 

이후, 해당 위치에서 데이터를 검색하여 사용자에게 제공합니다.  

&nbsp;
&nbsp;
&nbsp;

😊 이러한 메커니즘 덕분에, RAC 사용자는 어느 노드로 접속하든지 간에 맨 마지막에 커밋된 데이터를 일관되게 조회할 수 있으며,  
&nbsp;&nbsp;&nbsp;&nbsp;데이터의 정확성과 신뢰성을 보장받습니다.
