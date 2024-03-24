
## ⭐⭐ RAC 핵심17. rac 환경에서 spfile 의 내용을 잘못설정했을때 조치방법⭐⭐

## **💎실습1**

😄 잘못된 파라미터를 하나 더 추가한 경우에 대한 조치사항 입니다.  
&nbsp;
&nbsp;

**1️⃣ 양쪽 인스턴스에 셋팅된 db_files 파라미터 값을 조회하시오 !**

> 현재 인스턴스에 셋팅된 값 확인 

  SQL#1> select  inst_id, name, value
               from gv$parameter
               where name='db_files';

> spfile 안의 내용을 확인

  SQL#1> select  inst_id, name, value, sid
               from gv$spparameter
               where name='db_files';

**2️⃣ db_files 를 특정 인스턴스의 값을 3000으로 설정되게 하시오 !**

  SQL#1> alter   system  set  db_files=3000  scope=spfile  sid='racdb1';

> spfile 안의 내용을 확인

SQL#1> select  inst_id, name, value, sid
             from gv$spparameter
             where name='db_files';

         1 db_files   2000       *
         1 db_files   3000       racdb1
         2 db_files   2000       *
         2 db_files   3000       racdb1

 **3️⃣ 양쪽 인스턴스를 내렸다가 올립니다.**  

> 1번을 올리면 먼저 올라오고 그 다음에 2번을 올리면 다음과 같이 에러가 발생합니다.

> ORA-01174: DB_FILES is 2000 buts needs to be 3000 to be compatible

spfile 안의  아래의 3000 으로 셋팅한 내용을 지워야 합니다. 


       1 db_files   2000       *
       1 db_files   3000       racdb1  <---------- 삭제해야함
       2 db_files   2000       *
       2 db_files   3000       racdb1  <---------- 삭제해야함

  SQL#1> alter  system   reset  db_files   scope=spfile  sid='racdb1'; 
  
  SQL#1> select  inst_id, name, value, sid
               from gv$spparameter
               where name='db_files';

  SQL#1> shudown  immediate
  SQL#2> shudown  immediate
  
  SQL#1> startup
  SQL#2> startup

**⚡ 문제1.  다음과 같이 장애상황을 만들어 놓고 해결하시오 !**

> spfile 안의 내용을 확인  

  SQL#1> select  inst_id, name, value, sid
               from gv$spparameter
               where name='processes';
  
           1 processes  300       *
           2 processes  300       *

  SQL#1> alter  system  set  processes=400  scope=spfile  sid='racdb1';  
  
  SQL#1> shutdown immediate  
  
  SQL#2> shutdown immediate  
  
  SQL#1> startup  
  
  SQL#2> startup  
  

>  😄 processes 파라미터는 반드시 양쪽 인스턴스가 똑같지 않다도 되는 파라미터 입니다.  
>      그래서 startup 할때 에러가 나지 않았습니다.

  SQL#1> select  inst_id, name, value, sid
               from  gv$spparameter
               where name='processes';
  
  SQL#1> show  parameter  processes    400
  SQL#2> show  parameter  processes    300

>  😄 인스턴스 이름으로 설정된 파라미터가 * 보다 우선순위가 높아서 1번 인스턴스에서  
>     show parameter processes 했을때 300 으로 보이지 않고 400으로 보이는겁니다.

**⚡문제2.  다시  아래의 내용을 spfile 에서 지우시오 !**


         2 processes  300        *
         2 processes  400        racdb1  <---- 삭제
         1 processes  300        *
         1 processes  400        racdb1  <---- 삭제

답: 
  SQL#1> alter   system  reset  processes   scope=spfile  sid='racdb1';
  
  SQL#1> select  inst_id, name, value, sid
               from gv$spparameter
               where name='processes';

