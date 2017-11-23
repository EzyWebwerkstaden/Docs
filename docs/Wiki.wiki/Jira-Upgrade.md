Log in via SSH to the linux jira server:  
`ec2-52-19-221-56.eu-west-1.compute.amazonaws.com` -> use _jira-ec2.ppk_ for auth  
login as: _ubuntu_

In Amazon create snapshot of EC2 instance of Jira
Snapshots -> Create snapshot -> Chosse volume and name

Download the latest linux bin for jira on the server.
First get the link to the latest update for Linux 64bit, wget the file:    
`wget https://www.atlassian.com/software/jira/downloads/binary/atlassian-jira-software-7.5.0-x64.bin`  

Then make sure the files gets change permission:  
 `chmod a+x atlassian-jira-software-7.5.0-x64.bin`

Execute the file:  
`sudo ./atlassian-jira-software-7.5.0-x64.bin` 

Choose to upgrade an existing JIRA installation (3 or enter).  
Default directory is _/opt/atalssian/jira_ so just press enter.  
Do not backup the JIRA home directory because it will take along time and we have already a snapshot.  

When the installation is done we need insert a snippet in _server.xml_ that allows us to use https:  
`sudo nano /opt/atlassian/jira/conf/server.xml`  

Insert snippet inside the Catalina ### ### Service: 
```
   <Service name="Catalina">

        <Connector port="8081"

                   maxThreads="150"
                   minSpareThreads="25"
                   connectionTimeout="20000"

                   enableLookups="false"
                   maxHttpHeaderSize="8192"
                   protocol="HTTP/1.1"
                   useBodyEncodingForURI="true"
                   redirectPort="8443"
                   acceptCount="100"
                   disableUploadTimeout="true"
                   bindOnInit="false"/>

      #INSERT SNIPPET HERE
```
Https snippet:
```
<Connector acceptCount="100" connectionTimeout="20000" disableUploadTimeout="true" enableLookups="false" 
maxHttpHeaderSize="8192" maxThreads="150"
minSpareThreads="25" port="8080" protocol="HTTP/1.1" redirectPort="8443" useBodyEncodingForURI="true"
scheme="https" proxyName="jira.ezy.se" proxyPort="443" secure="false"/>
```

Start JIRA with `sudo service jira start`

Done 😺 

Troubleshoot after startup by looking at the live logs.  
`sudo tail -f /opt/atlassian/jira/logs/catalina.out`

_jira-ec2.ppk_

```
PuTTY-User-Key-File-2: ssh-rsa
Encryption: none
Comment: imported-openssh-key
Public-Lines: 6
AAAAB3NzaC1yc2EAAAADAQABAAABAQDGS5BTw4gYLjk2Vn2opTT8O55wMTMd4/jK
L9txAa4H7aC3GArGKD4SU1DjW2WlyE1a0tERysipV6Dwi6qPT50sKBKAgxAOE0KW
dqtg46aleNnJLsOSW7EfchVdWkwKa2hqcWAbXHhfYSg9+x0VU6Q8PVixQDxGm9At
e7HlPwnAzfFoFeJjoqWYtoE/8McUYxptZBO2FZV+Y0k+PICx/kdr5tiaqvUrBKGp
6PXC5m5WB3M80BDUbeWwuSzAnupk+0m5OqotG8A38+TCeubqZhBU71Tx44rryppv
qS3ud22N8g2Vs3GlCCWt3pnQnkOj79zobt80wFkJ/1gTfbOw0Xyl
Private-Lines: 14
AAABAQC7CufWuyeSd3dr8+WPwFXcXngyp5Xp6fHw9XdufwEjZVglnL388FSsgH5a
FwooeGnO8/BVcgwZABlFDNkTWSuOTTY2lmn8fNpghwtc1x+IEhiMFlpPocHPQZM5
fUUgxvO1A8B9zpmUjlahME7coQrX753LhCXXDH4viO/ip7WRcIfpYqFkOwomRS44
W8eZAOkj2uXBeQ3K9rgESh8tGxv7xXsQg5s9D6NWvUtonKnAmOfB9hH8uGs70zOQ
DXElo22RvBmwm4FLVIuC7YVMOmnLl939wxwluyFt7DpuMvsW0VERhKlr0q4NogOH
5MBU0l05eq6HTgaaZK5GZoCI7Ln5AAAAgQDiTSnYmnilg7ChTAi8YLCDX37LazMe
QN+O6BlZiSaLt9shwq6dFFvDjzh7zI/GCbtO9gUgGXHmnFtMbDaKoIHORkVszVSA
hLKJd0feKqOIiIR5WX7nQaDNjxUpbue34C4UrU5CXKMngflOh1lhuOGZcbSopGxH
f9z/wa5Spq4vswAAAIEA4FGAKlLddhegWw4+j0hybyWIi9VyCRnl8cykGGDyQ/P6
yjU5135Z85ITw9Mr1gUTFTtuFjrQGZlFr52pQvT3KYxPyxloApsfAZCNPJKr/Fkc
jtPUyHfVkm60iGB2pnCl7r3qgrFOlM1Ar2/lnAAIN5qj4kHs1D1vHJDm6eiDtkcA
AACALoZa0QbyqA2J6760l95beswrO7Sj4iPxioyGi9ltH3MmmDOJS//uueO5s07o
utGLE8k4j3xyb5zdvS3PcPyISeulAIFTZik7MANu/CVsijxEHrlOcyLbxGucQMiT
XRkHLGyQMVxxHxHc3xDlWyHWoYwNKFqudcM3Dkks9QnXJ14=
Private-MAC: 48d2869a23f111f02a4cee83d16392f6463ac7e1
```

Reference:
* https://confluence.atlassian.com/jira064/installing-jira-on-linux-720411834.html  
* https://ezy.slack.com/files/patrikpotocki/F42Q64WET/jira_updates.mp4

