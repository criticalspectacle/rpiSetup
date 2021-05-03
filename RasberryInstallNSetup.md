Raspberry Pi Install & Setup
============================
* 준비물 : 라즈베리파이4, 5v전원, microSD카드, SD카드 리더기  
<br/>

A. 인스톨
-----------
0. [라즈베리 software](https://www.raspberrypi.org/software "라즈베리 소프트웨어 다운")에서 raspberry Pi Imager 다운
<br>

1. 빈 microSD카드 컴에 연결해서 Raspberry Pi Imager 실행  

   <img width="400" alt="RasberryPiImagerIcon" src="https://github.com/criticalspectacle/collaboration/blob/JISU/RasberryInstallSetup/img/A_01.png?raw=true">
<br>

2. microSD카드 포멧 후 os 설치 => Operating System(Rasberry PI OS), SD card지정해주고, write하기
   <img width="400" alt="RasberryInstallSetup" src="https://github.com/criticalspectacle/collaboration/blob/JISU/RasberryInstallSetup/img/A_02.png?raw=true">
<br>

3. (1) microSD카드 boot 파일에 ssh파일 / conf파일 추가  
    <img width="400" alt="Addssh&confFiles" src="https://github.com/criticalspectacle/collaboration/blob/JISU/RasberryInstallSetup/img/A_03-1.png?raw=true">


   (2) conf 파일 이름은 wpa_supplicant.conf 로 저장하고 내용은 다음과 같이 설정  
    

```javascript
        country=KR // 🔥대한민국으로 설정하는 것 중요함🔥 
        ctrl_interface=DIR=/var/run/wpa_supplicant
        GROUP=netdev 
        update_config=1 
        network={ 
        ssid="와이파이이름" 
        psk="와이파이비밀번호" 
        scan_ssid=1 
        }  
```
  
<img width="400" alt="ConfFileText" src="https://github.com/criticalspectacle/collaboration/blob/JISU/RasberryInstallSetup/img/A_03-2.png?raw=true">  
 <br> 


4.  microSD카드를 라즈베리파이에 옮기고 라즈베리파이에 전원을 넣은 뒤, 무선 공유기나 라우터에 접속하여 와이파이가 연결되었는지 확인한다.  
   
   (1) 무선 공유기, 라우터 접속은 내부 네트워크에서 [세자리.세자리.0.1] 또는 [세자리.세자리.219.1]로 접속할 수 있다.  

    
<img width="400" alt="WifiSetup" src="https://github.com/criticalspectacle/collaboration/blob/JISU/RasberryInstallSetup/img/A_04.png?raw=true">  
 

   (2) 무선 공유기나 라우터에 여러 기기가 접속할 수 있기때문에 라즈베리파이 전원을 켜기전 미리 무선 연결되어 있는 기기 리스트를 확인해두는 편이 좋다.  

   (3) os 설치 후 최초 연결된 라즈베리 파이이름은 _raspberryPI_로 설정되어 있다.  
<br>

5. host name / user name 변경
   
   (1) 최초 설정된 host name은 ip , user name은 raspberryPI로 되어있다. (비번은 raspberryPI) 따라서 보안을 위해 바꿔주는 것이 좋다.

   (2) 현재 host name 확인
   ``` c
   $ hostname
   ```  

   (3) 이전 호스트명 지우고 새로운 이름 입력하고 저장 
   ``` c
   $ sudo vi /etc/hostname
   새로운 이름

   $sudo vi /etc/hosts
   새로운 이름
   ```  

   (4) 리부팅하면 새로운 hostname 으로 설정됨 (결론 : 호스트 명 변경하기 위해서는 /etc/hostname, /etc/hosts 파일을 수정)
    ``` c
   $ sudo reboot
   ``` 
   
<br>

> ### **-> 와이파이 연결이 되었다면 C로, 되지않았다면 모니터와함께 B로 간다.**    
<br>

B1. 라즈베리 파이 로컬 셋업
--------------

1. 패스워드 바꾸기 (1. system option-> 3. Password) - 1234
2. 와이파이 셋팅하기 (1. System option -> 1. Wireless LAN -> 연결되어 있는 wifi이름입력) 
  https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md

    (1) 접속가능한 와이파이 체크 !  
       `$ sudo iwlist wlan0 scan`

    (2) 국가 설정해주기 (5. Localisations on Options -> L4. WLAN Country → 한국으로 해줘야됨 ! 1-4 KR korea(South) )

      <img width="400" alt="ScodeSSH02" src="https://github.com/criticalspectacle/collaboration/blob/main/RasberryInstallSetup/img/b1_01_country.png?raw=true">
    
    (3) SSID(ex.iptime) / SSID PW 설정하기
      
      <img width="400" alt="ScodeSSH02" src="https://github.com/criticalspectacle/collaboration/blob/main/RasberryInstallSetup/img/b1_02_ssid.png?raw=true">
    
    (4) 셋팅 됐는지 확인해주기(연결된 와이파이체크)  

       `$ sudo nano /etc/wpa_supplicant/wpa_supplicant.conf`

    (5) 와이파이 연결 테스트하기  

       `$ ping 8.8.8.8`

    (6) 안되면 reboot 하기  

       `$ sudo reboot`

    (7) 내 ip 확인  

       `$ ifconfig`  

       ssh server(3. Interface Option -> P2. SSH -> enabled (독일에 있음(원격이 되는 최소한의 조건 완성))-> 서버 열어주기

B2. 라즈베리 파이 원격 셋업
--------------
 Terminal 열어서 
1. ssh 라즈베리파이 아이디+라즈베리파이 local주소  
         ex) ssh pi@192.168.0.155
2. 엔터 → 라즈베리 파이 비밀번호 치기 (원격이 되는 최소한의 조건 완성, 내컴에서 ssh 서버로 들어간 거)  
   
   (1) 에러 발생 가능성

   <img width="400" alt="ScodeSSH02" src="https://github.com/criticalspectacle/collaboration/blob/main/RasberryInstallSetup/img/b2_01_error.png?raw=true">

   (2) 해결

   `$ ssh-keygen -R 서버이름 또는 IP주소`

3. 구성되어 있는 리눅스 업데이트 해주기  
   <https://www.raspberrypi.org/documentation/raspbian/updating.md>     
   `$ sudo apt update`

4. 업그레이드  
    `$ sudo apt full-upgrade`


C1. 라즈베리파이에 최신 nodejs 설치하기
--------------------------------
* 참고 : <https://github.com/nodesource/distributions>

1. 앞선단계에서 한 OS 최신 업데이트 중요 
   * ```$ sudo apt update``` & ```$ sudo apt full-upgrade```
2. node js 깔아주기
   * ```$ curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo bash -```
   * ```sudo apt install -y nodejs```
3. node js, npm 버전 확인하기
   * ```node -v```
   * ```npm -v```

C2. Node http 서버 띄우기
-----------------------
1. 디렉토리 만들기
   * ```$ kdir httpserver```
   * ```$ cd httpserver```
2. ```$ npm init```(opt.실행파일 이름은 server.js로)
3. ```$ npm i express```
4. ```$ sudo nano server.js``` 
   * sudo nano 수정하려는 파일명
   * server.js 코드 작성
<pre>
<code>
const express = require('express')
const app = express()
const port = 3000
app.get('/', (req, res) => {
res.send('Hello World!')
})
app.listen(port, () => {
console.log(`Example app listening at http://localhost:${port}`)
})
</code>
</pre>
5. 실행  ```$ node server.js```
  
C3. VS 코드로 라즈베리 파이에 연결하기
------------------------------
* 참고: <https://code.visualstudio.com/docs/remote/ssh>
1. vs code 에서 remote development 익스텐션 깔아준다.
2. f1 -> Remote-SSH: Connect to Host 선택
   
    <img width="300" alt="c_3_4" src="https://user-images.githubusercontent.com/34053864/115983393-c109d500-a5db-11eb-8cdb-8a27d9dcb449.png">
3. piUsername@ipAdress (예.pi@192.168.0.155)치고 엔터
4. rasberry pi의 password 입력-> 접속
* (참고 : vscode terminal = terminal)
  
   <img width="400" alt="c_3_2" src="https://user-images.githubusercontent.com/34053864/115983392-bfd8a800-a5db-11eb-9b82-baae2620db51.png">
  
- vscode 수정권한요청 ```$ sudo chown -R username  *```
  + VSCode에서 ssh로 rasberryPi 연결하고 vscode에 파일 내용 변경저장시,
  + 권한 없다고 한다면, username으로 소유자를 변경해주기 

