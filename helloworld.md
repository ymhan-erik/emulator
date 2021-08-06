
# 1 외형 소개
![image](https://user-images.githubusercontent.com/45474081/128463223-d7fe784a-28f2-4129-8710-078767d291f4.png)

# 2 무작정 따라해보기 
   R4~R5 R6~L1을 서로 연결하고, 점보패킷을 플레이하고, 덤프해본다.  
         !R0, R1, R2, R3은 사용하지 않는 포트
       
   그림처럼 구성한다.
         
   ![image](https://user-images.githubusercontent.com/45474081/128464412-2c5a4299-6b45-47d0-a856-af756bf7bfa1.png)    
   좌측에서 부터,      
      터미널포트,   L1(광-쿠퍼커넥터), 이더넷포트, R4,R5,R6     
        R4~R5 R6~L1을 서로 연결하고, 점보패킷을 플레이하고, 각 포트로 수신한 패킷을 저장하여, pcap으로 저장해보자.
        
## 2-1 터미널에 접속한다.
   uart와 이더넷으로 접속이 가능하다.
      리눅스는 ttyUSB로 접속하거나, "ssh root@192.168.0.101" 명령으로 접속한다. 파일전송은 sftp로 하면된다.
      
      윈도우는 "테라텀" 또는 "putty" 로 "192.168.0.101"에 접속한다.  
      파일전송은 "filezillar"를 이용한다.
      
리눅스에서 에뮬레이터에 접속하면 다음 화면이 뜬다.  
cd /emulator  
ls  
     ![image](https://user-images.githubusercontent.com/45474081/128465425-094b95a9-3a5b-4871-ba71-0cb560cd4c10.png)  
   
      
## 2-2 ./emul_pcap_play 도움말을 쳐본다.  
![image](https://user-images.githubusercontent.com/45474081/128465632-15373c65-59bd-4b22-a7d4-fd5bfd2c3e11.png)  
    
    
## 2-2 점보패킷 bin 파일을 재생해본다.
./emul_pcap_play 1 tx_jumbo.bin 40 0 0 0  
![image](https://user-images.githubusercontent.com/45474081/128465744-7ceb0ed9-ccde-479f-8127-903404409717.png)  
   
위 그림처럼 나오면 정상이다.  
   
*tx_jumbo.bin을 40ms주기로 재생되는 pcap파일이기 때문에 '40'이 들어갔다*  
    
## 2-3 패킷 카운트를 살펴보자
./diag_pkinfo
    
![image](https://user-images.githubusercontent.com/45474081/128466084-5329e9e3-5ee7-4521-a2a7-2eaed67dcb7c.png)
  
  
  Tx frame number per 1-sec : 1초당 재생되는 프레임수를 나타낸다. 정상이면 min과 max는 같고, acc를 계속증가한다. 
    
  "Packet Count Check"를 보면  byte 카운트가 증가하면 정상이다.   
      포트를 서로 연결했기 때문에, tx/rx 포트의 bytecount가 비슷하게 증가한다.
      *rx쪽 VlanidMatch가 증가하면 packet을 덤프할수 있다.*  
      *이게 증가하지 않으면 덤프할수 있는 패킷이 없는 것이다. *  
      
## 2-4 패킷을  dram 덤프 해보자.  
./emul_capture 8 0
    
![image](https://user-images.githubusercontent.com/45474081/128467319-8871f2f9-3031-4952-954e-526520618f38.png)  
   
   
   
  그림과 같이 나오면 정상이다. 
  dram에 8x10ms만큼시간 동안 들어온 패킷이 저정됐다.
    
## 2-5 dram에 저장된 패킷을 pcap 파일로 만들어보자.
ls ./util
![image](https://user-images.githubusercontent.com/45474081/128466870-b26d37a0-c915-4d06-bcea-b10efc28d30a.png)  
  
  
./util/save_port0  
![image](https://user-images.githubusercontent.com/45474081/128466953-52b2c7f8-4ecc-4764-828f-7ec11adee6f0.png)  
   
   
   
ls ./util
![image](https://user-images.githubusercontent.com/45474081/128466982-70234dca-cb3f-44d7-9d83-0a257fbebb83.png)  
    
그림에서 *ultest_102_br0.pcapng*파일이 생긴것이 보일것이다.   
와이어샤크로 열어보면, 저장된 데이타를 볼 수 있다.
    

# 따리하기 끝, 궁금한 사항은 0105307798, erik@info-cube.co.kr로 문의하세요.









  
  
  
  
  
