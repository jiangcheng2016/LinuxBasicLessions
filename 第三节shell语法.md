# 第三节 ```Shell```常用语法

#### 1.概论

```shell```是我们通过命令行与操作系统沟通的语言。

```shell```脚本可以直接在命令行中执行，也可以将一套逻辑组织成一个文件，方便复用。

```AC Terminal```中的命令行可以看成是一个```shell脚本在逐行执行```。

```Linux```中常见的```shell```脚本有很多种，常见的有：

* Bourne shell(/usr/bin/sh 或 /bin/sh)
* Bourne Again Shell(bin/bash)
* C Shell(/usr/bin/csh)
* K Shell( /usr/bin/ksh)
* zsh
* ...

```Linux```系统中一般默认使用```bash```，所以接下来讲解```bash```中的语法

文件开头需要写 ```#! /bin/bash```，指明```bash```为脚本解释器



##### 脚本示例

新建一个 ```test.sh```文件，内容如下：

```shell
#! /bin/bash
echo "Hello World!"
```

##### 运行方式

作为可执行文件

```bash
chmod +x test.sh #使脚本具有可执行权限
./test.sh #当前路径下执行

/home/acs/test.sh  # 绝对路径下执行

~/test.sh #家目录路径下执行

#用解释器执行
bash test.sh

#1.为test文件加执行权限，
acs@740d1bdabef2:~$ chmod +x test.sh       acs@740d1bdabef2:~$ ls                     homework  main.cpp  test.sh	#文件列表，此时test.sh 会改变颜色
#2.直接执行当前目录的test.sh
acs@740d1bdabef2:~$ ./test.sh 
Hello World!  #脚本输出         

#3. 绝对路径下执行文件
acs@740d1bdabef2:~$ /home/acs/test.sh 
Hello World!	#脚本输出

#4.家目录下执行
acs@740d1bdabef2:~$ ~/test.sh 
Hello World!	#脚本输出

```

![](G:\Personal\CSDN\Linux基础系统篇章\lesson3-1\微信截图_20211219133559.png)

#### 2.注释

##### 单行注释

每行中 ```#```之后的内容为注释内容

```shell
# 这是一行注释

echo "Hello World" # 这也是注释

```

##### 多行注释

格式：

```shell
:<<EOF
第一行注释
第二行注释
第三行注释
EOF

```

其中，```EOF```可以换成其它任意字符。例如：

```shell
:<<ZS
注释改为ZS
第二行注释
第三行注释
ZS

```

#### 3.变量

##### 定义变量

定义变量，不需要加```$```符号，例如：

```shell
name1 = 'jtx' #单引号定义字符串
name2 = "jtx" #双引号定义字符串
name3 = jtx #也可以不加引号，同样表示字符串
```

##### 使用变量

使用变量，需要加上  ```$```符号，或者 ```${}```符号。 花括号是可选的，主要为了帮助解释器识别变量边界。

```shell
name=jtx
echo $name	#输出jtx
echo ${name}	#输出jtx
echo ${name}string	#输出jtxstring

```

##### 只读变量

使用 ```readonly``` 或者 ```declare```可以将变量变为只读。

```shell
name=jtx
readonly name
declare -r name		#两种写法都可以

name=aaa	# 会报错，因为此时name只读 
```

![image-20211220145302290](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211220145302290.png)

![image-20211220145429324](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211220145429324.png)

##### 删除变量

```unset``` 可以删除变量。

```shell
name=jtx
unset name
echo $name	#输出空行, 因为name变量已经被删除
```

![image-20211220145641848](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211220145641848.png)

##### 变量类型

* 自定义变量(局部变量)
  * 子进程不能访问的变量
* 环境变量(全局变量)
  * 子进程可以访问的变量

```1.```自定义变量改成环境变量：

