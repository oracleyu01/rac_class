## ⭐⭐ RAC핵심8. 오라클 클러스터 서비스의 혜택 ⭐⭐


### 1️⃣ 오라클 클러스터 소프트웨어를 사용하면 다음과 같은 혜택을 누릴 수 있습니다:

⚡ 고가용성: 연중무휴, 하루 24시간 데이터베이스에 접속할 수 있는 환경을 제공합니다.

⚡ 확장성: 사용자 수 증가에 따라 필요에 맞게 노드를 추가하여 시스템을 확장할 수 있습니다.

전통적인 서버 확장이 1년 이상의 프로젝트를 요구하는 반면, RAC를 사용하면 새로운 노드를 추가하는 것만으로 확장이 가능합니다.

<img src="https://github.com/oracleyu01/rac_class/blob/main/rac%EA%B7%B8%EB%A6%BC.png" width="500" height="400">

### 2️⃣  클러스터 서비스 3가지 ?

**☀️ 1. CSS (Cluster Synchronization Services):**

노드 간의 '심장박동'을 통해 서로의 상태를 확인하는 역할을 합니다. 

심장박동이 끊길 경우 해당 노드를 실패한 것으로 간주하고 재부팅을 시도합니다.


노드1  ♥ ---------------------------------->  노드2  
   &nbsp;  &nbsp;  &nbsp;   &nbsp;  &nbsp;      <----------------------------------- ♥

**☀️ 2.CRS (Cluster Ready Services):**

고가용성 작업을 보장하는 서비스로, failover 기능을 지원합니다.

Failover란, 사용자가 연결된 노드가 다운될 경우 자동으로 다른 노드로 재접속을 가능하게 하는 기능입니다.

**☀️ 3.EVM (Event Management):**

클러스터에 문제가 발생했을 때 해당 이벤트를 기록하는 서비스입니다.


### 3️⃣ 실습: Failover가 가능하도록 네트워크 설정하는 방법

<img src="https://github.com/oracleyu01/rac_class/blob/main/rac%EA%B7%B8%EB%A6%BC.png" width="500" height="400">

`tnsnames.ora 파일의 내용`

```
$ cd $ORACLE_HOME/network/admin
$ vi tnsnames.ora

racdb_taf=
 (DESCRIPTION =
 (ADDRESS_LIST=
 (LOAD_BALANCE=on)
 (FAILOVER=on)
 (ADDRESS = (PROTOCOL = TCP)(HOST = 10.0.2.111)(PORT = 1521))
 (ADDRESS = (PROTOCOL = TCP)(HOST = 10.0.2.112)(PORT = 1521))
)
 (CONNECT_DATA =
 (SERVICE_NAME = racdb)
 (FAILOVER_MODE=(TYPE=select)(METHOD=basic))
 )
)
```


이 설정을 통해 클라이언트 연결이 한 노드에서 실패할 경우 다른 노드로 자동으로 재접속이 이루어집니다.

### ⚡ RAC에서 사용하는 IP 주소 4가지:

1.Public IP: 외부에서 접속할 때 사용합니다.

2.Private IP: 노드 간 내부 통신에 사용합니다.

3.Virtual IP: 노드 실패 시 다른 노드로의 failover에 사용합니다.

4.SCAN IP: 클라이언트 연결 요청을 적절한 노드로 분산하는 SCAN 리스너의 IP 주소입니다.





