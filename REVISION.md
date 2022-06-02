

## 에뮬레이터 버전에 따른 구분
#### 버전확인 방법 1  
![image](https://user-images.githubusercontent.com/45474081/128462546-b9472dde-bcf5-48af-85da-e1e0f3f2372c.png)



#### 버전확인 방법 1  
![image](https://user-images.githubusercontent.com/45474081/128462478-46df621c-343d-4c3b-90f0-78c4f8c7c0d1.png)


#### HW버전 간 차이점
  HW v1.x : 2020년 10월 이전에 납품된 모델로 단종  --> SW/FW 업데이트가 안됨.  
  HW v2.x 이상 : --> SW/FW 업데이트 가능  

**SW/FW Version 3.4**  
      FPGA Bitstream Date     : 20220602   
      FPGA Bitstream Version  : 94034000 (v3.4)     

**SW/FW Version 3.3**  
      FPGA Bitstream Date     : 20220421   
      FPGA Bitstream Version  : 00033000 (v3.3)   
      
**SW/FW Version 3.2**  
      FPGA Bitstream Date     : 20220317  
      FPGA Bitstream Version  : 00032000 (v3.2)
      
**SW/FW Version 3.0**  
      FPGA Bitstream Date     : 20210804  
      FPGA Bitstream Version  : 00030002 (v3.0)
      
**SW/FW Version 2.x**  
      FPGA Bitstream Date     : 20201013  
      FPGA Bitstream Version  : 10021000 (v2.1)
 
**SW/FW Version 1.x**  
      FPGA Bitstream Date     : 20201013  
      FPGA Bitstream Version  : 10011000  (v1.1)


|FW/SW버전|HW버전|점포패킷지원|PTPMaster|ClockCFG|etc|
|------|:---:|:---:|:---:|:---:|:---:|
|1.x|v1.x|x|x|4v1| |
|2.x|v2.0|x|x|4v1| | 
|3.0|v2.0|지원|지원|7V3| |
|3.2|V2.0|지원|지원|7V3| |
|3.3|V2.0|지원|지원|7V3| 25G/10G 지원. 25G 명령 추가|
|3.4|V2.0|지원|지원|7V3| 단일클럭도메인|
