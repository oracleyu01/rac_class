
## ⭐⭐ RAC 핵심12. RAC 환경에서 리두 로그 파일 추가하기 ⭐⭐

Oracle RAC 환경에서 리두로그 파일 관리하는 방법은 standard alone 과 차이가 있습니다.

<img src="https://github.com/oracleyu01/rac_class/blob/main/%EB%A1%9C%EA%B7%B8%ED%8C%8C%EC%9D%BC2.png" width="500" height="400">

RAC에서는 각 인스턴스가 자신만의 thread를 가지며, 이 thread 번호를 사용하여 해당 인스턴스의    
리두 로그 파일을 관리합니다.   
 

### 1️⃣ RAC 환경에서 리두 로그파일 추가하는 실습

**⚡실습** 

 standard alone 환경에서 리두 로그 그룹추가 명령어 

 SQL> alter   database   add  logfile   group   5
      '/u01/app/oracle/oradata/yys/redo05.log'  size  10m;
 
스토리지가 ASM 이고 RAC 환경에서는 추가하는 명령어가 더 간단합니다.
 
 SQL#1> alter  database  add   logfile  thread  1 group  5;

 OMF 기능이 켜있는 상태에서 file 을 추가하면 default 사이즈가 100mb 입니다.

 SQL#1> alter system switch logfile;
  &nbsp;
  &nbsp;

**문제1.  2번 인스턴스용 리두 로그 그룹을 그룹번호 6번으로 해서 추가하시오 !**

 SQL#2> alter  database  add  logfile  thread  2   group  6; 

 SQL#2>  alter system switch logfile;
  &nbsp;
  &nbsp;

**문제2. 1번 인스턴스와 2번 인스턴스에서 각각 로그 스위치를 일으키고 unused 상태가 아니게 하시오 !**
 &nbsp;
  &nbsp;
  답:
   &nbsp;
  &nbsp;
  
**문제3. 5번 리두 로그 그룹을 삭제하시오 !**

SQL#1> alter   database  drop   logfile   group   5; 
 &nbsp;
  &nbsp;

설명: 삭제를 할 때는 상태가 current 나 active 면 안됩니다. 

SQL#1> alter system switch logfile;

SQL#1> alter system checkpoint;

SQL#1>  alter system switch logfile;

SQL#1> alter   database  drop   logfile   group   5; 
 &nbsp;
  &nbsp;

**문제6.  리두 로그 그룹 6번을 삭제하시오 !**
&nbsp;
  &nbsp;
  답:
   &nbsp;
  &nbsp;
 &nbsp;
  &nbsp;
  &nbsp;
  &nbsp;
  
😊 RAC 환경에서는 리두 로그 파일은 각각 별개의 group 에 write 를 한다는 것을 기억해주세요.


