title: centos6 mongodb 3.0.x 升级到 3.2.5 (并迁移数据)
date: 2016-04-28 13:59:28
tags: [mongodb,3.2.5]
---------------------
# centos6 mongodb 3.0.x 升级到 3.2.5 (并迁移数据)

## 安装mongodb

* 创建yum repo文件 **/etc/yum.repos.d/mongodb-org-3.2.repo** ,这样可以使用yum命令安装mongodb了.

文件内容:

```java
[mongodb-org-3.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.2.asc
```

* 执行安装命令

```java
sudo yum install -y mongodb-org
```

* 启动mongodb 

```java
sudo service mongod start
```

## 配置mongodb

在启动mongodb之前可以配置下mongodb的db path 等.

创建一个目录用来保存log 和db 文件 ,这里以 /www/mongodb/ 为例

注意: 要赋予该目录相应的权限  
```java
chown -R mongod.mongod /www/mongodb
```

* 关闭只能localhost连接: 注释掉 bindIp 那一行
* 修改log路径: 修改 systemLog下的path 值改为: /www/mongodb/logs/mongod.log
* 修改 storage下的 dbPath 值改为: /www/mongodb/db

好了, 可以启动mongodb服务了.

```java
sudo service mongod start
```

## 迁移旧的db,并导入到新建的mongodb中

### 备份数据库:
```java
mongodump -h 127.0.0.1 -d {数据库名} -o {要保存的目录}
```
例如:
mongodump -h 127.0.0.1 -d ddc -o /mnt/wwwroot/ddc.dmp

### 导入数据库:
```java
mongorestore -h 127.0.0.1 -d {数据库名} {备份的数据库目录}
```
例如:
mongorestore -h 127.0.0.1 -d ddc /mnt/wwwroot/ddc.dmp/ddc

