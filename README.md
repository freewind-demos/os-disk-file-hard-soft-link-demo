OS Disk File Hard Soft Link Demo
================================

文件系统中，我们可以为文件生成链接，就像是别名一样。

链接分为硬连接和软连接：

- 硬连接：一个文件和它的“硬连接”，都是同一个文件的别名，拥有相同的`inode`
- 软连接：软连接是一个新的文件，记录了指向信息，拥有不同的`inode`

注：`inode`是文件真正的唯一标识，文件名只是方便使用的别名

创建普通文件
----------

`hello.txt`是一个普通文件：

```
touch hello.txt
```

`ls -i`显示inode
--------------

通过`-i`显示文件的inode：

```
$ ls -i hello.txt

20773646 hello.txt
```

产生一个硬链接：

```
$ ln hello.txt hello-hard.txt
```

```
$ ls -i hello.txt hello-hard.txt

20773646 hello.txt
20773646 hello-hard.txt
```

可见两者的inode是完全一样的，也就是说，这两个文件名指向的是同一个文件。

`ls -l`显示文件被硬连接的数量
------------------

```
$ ls -l

-rw-r--r--  1 freewind  staff  731 Sep  4 18:29 README.md
-rw-r--r--  2 freewind  staff    6 Sep  4 18:19 hello-hard.txt
-rw-r--r--  2 freewind  staff    6 Sep  4 18:19 hello.txt
```

其中的第2列的数字，就是该文件被“硬连接”的次数。
可以看到`README.md`只有一个，而`hello.txt`和`hello-hard.txt`对应的数字是`2`

软连接
---

为了证明上面两点是“硬连接”的特性，我们再创建一个软连接对比：

```
$ ln -s hello.txt hello-soft.txt
```

```
$ ls -i hello*.txt

20773646 hello.txt
20773646 hello-hard.txt
20773658 hello-soft.txt
```

可见软连接产生的inode是不同的，它是一个全新的文件。

```
$ ls -l hello*.txt

-rw-r--r--  2 freewind  staff     6 Sep  4 18:19 hello-hard.txt
lrwxr-xr-x  1 freewind  staff     9 Sep  4 18:20 hello-soft.txt -> hello.txt
-rw-r--r--  2 freewind  staff     6 Sep  4 18:19 hello.txt
```

产生软连接后，`hello.txt`与`hello-hard.txt`对应的“硬连接数量”不变，还是`2`。
软连接`hello-soft.txt`对应数量为1，说明它是一个全新的文件。

同时还显示了`hello-soft.txt`指向了`hello.txt`。