```shell
#注意：一个bash相当于一个子进程

acs@740d1bdabef2:~$ name=jtx  #定义name变量
acs@740d1bdabef2:~$ echo $name 	#在当前bash下输出name的值
jtx							#输出结果
acs@740d1bdabef2:~$ bash	#新建一个bash
acs@740d1bdabef2:~$ echo $name	#在新建的bash下输出name的值
                                #由于此时变量为局部变量，故此时输出为空       
acs@740d1bdabef2:~$ exit	#退出新建的bash
exit
acs@740d1bdabef2:~$ export name 	#将name修改为环境变量
acs@740d1bdabef2:~$ bash			#新建一个bash
acs@740d1bdabef2:~$ echo $name		#在新建的bash中输出name的值
jtx									#此时 name为环境变量，故新的bash可以访问到
acs@740d1bdabef2:~$ exit
exit 
```

```2.```环境变量改为自定义变量：

```shell
#通过此例，可以看到 declare 的用法
acs@740d1bdabef2:~$ export name=jtx      #定义全局变量                       
acs@740d1bdabef2:~$ bash                 #新建一个子进程                      
acs@740d1bdabef2:~$ echo $name           #子进程可以访问到name          
jtx                                              
acs@740d1bdabef2:~$ exit                #退出当前子进程                   
exit                                                           
acs@740d1bdabef2:~$ declare +x name    #通过declare将name变为局部变量             
acs@740d1bdabef2:~$ bash                                       
acs@740d1bdabef2:~$ echo $name			#子进程无法访问到name

acs@740d1bdabef2:~$ exit                                       
exit                                                           
acs@740d1bdabef2:~$ declare -x name     #通过declare 将name变为全局变量      
acs@740d1bdabef2:~$ bash                 
acs@740d1bdabef2:~$ echo $name          #子进程又可以访问到name                      
jtx                                                             
acs@740d1bdabef2:~$ exit                                       
exit                                                           
```

##### 字符串

字符串可以使用单引号，也可以使用双引号，也可以不用引号。

单引号与双引号的区别：

* 单引号中的内容会原样输出，不会执行，不会获取变量的值
* 双引号中的内容可以执行、可以获取变量的值

```shell
name=jtx
echo 'hi, $name \"jtx\"' #单引号字符串，输出 hi,$name \"jtx\"  
echo "hi, $name \"jtx\"" #双引号字符串，输出 hi,jtx "jtx"  
```



![image-20211220152347029](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211220152347029.png)



获取字符串长度

```shell
name=jtx
echo ${#name}	#输出3
```

截取子串

```shell
name="hello,jtx"
echo ${name:0:4} #截取从0开始的4个字符
```

![image-20211220152747603](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211220152747603.png)

#### 4.默认变量

##### 文件参数变量

在执行```shell```脚本时，可以向脚本传递参数。

```$0```是文件名(包含路径)。例如：

```shell
#! /bin/bash

echo "文件名:"$0
echo "第一个参数："$1
echo "第二个参数："$2
echo "第三个参数："$3
echo "第四个参数："$4

```

然后执行该脚本：

```shell
acs@740d1bdabef2:~$ ./test4.sh     #没有参数的情况下， $1,$2,$3,$4均为空                    
./test4.sh




acs@740d1bdabef2:~$ ./test4.sh 1 2 3 4                         
./test4.sh                                                     
第二个参数：1                                  
第一个参数：2
第一个参数：3               
第一个参数：4                 
```

##### 其它参数相关变量

| 参数             | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| ```$#```         | 代表w文件传入的参数个数，如上例中值为4                       |
| ```$*```         | 由所有参数构成的用空格隔开的字符串，如上例中值 ```“$1” “$2” “$3” “$4”``` |
| ```$@```         | 每个参数分别用双引号括起来的字符串，如上例中值为 ```“$1” "$2" "$3" "$4"``` |
| ```$$```         | 脚本当前运行的进程ID                                         |
| ```$?```         | 上一条命令的退出状态(注意不是```stdout```，而是```exit code```)。0表示正常退出，其他值表示错误 |
| ```$(command)``` | 返回```command```这条命令的```stdout```（可嵌套)             |
| `command`        | 返回```command``` 这条命令的```stdout```(不可嵌套)           |

#### 5.数组

数组中可以存放多个不同类型的值，只支持一维数组，初始化不需要指明数组大小。

