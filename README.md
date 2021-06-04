# neOCampus / eCOnect / ... base container for python developments #
_____________________________________________________________________

**WHY THIS ???**
**Because `docker` is a root only command hence we allow you to login root within your container**
*(behind the hood is `supervisord`)*

This docker container enables you to become **root** in your container. Especially, this container features the following cvapabilities:

  - SSH daemon --> enables you to log as root within the container
  - one port to have your `/app/xxx` reachable from the internet
  - **NO persistant volume** --> if you need it, ask me !
  - *[TODO] Prometheus end-point (internal monitoring)*

![docker-base container overview](img/docker-base-overview.png)


TO BE CONTINUED


### Environment variables ###
When you start this application (see below), you can pass several environment variables:

  - **DEBUG=1** this is our application debug feature
  - **SIM=1** this is our application simulation feature: kind of *read-only* mode (i.e no write to any database)
  - **DJANGO_DEBUG=1** this is debug to Django's internals
  - **DJANGO_SECRET_KEY** Django's internal secret key [mandatory]
  - **MQTT_SERVER** and **MQTT_PORT**
  - **MQTT_USER** and **MQTT_PASSWD** are sensOCampus own MQTT credentials
  - **MQTT_TOPICS** json formated list of topics to subscribe to (usually #/device)
  - **MQTT_UNITID** is a neOCampus identifier fr msg filtering
  - **PGSQL_USER** and **PGSQL_PASSWD** are Postgres credentials for sensOCampus' internal database
  - **PGSQL_SERVER**=172.17.0.1   this is the docker gateway
  - **PGSQL_PORT**=5432
  - **PGSQL_DATABASE**=sensocampus    name of the database


### [HTTP] git clone ###
Only **first time** operation.

`git clone https://fthiebolt@bitbucket.org/fthiebolt/sensocampus.git`  

### git pull ###
```
cd sensocampus
git pull
```

### git push ###
```
cd sensocampus
./git-push.sh
```

**detached head case**
To commit mods to a detached head (because you forget to pull head mods before undertaking your own mods)
```
cd <submodule>
git branch tmp
git checkout master
git merge tmp
git branch -d tmp
```

### start container ###
```
cd sensocampus
DJANGO_DEBUG=1 SIM=1 DEBUG=1 \
MQTT_PASSWD='passwd' PGSQL_PASSWD='passwd' DJANGO_SECRET_KEY='<xxxxxx>' \
docker-compose --verbose up -d
```  

### fast update of existing running container ###
```
cd sensocampus
git pull
DEBUG=1 MQTT_PASSWD='passwd' PGSQL_PASSWD='passwd' DJANGO_SECRET_KEY='<xxxxxx>' docker-compose --verbose up --build -d
```  

### ONLY (re)generate image of container ###
```
cd sensocampus
docker-compose build --force-rm --no-cache
[alternative] docker build --no-cache -t sensocampus2 -f Dockerfile .
```

### start container for maintenance ###
```
cd sensocampus
docker run -v /etc/localtime:/etc/localtime:ro -v "$(pwd)"/app:/opt/app:rw -it sensOCampus2 bash
```

### ssh root @ container ? ###
Yeah, sure like with any VM:
```
ssh -p xxxx root@locahost
```  

