Octopus deploy
--------------

### Octopus Documentation

You will find most information you need here. [1][]

### Server and access

<http://octopus.ezyserver.se>

### Name convention

Always create a project group. that's named after the client name.

For example : Feriehus , RCCL etc. For each project name it with project
name + type of deployment.   If the project is always depedent on some
other project. This could be the chase for Feriehus. So you always want
to deploy both projects at the same time. Then name your deploy
project + project + type[if applicable]

For example: Feriehus Web , ezyHolidayRentals Web , ezyHolidayRentals
Import or Feriehus ezyHolidayRentals Full Release (This should be under
the Fierehus project group)  and for RCCL : RCCL Web , Celebrity Web ,
RCCL France Web , RCCL Arabia Web , RCCL Import , RCCL France Import ,
RCCL Arabia Import. or RCCL All Imports , RCCL ALL Web

### How to setup a ezy-octopus project 

1.  Install octopack on the project you want to deploy , Follow this
    guide [Installtion guide][]
2.  Setup a sub-project in teamcity under your client named your octopus
    project for example “RCCL Import”.
3.  Copy both exisiting steps in for example RCCL Import, name it
    Octopus Pack and Octopus Release
4.  Edit both project so the properties are right in the different
    steps. make sure you look thru all steps.
5.  If this is a new server you will need to add it to octopus [this
    guide][]
6.  Add the server in the correct [2][] , think we will have test
    ,staging and production (this can be discussed if we want more
    enviorments)
7.  When adding the server, tag it with correct client and role. for
    example, rccl-web, rccl-app , rccl-arabia-app, feriehus-web
    feriehus-app. If the server already exists, add the correct tags to
    it.
8.  Setup the octopus project, for websites follow [guide.][]
9.  The final step is to make sure you do Virtual Directory etc for
    upload folders. Cause octopus will use a dynamic folder.

### License
https://github.com/EzyWebwerkstaden/Wiki/blob/master/Licenser/octopuslicense.txt

## Docs
 http://octopusdeploy.com/documentation  
 https://github.com/OctopusDeploy/OctoPack|OctoPack  
 http://octopusdeploy.com/documentation/install/tentacle|Follow  
 http://octopusdeploy.com/documentation/features/environments|Enviorment  
 http://octopusdeploy.com/documentation/features/iis|this  

## Polling tentacle
### Prequisites
Port allowance from server
outgoing : 80(http)/443(https)/10943(octopus server port)

### url allowance(if there is a url banner/filtering)
allowed url: https://octopus.ezyhosting.se/

### Tentacle setup details
Url: https://octopus.ezyhosting.se  
Username: Deploy   
Password: Deploy123  



## Master key
4mDq4p30LHzH5Fr9K6uEIg==

##Create new certificate
```
cd C:\Program Files\Octopus Deploy\Tentacle
tentacle --service --stop 
tentacle --new-certificate --export-file=VALUE 
tentacle --import-certificate --from-file=VALUE 
tentacle --service --start
```
Then delete in tentacle monitor and install again if its a polling tentacle (radixx)  

## Update LetsEncrypt certificate for octopus server
* In Octopus Manager remove binding `https://octopus.ezyhosting.se/` 
* Run C:\@letsencrypt\letsencrypt as Admin 
* Enter M for manual, host is `octopus.ezyhosting.se` and web root is `C:\inetpub\wwwroot`
* Check that SSL is updated in IIS
* Add back the binding of `https://octopus.ezyhosting.se/` in Octopus Manager
