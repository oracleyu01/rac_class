
## ⭐⭐ RAC 핵심15. srvctl 명령어 사용법  ⭐⭐


 srvctl  명령어를 이용하면 각각의 터미널에서 인스턴스를 각각 내리거나 올리지 않고 하나의 터미널 창에서  
  한번에 여러개의 인스턴스를 내리거나 올릴 수 있습니다.

## **1️⃣ 실습**

**1️⃣  1번 인스턴스와 2번 인스턴스를 동시에 내리는 명령어**

    문법: $ srvctl stop instance  -d db이름  -i  1번 인스턴스이름,2번 인스턴스 이름

$ srvctl  stop  instance  -d  racdb  -i  racdb1,racdb2

또는 

$ srvctl  stop   database  -d  racdb 

SQL> show  parameter  background

alert log file 을 빨리 열수 있도록 각각의 노드에서 alias 를 .bash_profile 에 넣습니다.

#1번 노드용
alias  trace='cd  /u01/app/oracle/diag/rdbms/racdb/racdb1/trace'

#2번 노드용
alias  trace='cd  /u01/app/oracle/diag/rdbms/racdb/racdb2/trace'  

&nbsp;

**2️⃣  1번 인스턴스와 2번 인스턴스를 동시에 올리는 명령어**

    문법: $ srvctl start instance  -d db이름  -i  1번 인스턴스이름,2번 인스턴스 이름

$ srvctl  start  instance  -d  racdb  -i  racdb1,racdb2

 또는

$ srvclt  start  database  -d  racdb  

&nbsp;

**3️⃣ 서버를 내렸다가 올리면 db 도 자동으로 올라오게 셋팅되어져 있는지 확인하시오 !**

$ srvctl  config  database  -d  racdb  -a 

> 관리 정책: AUTOMATIC  <---  리눅스 서버를 rebooting 하면 db 도 자동으로 올라옵니다.

&nbsp;

**⚡ 문제1. 책 4-24 페이지를 보고 db 올리는것을 수동으로 변경되게하시오 !**  

리눅스 서버를 내렸다 올려도 db 가 자동으로 올라오지 못하게 하시오 !

$ srvctl  modify  database  -d  racdb  -y  manual 

$ srvctl  config  database  -d  racdb  -a 

**⚡ 문제2. 다시 automatic 으로 변경하세요 ~**

$ srvctl  modify  database  -d  racdb  -y  automatic

$ srvctl  config  database  -d  racdb  -a  

&nbsp;
&nbsp;


**😄 dba 를 위한 tip !**

>   서버를 내렸다 올릴때 db 가 자동으로 올라오지 않고 수동으로 올려야하는 경우는   복구 작업을 할 때인데 
>   복구 작업을 할때 서버를 리부팅해야 하는 경우가 있는데   이때 data file 이 깨져있으면 db 가 안올라옵니다. 
>   이때는 위의 작업을 하고   서버를 리부팅해줘야합니다.

