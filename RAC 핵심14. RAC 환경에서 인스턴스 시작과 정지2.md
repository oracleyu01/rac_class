
## ⭐⭐ RAC 핵심14. RAC 환경에서 인스턴스 시작과 정지  ⭐⭐

## **1️⃣ shudown 옵션 4가지**

  1. shutdown  normal
  2. shutdown  transactional  -->  누군가 DML 작업을 하고 있으면 인스턴스가   
                                                       &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   안내려가고 commit 이나 rollback 을 해서  
                                                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; transaction 을 종료해야 내려갑니다.   
  3. shutdown  immediate
  4. shutdown  abort

 **그런 데  rac 에만 있는 shutdown 옵션이 있습니다.**

      shutdown  transactional  local 

> 다른 인스턴스에 DML 작업이 끝날 때까지 기다리지 않고   
> 내가 내리려는 인스턴스에 DML 작업이 없으면 그냥
> 인스턴스를 내리겠다는 옵션입니다.

     shutdown  transactional    

 그림설명 : https://cafe.daum.net/oracleoracle/SoJX/66

  
> 1번 인스턴스를 shutdown transactional 로 내리려면 1번 , 2번 둘다    DML 작업을
> 하고 있는 세션들이 다 commit 이나 rollback 을 해야   내려갑니다.  그런데 shutdown
> transactional local 이라고 하면   1번 인스턴스쪽에 transaction 만 종료되었으면 인스턴스를
> 내립니다.

  
  **⚡ 실습1. rac 환경에서 shutdown trasactional 실습** 

scott 유져를 생성하고 demobld.sql 를 돌립니다.*

    SYS> create  user  scott   identified  by tiger;
    SYS> grant  dba  to scott;
    SYS> connect  scott/tiger
    SCOTT>  @demobld.sql

  &nbsp;  
  &nbsp;  
  
#1. 1번노드와 2번 노드에서 둘다 scott 유져로 접속합니다.**

#2. 1번 노드에서는 KING 의 월급을 9000으로 변경하고  2번 노드에서는 ALLEN 의 월급을 8000으로 변경합니다. 그리고 아직 둘다 COMMIT 하지 않습니다.

#3. 새로운 터미널 창을 열어서 SYS 유져로 1번 노드에 접속하여 shutdown  transactional 을 수행합니다.

#4. 양쪽 노드에 scott 으로 접속한 세션들에서 commit 을 수행합니다.

#5. 그러면 내려가는지 확인합니다.*

#6. 다시 1번 인스턴스를 올립니다.*
  &nbsp;
    &nbsp;
      &nbsp;
        &nbsp;

  ⚡ **문제1.  이번에는 2번 인스턴스에만 scott 으로 접속해서 JONES 의 월급을 7000으로 변경하고  
  1번 인스턴스를 shutdown transactional 이라고 하면 내려가는지
  확인하시오 !** 


  &nbsp;
  &nbsp;


⚡ **문제2.  shutdown  transactional local 을 직접 테스트 해보시오 ~**
