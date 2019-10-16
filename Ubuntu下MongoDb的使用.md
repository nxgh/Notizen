## 安装

直接使用 apt 命令进行安装

`sudo apt install mongodb`

装好以后应该会自动运行mongod程序，通过`pgrep mongo -l`查看进程是否已经启动

## 手动启动mongodb

重启系统以后 mongo 程序要自己重新手动启动



1. 运行 `locate mongo`查看系统默认把mongo装到了哪里，主要关注以下三个文件
   -  名为 mongod 的程序的位置，它相当于 mongo 数据库的Server，需要一直在后台运行
   - mongo 数据库 log 日志文件的位置
   - mongo 的log 日志位置

2. 进入 mongod 所在的目录`/usr/bin/mongod` ，运行 
```bash
sudo ./mongod --dbpath /var/lib/mongodb/ --logpath /var/log/mongodb/mongodb.log --logappend &
```

   - **--dbpath：**指定mongo的数据库文件在哪个文件夹
   - **--logpath：**指定mongo的log日志是哪个，这里log一定要指定到具体的文件名
   - **--logappend：**表示log的写入是采用附加的方式，默认的是覆盖之前的文件

   - **&：**表示程序在后台运行

如果是系统非正常关闭，这样启动会报错，由于mongodb自动被锁上了，这时需要进入mongodb数据库文件所在的目录（**/var/lib/mongodb/**）,删除目录中的**mongodb.lock**文件,然后再进行上述操作



## 启动与关闭

```bash
sudo service mongodb start
sudo service mongodb stop
```



## 创建root/admin用户

```mongo

db.createUser(  
  {  
    user: "admin",  
    pwd: "password",  
    roles: [{role: "userAdminAnyDatabase", db: "admin"}]  
  }  
)
```

## 修改mongod.conf文件

```
security:
  authorization: enabled//启用授权
```

## 重启MongoDB服务器

```
service mongod restart
```

## 创建数据库读写权限用户

```shell
use admin
db.auth("admin","password");
use ballmatch
db.createUser({
    user: "football",
    pwd: "password",
    roles: [{role: "readWrite",db: "ballmatch"}]
})
```

### 修改用户密码

```
db.updateUser( "admin",{pwd:"password"})
```

### 删除用户

```js
// 删除用户(需要root权限，会将所有数据库中的football用户删除)

db.system.users.remove({user:"football"});

// 删除用户(权限要求没有那么高，只删除本数据中的football用户)

db.dropUser("football");
```

