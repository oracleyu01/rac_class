
## ⭐⭐ RAC 핵심16. RAC 환경에서 초기화 파라미터 파일 관리⭐⭐


### 1️⃣ RAC 환경 에서의 파라미터 파일의 위치는 ?

<img src="https://github.com/oracleyu01/rac_class/blob/main/init.png" width="500" height="400">

> 1번 노드, 2번 노드 둘다  동일한 spfile 을 바라봐야합니다. 
> 1번 노드 따로 2번 노드 따로 두고 관리하게 되면 문제가 생깁니다. 

## **💎실습1**   

**1️⃣ 1번 노드와 2번 노드의 인스턴스가 동일한 spfile 을 사용하고 있는지 확인하기**

    SQL#1> show  parameter  spfile
    
    SQL#2> show  parameter  spfile  

 &nbsp;

**2️⃣ 1번 노드와 2번 노드에서 각각 ORACLE_HOME 밑에 dbs 밑으로 이동해서 인스턴스이름.ora 가 있는지 확인하고 있으면 열어서  
&nbsp;&nbsp;&nbsp;내용을 확인하시오 !**  
     &nbsp;
      &nbsp;


**3️⃣  spfile 의 내용을 보면 1번 인스턴스를 위한 파라미터가 있고 또 2번 인스턴스를 위한 파라미터가 있고 모든 인스턴스를 위한  
&nbsp;&nbsp;&nbsp;파라미터가 있습니다.** 

     col  name for a10
     col  value  for a10
    
     select  inst_id, name, value
       from  gv$parameter
       where  name='db_files';

>     😄  rac 환경에서는 양쪽 인스턴스 둘다 똑같은 값으로 설정해야하는 파라미터가 있고
>         양쪽 노드 둘다 다르게 설정해야하는 파라미터가 있습니다.  이걸 안지켜주면
>         하나의 인스턴스는 올라오는데 다른 인스턴스가 안올라오게 되니 반드시 지켜줘야합니다.

**★ 양쪽 노드 둘다 똑같은 값으로 설정해야하는 파라미터**

     select  inst_id, name, value, ordinal    
        from gv$spparameter                                    
        where name='파라미터 이름'; 

  

>   ordinal 이 1은 양쪽다 똑같이 셋팅

**★ 양쪽 노드 둘다 다르게 설정해야하는 파라미터** 

     select  inst_id, name, value, ordinal    
        from gv$spparameter                                    
        where name='파라미터 이름'; 

  

>   ordinal 이 1은 양쪽다 똑같이 셋팅

## **💎실습2**

> db_files 는 양쪽 노드 둘다 똑같이 설정해야하는 파라미터 입니다.  
> 이 파라미터는 database 에서 생성할 수 있는 파일들의 최대 갯수를 지정하는 파라미터  
> 입니다. 그런데 디폴트 값인 200개는 요즘 시대에는 너무 터무니 없이 작습니다.  
> 그래서 이 값을 2000개로는 늘리는 실습을 하겠습니다.  

**1️⃣ 1번 노드와 2번 노드에서 각각 db_files 가 어떻게 셋팅되어있는지 확인합니다.**

-- 현재 인스턴스에 반영된 상태를 확인 

SQL#1> show parameter db_files
SQL#2> show parameter db_files 
SQL#1> select  inst_id, name, value 
             from gv$parameter
             where name='db_files';

-- 현재 spfile 에는 어떻게 셋팅되어있는지 확인 

SQL#1> select  inst_id, name, value 
             from gv$spparameter
             where name='db_files';

> 설명: value 에 값이 없으면 default 값으로 셋팅되어 있는것입니다. 

**2️⃣ db_files 를 200 에서 2000 으로 수정합니다.** 

SQL#1> alter  system   set   db_files=2000  scope=spfile   sid='*';

**3️⃣ 양쪽 노드를 shutdown immediate 로 내렸다가 올립니다.** 

SQL#1> shutdown immediate
SQL#2> shutdown immediate

SQL#1> startup
SQL#2> startup

**4️⃣ 잘 반영되었는지 확인합니다.**

SQL#1> select  inst_id, name, value 
             from gv$spparameter
             where name='db_files';

SQL#1> select  inst_id, name, value 
             from gv$spparameter
             where name='db_files';


**😄 양쪽다 똑같이 셋팅하려면 ?   아래 처럼 sid='*' 을 써야합니다.** 

SQL#1> alter  system   set   db_files=2000  scope=spfile   sid='*';

**😄 다르게 한다면 ?**  
 
SQL#1> alter  system   set  undo_tablespace='undotbs1'  scope=spfile   sid='racdb1';
SQL#2> alter  system   set  undo_tablespace='undotbs2'  scope=spfile   sid='racdb2';  
&nbsp;

**⚡문제.  현재 db 에 띄울 수 있는 프로세서의 갯수가 150개 밖에 안됩니다. 이 갯수를 300개로 늘리세요.**   
 

> 파라미터는 processes 입니다.  양쪽 노드 둘다 300개로 늘리세요 ~

    SQL#1> select  inst_id, name, value 
                 from gv$spparameter
                 where name='processes';
    
    SQL#1> select  inst_id, name, value 
                 from gv$spparameter
                 where name='processes';