数组**下标从0开始**。

##### 定义

数组用小括号表示，元素之间用空格隔开。例如：

```shell
array=(1 abc "def" jtx)
```

 也可以直接定义数组中某个元素的值：

```shell
array[0]=1
array[1]=abc
array[2]="def"
array[3]=jtx
```

##### 读取数组中某个元素的值

格式：

```shell
${array[index]}
```

例如：

```shell
array=(1 abc "def" jtx)
echo ${array[0]}
echo ${array[1]}
echo ${array[2]}
echo ${array[3]}
```

##### 读取整个数组

格式：

```shell
${array[@]} #第一种写法
${array[*]} #第二种写法
```

例如：

```shell
array=(1 abc "def" jtx)

echo ${array[@]}	#第一种读取整个数组的写法
echo ${array[*]} 	#第二种读取整个数组的写法
```

##### 数组长度

类似于字符串

```shell
${#array[@]}	#第一种写法
${#array[*]}	#第二种写法
```

例如：

```shell
array=(1 abc "def" jtx)

echo ${#array[@]}	#第一种写法
echo ${#array[*]} 	#第二种写法
```

![image-20211220220631759](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211220220631759.png)

#### 6.```expr```命令

```expr```命令用于求表达式的值，格式为：

```shell
expr 表达式
```

表达式说明：

* 用空格隔开每一项
* 用反斜杠放在```shell```特定的字符前面(发现表达式运行错误时，可以试试转义)
* 对包含空格和其它特殊字符的字符串要用括号括起来
* ```expr```会在```stdout```中输出结果。如果为逻辑关系表达式，则结果为真，```stdout```为1，否则为0。
* ```expr```的```exit code```：如果为逻辑关系表达式，则结果为真，```exit code```为0，否则为1。

#####  字符串表达式

* ```length string```

  返回```string```的长度

* ```index string charset```

  ```charset```中任意单个字符在```string```中最前面的字符位置，下标从1开始。如果在```string```中完全不存在```charset```的字符，则返回0。

* ```substr string position length```

  返回```string```字符串从```position```开始，长度最大为```length```的子串。如果```position```或```length```为负数，0或非数值，则返回空字符串。

例如：

```shell
str="Hello World!"

echo `expr length "$str"`	# 表达式要放在 `` 里面，表示执行改命令，输出12
echo `expr index "$str" aWd` #输出7，下标从1开始
echo `expr substr "$str" 3 3` #输出llo
```

##### 整数表达式

```expr``` 支持普通的算术操作，算术表达式优先级低于字符串表达式，高于逻辑关系表达式。

* ```+ -```

  加减运算。两端参数会转换为整数，如果转换失败则报错。

* ```* / %```

  乘，除，取模。两端参数会转换为整数，如果转换失败则报错。

* ```( )``` 括号里面的内容优先运算，但需要用反斜杠转义。

 例如：

```shell
a=3
b=4

echo `expr $a + $b`  #输出7
echo `expr $a - $b`	 #输出-1
echo `expr $a \* $b` #输出12， *需要转义
echo `expr $a / $b`  #输出0，整除
echo `expr $a % $b`  #输出3
echo `expr \($a + 1\) \* \($b + 1\)` #输出20， 值为(a+1) * (b+1)
```

##### 逻辑关系表达式

* ``` |```

  如果第一个参数非空且非0，则返回第一个参数的值。否则，当第二个参数非空且非零时，返回第二个参数；否则返回0。如果第一个参数非空或非零，不会计算第二个参数

* ``` & ```

  如果两个参数都非空且非零，则返回第一个参数，否则返回0。 如果第一个参数为空或为零时，不会计算第二个参数。

* ``` <  <= = == != >= > ```

* 比较两端的参数，如果为```true```，则返回1，否则返回0。```==```是```=``` 的同义词。```expr```首先尝试将两端转为整数，并做算术比较，如果转换失败，则按字符集排序规则做字符比较。

* ``` () ```

  括号里面的内容优先运算，但需要用反斜杠转义。

例如：

