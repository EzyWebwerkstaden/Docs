# VPN-solution for remote workers

We have set up an SSL-VPN solution via Your-IT now. To make our local servers, testservers and productionserver available from outside.

It’s a simple solution.
Login on this page (with your *domain* credentials):  ex. myEZYusername / myEZYpassword   

https://94.103.206.196:6443/sslvpn_download.shtml
 
Download the correct client for you. Install it.  
  
****Connect to: 94.103.206.196:6443****
And use your *domain* username and password. Press connect.

![SSL vpn app](http://www.ezymail.se/images/firebox.png)

If you are asked entery you *domain* password again.  

Once connected you will be on the local network. So in order to reach productionservers you need to either use the dns-name or the local ip-number.   

Productionsservers local series are: 192.168.80.XXX.  
Testservers ip-series are: 192.168.20.XXX or dns-name will work as well.

Full list: https://github.com/EzyWebwerkstaden/Wiki/wiki/Servrar-Mediateknik
