# 第四节ssh

账号：acs_2054

hostname: 123.57.47.211

password:0a2cd4ad





#### 作业

```bash
创建好作业后，先进入文件夹/home/acs/homework/lesson_4/，然后：
(0) 进入homework_0文件夹，要求：
    [1] 该文件夹内容为空
    [2] 配置服务器账号的密钥登陆方式。服务器信息可以通过如下命令获得：
        homework 4 getinfo
        将服务器账号的名称（Host）配置成：myserver
(1) 进入homework_1文件夹，下列描述中的“本地”均表示当前文件夹。要求：
    [1] 在myserver服务器上创建并清空文件夹：~/homework/lesson_4/homework_1/
    [2] 将本地的main.cpp文件上传到myserver中的~/homework/lesson_4/homework_1/目录中。
    [3] 在本地创建文件夹dir。
    [4] 将myserver中的/etc/lsb-release文件复制到dir中。
(2) 进入homework_2文件夹，下列描述中的“本地”均表示当前文件夹，要求：
    [1] 在myserver服务器上创建并清空文件夹：~/homework/lesson_4/homework_2/
    [2] 将本地的dir文件夹上传到myserver中的~/homework/lesson_4/homework_2/目录中。
(3) 进入homework_3文件夹，下列描述中的“本地”均表示当前文件夹，要求：
    [1] 在本地创建文件夹dir。
    [2] 将myserver中的/var/lib/locales/supported.d文件夹下载到本地dir文件夹中。
(4) 进入homework_4文件夹，编写脚本remote_mkdir.sh和remote_rmdir.sh，要求：
    [1] 在myserver服务器上创建并清空文件夹：~/homework/lesson_4/homework_4/
    [2] 本地目录下仅包含remote_mkdir.sh和remote_rmdir.sh
    [3] remote_mkdir.sh和remote_rmdir.sh具有可执行权限
    [4] remote_mkdir.sh接收一个传入参数。格式为 ./remote_mkdir.sh directory_name
        该操作可以在myserver服务器上的~/homework/lesson_4/homework_4/目录下，创建一个名为directory_name的文件夹
    [5] remote_rmdir.sh接收一个传入传输。格式为 ./remote_rmdir.sh directory_name
        该操作可以将myserver服务器上的~/homework/lesson_4/homework_4/目录下的名为directory_name的文件夹删掉。
    [6] 注意：传入的文件参数可能包含空格。两个脚本均不需要判断传入参数的合法性。

```

#### Answer

```shell
[0]
通过 homework 4 getinfo获取用户名和地址
在.ssh/下创建config文件
然后输入以下内容：
Host myserver
	HostName 123.57.47.211
	User acs_2054

然后使用命令 ssh-keygen 一路回车，生成 id_rsa:私钥，is_rsa.pub：公钥

然后通过命令 ssh-copy-id myserver将公钥复制到myserver中

[1]
使用 ssh acs_2054@myserver 连接到myserver

mkdir homework
cd homework
mkdir lesson_4
cd lesson_4
mkdir homework_1

在本地服务器使用命令：scp main.cpp myserver:homework/lesson_4/homework_1/、

mkdir dir	#create dir direction

scp myserver:/etc/lsb-release dir	#将myserver中的/etc/lsb-release文件复制到dir中。

[2]

使用 ssh acs_2054@myserver 连接到myserver

cd homework
cd lesson_4
mkdir homework_2

在本地服务器使用命令：scp -r dir/ myserver:homework/lesson_4/homework_2

[3]
cd homework
mkdir lesson_4
cd lesson_4
mkdir homework_3
mkdir dir

scp -r myserver:/var/lib/locales/supported.d dir

[4]
使用 ssh acs_2054@myserver 连接到myserver
cd homework
cd lesson_4
mkdir homework_4

在本地目录下 touch remote_mkdir.sh和remote_rmdir.sh
然后 chmod +x remote_mkdir.sh remote_rmdir.sh

vim remote_mkdir.sh,代码如下：
#! /bin/bash
ssh myserver mkdir homework/lesson_4/homework_4/\"$1\"

vim remote_rmdir.sh,代码如下
#! /bin/bash
ssh myserver rm -r homework/lesson_4/homework_4/\"$1\"

```