```shell
a=3
b=4

echo `expr $a \> $b`  #输出0, 需要转义
echo `expr $a '<' $b`	 #输出1, 用''也可以
echo `expr $a '>=' $b` #输出0
echo `expr $a \<\ $b`  #输出1

c=0
d=5

echo `expr $c \& $d` #输出0
echo `expr $a \& $b` #输出3
echo `expr $c \| $d` #输出5
echo `expr $a \| $b` #输出3
```

![image-20211220215909487](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211220215909487.png)


#### 7.```read```命令

```read```命令用于从标准输入中读取单行数据。当读到文件结束符时，```exit code```为1，否则为0。

##### 参数说明

* ```-p```：后面可以接提示信息
* ```-t```：后面跟秒数，定义输入字符的等待时间，超过等待时间后会自动忽略此命令，但```-t```后面的命令继续执行

例如：

```shell
acs@740d1bdabef2:~$ read name	#读入name的值
hello jtx	#输入name的值
acs@740d1bdabef2:~$ echo $name
hello jtx	#输出name的值
acs@740d1bdabef2:~$ read -p "what's your name?" -t 10 name #读入name的值，等待时间为10s，-p提示语 
what's your name?my name is jtx	#输入name的值
acs@740d1bdabef2:~$ echo $name	#输出name的值
my name is jtx	#标准输出
acs@740d1bdabef2:~$ 
```

![image-20211220221324454](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211220221324454.png)

#### 8.```echo```命令

```echo```用于输出字符串。命令格式：

```shell
echo string
```

##### 显示普通字符串

```shell
echo "Hello jtx"
echo Hello jtx	#可以直接输出想表达的字符串
```

##### 显示转义字符

 ```shell
echo "\"Hello jtx\""	# 转义双引号
echo \"Hello jtx\"	#省略外层双引号

#示例
acs@740d1bdabef2:~$ echo "\"Hello jtx\""
"Hello jtx"
acs@740d1bdabef2:~$ echo \"Hello jtx\"
"Hello jtx"

 ```

##### 显示变量

```shell
name=jtx
echo "wo shi $name" #输出 wo shi jtx

#示例
acs@740d1bdabef2:~$ name=jtx
acs@740d1bdabef2:~$ echo "wo shi $name"
wo shi jtx	#输出结果
```

##### 显示换行

```shell
echo -e "Hi\n"	# -e 开启转义
echo "jwjtx"

#示例
acs@740d1bdabef2:~$ echo -e "Hi \n jwjtx" 
Hi              #输出，换行                                                        
 jwjtx                                                                                  
acs@740d1bdabef2:~$ echo "Hi \n jwjtx"      
Hi \n jwjtx           #输出，不换行       
```

##### 显示不换行

```shell
echo -e "Hi \c"	# -e 开启转义 \c 不换行
echo "jwjtx"

#示例
acs@740d1bdabef2:~$ echo -e "Hi \c" 
Hi acs@740d1bdabef2:~$ 	#输出 Hi 后，没有换行
```

##### 显示结果定向至文件

```shell
echo "Hello World" > out.txt 	#将内容输出到out.txt中
```

##### 原样输出字符串，不进行转义或取变量(用单引号)

```shell
name=jtx
echo '$name\"'

#输出结果
acs@740d1bdabef2:~$ name=jtx
acs@740d1bdabef2:~$ echo '$name\"'
$name\"				#输出结果
```

##### 显示命令的执行结果

```shell
echo `date`

#输出结果
acs@740d1bdabef2:~$ echo `date`
Tue Dec 21 16:02:24 CST 2021

```

#### 9.```printf```命令

```printf```命令用于格式化输出，类似于```C++```的```printf```函数。

默认不会在字符串末尾添加换行符。

命令格式：

```shell
printf format-string [arguments...]
```

##### 用法示例

脚本内容：

```shell
printf "%10d.\n" 123	# 占10位，右对齐
printf "%-10.2f.\n" 566.545532	#占10位，保留2位小数，左对齐
printf "my name is %s\n" "jtx"	#格式化输出字符串
printf "%d * %d = %d\n" 2 3 `expr 2 \* 3`	#表达式的值作为参数

#输出样例
       123.
566.55    . 
my name is jtx 
2 \* 3 = 6

```

