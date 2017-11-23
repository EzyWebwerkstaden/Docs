## Update TeamCity

* Download latest version from here https://www.jetbrains.com/teamcity/download/#section=windows

* Run installer  
Install to default location `c:\@TeamCity`  
Select `Uninstall previous version`

* If installer stuck on stopping service, stop the TeamCity service manually  

* Install Server  
Uncheck to install Build Agent and Build Agent Core

* Select to `Run Teamcity Server under the SYSYTEM account`

* Change the TeamCity folder to support 64bit  
Backup `c:\@TeamCity\jre` folder.  
Make a copy of folder `c:\@TeamCity\jre 64` and rename it to `c:\@TeamCity\jre`  

* Start service

### For references see:  
https://ezy.slack.com/files/U026LKXL3/F42PJE946/teamcity_octopus_upgrades.mp4


## Install a plugin

* Download the plugin as a .zip file

* Place the file in `C:\@TeamCityConfig\plugins`

* Restart TeamCity (Stop and start service)

* See that the plugin is installed under
`http://build.ezyserver.se/admin/admin.html?item=plugins`
