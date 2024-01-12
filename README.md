# SOC LAB4-2
解說YOUTUBE ADDRESS   
https://www.youtube.com/watch?v=3IaNn1kP8xA&list=PLTA_T2FLzYNAvdvpEQvTjXaPARtmV-0GM&index=14
## LAB4-2 簡單實作
#### 實作要點
1. lab3 fir & lab4-1 exmem-fir intergrete user_project with "wishbond" interface
2. 執行 RISC-V FIR (user_project裡面)
這裡的FIR 和 lab4-1(CPU 執行) & lab3(testbench)執行步驟不同，lab4-2由firmware控制hardware執行

## 架構
#### 0x3800 for exmem-FIR
#### 0x3000 for FIR-AXILITE(BRAM) & FIR-AXISTREAM(BRAM)
![image](https://hackmd.io/_uploads/H1XmFeB_T.png)

![image](https://hackmd.io/_uploads/BJjNHMhOT.png)


## 程式碼解說
### 1.user_poject_wrapper
因為定義關係將wbs_adr_i[31:16] 分為 0x3800 and 0x3000
並且分別定義2種io pin for different "fir"
![image](https://hackmd.io/_uploads/ByZdNfhda.png)

### 2.user_project  

> wishbond 和 axi-lite 和 axi-stream溝通介面
定義wb and axi 的ADDRESS

![image](https://hackmd.io/_uploads/ByNYsm3uT.png)

#### WB-AIX 的訊號對街口定義
#### using AXI-LITE & AXI-STREAM 溝通(這裡是最重要的精髓)
![image](https://hackmd.io/_uploads/BJ5Ao7hup.png)






AXI-LITE READ & WRITE  
![image](https://hackmd.io/_uploads/rkMX2mnu6.png)
![image](https://hackmd.io/_uploads/rykN3m2dp.png)

AXI-STREAM  
![image](https://hackmd.io/_uploads/HkZDnQ3dp.png)
### 3.user_project_example.counter
write an interface for valid&ready inside the counter of 10  

![image](https://hackmd.io/_uploads/H1P6nXh_T.png)
### fir_dut
including tap_bram & data_bram   

![image](https://hackmd.io/_uploads/ry5fkX3Oa.png)


## Interface Protocol between firmware, user project and testbench 
#### firmware 透過 wishbond 跟user_project 溝通
#### Testbench 透過flash control 將資料傳達給CPU執行firmware 

### Testbench: code
![image](https://hackmd.io/_uploads/Bkkfk-S_T.png)

### Firmware(參考fir.c):
![image](https://hackmd.io/_uploads/B1dSxbSOT.png)
#### 程式前面定義各個ip的位置，以下解說程式
```
112 :
reg_mprj_datal 有32bit，前16 -> CPU ; 後16 -> user_project 
因為在cpu運作所以需要左移16bits
```
```
125:
ap_start signal 
```
```
127~147 :
用bits_control => ap_start , ap_done, ap_idle
這邊的訊號會透過mprj傳輸到fir.v
```
## Waveform - Hardware
3800 for exmem-fir
![image](https://hackmd.io/_uploads/SkIItAjua.png)

0x3000 for FIR-AXILITE(BRAM) & FIR-AXISTREAM(BRAM)
![image](https://hackmd.io/_uploads/Byfwffh_a.png)

## Waveform - Software
Firmware 放在SPI Flash 運行
![image](https://hackmd.io/_uploads/rJ-jp7h_T.png)


將Firmware 放在User RAM運行 (0x3800_xxxx)
![image](https://hackmd.io/_uploads/BksP672OT.png)



## LAB4 實驗步驟
LAB4基礎知識 https://hackmd.io/@861r6vBbQbu3FLGNe3SkyQ/SkC0vJ1UT  
LAB4-0 https://hackmd.io/@861r6vBbQbu3FLGNe3SkyQ/SkC0vJ1UT  
LAB4-1 https://hackmd.io/@861r6vBbQbu3FLGNe3SkyQ/HybFQWkIT  
LAB4-2 https://hackmd.io/@861r6vBbQbu3FLGNe3SkyQ/BJXPryyIp 