![image-20211221202343082](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211221202343082.png)

#### 10.```test```命令与判断符号```[]```

##### 逻辑运算符```&&```和```||```

* ```&&```表示与，```||```表示或

* 二者具有短路原则：

  ```expr1 && expr2```：当```expr1```为假时，直接忽略```expr2```

  ```expr1 || expr2```：当```expr1```为真时，直接忽略```expr2```

* 表达式的```exit code```为0，表示真；为非零，表示假。(**与我们平常所见相反**)

##### ```test```命令

在命令行中输入```man test```，可以查看```test```命令的用法。

```test```命令用于判断文件类型，以及对变量做比较。

```test```命令用```exit code```返回结果，而不是使用```stdout```。0表示真，非0表示假。

例如：

```shell
test 2 -lt 3	# 2<3，为真，返回值为0
echo $?			#输出上个命令的返回值，输出0

acs@740d1bdabef2:~$ test 2 -lt 3
acs@740d1bdabef2:~$ echo $?
0
```

```shell
acs@740d1bdabef2:~$ ls
EOF  homework  main.cpp  test.sh  test4.sh  test5.sh  test6.sh  test8.sh  test_zs.sh  testbl.sh
acs@740d1bdabef2:~$ test -e test.sh && echo "exist" || echo "not exist"	#判断test.sh是否存在，存在输出"exist", 不存在输出"not exist"
exist
acs@740d1bdabef2:~$ test -e test1.sh && echo "exist" || echo "not exist" #同理判断test1.sh是否存在
not exist
```

##### 文件类型判断

命令格式：

```shell
test -e filename	#判断文件是否存在
```

相关参数：

* ```-e```：代表文件是否存在
* ```-f```：判断是否为文件
* ```-d```：判断是否为目录

##### 文件权限判断

命令格式：

```shell
test -r filename	#判断文件是否可读
```

相关参数：

* ```-r```:文件是否可读
* ```-w```：文件是否可写
* ```-x```：文件是否可执行
* ```-s```：是否为非空文件

##### 整数间的比较

命令格式：

```shell
test $a -eq $b	# a 是否等于 b
```

相关参数：

* ```-eq```：两端是否相等
* ```-ne```：两端是否不相等
* ```-gt```：左边是否大于右边
* ```-lt```：左边是否小于右边
* ```-ge```：左边是否大于等于右边
* ```-le```：左边是否小于等于右边

##### 字符串比较

| 测试参数                | 代表意义                                                    |
| ----------------------- | ----------------------------------------------------------- |
| ```test -z string```    | 判断string是否为空，如果为空，则返回```true```              |
| ```test -n string```    | 判断string是否非空，如果非空，则返回```true```(-n 可以省略) |
| ```test str1 == str2``` | 判断```str1```是否等于```str2```                            |
| ```test str1 != str2``` | 判断```str1```是否不等于```str2```                          |

##### 多重条件判定

命令格式：

```shell
test -r filename -a -x filename
```

相关参数：

* ```-a```：两条件是否同时成立
* ```-o```：两条件是否至少成立一个
* ```!```：取反。

##### 判断符号```[]```

```[]```和```test```用法几乎一模一样，更常用于```if```语句中。另外```[[]]```是```[]```的加强版，支持的特性更多。

例如：

```shell
[ 2 -lt 3]	#2<3,为真，返回值为0
echo $?	#输出上个命令的返回值，0

#样例
acs@740d1bdabef2:~$ [ 2 -lt 3 ]
acs@740d1bdabef2:~$ echo $?
0
```

```shell
acs@740d1bdabef2:~$ ls
EOF  homework  main.cpp  test.sh  test4.sh  test5.sh  test6.sh  test8.sh  test_zs.sh  testbl.sh
acs@740d1bdabef2:~$ [ -e test.sh ] && echo "exist" || echo "not exist"
exist
acs@740d1bdabef2:~$ [ -e test1.sh ] && echo "exist" || echo "not exist"
not exist
```

