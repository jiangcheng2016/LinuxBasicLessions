

#### 作业

```bash
创建好作业后，先进入文件夹/home/acs/homework/lesson_3/，然后：
(0) 进入homework_0文件夹，编写自动完成lesson_1作业的脚本helper.sh。要求：
    [1] 当前目录下仅包含helper.sh
    [2] helper.sh具有可执行权限
    [3] 在任意路径依次执行下列命令后，lesson_1的作业可以得到满分：
        1) homework 1 create
        2) /home/acs/homework/lesson_3/homework_0/helper.sh
(1) 进入homework_1文件夹，编写脚本check_file.sh。要求：
    [1] 当前目录下仅包含check_file.sh。
    [2] check_file.sh具有可执行权限。
    [3] check_file.sh接收一个传入参数。格式为 ./check_file.sh file
    [4] 判断传递参数，分别在标准输出中输出如下内容（不包括双引号）：
        1) 如果传入参数个数不是1，则输出一行："arguments not valid"，然后退出，退出状态等于1。
        2) 如果file文件不存在，则输出一行："not exist"，然后退出，退出状态等于2。
        3) 如果file文件存在，则输出分别进行如下5个判断，然后退出，退出状态等于0。
            1] 如果file为普通文件，则输出一行："regular file"
            2] 如果file为目录（文件夹），则输出一行："directory"
            3] 如果file具有可读权限，则输出一行："readable"
            4] 如果file具有可写权限，则输出一行："writable"
            5] 如果file具有可执行权限，则输出一行："executable"
(2) 进入homework_2文件夹，编写脚本main.sh。要求：
    [1] 当前目录下仅包含main.sh
    [2] main.sh具有可执行权限
    [3] 该文件从stdin(标准输入)中读取一个整数n
    [4] 在stdout(标准输出)输出斐波那契数列的第n项。即：a[0] = 1, a[1] = 1, a[i] = a[i - 1] + a[i - 2], 求a[n]。
    [5] 数据保证 0 <= n <= 20，脚本不需要判断n的合法性。
(3) 进入homework_3文件夹，编写脚本main.sh。要求：
    [1] 当前目录下仅包含main.sh
    [2] main.sh具有可执行权限
    [3] 该文件从stdin(标准输入)中读取两行整数n和m
    [4] 在stdout(标准输出)中输出1~n的按字典序从小到大的顺序数第m个全排列，输出一行，用空格隔开所有数，行末可以有多余空格。
    [5] 数据保证 1 <= n <= 10, 1 <= m <= min(100, n!)，脚本不需要判断数据的合法性。
(4) 进入homework_4文件夹，编写脚本main.sh。要求：
    [1] 当前目录下仅包含main.sh
    [2] main.sh具有可执行权限
    [3] main.sh接收两个传入参数。格式为 ./main.sh input_file output_file
    [4] 从input_file中读取一个正整数n，然后将前n个正整数的平方和写入output_file中
    [5] 数据保证 1 <= n <= 100，脚本不需要判断所有数据的合法性。

```

#### 答案

#####  [1] 答案

###### 1.创建文件```helper.sh```，可通过```touch helper.sh```或```vim helper.sh```
###### 2.给```helper.sh```加可执行权限：```chmod +x helper.sh```
###### 3.编写代码如下：

```shell
#! /bin/bash


#**************homework 0 ******************

dir0=/home/acs/homework/lesson_1/homework_0

homework 1 create 0

for i in a b c
do
    mkdir ${dir0}/dir_${i}
done


#**************homework 1 ******************

dir1=/home/acs/homework/lesson_1/homework_1

homework 1 create 1

for i in a b c
do
    cp ${dir1}/${i}.txt ${dir1}/${i}.txt.bak
done


#**************homework 2 ******************

dir2=/home/acs/homework/lesson_1/homework_2

homework 1 create 2

for i in a b c
do
    mv ${dir2}/${i}.txt ${dir2}/${i}_new.txt
done


#**************homework 3 ******************

dir3=/home/acs/homework/lesson_1/homework_3

homework 1 create 3

for i in  a b c
do
    mv ${dir3}/dir_a/${i}.txt ${dir3}/dir_b
done


#**************homework 4 ******************

dir4=/home/acs/homework/lesson_1/homework_4

homework 1 create 4

rm ${dir4}/*

#**************homework 5 ******************

dir5=/home/acs/homework/lesson_1/homework_5

homework 1 create 5

rm ${dir5}/* -r

#**************homework 6 ******************

dir6=/home/acs/homework/lesson_1/homework_6

homework 1 create 6

mkdir ${dir6}/dir_a
mv ${dir6}/task.txt ${dir6}/dir_a/done.txt

#**************homework 7 ******************

dir7=/home/acs/homework/lesson_1/homework_7

homework 1 create 7

for i in 0 1 2
do
    mkdir ${dir7}/dir_${i}
    for j in a b c
    do
        cp ${dir7}/${j}.txt ${dir7}/dir_${i}/${j}${i}.txt
    done
done



#**************homework 8 ******************

dir8=/home/acs/homework/lesson_1/homework_8

homework 1 create 8

rm ${dir8}/dir_a/a.txt

mv ${dir8}/dir_b/b.txt ${dir8}/dir_b/b_new.txt

cp ${dir8}/dir_c/c.txt ${dir8}/dir_c/c.txt.bak


#**************homework 9 ******************

dir9=/home/acs/homework/lesson_1/homework_9

homework 1 create 9

rm ${dir9}/*.txt
```

#####  [2] 

###### 1.创建文件```check_file.sh```，可通过```touch check_file.sh```或```vim check_file.sh```

###### 2.给```check_file.sh```加可执行权限：```chmod +x check_file.sh```

###### 3.编写代码如下：

```shell
#! /bin/bash

if [ $# -ne 1 ]
then
    echo arguments not valid
    exit 1
fi

if [ ! -e "$1" ]
then
    echo not exist
    exit 2
fi

if [ -f "$1" ]
then
    echo regular file
fi

if [ -d "$1" ]
then
    echo directory
fi

if [ -r "$1" ]
then
    echo readable
fi

if [ -w "$1" ]
then
    echo writable
fi

if [ -x "$1" ]
then
    echo executable
fi

```

#####  [3] 

###### 1.创建文件```main.sh```，可通过```touch main.sh```或```vim main.sh```

###### 2.给```mian.sh```加可执行权限：```chmod +x main.sh```

###### 3.编写代码如下：

```shell
#! /bin/bash

read n

res[0]=1
res[1]=1

for ((i=2;i<=n;i++))
do
    x=`expr $i - 1`
    y=`expr $i - 2`

    res[$i]=`expr ${res[$x]} + ${res[$y]}`

done

echo ${res[$n]}
```

#####  [4] 

###### 1.创建文件```main.sh```，可通过```touch main.sh```或```vim main.sh```

###### 2.给```mian.sh```加可执行权限：```chmod +x main.sh```

###### 3.编写代码如下：

```shell

```



#####  [5] 

###### 1.创建文件```main.sh```，可通过```touch main.sh```或```vim main.sh```

###### 2.给```mian.sh```加可执行权限：```chmod +x main.sh```

###### 3.编写代码如下：

```shell
#! /bin/bash

input=$1
output=$2

read n < $input

sum=0

for ((i=1;i<=n;i++))
do
    y=`expr $i \* $i`
    sum=`expr $sum + $y`

done

echo $sum > $output

```

