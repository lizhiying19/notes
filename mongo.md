workshop第三次课相关

# docker


## 基本用法
- 查看信息
```bash
docker images           #查看docker镜像
docker ps               #查看docker容器
```
- 启动容器和终止容器
```BASH
docker start 容器名称/容器ID
docker stop 容器名称/容器ID
```
- 删除容器
```bash
docker rm 容器ID
docker rm -f 容器ID     #若删除正在运行的容器
```

- 进入docker容器
```BASH
docker exec -it 容器名称 /bin/bash
```
- 退出容器到宿主机
```BASH
# 退出容器不能用exit命令，或者ctrl+c，会杀死容器的，正确的方法是使用Ctrl+p组合键就可以了
```
- 从镜像实例化启动容器
```BASH
# 暂时略
```
- 其他关于docker：docker构建镜像

其实docker的设计跟git差不多，容器也可以提交，提交后就变成了一个镜像，然后就可以利用这个镜像继续实例化启动容器，还可以对镜像进行打包成一个文件，可以发送给其它人使用，或者自己当做备份。  

构建方法：使用`Dockerfile` + `docker build -t 镜像名 目录`




---
## docker-compose介绍

参考： 
- https://blog.csdn.net/pushiqiang/article/details/78682323?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2.control
- 官方文档：https://docs.docker.com/compose/gettingstarted/



### 简介
 Docker Compose是一个用来定义和运行复杂应用的Docker工具。一个使用Docker容器的应用，通常由多个容器组成。使用Docker Compose不再需要使用shell脚本来启动容器。 
Compose 通过一个配置文件来管理多个Docker容器，在配置文件中，所有的容器通过services来定义，然后使用docker-compose脚本来启动，停止和重启应用，和应用中的服务以及所有依赖服务的容器，非常适合组合使用多个容器进行开发的场景。


### docker-compose文件结构
`docker-compose.yml`和
`dockerfile`
分别有什么作用，怎么关联起来的，官方网站上讲的比较清楚

1. **dockerfile**

编写一个用来构建Docker镜像的`Dockerfile`，该镜像包含应用所需的所有依赖关系，包括Python本身。
具体该文件告诉了docker哪些信息，看官网。

2. 在compose文件中定义services：**docker-compose.yml**

compose文件定义了两个services，即定义了web和mongo两个容器。 

`Web service`使用当前目录中的`Dockerfile`构建的映像。topmat_site项目中是用build_image.sh中定义了镜像名等

`Mongo service`使用从Docker Hub注册表中提取的公共`mongo`映像。




---
## 实例-mongo容器创建
参考：
- https://cloud.tencent.com/developer/article/1378206
- https://www.cnblogs.com/smiler/p/10112676.html


创建新容器，参考 https://cloud.tencent.com/developer/article/1378206

```bash
# 1.获取mongo镜像
docker pull mongo
# 2.创建mongodb容器
docker run -itd --name mongo -p 27017:27017 mongo
```
`docker run`命令解析：
```BASH
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
# docker run 命令必须指定一个容器镜像，不要和容器名混淆了
# 常用options
-i: 以交互模式运行容器，通常与 -t 同时使用；
-t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用；
-d: 后台运行容器，并返回容器ID；
--name 容器名: 为容器指定一个名称；
--link=[]: 添加链接到另一个容器；
-v，--volume: 绑定一个卷，例如-v /data:/data，表示主机的目录 /data 映射到容器的 /data
-p: 指定端口映射，格式为：主机(宿主)端口:容器端口。
    例如-p 127.0.0.1:80:8080，表示绑定容器的 8080 端口，并将其映射到本地主机 127.0.0.1 的 80 端口上。
```

```bash
# 3.进入容器mongo
docker exec -it mongo /bin/bash
```
```bash
# 4.进入容器后，输入mongo，就可以进入mongo shell
# 注意：像use 数据库名/show dbs等，这种都是mongo的命令，需要进入到mongo shell之后才能work

(base) lzy@lzy-ZHENGJIUZHE-REN7000-25ICZ:~$ docker exec -it mongo /bin/bash
root@d5c70a5920e4:/# use topmat
bash: use: command not found
root@d5c70a5920e4:/# mongo
MongoDB shell version v4.4.2
connecting to: mongodb://127.0.0.1:27017/?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("cca9067d-81f1-4e59-b790-e2feb667c604") }
MongoDB server version: 4.4.2
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
	https://docs.mongodb.com/
Questions? Try the MongoDB Developer Community Forums
	https://community.mongodb.com
---
The server generated these startup warnings when booting: 
        2020-12-22T04:28:48.609+00:00: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http://dochub.mongodb.org/core/prodnotes-filesystem
        2020-12-22T04:28:48.987+00:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
---
---
        Enable MongoDB's free cloud-based monitoring service, which will then receive and display
        metrics about your deployment (disk utilization, CPU, operation statistics, etc).

        The monitoring data will be available on a MongoDB website with a unique URL accessible to you
        and anyone you share the URL with. MongoDB may use this information to make product
        improvements and to suggest MongoDB products and deployment options to you.

        To enable free monitoring, run the following command: db.enableFreeMonitoring()
        To permanently disable this reminder, run the following command: db.disableFreeMonitoring()
---
> use topmat
switched to db topmat
> db
topmat
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
> 

```