注意：

* ```[]``` 内的每一项都要用空格隔开
* 中括号内的变量，最好用双引号括起来
* 中括号内的常数，最好用单或双引号括起来

例如：

```shell
name="hi jtx"
[ $name == "hi jtx" ]	#错误，等价于[ hi jtx == "hi jtx" ]
[ "$name" == "hi jtx" ] #正确

#示例
acs@740d1bdabef2:~$ name="hi jtx"	
acs@740d1bdabef2:~$ [ $name == "hi jtx" ]	#错误，参数太多
-bash: [: too many arguments
acs@740d1bdabef2:~$ [ "$name" == "hi jtx" ]	#正确，0为真
acs@740d1bdabef2:~$ echo $?
0
```

![image-20211221214227847](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211221214227847.png)

#### 11.判断语句

##### ```if...then```形式

类似于我们经常使用的```if-else```语句。

#### 单层```if```

命令格式：

```shell
if condition
then
	语句1
	语句2
	...
fi
```

示例：

```shell
a=3
b=4

if [ "$a" -lt "$b" ] && [ "$a" -gt 2 ]
then
	echo ${a}in range
fi
```

输出结果：

```shell
3in range
```

![image-20211222205308984](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211222205308984.png)

##### 单层```if-else```

命令格式

```shell
if condition
then 
	语句1
	语句2
	...
else
	语句1
	语句2
	...
fi
```

示例：

```shell
a=3
b=4

if ![ "$a" -lt "$b" ]
then
	echo ${a} not lt ${b}
else
	echo ${a} lt ${b}
fi
```

输出结果：

```shell
3 lt 4
```

![image-20211222211502102](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211222211502102.png)



##### 多层```if-else-elif-else```

命令格式：

```shell
if condition
then
	语句1
	语句2
	...
elif condition
then
	语句1
	语句2
	...
elif condition
then 
	语句1
	语句2
	...
fi
```

示例：

```shell
a=3

if [ $a -eq 1 ]
then
	echo ${a} equals 1
elif [ $a -eq 2 ]
then
	echo ${a} equals 2
elif [ $a -eq 3 ]
then 
	echo ${a} equals 3
else
	echo others
fi
```

输出结果

```shell
3 equals 3
```

![](G:\Personal\CSDN\Linux基础系统篇章\lesson3-1\if-elif-else.png)



##### ```case...esac```形式

类似于我们常用的```switch```语句。

 命令格式：

```shell
case $变量名称 in
	值1)
		语句1
		语句2
		...
		;;	#类外于我们用的break语句
	值2)
		语句1
		语句2
		...
		;;
	*)	#类似于我们用的 default
		语句1
		语句2
		...
		;;
esac
```

示例：

```shell
a=4

case $a in
	1)
		echo ${a} equals 1
		;;
	2)
		echo ${a} equals 2
		;;
	3)
		echo ${a} equals 3
		;;
	*)
		echo "others"
esac
```

输出结果

```shell
others
```

![image-20211222213334693](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211222213334693.png)



#### 12.循环语句

##### ```for...in...do...done```

命令格式：

```shell
for var in var1 var2 var3
do 
	语句1
	语句2
	语句3
	...
done
```

示例1，输出 ```a 2 cc ```，每个元素一行：

```shell
for i in a 2 cc
do 
	echo $i
done
```

![image-20211224203541573](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211224203541573.png)



示例2， 输出当前路径下的所有文件名，每个文件名一行：

```shell
for file in `ls`
do 
	echo $file
done
```

![image-20211224203724023](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211224203724023.png)

示例3， 输出```1-10```

```shell
for i in $(seq 1 10)
do 
	echo $i
done
```

![image-20211224203915366](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211224203915366.png)



示例4， 使用```{1..10}``` 或 ```{a..z}```

```shell
for i in {1..10}
do 
	echo $i
done
```

![image-20211224204345707](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211224204345707.png)



##### ```for((...;...;...)) do...done```

命令格式：

```shell
for((表达式; 条件; 表达式))
do 
	语句1
	语句2
	...
done
```

