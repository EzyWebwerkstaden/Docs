ELK stack
-----------------------
The ELK stack contains three different parts:  
- Elastic Search
- Logstash
- Kibana
- (Redis)
- (ES Curator)

These parts runs in separate docker containers.  
Logstash collects logs from Redis using a configuration (logstash.conf), describes where to fetch the logs (Redis) and in what format, and where to put them (Elastic Search).  
To view the logs and query the data, access Kibana that is a graphical interface that supports Elastic Search.

![ELK overview](https://github.com/EzyWebwerkstaden/Wiki/blob/master/Bilder/elk.png)

**To access Kibana:**  
`http://log.test.api.ezyflight.se/app/kibana`

**For instructions to setup the ELK stack from the start, view this repo:**  
[https://github.com/EzyWebwerkstaden/docker-elk](https://github.com/EzyWebwerkstaden/docker-elk)  

**To access the UAT log server**  
[https://github.com/EzyWebwerkstaden/Wiki/wiki/Servrar-Mediateknik](https://github.com/EzyWebwerkstaden/Wiki/wiki/Servrar-Mediateknik)

**Deleting old logs are handled with a docker container with Curator 4.x that supports Elastic Search 5.x**  
This will run once every day and remove index older than 30 days on UAT logging  
[https://github.com/Elders/Curator-docker](https://github.com/Elders/Curator-docker)  
`docker run -d --name curator_cron --link ezyflight-elasticsearch:elasticsearch eldersoss/curator-docker --host elasticsearch --port 9200 delete_indices --filter_list \'{\"filtertype\":\"age\", \"source\":\"name\",\"direction\":\"older\", \"timestring\":\"%Y.%m.%d\", \"unit\":\"days\", \"unit_count\":\"30\"}\'`  

(The quotes in the command have to be escaped since they are otherwise stripped from the command)

Prerequisites
-----------------------------------
When setting up a new server that will run Elastic Search, and you will have a descent amount of data, the memory threshold for the JVM must med increased. For production use this should always be set.

`sudo sysctl -w vm.max_map_count=262144`

Important
-------------------------
When setting up a new Docker container with Elastic Search, make sure to mount a volume where the data i stored, outside the container itself.
This can be done by specifying `-v` flag and add the path to where you want to save the data, when running the docker container.
`docker run -d --name elasticsearch --restart=always -p 9200:9200 -v /esdata:/data elasticsearch`

Explanation of the run command:  
`-d` = run as a daemon which means it runs in the backround, even when you terminate you connection to it.  
`--name` = name of the container, so you can refer to it with docker commands, for example restart etc.  
`--restart=always` = when the container have any problems where i crash, it will restart automatically.  
`-p` = the port inside the container where the application listens to 9200 is mapped to the outside port of 9200. This will make it possible to communicate with the application within the container.  
`-v` = mounting volume where data are saved.

Troubleshooting
-----------------------
1. List all running instances to check everything is up and running.  
    a. See command below.

2. Check that you can connect to the Redis instance. Use for example RedisDesktopManager.  
    a. If not, check that the container is running and restart/start.  
    b. If it is running check the logs in the instance by using the command below.

3. Check that logstash is collecting logs from Redis  
    a. Add a log in Redis and monitor if they are picked up by Logstash.  
    b. If not check the Logstash logs.  
    c. Logstash can also pickup logs but cannot push them to ES. This will show in the logs as well.

4. Check that ES container is running  
    a. Check runnin instance of ES  
    b. Check logs

5. Check Kibana container is running  
    a. Check runnin instance of Kibana  
    b. Check logs

6. If low on HDD  
    a. Check Docker json log files sizes /etc/docker/containers/{container-id}/{container-id}-log.json  
    b. remove big log files, could need a reboot to work after.
 
Common information
-------------------------
1. Currutor is used to remove old indexed data in Elastic Search. If not the server will eventually lack disk space, making it malfunction.
2. In production you want to have a cluster of Elastic Search instances to avoid data loss and to increase reliability
3. Mount volume outside the container to avoid data loss.
4. Docker will automatically set a name when starting a container image, if the `--name` flag is not set.

Common commands
-------------------------
List images  
`docker images --format`
`--format` will pretty print the output

List running instances  
`docker ps`

List all instances, stopped instances included  
`docker ps -a`

Build the container from an image.  
`docker build -f elasticsearch.Dockerfile -t elasticsearch .`  
A docker file is instructions how to build a container from the image. Images can be pulled from the Docker Hub registry, or like in this case, a local docker file.

Run the container  
`docker run -d --name elasticsearch -p 9200:9200 elasticsearch`  
Run an instance of a docker image named elasticsearch (That we named in the command above).

Restart container  
`docker restart elasticsearch`

Start a (stopped) container  
`docker start elasticsearch`

Stop a (running) container  
`docker stop elasticsearch`  
This will only "pause" the container. No data or state is lost.

Delete instance of container  
`docker rm elasticsearch`

Delete  image  
`docker rmi -f`  
`-f` will force delete.

Check logging from standard output in a container.  
`docker logs -f [containername]`.  
`-f` means follow so the terminal will show (tailing) new log entries.

### Previous information (Made by Patrik)

### Kibana
För att komma till dashboarden så är de följande länk
```
http://{host}:9292/index.html#/dashboard/file/logstash.json
```
### Elastic search
För att sanity checka elastic search så kan man alltid gå mot:
```
http://{host}:9200/_aliases?pretty
```
där ska man se dagensdatum.

### Docker
```
  docker run -d \
  --name logstash \
  -p 9292:9292 \
  -p 9200:9200 \
  -v /var/logstash/logstash.conf:/opt/logstash/conf.d/logstash.conf \
  pblittle/docker-logstash
```
Behövs skapas en logstash.conf på servern.

```
input {
 redis {
   host => "94.103.199.211"
   type => "redis"
   data_type => "list"
   key => "{project}-logstash"
 }
}
output {
    stdout {
    codec => rubydebug
  }
    elasticsearch {
     embedded => true
    }
}
```

#### Curator
```
docker run --rm bobrik/curator --help
```

tex. för att ta bort index som är äldre än 30 dagar. 
```
 docker run --rm bobrik/curator --host 172.17.0.1 delete indices --older-than 30 --time-unit days --timestring '%Y.%m.%d'
```

För att kolla storlek av all data kan man göra en GET mot:
```
http://{host}:9200/_cat/shards?v
```

Backups
```
curator snapshot --older-than 20 --repository REPOSITORY_NAME
```

För att skapa upp ett repo för snapshots kan man göra en "PUT"
```
PUT /_snapshot/newrepo
{
  "type":"fs",
  "settings": {
    "location":"/just/a/location/testrepo"
  }
}
```

Det går även att köra med s3 osv. om man inte vill lagra snapshots till disk.
https://github.com/elastic/elasticsearch-cloud-aws#aws-cloud-plugin-for-elasticsearch

### NLog
För att använda nlog för att logga till logstash/redis. Använd.
https://github.com/EzyWebwerkstaden/NLog.Redis .

I web.configen behöver man följande.
```
<configuration>
  <configSections>
   <section name="nlog" type="NLog.Config.ConfigSectionHandler, NLog" />
</configSections>
  <nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" throwExceptions="true">
    <extensions>
          <add assembly="NLog.Redis" />
    </extensions>
    <targets>
      <target xsi:type="Redis" name="redis" host="192.168.20.133" port="6379" db="0"  key="{project}-logstash" dataType="list" layout="${date:format=yyyyMMddHHmmss} ${uppercase:${level}} ${message}" />
    </targets>
    <rules>
      <logger name="*" minlevel="Info" writeTo="redis" />
    </rules>
</nlog>
<configuration>
```

För att skapa upp en logger kan man antigen använda DI eller så kan man använda följande rad:
```
private static Logger _logger = LogManager.GetCurrentClassLogger();
```

Sedan i kod är de bara att använda som vanligt eller så kan man använda en extension för att kunna lägga upp fields och tags. tex.

```
                _logger.Log(LogLevel.Info, "message", fields: new Dictionary<string, object>
      {
        {"REMOTE_ADDR", ip ?? ""},
        {"Url", url ?? ""},
        {"Referrer", referrer ?? ""},
        {"Method",method ?? ""}, 
        {"Endpoint",endpointUrl ?? ""},
        {"HTTP_USER_AGENT", ua ?? ""},
        
      }, tags: new[] { "requests", "project", method }
      );
```
### log4net

### Data lose ? 
Här är en ganska bra post om data lose. handlar mest om stora kluster. Men så man ha i åtanke vad det finns för data-lose chases för elastic search.
https://aphyr.com/posts/323-call-me-maybe-elasticsearch-1-5-0

### RCCL Prod uppsättning
Vi använder dessa dockerfile
```
https://github.com/EzyWebwerkstaden/docker-elk
```
Allt är enligt dokumentation på den sidan, men vi attachar en map så vi kan presista datan för elasticsearch
```
docker run -d --name elasticsearch --restart=always -p 9200:9200 -v /esdata:/data elasticsearch
```
Se till att ha med om man vill att container ska starta om efter reboot.
```
--restart=always
```

http://94.103.206.203:5601/

### New way
Setup elk with docker and Redis on default port, if Redis already exists change port
```
sudo docker run -d --name ezyflight-elasticsearch --log-opt max-size=50m --restart=always -p 9200:9200 -v "/esdata":/usr/share/elasticsearch/data elasticsearch && sudo docker run --restart=always --log-opt max-size=50m --name ezyflight-kibana --link ezyflight-elasticsearch:elasticsearch -p 5601:5601 -d mcascallares/kibana-on-steroids && docker run --name ezyflight-logstash --log-opt max-size=50m --restart=always -d logstash -e 'input { redis { host => "qazaqlogging.redis.cache.windows.net" port => 6379 type => "redis" data_type => "list" key => "logstash" password => ""} } output { elasticsearch { hosts => ["172.17.0.1:9200"] } stdout { codec => rubydebug }}'
```
Run Bash in a container
` sudo docker exec -i -t ezyflight-elasticsearch bash`

### Nginx config kibana
```
server {
       listen         80;
       server_name log.qazaq.ezyflight.se;
       return         301 https://$server_name$request_uri;
}
server {
    listen 443 ssl spdy;
    server_name log.qazaq.ezyflight.se;

    ssl_certificate /etc/letsencrypt/live/log.qazaq.ezyflight.se/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/log.qazaq.ezyflight.se/privkey.pem;

    location / {
        proxy_pass http://localhost:5601;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        auth_basic "Restricted";                                #For Basic Auth
        auth_basic_user_file /etc/nginx/.htpasswd;  #For Basic Auth

    }
}
```

### Nginx config elastic
```

server {
       listen         80;
        server_name elastic.qazaq.ezyflight.se;
       return         301 https://$server_name$request_uri;
}
server {

 listen 443 ssl spdy;
    server_name elastic.qazaq.ezyflight.se;

ssl_certificate /etc/letsencrypt/live/log.qazaq.ezyflight.se/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/log.qazaq.ezyflight.se/privkey.pem;


    location / {
        proxy_pass http://localhost:9200;
        proxy_http_version 1.1;
             proxy_set_header Connection "Keep-Alive";
      proxy_set_header Proxy-Connection "Keep-Alive";
        proxy_set_header Host $host;
 auth_basic "Restricted";                                #For Basic Auth
        auth_basic_user_file /etc/nginx/.htpasswd;  #For Basic Auth

    }
}
```