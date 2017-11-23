### Våra produktionservrar

#### skarp sql server 94.103.206.202 192.168.80.12

Login windows: administrator / Invirt2014 Login sqlServer: sa / yaiN5dei

#### produktion web 204 94.103.206.204 192.168.80.14

Login windows: administrator / Knaip7co

#### produktion web 205 94.103.206.205 192.168.80.15

Login windows: administrator / Invirt2014

#### Apisci server 94.103.206.206 192.168.80.16

Login windows: administrator / Invirt2014

#### produktion linux 94.103.206.203 / 192.168.80.13
User: ezy
Pass: Valdigtlangtlosenord
#### Feriehus

Administrator:koalaLampa2014

`{br}`

192.168.80.10 = 94.103.206.200 feriehus-web1{br} 192.168.80.11 =
94.103.206.201 feriehus-sql1{br}

#### euromaster produktion 94.103.206.209 / ?

Login windows: administrator / Invirt2014

#### För att nå Prodserverna hemmifrån

Inloggninssidan finns på adress, https://94.103.206.194:4100 User:
ezyconny Pass: kuligasessioner

### Våra Testservrar

#### testserver1.ezyserver.se 94.103.199.200 192.168.20.120

Login windows: manager / Inside150999

#### build.ezyserver.se  94.103.199.208 192.168.20.130

Login windows: manager / Inside150999 lokala kontot:
Administrator/InsideBuild4321

#### testserver2.ezyserver.se  -- --94.103.199.201 192.168.20.140

Login windows: manager / Inside150999

#### testserver4.ezyserver.se  -- --94.103.199.202 192.168.20.150

Login windows: manager / Inside150999  
Login windows: administrator / Insideml310

#### testserver7.ezyserver.se  94.103.199.204 192.168.20.90

login: manager / Inside150999 MySql   root password: grim1017

#### documents 94.103.199.203 192.168.20.222

Login windows: manager / Inside150999  (ezy domän)  

#### sql.ezyserver.se/sql14 94.103.199.203 192.168.20.141
###  WHEN CREATING NEW DB:  https://github.com/EzyWebwerkstaden/Wiki/wiki/CREATE-NEW-DB-ON-sql.ezyserver.se

Login windows: administrator / Insideml310

login sql:** ezyMasterlogin / ?#F3Rppz

https://github.com/EzyWebwerkstaden/Wiki/wiki/DB-logins-sql.ezyserver.se

(GAMLA 120:an ligger kvar pga. SQLserver 2005)

#### sql.ezyserver.se 94.103.199.209 192.168.20.122

Login windows: manager / Inside150999 login sql: sa / ezykl2005

(GAMLA 150:an ligger kvar pga. SQLserver 2005)

#### Results Test / 94.103.199.213 / 192.168.20.135
manager / Inside133999

#### sql.ezyserver.se  94.103.199.222 192.168.20.152

Login windows: manager / Inside150999 login sql: sa / Grim1017   

#### ELK logging 192.168.20.133 /  94.103.199.211 / linux server¶

ezy/fullfart   

  [https://94.103.206.194:4100 ]: https://94.103.206.194:4100%C2%A0

####  / 94.103.206.212 / 192.168.80.22  / prod mysql mariadb
Inlogg, ezy:numaji ( db login: root / X0Ij7WcT3Yh35XD20eJE )
Ipadress, 94.103.206.212

### Hård omstart
Ezy:Longt2014
https://ezystat.westhost.se/webreboot.asp

### smtp.ezyserver.se - linux ubuntu  
Denna servern används som utskicks server.

`root/grim1017`

För att skapa upp nya mailboxar/alias osv http://smtp.ezyserver.se/postfixadmin

patrik@ezy.se / grim1017

webmail för mailboxarna skapade i postfixadmin http://smtp.ezyserver.se/squirrelmail  

##### För att kolla loggen  
Så kan man SSH till smtp.ezyserver.se root/grim1017  
Sen är de bara att skriva trail /var/log/mail.*  
##### To add new ip whitelist
SSH to   smtp.ezyserver.se `root/grim1017`  
in ssh: `nano /etc/postfix/main.cf` and add ip to line mynetworks at the end