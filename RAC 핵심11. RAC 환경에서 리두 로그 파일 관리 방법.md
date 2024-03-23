## ⭐⭐ RAC 핵심11. RAC 환경에서 리두 로그 파일 관리 방법 ⭐⭐

Oracle RAC 환경에서 리두로그 파일 관리하는 방법은 standard alone 과 차이가 있습니다.

<img src="https://github.com/oracleyu01/rac_class/blob/main/%EB%A1%9C%EA%B7%B8%ED%8C%8C%EC%9D%BC.png" width="500" height="400">

RAC에서는 각 인스턴스가 자신만의 thread를 가지며, 이 thread 번호를 사용하여 해당 인스턴스의  리두 로그 파일을 관리합니다.   
 
Oracle에서는 이러한 작업을 통해 데이터베이스가 다중 노드에서 실행될 때 각 인스턴스가 동일한 데이터베이스에 안전하게 쓸 수 있도록 합니다

### 1️⃣ Voting Disk ?

**⚡실습** 

**#1. 1번 인스턴스에서 thread 번호를 확인합니다.**

SQL#1> show  parameter  thread

**#2. 2번 인스턴스에서 thread 번호를 확인합니다.**

SQL#2> show parameter  thread

**#3. 1번 인스턴스에서 리두로그 그룹과 상태와 thread 번호를 확인합니다.**

SQL#1> select  group#, status, sequence#, thread#
              from v$log;

※ thread 번호는 rac 환경에서 어느 인스턴스의 리두 로그 그룹인지를 구분해주기 위한 번호 

**#4. 1번 인스턴스에서 로그 스위치를 일으킵니다.**

SQL#1> alter  system  switch  logfile; 

**#5. 1번 인스턴스에서 리두로그 그룹과 상태와 thread 번호를 확인합니다.**

SQL#1> select  group#, status, sequence#, thread# from v$log;

**#6. 2번 인스턴스에서 리두로그 그룹과 상태와 thread 번호를 확인합니다.**

SQL#2> select group#, status, sequence#, thread# from v$log;

**#7. 2번 인스턴스에서 로그 스위치를 일으킵니다.**

SQL#2> alter  system  switch  logfile;

**#8. 2번 인스턴스에서 리두로그 그룹과 상태와 thread 번호를 확인합니다.**

SQL#2> select group#, status, sequence#, thread# from v$log;

 &nbsp;
  &nbsp;
  &nbsp;
  &nbsp;
  
😊 RAC 환경에서는 리두 로그 파일은 각각 별개의 group 에 write 를 한다는 것을 기억해주세요.


