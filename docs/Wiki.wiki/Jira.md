# Auto skapa issue från mail.
För den som vill kunna erbjuda kunder att maila in nya issues i JIRA så funkar det fint. Jag har precis lagt upp det för GoAir. Enklast är att göra enligt:  
1. Logga in på Microsoft Online för jira@ezy.se (pEN3g5g%)  
2. Skapa en ny e-post mapp: "GoAir"  
3. Skapa förslagsvis två regler:  
4. Mail från "@goair.in"  
5. Mail från "@ezy.se" OCH där ämnet innehåller "GoAir:"  
6. Flytta dessa till nya mappen "GoAir"  
7. I JIRA under [Settings] -> "System" -> "Mail" -> "Incoming" -> "Add incoming mail handler"  
8. Ange namn, server "Ezy outlook", Handler "Create a new issue or add...", Folder name "GoAir" (enl ovan) -> Next  
9. Ang Projekt, Issue type och "Default Reporter" (den på GoAir/Kund som brukar anmäla)

Done!

# Custom config for https in jira
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
                   disableUploadTimeout="true"/>

<Connector acceptCount="100" connectionTimeout="20000" disableUploadTimeout="true" enableLookups="false" maxHttpHeaderSize="8192" maxThreads="150"
 minSpareThreads="25" port="8080" protocol="HTTP/1.1" redirectPort="8443" useBodyEncodingForURI="true"
 scheme="https" proxyName="jira.ezy.se" proxyPort="443" secure="false"/>
```