# mongo
## 基本操作简介
进入mongo容器后，输入mongo就可以直接无密码登录到数据库。

注意区分db和collection，查询具体的数据肯定都是针对当前db中的具体collection的。

```bash
show dbs                  #查看全部数据库
show collections          #显示当前数据库中的集合（类似关系数据库中的表）
show users                #查看当前数据库的用户信息

use <db name>             #切换数据库
db                        #查看当前所在数据库

db.help()                 #显示数据库操作命令，里面有很多的命令 
db.foo.help()             #显示集合操作命令，同样有很多的命令，foo指的是当前数据库下，一个叫foo的集合
db.foo.find()             #对于当前数据库中的foo集合进行数据查找（由于没有条件，会列出所有数据） 
db.foo.find({ a : 1 })    #对于当前数据库中的foo集合进行查找，条件是数据中有一个属性叫a，且a的值为1
db.表名.find({'key':'value'});

db.getMongo();            #查看当前db的链接机器地址
```
**mongodDB备份与恢复**
```BASH
# mongodDB备份
mongodump -h <dbhost> -d <dbname> -o <dbdirectory>

-h：
MongDB所在服务器地址，例如：127.0.0.1或localhost，当然也可以指定端口号：127.0.0.1:27017
-d：
需要备份的数据库实例名，例如：users
-o：
指定备份的数据存放的目录位置，例如：/root/mongdbbak/，当然该目录需要提前建立，在备份完成后，系统自动在/root/mongdbbak/目录下建立一个users目录，这个目录里面存放该数据库实例的备份数据。数据形式是以JSON的格式文件存储。

# 例如： 
mongodump -h localhost -d users -o /root/mongdbbak/
```
```BASH
# mongodDB恢复
mongorestore -h <hostname><:port> -d dbname <path>

--host <:port>, -h <:port>：
MongoDB所在服务器地址，默认为:localhost:27017
-d ：
需要恢复的数据库实例名，例如：users，当然这个名称也可以和备份时候的不一样，比如user2
--drop：
恢复的时候，先删除当前数据，然后恢复备份的数据。就是说，恢复后，备份后添加修改的数据都会被删除，谨慎使用！
--dir：
指定备份的目录。

# 例如：
mongorestore -h localhost -d users --dir /root/mongdbbak/users
```


## mongodb可视化工具：robo 3T
### 安装
- 安装参考：https://www.jianshu.com/p/67a7147b3354
- 链接数据库之前需要，启动mongo服务？用docker的话呢：貌似不需要，容器里面环境都是ready的状态

### 连接docker中的容器
Robo 3T连接docker中的mongoDB容器：
参考：
- https://cloud.tencent.com/developer/article/1556249 不一定非要先`use admin`？no，**必须的**
- https://blog.csdn.net/weixin_47534571/article/details/109591725
- https://www.jianshu.com/p/a54d7db534b0


创建管理员用户：参考 https://blog.csdn.net/weixin_47534571/article/details/109591725
```BASH
docker exec -it mongo mongo admin
# 进入mongo容器
db.createUser({ user:'admin',pwd:'123456',roles:[ { role:'userAdminAnyDatabase', db: 'admin'}]});
# 创建一个名为 admin，密码为 123456 的用户。
db.auth('admin', '123456')
# 尝试使用上面创建的用户信息进行连接。
db.createUser({ user: "root" , pwd: "root", roles: ["root"]})
# 创建超级用户

# 之后在robo 3T中创建连接，显示database是admin，然后输入用户名root，密码root
```

### mongodb删除用户
参考：https://www.cnblogs.com/cxt618/p/10516867.html
```bash
db.dropUser(<user_name>)    # 删除某个用户，接受字符串参数
db.dropUser(“admin”)        # 示例
db.dropAllUser()            # 删除当前库的所有用户
```

### robo 3T使用
参考：https://blog.csdn.net/baidu_39298625/article/details/99654596


# 设置端口号
`npm run dev`前端跑起来之后，不知道跑在哪个端口上，在哪里定义的。
- 参考： https://blog.csdn.net/farme_guan/article/details/105249733
- 相关配置：config/index.js