## ⭐⭐ RAC핵심7. RAC 환경에서 병렬 처리의 원리 ⭐⭐  
&nbsp;


### 1️⃣ Oracle RAC 환경에서는 병렬 처리를 통해 SQL 쿼리의 성능을 향상시킬 수 있습니다. 

예를 들어, 아래의 병렬 쿼리는 병렬 힌트를 이용하여 실행됩니다.

select /*+ parallel(a,2) parallel(b,2) */ count(*)
from dba_objects a, dba_objects b;

<img src="https://github.com/oracleyu01/rac_class/blob/main/rac%EA%B7%B8%EB%A6%BC.png" width="500" height="400">

RAC는 물리적으로 독립된 여러 장비로 구성되기 때문에 병렬 쿼리 수행 시 많은 CPU 자원을 활용할 수 있는 장점이 있습니다.

예를 들어, 4개의 노드로 구성된 RAC에서 EMP 테이블에 대한 병렬 풀 스캔을 수행하면, 

각 노드에 있는 병렬 슬레이브 프로세서가 EMP 테이블을 4등분하여 각각 조회함으로써 데이터 검색 속도가 상당히 빨라집니다.

### 2️⃣ 병렬 처리 시 주의사항 !

병렬 슬레이브 프로세서들은 노드 간에 메시지와 데이터를 주고받으며 이러한 통신은 빠른 속도로 이루어져야 합니다.

이를 위해 다음과 같은 네트워크 구성 요소를 활용할 수 있습니다.

**⚡ 1Gigabit Ethernet:** 초당 125메가바이트 전송 속도를 제공합니다.

**⚡ 10Gigabit Ethernet:** 더 높은 전송 속도를 제공합니다.

**⚡ InfiniBand:** 현재 가장 빠른 네트워크 전송 속도를 보장합니다.

RAC 환경 구성 시 프라이빗 네트워크는 InfiniBand로 구성하는 것이 권장되며, 이는 운영 시 데이터 전송의 지연을 최소화합니다.

실제 사례로, 건강 보험 심사평가원은 OPS로 운영되던 시스템을 RAC으로 전환하고 HP의 고성능 서버인 Superdome을 도입함으로써 
성능 문제를 해결하였습니다.

Oracle은 병렬 작업 수행 시 "인스턴스 내 병렬화"(Intra-instance Parallelism)를 우선 시도하고, 

한 노드의 자원으로는 충분하지 않을 경우 "인스턴스 간 병렬화"(Inter-instance Parallelism)를 시도합니다.

### 3️⃣ RAC 환경에서 병렬 작업과 관련된 중요 파라미터!

**⚡ instance_groups:** 병렬 처리에 참여할 인스턴스 그룹을 지정합니다.

**⚡ parallel_instance_group:** 특정 인스턴스 그룹에 대한 병렬 처리를 설정합니다.


<img src="https://github.com/oracleyu01/rac_class/blob/main/rac%EA%B7%B8%EB%A6%BC.png" width="500" height="400">

alter session set parallel_instance_group='seoul';

select /*+ parallel(e1) full(e1) */ count(*)
from emp e1;

이 설정을 통해 RAC 환경에서 병렬 처리의 성능을 최적화할 수 있습니다.  

&nbsp;
&nbsp;

😊 rac 에서 병렬 쿼리 수행시 필요한 파라미터 2개를 꼭 기억하고 계세요 !


