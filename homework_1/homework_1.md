# homework 1

### question

```bash
#	创建好作业后，先进入文件夹/home/acs/homework/lesson_1/，然后：
    (0) 进入homework_0文件夹，分别创建文件夹dir_a, dir_b, dir_c
    (1) 进入homework_1文件夹，将a.txt, b.txt, c.txt 分别复制成: a.txt.bak, b.txt.bak, c.txt.bak
    (2) 进入homework_2文件夹，将a.txt, b.txt, c.txt 分别重命名为: a_new.txt, b_new.txt, c_new.txt
    (3) 进入homework_3文件夹，将dir_a文件夹下的a.txt, b.txt, c.txt分别移动到文件夹dir_b下
    (4) 进入homework_4文件夹，将普通文件a.txt, b.txt, c.txt删除
    (5) 进入homework_5文件夹，将文件夹dir_a, dir_b, dir_c删除
    (6) 进入homework_6文件夹，查看task.txt的内容，并按其指示进行操作
    (7) 进入homework_7文件夹，创建文件夹dir_0, dir_1, dir_2，
        将a.txt, b.txt, c.txt复制到dir_0下，重命名为a0.txt, b0.txt, c0.txt;
        将a.txt, b.txt, c.txt复制到dir_1下，重命名为a1.txt, b1.txt, c1.txt;
        将a.txt, b.txt, c.txt复制到dir_2下，重命名为a2.txt, b2.txt, c2.txt;
    (8) 进入homework_8文件夹，分别在dir_a, dir_b, dir_c文件夹下查看task.txt的内容，并分别按照指示进行操作
    (9) 进入homework_9文件夹，将其中所有txt类型的文件删除
```

### Answer

```shell
(0) 
cd homework_0
mkdir dir_a dir_b dir_c

(1)
cd homework_1
cp a.txt a.txt.bak
cp b.txt b.txt.bak
cp c.txt c.txt.bak

(2)
cd homework_2
mv a.txt a_new.txt
mv b.txt b_new.txt
mv c.txt c_new.txt

(3)
cd homework_3
mv dir_a/a.txt dir_b/a.txt
mv dir_a/b.txt dir_b/b.txt
mv dir_a/c.txt dir_b/c.txt

#简便方法
mv dir_a/* dir/b

(4)
cd homework_4
rm a.txt b.txt c.txt 

(5)
cd homework_5 
rm dir_a dir_b dir_c -r.

#因为homework_5文件夹下只有文件夹dir_a, dir_b, dir_c，故可以使用
rm * -r


(6)
cd homework_6
cat task.txt 
#将task.txt重命名为done.txt, 创建目录dir_a，将done.txt移动到目录dir_a下
mv task.txt done.txt
mkdir dir_a
mv done.txt dir_a/done.txt

#或者先创建 dir_a ，然后直接
mv task.txt dir_a/done.txt

(7)
cd homework_7
mkdir dir_0 dir_1 dir_2
cp a.txt dir_0/a0.txt
cp b.txt dir_0/b0.txt
cp c.txt dir_0/c0.txt
cp a.txt dir_1/a1.txt
cp b.txt dir_1/b1.txt
cp c.txt dir_1/c1.txt
cp a.txt dir_2/a2.txt
cp b.txt dir_2/b2.txt
cp c.txt dir_2/c2.txt

(8)
cd homework_8
cd dir_a
cat task.txt 
#将a.txt删除
rm a.txt 
cd ..
cd dir_b
cat task.txt 
#将b.txt重命名为b_new.txt
mv b.txt b_new.txt
cd ..
cd dir_c
cat task.txt 
#将c.txt复制成c.txt.bak
cp c.txt c.txt.bak

#中间的 cd .. 和 cd dir_b 可以结合使用， 
cd ../dir_b

(9)
cd homework_9
rm *.txt
```



### Result

![image-20211031165354996](C:\Users\86178\AppData\Roaming\Typora\typora-user-images\image-20211031165354996.png)