示例：输出1-10，每个数占一行

```shell
for ((i = 1; i <= 10; i ++))
do 
	echo $i
done
```

![image-20211224214222916](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211224214222916.png)



##### ```while...do...done```循环

命令格式：

```shell
while condition
do
	语句1
	语句2
	...
done
```

示例，文件结束符为```Ctrl+d```，输入文件结束符后```read```指令返回```false```。

```shell
while read name
do 
	echo $name
done
```

![image-20211224214441219](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211224214441219.png)

##### ```until...do...done```循环

当条件为真时结束。

命令格式：

```shell
until condition
do 
	语句1
	语句2
	...
done
```

示例，当用户输入```yes```或者```YES```时结束，否则一直等待读入。

```shell
until [ "${word}" == "yes" ] || [ "${word}" == "YES" ]
do
	read -p "input yes/YES to stop:" word
done
```

![image-20211224215538344](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211224215538344.png)

##### ```break```命令

跳出当前一层循环，```shell```里面的```break```不能跳出```case```语句。

示例

```shell
while read name
do
	for ((i=1;i<=10;i++))
	do
		case $i in
			8)
				break
				;;
			*)
				echo $i
				;;
		esac
	done
done
```

该示例每读入非```EOF```的字符串，会输出```1-7```。（因为在8的时候，break就跳出了循环)

该程序可以输入```Ctrl+d```文件结束符来结束，也可以直接用```Ctrl+c```杀掉该进程。



##### ```continue```命令

跳出当前循环。

示例：

```shell
#输出1-10中的奇数

for ((i=1;i<=10;i++))
do
	if [ `expr $i % 2` -eq 0 ]
	then
		continue
	fi
		echo $i
done
```

![image-20211224220834907](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211224220834907.png)

##### 死循环的处理方式

如果```AC Terminal```可以打开程序，则输入```Ctrl + c```即可。

否则可以直接关闭进程：

* 1.使用```top```命令找到进程的```PID```
* 2.输入```kill -9 PID```即可关掉此进程



#### 13.函数

```bash```中的函数跟平时使用的函数，但```return```的返回值与平时使用的函数可能不同，返回的是```exit code```，取值为```0-255```，```0```表示正常结束。

如果想获取函数的输出结果，可以通过```echo```输出到```stdout```中，然后通过```$(function_name)```来获取```stdout```中的结果。

函数的```return```值可以通过```$?```来获取。

命令格式：

```shell
[function] func_name() {	# function关键字可以省略
	语句1
	语句2
	...
}
```



##### 不获取```return```值和```stdout```值

示例

```shell
func() {
	name=jtx
	echo "hi, $name"
}

#调用func
func
```

输出结果：

![image-20211225151358979](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211225151358979.png)

##### 获取```return```值和```stdout```值

不写```return```时，默认```return 0```。

示例

```shell
func(){
	name=jtx
	echo "hi,$name"
	
	return 123
}

output=$(func)
ret=$?

echo "output = $output"
echo "return = $ret"

```

![image-20211225153322561](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211225153322561.png)



#####  函数的输入参数

在函数内，```$1```表示第一个输入参数，```$2```表示第二个输入参数，依次类推。

函数内的```$0```仍然是文件名，而不是函数名。

示例：

```shell
func() {	#递归计算 $1 + ($1 - 1) + ($2 - 2) + ... + 0 
	flag=""
	while [ "${flag}" != "y" ] && [ "${flag}" != "n"]
	do
		read -p "enter the func($1) ? please input y/n: " word
	done
	
	if [ "${flag}" == 'n' ]
	then
		echo 0
		return 0
	fi
	
	if [ $1 -le 0 ]
	then
		echo 0
		return 0
	fi
	
	sum=$(func $(expr $1 - 1))
	echo $(expr $sum + $1)
	
}

echo $(func 10)
```

输出结果：55

![image-20211225203744381](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211225203744381.png)

##### 函数内的局部变量

可以在函数内定义局部变量，作用范围仅在当前函数内。

可以在递归函数中定义局部变量。

命令格式：

