## ⭐⭐ RAC 핵심6. GRD 가 관리하는 서비스 2가지가 무엇인가 ?  ⭐⭐


<img src="https://github.com/oracleyu01/rac_class/blob/main/LMS.png" width="500" height="400">

1️⃣ **GSC ( Global  Cache  Service )**   --> 노드간의 데이터를 전송하는 서비스
    

   데이터를 전송하는 데몬 프로세서 :  LMS 

2️⃣ **GES ( Global   Enqueue Service)** --> 노드간에 발생하는 lock 을 관리하는 서비스
    

   노드간 락(enqueue) 을 담당하는 데몬 프로세서 : LMON


😊 이 두 서비스는 RAC 환경에서 데이터의 일관성과 동시성을 관리하는 데 중요한 역할을 합니다.
