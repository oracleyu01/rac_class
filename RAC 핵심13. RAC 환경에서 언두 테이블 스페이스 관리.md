
## ⭐⭐ RAC 핵심13. RAC 환경에서 언두 테이블 스페이스 관리   ⭐⭐

'그림설명

<img src="https://github.com/oracleyu01/rac_class/blob/main/undo.png" width="500" height="400">

 1. redo data  :   복구할 때 필요한 데이터
 2. undo data :    취소할 때 필요한 데이터 (rollback 할때)

※ rac 환경에서는 undo data 를 별개의 테이블 스페이스에 따로따로 write 합니다.

 쓰기는 따로따로 쓰지만 어느 인스턴스에서는 둘다 똑같이 읽기는 가능해야합니다.

**⚡ 실습**  
&nbsp;

#1. 1번 노드에서 사용하는 undo tablespace 가 뭔지 확인합니다.  
#2. 2번 노드에서 사용하는 undo tablespace 가 뭔지 확인합니다.  
#3. undotbs3 이라는 undo tablespace 를 생성합니다.   
#4. undotbs3 을 1번 인스턴스용 undo tablespace 로 지정합니다.   
&nbsp;

**구현:**  
&nbsp;
**#1. 1번 노드에서 사용하는 undo tablespace 가 뭔지 확인합니다.**

SQL#1>  show  parameter  undo_tablespace  
&nbsp;

**#2. 2번 노드에서 사용하는 undo tablespace 가 뭔지 확인합니다.**

SQL#2> show  parameter  undo_tablespace  
&nbsp;

**#3. undotbs3 이라는 undo tablespace 를 생성합니다. **

SQL#1>  create  undo  tablespace  undotbs3  
               datafile   '+DATA'   size 50m;  

SQL#1> select tablespace_name, file_name from dba_data_files;  
&nbsp;

**#4. undotbs3 을 1번 인스턴스용 undo tablespace 로 지정합니다. **

SQL#1> select instance_name from v$instance; 

SQL#1> alter  system  set  undo_tablespace='undotbs3'  sid='racdb1' ;

 설명:  
 sid='racdb1'  <--- 1번 인스턴스용 parameter를 변경해라!  
           sid='racdb2'   <--  2번 인스턴스용 parameter 를 변경해라!  
           sid='*'           <--- 모든 인스턴스용 parameter 를 변경해라!  

SQL#1> show  parameter undo_tablespace

SQL#2> show  parameter undo_tablespace  
&nbsp;

**문제1.  undotbs4 라는 undo tablespace 를 사이즈 50m 로 생성하고  
      2번 인스턴스용 undo tablespace 로 지정하시오 !**

SQL#2>  create  undo  tablespace  undotbs4
              datafile   '+data'  size  50m;

SQL#2> alter  system  set  undo_tablespace='undotbs4'  sid='racdb2'; 

SQL#2> show  parameter undo_tablespace  
&nbsp;

**문제2.  1번 인스턴스의 undo tablespace 를 다시 undotbs1 로 변경하시오**

SQL#1> alter  system  set  undo_tablespace='undotbs1'  sid='racdb1';

SQL#1> show  parameter  undo_tablespace  
&nbsp;

**문제3.  2번 인스턴스의 undo tablespace 를 다시 undotbs2 로 변경하시오 !**

SQL#2> alter  system  set  undo_tablespace='undotbs2'  sid='racdb2';

SQL#2> show  parameter  undo_tablespace  
&nbsp;

**문제4.  undotbs3 테이블 스페이스를 drop 하시오 !**

SQL#1> drop   tablespace  undotbs3  including  contents  and  datafiles;   
&nbsp;

**문제5. undotbs4 테이블 스페이스를 drop 하시오 !**

SQL#2> drop  tablespace  undotbs4  including  contents and datafiles;  
&nbsp;




😊 위의 내용을 .bash_profile 에 설정한 화면 캡쳐해서 답글 올리시고 검사받으시면 됩니다.