```shell
local 变量名=变量值
```

例如：

```shell
#! /bin/bash
 
func() {
     local name=jtx
     echo $name
}

func
echo $name

#输出结果中，第一行的jtx为函数内输出，第二行空位函数外调用找不到此值
```

![image-20211225204215801](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211225204215801.png)





#### 14.```exit```命令

```exit```命令用来退出当前```shell```进程，并返回一个退出状态；使用```$?```可以接收这个退出状态。

```exit```命令可以接受一个整数值作为参数，代表退出状态。如果不指定，默认状态值是0。

```exit```退出状态只能是一个介于```0~255```之间的整数，其中只有```0```表示成功，其他值都表示失败。



示例：

```shell
#! /bin/bash

if [ $# -ne 1 ] #如果传入参数个数等于1，则正常退 出
then
    echo "exit normal"
    exit 1
else
    echo "exit not normal"
    exit 0
fi

```

![image-20211225205831468](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211225205831468.png)



#### 15.文件重定向

每个进程默认打开3个文件描述符：

* ```stdin```标准输入，从命令行读取数据，文件描述符为0
* ```stout```标准输出，向命令行输出数据，文件描述符为1
* ```stderr```标准错误输出，向命令行输出数据，文件描述符为2

可以用文件重定向将这三个文件重定向到其他文件中。

<hr>

##### 重定向命令表

| 命令                     | 说明                                                |
| ------------------------ | --------------------------------------------------- |
| ```command  >  file```   | 将```stdout```重定向到```file```中                  |
| ```command  <  file```   | 将```stdin```重定向到```file```中                   |
| ```command  >>  file```  | 将```stdout```以追加方式重定向到```file```中        |
| ```command  n>  file```  | 将文件描述符```n```重定向到```file```中             |
| ```command  n>>  file``` | 将文件描述符```n```以追加的方式重定向到```file```中 |

##### 输入和输出重定向

```shell
echo -e "Hi,\c" > out.txt #将stdout重定向到 out.txt中
echo "hi,my name is jtx" >> out.txt	#将字符串追加到out.txt中

read str < out.txt 	#从out.txt中读取字符串

echo $str
```

![](G:\Personal\CSDN\Linux基础系统篇章\lesson3-1\redirection1.png)

##### 同时重定向```stdin```和```stdout```

```shell
#! /bin/bash

read a
read b

echo $(expr $a + $b)
```

```touch```一个```input.txt```，里面的内容为：

```bash
20 
30
```

执行命令：

```shell
acs@740d1bdabef2:~/redirection$ ls
input.txt  test.sh
acs@740d1bdabef2:~/redirection$ cat input.txt #查看input.txt内容
20
30
acs@740d1bdabef2:~/redirection$ ./test.sh < input.txt > output.txt	#input.txt内容作为输入，执行结果输出到output.txt中
acs@740d1bdabef2:~/redirection$ cat output.txt 	#查看output.txt
50
```



![image-20211225211844481](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211225211844481.png)

#### 16.引入外部脚本

引入外部脚本类似我们```c/c++```中的```include```，类似```java```中的```import```，```bash```也可以引入其他文件中的代码。

语法格式：

```shell
. filename	#一个点 空格 文件名

source filename	#第二种方法
```

示例：

```touch```第一个文件```file1.sh```，内容：

```shell
#! /bin/bash

firstFile=firstName	#创建变量firstFile
```

```touch```第二个文件```file2.sh```，内容：

```shell
#! /bin/bash

source file1.sh	# 或者 . file1.sh

echo "this is firstFile: $name"	
```

![image-20211225212930347](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211225212930347.png)



#### ```Shell```常用语法完结

到这里，关于```Linux```基础中篇幅最大的```Shell```常用语法就结束了。内容相对来说不算太多，也不算太少，用心即可学好。理论是一方面，最重要的还是实践，关于命令这个东西，我个人觉得不需要背，用的次数多了便记住了，对于这部分也是这样，我们通过学习理论知识，对整个知识框架有了了解，那么接下来就是把理论运用于实践当中，熟能生巧！













