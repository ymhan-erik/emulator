
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
  
  
  Tx frame number per 1-sec : 1초당 재생되는 프레임수를 나타낸다. 
      정상이면, 1초당 재생되는 프레임 수의 min과 max는 같고, acc는 계속 증가 한다.
    
  "Packet Count Check"를 보면  byte 카운트가 증가하는게 보일 것이다. 정상이다.   
       포트를 서로 연결했기 때문에, TX/RX 포트의 "bytecount"가 비슷하게 증가한다.
      RX쪽 "VlanidMatch"가 증가하면 packet을 덤프 할 수 있다.
      "VlanidMatch"가 증가하지 않으면 덤프 할 있는 패킷이 없는 것이다.
           
         만일, RX Byte 카운트는 크게 증가하는데, "VlanidMatch"가 증가하지 않으면 vlan.id와 vlan.type이 안맞는 경우이다.
         이 경우 "3. diag_hubsetvl 사용해보기"를 살펴보고 맞춰준다.
         
      
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
    

## 2-x 따리하기 끝, 궁금한 사항은 0105307798, erik@info-cube.co.kr로 문의하세요.

 
 
  
 
# 3. diag_hubsetvl 사용해보기.
rx 바이트카운트가 확!확! 크게 증가하는데, "VlanidMatch"가 증가하지 않는 경우에 이항을 참조한다.  
에뮬레이터에 덤프기능은 수신하는 패킷중에서 vlan.id, vlan.type이 맞는 uplan 패킷만 덤프 할 수 있다.

    "수신하는 패킷"은 rx Byte 카운트 증가시킨다.
    "수신하는 패킷중"에서 vlan.id, vlan.type이 맞는 패킷 : "VlanidMatch" 증가 <- 이것이 증가되면 덤프가 가능하다.ㅁㄴ

## 3-1. 캡처 할 패킷에 vlan.id = 0x3e8 vlan.type = 0xaefe 인 경우

  ./diag_hubsetvl -h  
  ![image](https://user-images.githubusercontent.com/45474081/128468953-4ffc2308-1a8b-450d-b123-7780cd0c68d4.png)  
    
  ./diag_hubsetvl 0 1 1 0x03E8 0xAEFE  
  ![image](https://user-images.githubusercontent.com/45474081/128468992-ddf7b9b7-9352-4591-b366-a84c7ba2c57d.png)  
     
     
## 3-1. 캡처 할 패킷에 vlan.type = 0xaefe 하나인데. vlan.id 가 4개인경우, 0x0000, 0x0010, 0x03e8, 00798  
  ./diag_hubsetvl **0** 1 1 0x0000 0xAEFE
  ./diag_hubsetvl **1** 1 1 0x0010 0xAEFE
  ./diag_hubsetvl **2** 1 1 0x03e8 0xAEFE
  ./diag_hubsetvl **3** 1 1 0x0798 0xAEFE
  
  다음 그림처럼 나오면 정상이다.
  ![image](https://user-images.githubusercontent.com/45474081/128469499-8d343ff9-440e-4517-910d-947a0cc70534.png)


## 이렇게 하고도  rx 바이트카운트가 확!확! 크게 증가하는데, "VlanidMatch"가 증가하지 않는 경우.
**들어오는 패킷에 vlan.type, vlan.id가 맞지 않거나, 다른 타입에 패킷이 들어오는 것이다**   
   
## 끝, 궁금한 사항은 0105307798, erik@info-cube.co.kr로 문의하세요.
  
  
  
