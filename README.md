# docker-mongo-bionic

docker-mongo-bionic

## mout data path to local

```
mkdir data/

chmod -R 0777 data/
```

## run 

```
docker-compose up -d
```

# add new user 

## login container 

```
[docker@docker-compose mongodb]$ docker-compose ps
         Name                        Command               State            Ports
-------------------------------------------------------------------------------------------
mongodb_mongo-express_1   tini -- /docker-entrypoint ...   Up      0.0.0.0:8082->8081/tcp
mongodb_mongo_1           docker-entrypoint.sh mongod      Up      0.0.0.0:27017->27017/tcp
[docker@docker-compose mongodb]$ docker exec -it mongodb_mongo_1 bash
```

## syntax

```
use admin
db.createUser(
  {
    user: "myUserAdmin",
    pwd: passwordPrompt(), // or cleartext password
    roles: [
      { role: "userAdminAnyDatabase", db: "admin" },
      { role: "readWriteAnyDatabase", db: "admin" }
    ]
  }
)
```

## login mongo shell with user password

```
root@e0f02dfbfeec:/# mongo mongodb://root:example@mongo:27017/
MongoDB shell version v4.2.8
connecting to: mongodb://mongo:27017/?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("7d40adbe-7d21-4b4b-91c6-cfe3d24e522f") }
MongoDB server version: 4.2.8
Server has startup warnings:
2021-12-28T12:23:48.152+0000 I  CONTROL  [initandlisten]
2021-12-28T12:23:48.152+0000 I  CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2021-12-28T12:23:48.152+0000 I  CONTROL  [initandlisten] **        We suggest setting it to 'never'
2021-12-28T12:23:48.152+0000 I  CONTROL  [initandlisten]
2021-12-28T12:23:48.152+0000 I  CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2021-12-28T12:23:48.152+0000 I  CONTROL  [initandlisten] **        We suggest setting it to 'never'
2021-12-28T12:23:48.152+0000 I  CONTROL  [initandlisten]
---
Enable MongoDB's free cloud-based monitoring service, which will then receive and display
metrics about your deployment (disk utilization, CPU, operation statistics, etc).

The monitoring data will be available on a MongoDB website with a unique URL accessible to you
and anyone you share the URL with. MongoDB may use this information to make product
improvements and to suggest MongoDB products and deployment options to you.

To enable free monitoring, run the following command: db.enableFreeMonitoring()
To permanently disable this reminder, run the following command: db.disableFreeMonitoring()
---

> db.createUser({user: "wachira",pwd: passwordPrompt(),roles: [{ role: "userAdminAnyDatabase", db: "admin" },{ role: "readWriteAnyDatabase", db: "admin" }]})
Enter password:
Successfully added user: {
        "user" : "wachira",
        "roles" : [
                {
                        "role" : "userAdminAnyDatabase",
                        "db" : "admin"
                },
                {
                        "role" : "readWriteAnyDatabase",
                        "db" : "admin"
                }
        ]
}
> exit
bye
root@e0f02dfbfeec:/#
```
