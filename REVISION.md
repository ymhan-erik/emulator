

## 에뮬레이터 버전에 따른 구분
#### 버전확인 방법 1  
![image](https://user-images.githubusercontent.com/45474081/128462546-b9472dde-bcf5-48af-85da-e1e0f3f2372c.png)



#### 버전확인 방법 1  
![image](https://user-images.githubusercontent.com/45474081/128462478-46df621c-343d-4c3b-90f0-78c4f8c7c0d1.png)


#### 버전 간 차이점
  2020년 10월 이전에 납품된 모델은 v1.x
  2021년  8월 이전에 납품된 모델은 v2.x or v3.0
  2022년  1월 이후에 납품된 모델은 v3.2

**Version 3.2**  
      FPGA Bitstream Date     : 20220317  
      FPGA Bitstream Version  : 00032000 (v3.2)
      
**Version 3.0**  
      FPGA Bitstream Date     : 20210804  
      FPGA Bitstream Version  : 00030002 (v3.0)
      
**Version 2.x**  
      FPGA Bitstream Date     : 20201013  
      FPGA Bitstream Version  : 10021000 (v2.1)
 
**Version 1.x**  
      FPGA Bitstream Date     : 20201013  
      FPGA Bitstream Version  : 10011000  (v1.1)


|버전|HW|점포패킷지원|PTPMaster|ClockCFG|etc|
|------|:---:|:---:|:---:|:---:|:---:|
|1.x|v1.x|x|x|4v1| |
|2.x|v2.0|x|x|4v1| | 
|3.0|v2.0|지원|지원|7V3| |
|3.2|V2.0|지원|지원|7V3|  22년2월 생산분에서 sysclk_pps와 ptpclk_pps 간 동기가 안되는 문제 해결 |
