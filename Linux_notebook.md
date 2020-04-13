# Linux命令行与Shell脚本学习笔记

## 1 基本bash shell命令

### 1. ls 显示列表

- -a：显式所有文件，包括隐藏文件
- -F：在文件夹后面加上"/"，可以很容易的区分文件和文件夹
- -R：遍历所有文件夹中的子文件夹
- -l：

```bash
-rw-rw-rw- 1 ubuntu ubuntu  72 Jun 13 14:10 log.txt
drwxrwxrwx 1 ubuntu ubuntu 512 Jun 14 09:24 test

各自段值解释如下：
 文件类型，比如目录（ d ）、文件（ - ）、字符型文件（ c ）或块设备（ b ）； 
 文件的权限（参见第6章）； 
 文件的硬链接总数； 
 文件属主的用户名； 
 文件属组的组名； 
 文件的大小（以字节为单位）； 
 文件的上次修改时间； 
 文件名或目录名。 
```

- -l **file/director**y：显式匹配到的文件或者文件夹（完全匹配）
- -l ***/?**：模糊匹配，？只匹配1个字符，星号匹配任意字符
- -l [ai]：使用中括号匹配，ls -l my_scr[ai]pt ，会匹配 my_scrapt或者my_script
- -l [a-i]：范围查找
- -l [!a]：查找时排除a

### 2. 创建文件

### 3. 复制文件

**cp -i *source destination***：加上i，在目标文件存在的情况下，会提示是否覆盖目标文件

```bash
ubuntu@admin:~$ cp -i input.txt test/input.txt
cp: overwrite 'test/input.txt'? y
```

**cp -R source destination**：会复制source中所有的文件或者子目录，相当于建立了一个完全一样的副本。

### 4. 链接文件

符号链接：

**ln -s data_file  sl_data_file** 两个物理文件链接，将sl_data_file链接到data_file

```bash
$ ln -s data_file  sl_data_file 
$ ls -l *data_file 
-rw-rw-r-- 1 christine christine 1092 May 21 17:27 data_file 
lrwxrwxrwx 1 christine christine    9 May 21 17:29 sl_data_file -> data_file
```

硬链接：源文件事先存在，硬链接会建立独立的虚拟文件，其中包含了源文件的信息及位置，根本上而言两者就是同一个文件，引用硬链接文件等于引用了源文件。

**ln code_file  hl_code_file**

```bash
$ ln code_file  hl_code_file 
$ ls -li *code_file 
296892 -rw-rw-r-- 2 christine christine 189 May 21 17:56 code_file 
296892 -rw-rw-r-- 2 christine christine 189 May 21 17:56 hl_code_file 
```

### 5. 重命名文件

**mv fall fell**：只影响文件名，其他任何信息不受影响。

### 6. 删除文件

**rm -i test_file**：-i命令会提示，是不是真的要删除文件

### 7. 创建目录

mkdir

想要同时创建多个目录和子目录，需要加上-p参数

```bash
mkdir -p New_Dir/Sub_Dir/Under_Dir 
```

### 8. 删除目录

rmdir：基本命令，默认情况下该命令只能删除空目录；

rm -ri my_dir：加上-ri可以使命令先进入文件夹中，将文件删除，然后再删除目录本身；

**rm -rf my_dir：终极命令，不会显示任何信息，直接将指定目录及其下面子文件，子目录全部删除；**

### 9. 查看文件内容

查看文件类型：file my_file

查看整个文件：**cat** 

- -n：加上-n会显示行号

  ```bash
  1  hello 
  2 
  3  This is a test file. 
  4 
  5 
  6  That we'll use to     test the cat command.
  ```

- -b：只给有文本的行加上行号

  ```bash
  1  hello 
   
  2  This is a test file. 
   
   
  3  That we'll use to       test the cat command. 
  ```

  -T：不显示制表符

  ```bash
  hello 
   
  This is a test file. 
   
   
  That we'll use to^Itest the cat command.
  ```

**more命令**

  cat 命令的主要缺陷是：一旦运行，你就无法控制后面的操作。为了解决这个问题，开发人员编写了 more 命令。 more 命令会显示文本文件的内容，但会在显示每页数据之后停下来。

more 命令是分页工具。在本章前面的内容里，当使用 man 命令时，分页工具会显示所选的bash手册页面。和在手册页中前后移动一样，你可以通过按空格键或回车键以逐行向前的方式浏览文本文件。浏览完之后，按q键退出。

more 命令只支持文本文件中的基本移动。

**less命令**

从名字上看，它并不像 more 命令那样高级。但是 less 命令的命名实际上是个文字游戏（从俗语“less is more”得来），它实为 more 命令的升级版。它提供了一些极为实用的特性，能够实现在文本文件中前后翻动，而且还有一些高级搜索功能。 

less 命令的操作和 more 命令基本一样，一次显示一屏的文件文本。除了支持和 more 命令相同的命令集，它还包括更多的选项。

**查看部分文件：**

**tail命令**：tail 命令会显示文件最后几行的内容（文件的“尾部”）。默认情况下，它会显示文件的末尾10行。 

可以向 tail 命令中加入 -n 参数来修改所显示的行数。在下面的例子中，通过加入 -n 2 使tail 命令只显示文件的最后两行：

```bash
tail -n 2 log_file 

line19 
Last line - line20 
```

-f 参数是 tail 命令的一个突出特性。它允许你在其他进程使用该文件时查看文件的内容。tail 命令会保持活动状态，并不断显示添加到文件中的内容。这是实时监测系统日志的绝妙方式。 

**head命令**：顾名思义，会显示文件开头那些行的内容。默认情况下，它会显示文件前10行的文本。

类似于 tail 命令，它也支持 -n 参数，这样就可以指定想要显示的内容了。这两个命令都允许你在破折号后面输入想要显示的行数：

```
head -5 log_file 

line1 
line2 
line3 
line4 
line5 
```

文件的开头通常不会改变，因此 head 命令并像 tail 命令那样支持 -f 参数特性。 head 命令是一种查看文件起始部分内容的便捷方法。

------
------

## 2 更多bash shell命令

### 2.1 监听程序

#### 2.1.1 探查进程

默认情况下， ps 命令并不会提供那么多的信息

```bash
 $ ps
  PID TTY          TIME CMD 
  3081 pts/0    00:00:00 bash 
  3209 pts/0    00:00:00 ps 
 $ 
```

默认情况下， ps 命令只会显示运行在当前控制台下的属于当前用户的进程。在此例中，我们只运行了bash shell（注意，shell也只是运行在系统上的另一个程序而已）以及 ps 命令本身。 

上例中的基本输出显示了程序的进程ID（Process  ID，PID）、它们运行在哪个终端（TTY）以及进程已用的CPU时间。

Linux系统中使用的GNU  ps 命令支持3种不同类型的命令行参数： 

- Unix风格的参数，前面加单破折线； 
- BSD风格的参数，前面不加破折线； 
- GNU风格的长参数，前面加双破折线。 

 **Unix风格的部分参数** 

```bash
-A  显示所有进程 
-N  显示与指定参数不符的所有进程 
-a  显示除控制进程（session leader）和无终端进程外的所有进程 
-d  显示除控制进程外的所有进程 
-e  显示所有进程 
-C cmdlist  显示包含在cmdlist列表中的进程 
-G grplist  显示组ID在grplist列表中的进程 
-U userlist  显示属主的用户ID在userlist列表中的进程 
-g grplist  显示会话或组ID在grplist列表中的进程
-p pidlist  显示PID在pidlist列表中的进程 
-s sesslist  显示会话ID在sesslist列表中的进程 
-t ttylist  显示终端ID在ttylist列表中的进程 
-u userlist  显示有效用户ID在userlist列表中的进程 
-F  显示更多额外输出（相对-f参数而言） 
-O format  显示默认的输出列以及format列表指定的特定列 
-M  显示进程的安全信息 
-c  显示进程的额外调度器信息 
-f  显示完整格式的输出 
-j  显示任务信息 
-l  显示长列表 
-o format  仅显示由format指定的列 
-y  不要显示进程标记（process flag，表明进程状态的标记） 
-Z  显示安全标签（security context） ① 信息 
-H  用层级格式来显示进程（树状，用来显示父进程） 
-n namelist  定义了WCHAN列显示的值 
-w  采用宽输出模式，不限宽度显示 
-L  显示进程中的线程 
-V  显示ps命令的版本号 
```

如果你想查看系统上运行的所有进程，可用 -ef参数组合（ ps 命令允许你像这样把参数组合在一起）。 

```bash
$ ps -ef 
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 09:23 ?        00:00:00 /init
root         7     1  0 09:23 tty1     00:00:00 /init
ubuntu       8     7  0 09:23 tty1     00:00:00 -bash
ubuntu     393     8  0 11:00 tty1     00:00:00 ps
$

 UID：启动这些进程的用户。 
 PID：进程的进程ID。 
 PPID：父进程的进程号（如果该进程是由另一个进程启动的）。 
 C：进程生命周期中的CPU利用率。 
 STIME：进程启动时的系统时间。 
 TTY：进程启动时的终端设备。 
 TIME：运行进程需要的累计CPU时间。 
 CMD：启动的程序名称。
```

这个例子用了两个参数： -e 参数指定显示所有运行在系统上的进程； -f 参数则扩展了输出，这些扩展的列包含了有用的信息。 

如果想要获得更多的信息，可采用 -l 参数，它会产生一个长格式输出。

```bash
$ ps -l
F S   UID   PID  PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
0 S  1000     8     7  0  80   0 -  4197 -      tty1     00:00:00 bash
0 R  1000   394     8  0  80   0 -  4271 -      tty1     00:00:00 ps
$

注意使用了 -l 参数之后多出的那些列。 
 F ：内核分配给进程的系统标记。 
 S ：进程的状态（O代表正在运行；S代表在休眠；R代表可运行，正等待运行；Z代表僵化，进程已结束但父进程已不存在；T代表停止）。 
 PRI ：进程的优先级（越大的数字代表越低的优先级）。 
 NI ：谦让度值用来参与决定优先级。 
 ADDR ：进程的内存地址。 
 SZ ：假如进程被换出，所需交换空间的大致大小。 
 WCHAN ：进程休眠的内核函数的地址。 
```

#### 2.1.2 实时进程检测

ps 命令虽然在收集运行在系统上的进程信息时非常有用，但也有不足之处：它只能显示某个特定时间点的信息。如果想观察那些频繁换进换出的内存的进程趋势，用 ps 命令就不方便了。 

而 top 命令刚好适用这种情况。 top 命令跟 ps 命令相似，能够显示进程信息，但它是实时显示的。

```bash
top - 11:31:53 up  2:08,  0 users,  load average: 0.52, 0.58, 0.59
Tasks:   4 total,   1 running,   3 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  8285752 total,  4998416 free,  3057984 used,   229352 buff/cache
KiB Swap: 11796692 total, 11763428 free,    33264 used.  5094036 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
    1 root      20   0    8892    308    272 S   0.0  0.0   0:00.12 init
    7 root      20   0    8896    212    172 S   0.0  0.0   0:00.00 init
    8 ubuntu    20   0   16788   3428   3344 S   0.0  0.0   0:00.38 bash
  395 ubuntu    20   0   17620   2080   1508 R   0.0  0.0   0:00.06 top
  
最后一部分显示了当前运行中的进程的详细列表，有些列跟 ps 命令的输出类似。 
 PID：进程的ID。 
 USER：进程属主的名字。 
 PR：进程的优先级。 
 NI：进程的谦让度值。 
 VIRT：进程占用的虚拟内存总量。 
 RES：进程占用的物理内存总量。 
 SHR：进程和其他进程共享的内存总量。 
 S：进程的状态（D代表可中断的休眠状态，R代表在运行状态，S代表休眠状态，T代表跟踪状态或停止状态，Z代表僵化状态）。 
 %CPU：进程使用的CPU时间比例。 
 %MEM：进程使用的内存占可用内存的比例。
 TIME+：自进程启动到目前为止的CPU时间总量。 
 COMMAND：进程所对应的命令行名称，也就是启动的程序名。
```

默认情况下， top 命令在启动时会按照 %CPU 值对进程排序。可以在 top 运行时使用多种交互命令重新排序。每个交互式命令都是单字符，在 top 命令运行时键入可改变 top 的行为。键入f允许你选择对输出进行排序的字段，键入d允许你修改轮询间隔。键入q可以退出 top 。用户在 top命令的输出上有很大的控制权。用这个工具就能经常找出占用系统大部分资源的罪魁祸首。

#### 2.1.3 结束进程

在Linux中，进程之间通过信号来通信。进程的信号就是预定义好的一个消息，进程能识别它并决定忽略还是作出反应。进程如何处理信号是由开发人员通过编程来决定的。大多数编写完善的程序都能接收和处理标准Unix进程信号。

```bash
信号	名称	描述
1   HUP   挂起 
2   INT   中断 
3   QUIT  结束运行 
9   KILL  无条件终止 
11  SEGV  段错误 
15  TERM  尽可能终止 
17  STOP  无条件停止运行，但不终止 
18  TSTP  停止或暂停，但继续在后台运行 
19  CONT  在STOP或TSTP之后恢复执行 
```

**1. kill命令**

kill 命令可通过进程ID（PID）给进程发信号。默认情况下， kill 命令会向命令行中列出的全部PID发送一个 TERM 信号。遗憾的是，你**只能用进程的PID而不能用命令名**，所以 kill 命令有时并不好用。 
**要发送进程信号，你必须是进程的属主或登录为root用户。**

```bash
$ kill 3940 
-bash: kill: (3940) - Operation not permitted 
$ 
```

TERM 信号告诉进程可能的话就停止运行。不过，如果有不服管教的进程，那它通常会忽略这个请求。如果要强制终止， -s 参数支持指定其他信号（用信号名或信号值）。

```bash
 kill -s HUP 3940 
```

要检查 kill 命令是否有效，可再运行 ps 或 top 命令，看看问题进程是否已停止。 

**2. killall命令** 

killall 命令非常强大，它支持通过进程名而不是PID来结束进程。 killall 命令也支持通配符，这在系统因负载过大而变得很慢时很有用。 

```bash
 killall http* 
```

 ***以root用户身份登录系统时，使用 killall 命令要特别小心，因为很容易就会误用通配符而结束了重要的系统进程。这可能会破坏文件系统。***

### 2.2 监测磁盘空间

系统管理员的另一个重要任务就是监测系统磁盘的使用情况。不管运行的是简单的Linux台式机还是大型的Linux服务器，你都要知道还有多少空间可留给你的应用程序。 
在Linux系统上有几个命令行命令可以用来帮助管理存储媒体。本节将介绍在日常系统管理中经常用到的核心命令。 

#### 2.2.1  挂载存储媒体 

Linux文件系统将所有的磁盘都并入一个虚拟目录下。在使用新的存储媒体之前，需要把它放到虚拟目录下。这项工作称为挂载（mounting）。

 **1. mount命令**

Linux上用来挂载媒体的命令叫作 mount 。默认情况下， mount 命令会输出当前系统上挂载的设备列表。

```bash
rootfs on / type lxfs (rw,noatime)
none on /dev type tmpfs (rw,noatime,mode=755)
sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,noatime)
proc on /proc type proc (rw,nosuid,nodev,noexec,noatime)
devpts on /dev/pts type devpts (rw,nosuid,noexec,noatime,gid=5,mode=620)
none on /run type tmpfs (rw,nosuid,noexec,noatime,mode=755)
none on /run/lock type tmpfs (rw,nosuid,nodev,noexec,noatime)
none on /run/shm type tmpfs (rw,nosuid,nodev,noatime)
none on /run/user type tmpfs (rw,nosuid,nodev,noexec,noatime,mode=755)
binfmt_misc on /proc/sys/fs/binfmt_misc type binfmt_misc (rw,relatime)
cgroup on /sys/fs/cgroup type tmpfs (rw,relatime,mode=755)
cgroup on /sys/fs/cgroup/devices type cgroup (rw,relatime,devices)
C:\ on /mnt/c type drvfs (rw,noatime,uid=1000,gid=1000,case=off)
D:\ on /mnt/d type drvfs (rw,noatime,uid=1000,gid=1000,case=off)
E:\ on /mnt/e type drvfs (rw,noatime,uid=1000,gid=1000,case=off)

mount 命令提供如下四部分信息： 
 媒体的设备文件名 
 媒体挂载到虚拟目录的挂载点 
 文件系统类型 
 已挂载媒体的访问状态 
```

要手动在虚拟目录中挂载设备，需要以root用户身份登录，或是以root用户身份运行 sudo 命令。下面是手动挂载媒体设备的基本命令：

```bash
mount -t type device directory 
```

type 参数指定了磁盘被格式化的文件系统类型。Linux可以识别非常多的文件系统类型。如果是和Windows PC共用这些存储设备，通常得使用下列文件系统类型。 

- vfat：Windows长文件系统。

- 
  ntfs：Windows NT、XP、Vista以及Windows 7中广泛使用的高级文件系统。 

- iso9660：标准CD-ROM文件系统。

大多数U盘和软盘会被格式化成vfat文件系统。而数据CD则必须使用iso9660文件系统类型。

后面两个参数定义了该存储设备的设备文件的位置以及挂载点在虚拟目录中的位置。比如说，手动将U盘/dev/sdb1挂载到/media/disk，可用下面的命令： 

```bash
mount -t vfat /dev/sdb1 /media/disk 
```

媒体设备挂载到了虚拟目录后，root用户就有了对该设备的所有访问权限，而其他用户的访问则会被限制。

如果要用到 mount 命令的一些高级功能，表中列出了可用的参数。

```bash
-a  挂载/etc/fstab文件中指定的所有文件系统 
-f  使mount命令模拟挂载设备，但并不真的挂载 
-F  和-a参数一起使用时，会同时挂载所有文件系统 
-v  详细模式，将会说明挂载设备的每一步 
-I  不启用任何/sbin/mount.filesystem下的文件系统帮助文件 
-l  给ext2、ext3或XFS文件系统自动添加文件系统标签 
-n  挂载设备，但不注册到/etc/mtab已挂载设备文件中 
-p num  进行加密挂载时，从文件描述符num中获得密码短语 
-s  忽略该文件系统不支持的挂载选项 
-r  将设备挂载为只读的 
-w  将设备挂载为可读写的（默认参数） 
-L label  将设备按指定的label挂载 
-U uuid  将设备按指定的uuid挂载 
-O  和-a参数一起使用，限制命令只作用到特定的一组文件系统上 
-o  给文件系统添加特定的选项 

-o 参数允许在挂载文件系统时添加一些以逗号分隔的额外选项。以下为常用的选项。 
 ro ：以只读形式挂载。 
 rw ：以读写形式挂载。 
 user ：允许普通用户挂载文件系统。 
 check=none ：挂载文件系统时不进行完整性校验。 
 loop ：挂载一个文件。 
```

**2. umount命令** 
从Linux系统上移除一个可移动设备时，不能直接从系统上移除，而应该先卸载。

卸载设备的命令是 umount （是的，你没看错，命令名中并没有字母n，这一点有时候很让人困惑）。 umount 命令的格式非常简单： 

```bash
umount [directory | device ] 
```

umount 命令支持通过设备文件或者是挂载点来指定要卸载的设备。如果有任何程序正在使用设备上的文件，系统就不会允许你卸载它： 

```
[root@testbox mnt]# umount /home/rich/mnt 
umount: /home/rich/mnt: device is busy 
umount: /home/rich/mnt: device is busy 
[root@testbox mnt]# cd /home/rich 
[root@testbox rich]# umount /home/rich/mnt 
[root@testbox rich]# ls -l mnt 
total 0 
[root@testbox rich]# 
```

如果在卸载设备时，系统提示设备繁忙，无法卸载设备，通常是有进程还在访问该设备或使用该设备上的文件。这时可用 lsof 命令获得使用它的进程信息，然后在应用中停止使用该设备或停止该进程。 lsof 命令的用法很简单： lsof /path/to/device/node ，或者 lsof /path/to/mount/point 。 

#### 2.2.2 使用df命令

有时你需要知道在某个设备上还有多少磁盘空间。 df 命令可以让你很方便地查看所有已挂载磁盘的使用情况。 

```bash
$ df 
Filesystem           1K-blocks      Used Available Use% Mounted on 
/dev/sda2             18251068   7703964   9605024  45% / 
/dev/sda1               101086     18680     77187  20% /boot 
tmpfs                   119536         0    119536   0% /dev/shm 
/dev/sdb1               127462    113892     13570  90% /media/disk 
$ 

按字段顺序说明：
 设备的设备文件位置； 
 能容纳多少个1024字节大小的块； 
 已用了多少个1024字节大小的块； 
 还有多少个1024字节大小的块可用； 
 已用空间所占的比例； 
 设备挂载到了哪个挂载点上。 
```

df 命令有一些命令行参数可用，但基本上不会用到。一个常用的参数是 -h 。它会把输出中的磁盘空间按照用户易读的形式显示，通常用M来替代兆字节，用G替代吉字节。

```bash
$ df -h 
Filesystem            Size  Used Avail Use% Mounted on 
/dev/sdb2              18G  7.4G  9.2G  45% / 
/dev/sda1              99M   19M   76M  20% /boot 
tmpfs                 117M     0  117M   0% /dev/shm 
/dev/sdb1             125M  112M   14M  90% /media/disk 
$ 
```

*说明  Linux系统后台一直有进程来处理文件或使用文件。 df 命令的输出值显示的是Linux系统认为的当前值。有可能系统上有运行的进程已经创建或删除了某个文件，但尚未释放文件。这个值是不会算进闲置空间的。* 

#### 2.2.3 使用du命令

通过 df 命令很容易发现哪个磁盘的存储空间快没了。系统管理员面临的下一个问题是，发生这种情况时要怎么办。

另一个有用的命令是 du 命令。 du 命令可以显示某个特定目录（默认情况下是当前目录）的磁盘使用情况。这一方法可用来快速判断系统上某个目录下是不是有超大文件。 

默认情况下， du 命令会显示当前目录下所有的文件、目录和子目录的磁盘使用情况，它会以磁盘块为单位来表明每个文件或目录占用了多大存储空间。对标准大小的目录来说，这个输出会是一个比较长的列表。下面是 du 命令的部分输出：

```bash
$ du 
484     ./.gstreamer-0.10 
8       ./Templates 
8       ./Download 
8       ./.ccache/7/0 
24      ./.ccache/7 
368     ./.ccache/a/d 
384     ./.ccache/a 
424     ./.ccache 
```

每行输出左边的数值是每个文件或目录占用的磁盘块数。注意，这个列表是从目录层级的最底部开始，然后按文件、子目录、目录逐级向上。

这么用 du 命令（不加参数，用默认参数）作用并不大。我们更想知道每个文件和目录占用了多大的磁盘空间，但如果还得逐页查找的话就没什么意义了。

下面是能让 du 命令用起来更方便的几个命令行参数。 

- -c ：显示所有已列出文件总的大小。 
- -h ：按用户易读的格式输出大小，即用K替代千字节，用M替代兆字节，用G替代吉字节。
- -s ：显示每个输出参数的总计。

### 2.3 处理数据文件

#### 2.3.1 排序数据

处理大量数据时的一个常用命令是 sort 命令。顾名思义， sort 命令是对数据进行排序的。 默认情况下， sort 命令按照会话指定的默认语言的排序规则对文本文件中的数据行排序。 

```bash
$ sort file2 
1 
10 
100 
145 
2 
3 
45 
75 
$ 
```

**如果你本期望这些数字能按值排序，就要失望了。默认情况下， sort 命令会把数字当做字符来执行标准的字符排序，产生的输出可能根本就不是你要的。解决这个问题可用 -n 参数，它会告诉 sort 命令把数字识别成数字而不是字符，并且按值排序。**

```bash
$ sort -n file2 
1 
2 
3 
10 
45 
75 
100 
145 
$ 
```

另一个常用的参数是 -M ，按月排序。Linux的日志文件经常会在每行的起始位置有一个时间戳，用来表明事件是什么时候发生的。如果用 -M 参数， sort 命令就能识别三字符的月份名，并相应地排序。

```bash
$ sort -M file3 
Jan 
Feb 
Mar 
Apr 
May 
Jun 
Jul 
Aug 
Sep 
Oct 
Nov 
Dec 
$ 
```

其他一些sort的参数

```bash
单破折线  双破折线  描述
-b  --ignore-leading-blanks  排序时忽略起始的空白 
-C  --check=quiet  不排序，如果数据无序也不要报告 
-c  --check  不排序，但检查输入数据是不是已排序；未排序的话，报告 
-d  --dictionary-order  仅考虑空白和字母，不考虑特殊字符 
-f  --ignore-case  默认情况下，会将大写字母排在前面；这个参数会忽略大小写 
-g  --general-number-sort  按通用数值来排序（跟-n不同，把值当浮点数来排序，支持科学计数法表示的值） 
-i  --ignore-nonprinting  在排序时忽略不可打印字符 
-k  --key=POS1[,POS2]  排序从POS1位置开始；如果指定了POS2的话，到POS2位置结束 
-M  --month-sort  用三字符月份名按月份排序 
-m  --merge  将两个已排序数据文件合并 
-n  --numeric-sort  按字符串数值来排序（并不转换为浮点数） 
-o  --output=file  将排序结果写出到指定的文件中 
-R  --random-sort  按随机生成的散列表的键值排序 
    --random-source=FILE  指定-R参数用到的随机字节的源文件 
-r  --reverse  反序排序（升序变成降序） 
-S  --buffer-size=SIZE  指定使用的内存大小 
-s  --stable  禁用最后重排序比较 
-T  --temporary-directory=DIR  指定一个位置来存储临时工作文件 
-t  --field-separator=SEP  指定一个用来区分键位置的字符 
-u  --unique  和-c参数一起使用时，检查严格排序；不和-c参数一起用时，仅输出第一例相似的两行 
-z  --zero-terminated  用NULL字符作为行尾，而不是用换行符 
```

#### 2.3.2	 搜索数据

你会经常需要在大文件中找一行数据，而这行数据又埋藏在文件的中间。这时并不需要手动翻看整个文件，用 grep 命令来帮助查找就行了。 grep 命令的命令行格式如下。 

```bash
grep [options] pattern [file] 
```

grep 命令会在输入或指定的文件中查找包含匹配指定模式的字符的行。 grep 的输出就是包含了匹配模式的行。 

```bash
$ grep three file1 
three 
$ grep t file1 
two 
three 
$ 
```

如果要进行反向搜索（输出不匹配该模式的行），可加 -v 参数。

```bash
$ grep -v t file1 
one 
four 
five 
$ 
```

如果要显示匹配模式的行所在的行号，可加 -n 参数。 

```bash
$ grep -n t file1 
2:two 
3:three 
$ 
```

如果只要知道有多少行含有匹配的模式，可用 -c 参数。 

```bash
$ grep -c t file1 
2 
$ 
```

如果要指定多个匹配模式，可用 -e 参数来指定每个模式。 

```bash
$ grep -e t -e f file1 
two 
three 
four 
five 
$ 
```

默认情况下， grep 命令用基本的Unix风格正则表达式来匹配模式。Unix风格正则表达式采用特殊字符来定义怎样查找匹配的模式。

以下是在 grep 搜索中使用正则表达式的简单例子。 

```bash
$ grep [tf] file1 
two 
three 
four 
five 
$ 
```

正则表达式中的方括号表明 grep 应该搜索包含 t 或者 f 字符的匹配。如果不用正则表达式，grep 就会搜索匹配字符串 tf 的文本。 

egrep 命令是 grep 的一个衍生，支持POSIX扩展正则表达式。POSIX扩展正则表达式含有更多的可以用来指定匹配模式的字符。 

fgrep 则是另外一个版本，支持将匹配模式指定为用换行符分隔的一列固定长度的字符串。这样就可以把这列字符串放到一个文件中，然后
在 fgrep 命令中用其在一个大型文件中搜索字符串了。 

#### 2.3.3 压缩数据

Linux包含了多种文件压缩工具。虽然听上去不错，但这实际上经常会在用户下载文件时造成混淆。

```bash
工具  文件扩展名  描述
bzip2     .bz2  采用Burrows-Wheeler块排序文本压缩算法和霍夫曼编码 
compress  .Z    最初的Unix文件压缩工具，已经快没人用了 
gzip      .gz   GNU压缩工具，用Lempel-Ziv编码 
zip       .zip  Windows上PKZIP工具的Unix实现 
```

**compress 文件压缩工具已经很少在Linux系统上看到了。gzip 是Linux上最流行的压缩工具。** 

gzip软件包是GNU项目的产物，意在编写一个能够替代原先Unix中 compress 工具的免费版本。这个软件包含有下面的工具。 

- gzip ：用来压缩文件。 

- gzcat ：用来查看压缩过的文本文件的内容。 

- gunzip ：用来解压文件。 

这些工具基本上跟 bzip2 工具的用法一样。 

```bash
 $ gzip myprog  
 $ ls -l my* 
-rwxrwxr-x 1 rich rich 2197 2007-09-13 11:29 myprog.gz 
 $ 
```

gzip 命令会压缩你在命令行指定的文件。也可以在命令行指定多个文件名甚至用通配符来一次性批量压缩文件。 

```bash
$ gzip my* 
$ ls -l my* 
 -rwxr--r--    1 rich     rich          103 Sep  6 13:43 myprog.c.gz 
 -rwxr-xr-x    1 rich     rich         5178 Sep  6 13:43 myprog.gz 
 -rwxr--r--    1 rich     rich           59 Sep  6 13:46 myscript.gz 
 -rwxr--r--    1 rich     rich           60 Sep  6 13:44 myscript2.gz 
$ 
```

gzip 命令会压缩该目录中匹配通配符的每个文件。 

#### 2.3.4 归档数据

虽然 zip 命令能够很好地将数据压缩和归档进单个文件，但它不是Unix和Linux中的标准归档工具。目前，Unix和Linux上最广泛使用的归档工具是 tar 命令。 

tar 命令最开始是用来将文件写到磁带设备上归档的，然而它也能把输出写到文件里，这种用法在Linux上已经普遍用来归档数据了。

下面是 tar 命令的格式： 

```bash
tar function [options] object1 object2 ... 
```

function 参数定义了 tar 命令应该做什么

```bash
功能  长名称  描述
-A  --concatenate  将一个已有tar归档文件追加到另一个已有tar归档文件 
-c  --create       创建一个新的tar归档文件 
-d  --diff         检查归档文件和文件系统的不同之处 
    --delete       从已有tar归档文件中删除 
-r  --append       追加文件到已有tar归档文件末尾 
-t  --list         列出已有tar归档文件的内容 
-u  --update       将比tar归档文件中已有的同名文件新的文件追加到该tar归档文件中 
-x  --extract      从已有tar归档文件中提取文件 
```

每个功能可用选项来针对tar归档文件定义一个特定行为。表中列出了这些选项中能和 tar命令一起使用的常见选项。 

```bash
选项  描述
-C dir   切换到指定目录 
-f file  输出结果到文件或设备file  
-j       将输出重定向给bzip2命令来压缩内容 
-p       保留所有文件权限 
-v       在处理文件时显示文件 
-z       将输出重定向给gzip命令来压缩内容 
```

这些选项经常合并到一起使用。首先，你可以用下列命令来创建一个归档文件： 

```bash
tar -cvf test.tar test/ test2/ 
```

上面的命令创建了名为test.tar的归档文件，含有test和test2目录内容。接着，用下列命令： 

```bash
tar -tf test.tar 
```

列出tar文件test.tar的内容（但并不提取文件）。最后，用命令： 

```bash
tar -xvf test.tar 
```

通过这一命令从tar文件test.tar中提取内容。如果tar文件是从一个目录结构创建的，那整个目录结构都会在当前目录下重新创建。 如你所见， tar 命令是给整个目录结构创建归档文件的简便方法。这是Linux中分发开源程序源码文件所采用的普遍方法。 

**下载了开源软件之后，你会经常看到文件名以.tgz结尾。这些是 gzip 压缩过的tar文件可以用命令 tar -zxvf filename.tgz 来解压**。

------
------

## 3 理解shell

### 3.1 shell类型

系统启动什么样的shell程序取决于你个人的用户ID配置。在/etc/passwd文件中，在用户ID记录的第7个字段中列出了默认的shell程序。只要用户登录到某个虚拟控制台终端或是在GUI中启动终端仿真器，默认的shell程序就会开始运行。

```bash
$ cat /etc/passwd
ubuntu:x:1000:1000:,,,:/home/ubuntu:/bin/bash
```

查看当前机中所有的shell

```bash
$ cat /etc/shells
/etc/shells: valid login shells
/bin/sh
/bin/bash
/bin/rbash
/bin/dash
/usr/bin/tmux
/usr/bin/screen
/bin/zsh
/usr/bin/zsh
```

这些shell程序各自都可以被设置成用户的默认shell。不过由于bash shell的广为流行，很少有人使用其他的shell作为默认shell。 

默认的交互shell会在用户登录某个虚拟控制台终端或在GUI中运行终端仿真器时启动。不过还有另外一个默认shell是/bin/sh，它作为默认的系统shell，用于那些需要在启动时使用的系统shell脚本。 

并不是必须一直使用默认的交互shell。可以使用发行版中所有可用的shell，只需要输入对应的文件名就行了，也就是用户可以选择使用哪种shell。例如，你可以直接输入命令 /bin/dash 来启动dash shell。

```bash
 /bin/dash  #进入shell
 exit #退出shell
```

### 3.2 shell的父子关系

用于登录某个虚拟控制器终端或在GUI中运行终端仿真器时所启动的默认的交互shell，是一个父shell。本书到目前为止都是父shell提供CLI提示符，然后等待命令输入。 

在CLI提示符后输入 /bin/bash 命令或其他等效的 bash 命令时，会创建一个新的shell程序。这个shell程序被称为子shell（child shell）。子shell也拥有CLI提示符，同样会等待命令输入。

当输入 bash 、生成子shell的时候，你是看不到任何相关的信息的，因此需要另一条命令帮助我们理清这一切。

```bash
$ ps -f 
UID        PID  PPID  C STIME TTY          TIME CMD 
501       1841  1840  0 11:50 pts/0    00:00:00 -bash 
501       2429  1841  4 13:44 pts/0    00:00:00 ps -f 
$ 
$ bash 
$ 
$ ps -f 
UID        PID  PPID  C STIME TTY          TIME CMD 
501       1841  1840  0 11:50 pts/0    00:00:00 -bash 
501       2430  1841  0 13:44 pts/0    00:00:00 bash    #父进程PPID指向-bash的PID，说明bash是-bash的子shell
501       2444  2430  1 13:44 pts/0    00:00:00 ps -f 
$ 
$ exit  #退出当前子shell
```

bash shell程序可使用命令行参数修改shell启动方式。表中列举了bash中可用的命令行参数。

```bash
-c string  从string中读取命令并进行处理 
-i         启动一个能够接收用户输入的交互shell 
-l         以登录shell的形式启动 
-r         启动一个受限shell，用户会被限制在默认目录中 
-s         从标准输入中读取命令 
```

exit 命令不仅能退出子shell，还能用来登出当前的虚拟控制台终端或终端仿真器软件。只需要在父shell中输入 exit ，就能够从容退出CLI了。 
运行shell脚本也能够创建出子shell。 

就算是不使用bash shell命令或是运行shell脚本，你也可以生成子shell。一种方法就是使用进程列表。

#### 3.2.1 进程列表

你可以在一行中指定要依次运行的一系列命令。这可以通过命令列表来实现，只需要在命令之间加入分号（;）即可。

```bash
$ pwd ; ls ; cd /etc ; pwd ; cd ; pwd ; ls ; echo $BASH_SUBSHELL
/home/Christine 
Desktop    Downloads  Music     Public     Videos 
Documents  junk.dat   Pictures  Templates 
/etc 
/home/Christine 
Desktop    Downloads  Music     Public     Videos 
Documents  junk.dat   Pictures  Templates 
0 #没有建立子shell
$ 
```

在上面的例子中，所有的命令依次执行，不存在任何问题。不过这并不是进程列表。命令列表要想成为进程列表，这些命令必须包含在括号里。 

```bash
$ (pwd ; ls ; cd /etc ; pwd ; cd ; pwd ; ls ; echo $BASH_SUBSHELL) 
/home/Christine 
Desktop    Downloads  Music     Public     Videos 
Documents  junk.dat   Pictures  Templates 
/etc 
/home/Christine 
Desktop    Downloads  Music     Public     Videos 
Documents  junk.dat   Pictures  Templates 
1 #建立了子shell
$
```

 尽管多出来的括号看起来没有什么太大的不同，但起到的效果确是非同寻常。括号的加入使命令列表变成了进程列表，生成了一个子shell来执行对应的命令。 

**进程列表是一种命令分组（command grouping）。另一种命令分组是将命令放入花括号中，并在命令列表尾部加上分号（;）。语法为 { command; } 。使用花括号进行命令分组并不会像进程列表那样创建出子shell。**

#### 3.2.2 别出心裁的子shell用法

在交互式的shell CLI中，还有很多更富有成效的子shell用法。进程列表、协程和管道（第11章会讲到）都利用了子shell。它们都可以有效地在交互式shell中使用。 

在交互式shell中，一个高效的子shell用法就是使用后台模式。

**1. 后台模式**

在后台模式中运行命令可以在处理命令的同时让出CLI，以供他用。演示后台模式的一个经典命令就是 sleep 。 

sleep 命令接受一个参数，该参数是你希望进程等待（睡眠）的秒数。这个命令在脚本中常用于引入一段时间的暂停。命令 sleep 10 会将会话暂停10秒钟，然后返回shell CLI提示符。 

要想将命令置入后台模式，可以在命令末尾加上字符 & 。把sleep命令置入后台模式可以让我们利用 ps 命令来小窥一番。

```bash
$ sleep 3000& 
[1] 2396 
$ ps -f 
UID        PID  PPID  C STIME TTY          TIME CMD 
christi+  2338  2337  0 10:13 pts/9    00:00:00 -bash 
christi+  2396  2338  0 10:17 pts/9    00:00:00 sleep 3000 
christi+  2397  2338  0 10:17 pts/9    00:00:00 ps -f 
$ 
```

sleep 命令会在后台（ & ）睡眠3000秒（50分钟）。当它被置入后台，在shell CLI提示符返回之前，会出现两条信息。第一条信息是显示在方括号中的后台作业（background  job）号（ 1 ）。第二条是后台作业的进程ID（ 2396 ）。

ps 命令用来显示各种进程。我们可以注意到命令 sleep 3000 已经被列出来了。在第二列显示的进程ID（PID）和命令进入后台时所显示的PID是一样的，都是 2396 。 除了 ps 命令，你也可以使用 jobs 命令来显示后台作业信息。 jobs 命令可以显示出当前运行在后台模式中的所有用户的进程（作业）。 

```bash
$ jobs 
[1]+  Running                 sleep 3000 & 
#作业ID  作业运行的状态  对应的命令
$ 

$ jobs -l #使用-l还可以列出更详细的信息，如PID
[1]+  2396 Running                 sleep 3000 & 
#作业ID  PID 作业运行的状态  对应的命令
$

[1]+  Done                 sleep 3000 &  #作业完成显示Done
```

**2. 将进程列表置于后台**

进程列表是运行在子shell中的一条或多条命令。使用包含了 sleep 命令的进程列表，并显示出变量 BASH_SUBSHELL。

```bash
$ (sleep 2 ; echo $BASH_SUBSHELL ; sleep 2) 
1 
$ 
```

将相同的进程列表置入后台模式会在命令输出上表现出些许不同。 

```bash
$ (sleep 2 ; echo $BASH_SUBSHELL ; sleep 2)& 
[2] 2401  #显示作业号和PID
$ 1 

#再按一次Enter键
[2]+  Done                  ( sleep 2; echo $BASH_SUBSHELL; sleep 2 ) 
$ 
```

在CLI中**运用子shell的创造性方法之一就是将进程列表置入后台模式**。你既可以在子shell中进行繁重的处理工作，同时也不会让子shell的I/O受制于终端。

将进程列表置入后台模式并不是子shell在CLI中仅有的创造性用法。协程就是另一种方法。

**3. 协程**

协程可以同时做两件事。它在后台生成一个子shell，并在这个子shell中执行命令。 要进行协程处理，得使用 coproc 命令，还有要在子shell中执行的命令。 

```bash
$ coproc sleep 10 
[1] 2544 
$ 
-----------------------------
$
jobs #查看作业
[1]+  Running    coproc COPROC sleep 10 & 
#子shell中执行的后台命令是 coproc COPROC sleep 10
#COPROC是coproc给协程起的名字，也可以自己设置名字
$
-----------------------------
$ coproc My_Job { sleep 10; } #命令用{}包裹，并且两边都要用空格分开
[1] 2570 
$ 
$ jobs 
[1]+  Running                 coproc My_Job { sleep 10; } & 
$ 
```

将协程与进程列表结合起来产生嵌套的子shell。只需要输入进程列表，然后把命令 coproc 放在前面就行了。

```bash
$ coproc ( sleep 10; sleep 2 ) 
[1] 2574 
$ 
$ jobs 
[1]+  Running     coproc COPROC ( sleep 10; sleep 2 ) & 
$ 
$ ps --forest 
  PID TTY          TIME CMD 
 2483 pts/12   00:00:00 bash 
 2574 pts/12   00:00:00  \_ bash 
 2575 pts/12   00:00:00  |   \_ sleep 
 2576 pts/12   00:00:00  \_ ps 
$ 
```

记住，生成子shell的成本不低，而且速度还慢。创建嵌套子shell更是火上浇油！ 在命令行中使用子shell能够获得灵活性和便利。要想获得这些优势，重要的是理解子shell的行为方式。对于命令也是如此。在下一节中，我们将研究内建命令与外部命令之间的行为差异。

### 3.3 理解shell内建命令

#### 3.3.1 外部命令

外部命令，有时候也被称为文件系统命令，是存在于bash shell之外的程序。它们并不是shell程序的一部分。外部命令程序通常位于/bin、/usr/bin、/sbin或/usr/sbin中。 

ps 就是一个外部命令。你可以使用 which 和 type 命令找到它。 

```bash
$ which ps 
/bin/ps 
$ 
$ type -a ps 
ps is /bin/ps 
$ 
$ ls -l /bin/ps 
-rwxr-xr-x 1 root root 93232 Jan  6 18:32 /bin/ps 
$ 
```

当外部命令执行时，会创建出一个子进程。这种操作被称为衍生（forking）。外部命令 ps 很方便显示出它的父进程以及自己所对应的衍生子进程。 

```bash
ubuntu@admin:~$ ps -f
UID        PID  PPID  C STIME TTY          TIME CMD
ubuntu       8     7  0 09:34 tty1     00:00:00 -bash
ubuntu      85     8  0 10:14 tty1     00:00:00 ps -f
```

可以看出ps -f 的父进程ID是-bash的PID

![](D:\学习\Markdown文件\图片库\13.png)

当进程必须执行衍生操作时，它需要花费时间和精力来设置新子进程的环境。所以说，外部命令多少还是有代价的。

#### 3.3.2 内建命令

内建命令和外部命令的区别在于前者不需要使用子进程来执行。它们已经和shell编译成了一体，作为shell工具的组成部分存在。不需要借助外部程序文件来运行。 

```bash
$ type cd 
cd is a shell builtin 
$ 
$ type exit 
exit is a shell builtin 
$ 
```

因为既不需要通过衍生出子进程来执行，也不需要打开程序文件，内建命令的执行速度要更快，效率也更高。

要注意，有些命令有多种实现。例如 echo 和 pwd 既有内建命令也有外部命令。两种实现略有不同。要查看命令的不同实现，使用 type 命令的 -a 选项。 

```bash
$ type -a echo 
echo is a shell builtin 
echo is /bin/echo 
$ 
```

```bash
$ which echo 
/bin/echo 
$ 
```

```bash
$ type -a pwd 
pwd is a shell builtin 
pwd is /bin/pwd 
$ 
```

```bash
$ which pwd 
/bin/pwd 
$
```


 命令 type -a 显示出了每个命令的两种实现。注意， which 命令只显示出了外部命令文件。 对于有多种实现的命令，如果想要使用其外部命令实现，直接指明对应的文件就可以了。例如，要使用外部命令 pwd ，可以输入 /bin/pwd 。 

**1. 使用history命令**

一个有用的内建命令是 history 命令。bash shell会跟踪你用过的命令。你可以唤回这些命令并重新使用。

```bash
$ history 
    1  ps -f 
    2  pwd 
    3  ls 
    4  coproc ( sleep 10; sleep 2 ) 
    5  jobs 
    6  ps --forest 
    7  ls 
```

你可以唤回并重用历史列表中最近的命令。这样能够节省时间和击键量。输入 !! ，然后按回车键就能够唤出刚刚用过的那条命令来使用。

```bash
$ ps --forest 
  PID TTY          TIME CMD 
 2089 pts/0    00:00:00 bash 
 2744 pts/0    00:00:00  \_ ps 
$ 
```

```bash
$ !! 
ps --forest 
  PID TTY          TIME CMD 
 2089 pts/0    00:00:00 bash 
 2745 pts/0    00:00:00  \_ ps 
$
```

 当输入 !! 时，bash首先会显示出从shell的历史记录中唤回的命令。然后执行该命令。 命令历史记录被保存在隐藏文件.bash_history中，它位于用户的主目录中。这里要注意的是，bash命令的历史记录是先存放在内存中，当shell退出时才被写入到历史文件中。

可以在退出shell会话之前强制将命令历史记录写入.bash_history文件。要实现强制写入，需要使用 history 命令的 -a 选项。 

***说明***  ：如 果 你 打 开 了 多 个 终 端 会 话 ， 仍 然 可 以 使 用 history  -a 命 令 在 打 开 的 会 话 中向.bash_history文件中添加记录。但是对于其他打开的终端会话，历史记录并不会自动更新。这是因为.bash_history文件只有在打开首个终端会话时才会被读取。要想强制重新读取.bash_history文件，更新终端会话的历史记录，可以使用 history -n 命令。

你可以唤回历史列表中任意一条命令。只需输入惊叹号和命令在历史列表中的编号即可。 

```bash
$ !20 
type -a pwd 
pwd is a shell builtin 
pwd is /bin/pwd 
$ 
```

**2. 命令别名**

alias 命令是另一个shell的内建命令。命令别名允许你为常用的命令（及其参数）创建另一个名称，从而将输入量减少到最低。 

你所使用的Linux发行版很有可能已经为你设置好了一些常用命令的别名。要查看当前可用的别名，使用 alias 命令以及选项 -p 。

```bash
$ alias -p 
[...] 
alias egrep='egrep --color=auto' 
alias fgrep='fgrep --color=auto' 
alias grep='grep --color=auto' 
alias l='ls -CF' 
alias la='ls -A' 
alias ll='ls -alF' 
alias ls='ls --color=auto' 
$ 
```

------

------

## 4 使用Linux环境变量

Linux环境变量能帮你提升Linux  shell体验。很多程序和脚本都通过环境变量来获取系统信息、存储临时数据和配置信息。在Linux系统上有很多地方可以设置环境变量，了解去哪里设置相应的环境变量很重要。 

### 4.1 什么是环境变量

bash  shell用一个叫作环境变量（environment variable）的特性来存储有关shell会话和工作环境的信息（这也是它们被称作环境变量的原因）。这项特性允许你在内存中存储数据，以便程序或shell中运行的脚本能够轻松访问到它们。这也是存储持久数据的一种简便方法。 

在bash shell中，环境变量分为两类： 

- 全局变量 
- 局部变量 

#### 4.1.1 全局环境变量

全局环境变量对于shell会话和所有生成的子shell都是可见的。局部变量则只对创建它们的shell可见。这让全局环境变量对那些所创建的子shell需要获取父shell信息的程序来说非常有用。

系统环境变量基本上都是使用全大写字母，以区别于普通用户的环境变量。 要查看全局变量，可以使用 env 或 printenv 命令。 

```bash
ubuntu@admin:~$ printenv
HOSTTYPE=x86_64
LESSCLOSE=/usr/bin/lesspipe %s %s
LANG=C.UTF-8
WSL_DISTRO_NAME=Ubuntu
USER=ubuntu
PWD=/home/ubuntu
HOME=/home/ubuntu
[...]
ubuntu@admin:~$
```

要显示个别环境变量的值，可以使用 printenv 命令，但是不要用 env 命令。 

```bash
$ printenv HOME 
/home/Christine 
$ 
$ env HOME 
env: HOME: No such file or directory 
$
```

 也可以使用 echo 显示变量的值。在这种情况下引用某个环境变量的时候，必须在变量前面加上一个美元符（ $ ）。 

```bash
$ echo $HOME
/home/ubuntu
$
```

在 echo 命令中，在变量名前加上 $ 可不仅仅是要显示变量当前的值。它能够让变量作为命令行参数。另外，子shell的环境变量和父shell的一致。

#### 4.1.2 局部变量

顾名思义，局部环境变量只能在定义它们的进程中可见。尽管它们是局部的，但是和全局环境变量一样重要。事实上，Linux系统也默认定义了标准的局部环境变量。不过你也可以定义自己的局部变量，如你所想，这些变量被称为用户定义局部变量。 

查看局部环境变量的列表有点复杂。遗憾的是，在Linux系统并没有一个只显示局部环境变量的命令。 set 命令会显示为某个特定进程设置的所有环境变量，包括局部变量、全局变量以及用户定义变量。 

```bash
$ set 
BASH=/bin/bash 
[...] 
BASH_ALIASES=() 
BASH_ARGC=() 
BASH_ARGV=() 
BASH_CMDS=() 
BASH_LINENO=() 
BASH_SOURCE=() 
[...] 
colors=/etc/DIR_COLORS 
my_variable='Hello World' 
[...] 
$ 
```

所有通过 printenv 命令能看到的全局环境变量都出现在了 set 命令的输出中。但在 set 命令的输出中还有其他一些环境变量，即局部环境变量和用户定义变量。 

### 4.2 设置用户定义变量

#### 4.2.1 设置局部用户变量

一旦启动了bash  shell（或者执行一个shell脚本），就能创建在这个shell进程内可见的局部变量了。可以通过等号给环境变量赋值，值可以是数值或字符串。 

```bash
$ echo $my_variable 
 
$ my_variable=Hello 
$ 
$ echo $my_variable 
Hello 
```

非常简单！现在每次引用 my_variable  环境变量的值，只要通过 $my_variable 引用即可。 如果要给变量赋一个含有空格的字符串值，必须用单引号来界定字符串的首和尾。 

```bash
$ my_variable=Hello World 
-bash: World: command not found 
$  
$ my_variable="Hello World" 
$ 
$ echo $my_variable 
Hello World 
$ 
```

没有单引号的话，bash shell会以为下一个词是另一个要执行的命令。注意，你定义的局部环境变量用的是小写字母，而到目前为止你所看到的系统环境变量都是大写字母。

记住，***变量名、等号和值之间没有空格***，这一点非常重要。如果在赋值表达式中加上了空格，bash  shell就会把值当成一个单独的命令： 

```bash
$ my_variable = "Hello World" 
-bash: my_variable: command not found 
$ 
```

设置了局部环境变量后，就能在shell进程的任何地方使用它了。但是，如果生成了另外一个shell，它在子shell中就不可用。，如果你在子进程中设置了一个局部变量，那么一旦你退出了子进程，那个局部环境变量就不可用。 

#### 4.2.2 设置全局变量

在设定全局环境变量的进程所创建的子进程中，该变量都是可见的。创建全局环境变量的方法是先创建一个局部环境变量，然后再把它导出到全局环境中。

这个过程通过 export 命令来完成，变量名前面不需要加 $ 。 

```bash
$ my_variable="I am Global now" 
$ 
$ export my_variable  #不用加$
$ 
$ echo $my_variable 
I am Global now 
$ 
$ bash 
$ 
$ echo $my_variable 
I am Global now 
$ 
$ exit 
exit 
$ 
$ echo $my_variable 
I am Global now 
$ 
```

修改子shell中全局环境变量并不会影响到父shell中该变量的值。 子shell甚至无法使用 export 命令改变父shell中全局环境变量的值。

### 4.3 删除环境变量

当然，既然可以创建新的环境变量，自然也能删除已经存在的环境变量。可以用 unset 命令完成这个操作。在 unset 命令中引用环境变量时，记住不要使用 $ 。

```bash
$ echo $my_variable 
I am Global now 
$ 
$ unset my_variable 
$ 
$ echo $my_variable 

$ 
```

什么时候该使用 $ ，什么时候不该使用 $ ，实在让人摸不着头脑。记住一点就行了：***如果要用到变量，使用 $*** ；***如果要操作变量，不使用 $*** 。这条规则的一个例外就是使用 printenv 显示某个变量的值。

在处理全局环境变量时，事情就有点棘手了。如果你是在子进程中删除了一个全局环境变量，这只对子进程有效。该全局环境变量在父进程中依然可用。 **子变父不变**。

### 4.4 默认shell的环境变量

默认情况下，bash  shell会用一些特定的环境变量来定义系统环境。这些变量在你的Linux系统上都已经设置好了，只管放心使用。bash shell源自当初的Unix Bourne shell，因此也保留了Unix Bourne shell里定义的那些环境变量。 

 bash shell支持的Bourne变量 

```bash
变量  描述
CDPATH  冒号分隔的目录列表，作为cd命令的搜索路径 
HOME  当前用户的主目录 
IFS  shell用来将文本字符串分割成字段的一系列字符 
MAIL  当前用户收件箱的文件名（bash shell会检查这个文件，看看有没有新邮件） 
MAILPATH  冒号分隔的当前用户收件箱的文件名列表（bash shell会检查列表中的每个文件，看看有没有新邮件） 
OPTARG  getopts命令处理的最后一个选项参数值 
OPTIND  getopts命令处理的最后一个选项参数的索引号 
PATH  shell查找命令的目录列表，由冒号分隔 
PS1  shell命令行界面的主提示符 
PS2  shell命令行界面的次提示符 
```

除了默认的Bourne的环境变量，bash shell还提供一些自有的变量

```bash
变量  描述
BASH  当前shell实例的全路径名 
BASH_ALIASES  含有当前已设置别名的关联数组 
BASH_ARGC  含有传入子函数或shell脚本的参数总数的数组变量 
BASH_ARCV  含有传入子函数或shell脚本的参数的数组变量 
BASH_CMDS  关联数组，包含shell执行过的命令的所在位置 
BASH_COMMAND  shell正在执行的命令或马上就执行的命令 
BASH_ENV  设置了的话，每个bash脚本会在运行前先尝试运行该变量定义的启动文件 
BASH_EXECUTION_STRING   使用bash -c选项传递过来的命令 
BASH_LINENO  含有当前执行的shell函数的源代码行号的数组变量 
BASH_REMATCH  只读数组，在使用正则表达式的比较运算符=~进行肯定匹配（positive  match）时，包含了匹配到的模式和子模式 
BASH_SOURCE  含有当前正在执行的shell函数所在源文件名的数组变量 
BASH_SUBSHELL  当前子shell环境的嵌套级别（初始值是0） 
BASH_VERSINFO  含有当前运行的bash shell的主版本号和次版本号的数组变量 
BASH_VERSION  当前运行的bash shell的版本号 
BASH_XTRACEFD  若设置成了有效的文件描述符（0、1、2），则'set -x'调试选项生成的跟踪输出可被重定向。通常用来将跟踪输出到一个文件中 
BASHOPTS  当前启用的bash shell选项的列表 
BASHPID  当前bash进程的PID 
COLUMNS  当前bash shell实例所用终端的宽度 
COMP_CWORD  COMP_WORDS变量的索引值，后者含有当前光标的位置 
COMP_LINE  当前命令行 
COMP_POINT  当前光标位置相对于当前命令起始的索引 
COMP_KEY  用来调用shell函数补全功能的最后一个键 
COMP_TYPE  一个整数值，表示所尝试的补全类型，用以完成shell函数补全 
COMP_WORDBREAKS  Readline库中用于单词补全的词分隔字符 
COMP_WORDS  含有当前命令行所有单词的数组变量 
COMPREPLY  含有由shell函数生成的可能填充代码的数组变量
COPROC  占用未命名的协进程的I/O文件描述符的数组变量 
DIRSTACK  含有目录栈当前内容的数组变量 
EMACS  设置为't'时，表明emacs shell缓冲区正在工作，而行编辑功能被禁止 
ENV  如果设置了该环境变量，在bash  shell脚本运行之前会先执行已定义的启动文件（仅用于当bash shell以POSIX模式被调用时） 
EUID  当前用户的有效用户ID（数字形式） 
FCEDIT  供fc命令使用的默认编辑器 
FIGNORE  在进行文件名补全时可以忽略后缀名列表，由冒号分隔 
FUNCNAME  当前执行的shell函数的名称 
FUNCNEST  当设置成非零值时，表示所允许的最大函数嵌套级数（一旦超出，当前命令即被终止） 
GLOBIGNORE  冒号分隔的模式列表，定义了在进行文件名扩展时可以忽略的一组文件名 
GROUPS  含有当前用户属组列表的数组变量 
histchars  控制历史记录扩展，最多可有3个字符 
HISTCMD  当前命令在历史记录中的编号 
HISTCONTROL  控制哪些命令留在历史记录列表中 
HISTFILE  保存shell历史记录列表的文件名（默认是.bash_history） 
HISTFILESIZE 最多在历史文件中存多少行 
HISTTIMEFORMAT 如果设置了且非空，就用作格式化字符串，以显示bash历史中每条命令的时间戳  
HISTIGNORE  由冒号分隔的模式列表，用来决定历史文件中哪些命令会被忽略 
HISTSIZE  最多在历史文件中存多少条命令 
HOSTFILE  shell在补全主机名时读取的文件名称 
HOSTNAME  当前主机的名称 
HOSTTYPE  当前运行bash shell的机器 
IGNOREEOF  shell在退出前必须收到连续的EOF字符的数量（如果这个值不存在，默认是1） 
INPUTRC  Readline初始化文件名（默认是.inputrc） 
LANG  shell的语言环境类别 
LC_ALL  定义了一个语言环境类别，能够覆盖LANG变量 
LC_COLLATE  设置对字符串排序时用的排序规则 
LC_CTYPE  决定如何解释出现在文件名扩展和模式匹配中的字符 
LC_MESSAGES  在解释前面带有$的双引号字符串时，该环境变量决定了所采用的语言环境设置 
LC_NUMERIC  决定着格式化数字时采用的语言环境设置 
LINENO  当前执行的脚本的行号 
LINES  定义了终端上可见的行数 
MACHTYPE  用“CPU-公司-系统”（CPU-company-system）格式定义的系统类型 
MAPFILE  一个数组变量，当mapfile命令未指定数组变量作为参数时，它存储了mapfile所读入的文本 
MAILCHECK  shell查看新邮件的频率（以秒为单位，默认值是60） 
OLDPWD  shell之前的工作目录 
OPTERR  设置为1时，bash shell会显示getopts命令产生的错误 
OSTYPE  定义了shell所在的操作系统 
PIPESTATUS  含有前台进程的退出状态列表的数组变量 
POSIXLY_CORRECT  设置了的话，bash会以POSIX模式启动 
PPID  bash shell父进程的PID 
PROMPT_COMMAND  设置了的话，在命令行主提示符显示之前会执行这条命令 
PROMPT_DIRTRIM  用来定义当启用了\w或\W提示符字符串转义时显示的尾部目录名的数量。被删除的目录名会用一组英文句点替换 
PS3  select命令的提示符 
PS4  如果使用了bash的-x选项，在命令行之前显示的提示信息 
PWD  当前工作目录 
RANDOM  返回一个0～32767的随机数（对其的赋值可作为随机数生成器的种子） 
READLINE_LINE  当使用bind –x命令时，存储Readline缓冲区的内容 
READLINE_POINT  当使用bind –x命令时，表示Readline缓冲区内容插入点的当前位置 
REPLY  read命令的默认变量 
SECONDS  自从shell启动到现在的秒数（对其赋值将会重置计数器） 
SHELL  bash shell的全路径名 
SHELLOPTS  已启用bash shell选项列表，列表项之间以冒号分隔 
SHLVL  shell的层级；每次启动一个新bash shell，该值增加1 
TIMEFORMAT  指定了shell的时间显示格式 
TMOUT  select和read命令在没输入的情况下等待多久（以秒为单位）。默认值为0，表示无限长 
TMPDIR  目录名，保存bash shell创建的临时文件 
UID  当前用户的真实用户ID（数字形式） 
```

你可能已经注意到，不是所有的默认环境变量都会在运行 set 命令时列出。尽管这些都是默认环境变量，但并不是每一个都必须有一个值。

### 4.5 设置PATH环境变量

当你在shell命令行界面中输入一个外部命令时，shell必须搜索系统来找到对应的程序。 PATH 环境变量定义了用于进行命令和程序查找的目录。在本书所用的Ubuntu系统中，PATH 环境变量的内容是这样的： 

```bash
$ echo $PATH 
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin: 
/sbin:/bin:/usr/games:/usr/local/games 
$ 
```

输出中显示了有8个可供shell用来查找命令和程序。 PATH 中的目录使用冒号分隔。 

如果命令或者程序的位置没有包括在 PATH 变量中，那么如果不使用绝对路径的话，shell是没法找到的。如果shell找不到指定的命令或程序，它会产生一个错误信息： 

```bash
$ myprog 
-bash: myprog: command not found 
$ 
```

PATH没有存放的命令，直接输名称会报错，解决的办法是保证 PATH 环境变量包含了所有存放应用程序的目录。 

可以把新的搜索目录添加到现有的 PATH 环境变量中，无需从头定义。 PATH 中各个目录之间是用冒号分隔的。你只需引用原来的 PATH 值，然后再给这个字符串添加新目录就行了。可以参考下面的例子。

```bash
$ echo $PATH 
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin: 
/sbin:/bin:/usr/games:/usr/local/games 
$ 
$ PATH=$PATH:/home/christine/Scripts 
$ 
$ echo $PATH 
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/ 
     games:/usr/local/games:/home/christine/Scripts 
$ 
$ myprog 
The factorial of 5 is 120. 
$ 
```

> 如果希望子shell也能找到你的程序的位置，一定要记得把修改后的 PATH 环境变量导出。

程序员通常的办法是将单点符也加入 PATH 环境变量。该单点符代表当前目录。

```bash
$ PATH=$PATH:. 
$ 
$ cd /home/christine/Old_Scripts 
$ 
$ myprog2 
The factorial of 6 is 720 
$ 
```

对 PATH 变量的修改只能持续到退出或重启系统。这种效果并不能一直持续。

### 4.6 定位系统环境变量

环境变量在Linux系统中的用途很多。你现在已经知道如何修改系统环境变量，也知道了如何创建自己的环境变量。接下来的问题是怎样让环境变量的作用持久化。 

在你登入Linux系统启动一个bash  shell时，默认情况下bash会在几个文件中查找命令。这些文件叫作启动文件或环境文件。bash检查的启动文件取决于你启动bash  shell的方式。启动bash shell有3种方式： 

- 登录时作为默认登录shell 
- 作为非登录shell的交互式shell 
- 作为运行脚本的非交互shell 

#### 4.6.1 登录shell

当你登录Linux系统时，bash shell会作为登录shell启动。登录shell会从5个不同的启动文件里读取命令：

- /etc/profile 
- $HOME/.bash_profile 
- $HOME/.bashrc 
- $HOME/.bash_login 
- $HOME/.profile 

/etc/profile文件是系统上默认的bash  shell的主启动文件。系统上的每个用户登录时都会执行这个启动文件。 

另外4个启动文件是针对用户的，可根据个人需求定制。我们来仔细看一下各个文件。

**1. /etc/profile文件 **

/etc/profile文件是bash  shell默认的的主启动文件。只要你登录了Linux系统，bash就会执行/etc/profile启动文件中的命令。不同的Linux发行版在这个文件里放了不同的命令。

**2. $HOME目录下的启动文件** 

剩下的启动文件都起着同一个作用：提供一个用户专属的启动文件来定义该用户所用到的环境变量。大多数Linux发行版只用这四个启动文件中的一到两个： 

- $HOME/.bash_profile 
- $HOME/.bashrc 
- $HOME/.bash_login 
- $HOME/.profile 

注意，这四个文件都**以点号开头，这说明它们是隐藏文件**（不会在通常的 ls 命令输出列表中出现）。它们位于用户的HOME目录下，所以每个用户都可以编辑这些文件并添加自己的环境变量，这些环境变量会在每次启动bash shell会话时生效。 

shell会按照按照下列顺序，运行第一个被找到的文件，余下的则被忽略：

```bash
$HOME/.bash_profile 
$HOME/.bash_login 
$HOME/.profile 
```

注意，这个列表中并没有$HOME/.bashrc文件。这是因为该文件通常通过其他文件运行的。 

> 记住，$HOME表示的是某个用户的主目录。它和波浪号（~）的作用一样。

#### 4.6.2 交互式shell进程

如果你的bash shell不是登录系统时启动的（比如是在命令行提示符下敲入 bash 时启动），那么你启动的shell叫作交互式shell。交互式shell不会像登录shell一样运行，但它依然提供了命令行提示符来输入命令。 

如果bash是作为交互式shell启动的，它就不会访问/etc/profile文件，只会检查用户HOME目录中的.bashrc文件。 

#### 4.6.3 非交互式shell进程

系统执行shell脚本时用的就是这种shell。不同的地方在于它没有命令行提示符。但是当你在系统上运行脚本时，也许希望能够运行一些特定启动的命令。

为了处理这种情况，bash  shell提供了 BASH_ENV 环境变量。当shell启动一个非交互式shell进程时，它会检查这个环境变量来查看要执行的启动文件。如果有指定的文件，shell会执行该文件里的命令，这通常包括shell脚本变量设置。

所用的Ubuntu/CentOS发行版中，这个环境变量在默认情况下并未设置。如果变量未设置， printenv 命令只会返回CLI提示符：

```bash
$ printenv BASH_ENV 
$ 
```

如果 BASH_ENV 变量没有设置，shell脚本到哪里去获得它们的环境变量呢？别忘了有些shell脚本是通过启动一个子shell来执行的。子shell可以继承父shell导出过的变量。 举例来说，如果父shell是登录shell，在/etc/profile、/etc/profile.d/ * .sh和$HOME/.bashrc文件中设置并导出了变量，用于执行脚本的子shell就能够继承这些变量。 

要记住，由父shell设置但并未导出的变量都是局部变量。子shell无法继承局部变量。 对于那些不启动子shell的脚本，变量已经存在于当前shell中了。所以就算没有设置BASH_ENV ，也可以使用当前shell的局部变量和全局变量。 

#### 4.6.4  环境变量持久化

对全局环境变量来说（Linux系统中所有用户都需要使用的变量），可能更倾向于将新的或修改过的变量设置放在/etc/profile文件中，但这可不是什么好主意。如果你升级了所用的发行版，这个文件也会跟着更新，那你所有定制过的变量设置可就都没有了。 

最好是在/etc/profile.d目录中建一个以.sh结尾的文件。把所有新的或修改过的全局环境变量设置放在这个文件中。 在大多数发行版中，存储个人用户永久性bash shell变量的地方是**$HOME/.bashrc**文件，这一点适用于所有类型的shell进程。用户将自己定义的环境变量放在这个文件中就行了。

但如果设置了 BASH_ENV 变量，那么记住，除非它指向的是$HOME/.bashrc，否则你应该将非交互式shell的用户变量放在别的地方。

### 4.7 数组变量

环境变量有一个很酷的特性就是，它们可作为数组使用。数组是能够存储多个值的变量。这些值可以单独引用，也可以作为整个数组来引用。 

要给某个环境变量设置多个值，可以把值放在括号里，值与值之间用空格分隔。 

```bash
$ mytest=(one two three four five) 
$ 

$ echo $mytest 
one 
$ 
```

只有数组的第一个值显示出来了。要引用一个单独的数组元素，就必须用代表它在数组中位置的数值索引值。索引值要用方括号括起来。

```bash
$ echo ${mytest[2]} 
three 
$
```

>  环境变量数组的索引值都是从零开始。

	要显示整个数组变量，可用星号作为通配符放在索引值的位置。 
```bash
$ echo ${mytest[*]} 
one two three four five 
$
```

 也可以改变某个索引值位置的值。 

```bash
$ mytest[2]=seven 
$ 
$ echo ${mytest[*]} 
one two seven four five 
$
```

 甚至能用 unset 命令删除数组中的某个值，但是要小心，这可能会有点复杂。看下面的例子。 

```bash
$ unset mytest[2] 
$ 
$ echo ${mytest[*]} 
one two four five 
$ 
$ echo ${mytest[2]} 

$ echo ${mytest[3]} 
four 
$ 
```

这个例子用 unset 命令删除在索引值为 2 的位置上的值。显示整个数组时，看起来像是索引里面已经没这个索引了。但当专门显示索引值为 2 的位置上的值时，就能看到这个位置是空的，**只是清空了该位置上的内容**。 

最后，可以在 unset 命令后跟上数组名来删除整个数组。 

```bash
$ unset mytest 
$ 
$ echo ${mytest[*]} 
 
$ 
```

------

------

## 5 理解Linux文件权限

### 5.1 Linux的安全性

Linux安全系统的核心是用户账户。每个能进入Linux系统的用户都会被分配唯一的用户账户。用户对系统中各种对象的访问权限取决于他们登录系统时用的账户。 

用户权限是通过创建用户时分配的用户ID（User ID，通常缩写为UID）来跟踪的。UID是数值，每个用户都有唯一的UID，但在登录系统时用的不是UID，而是登录名。登录名是用户用来登录系统的最长八字符的字符串（字符可以是数字或字母），同时会关联一个对应的密码。 

Linux系统使用特定的文件和工具来跟踪和管理系统上的用户账户。

#### 5.1.1 etc/passwd文件

Linux系统使用一个专门的文件来将用户的登录名匹配到对应的UID值。这个文件就是/etc/passwd文件，它包含了一些与用户有关的信息。下面是Linux系统上典型的/etc/passwd文件的一个例子。 

```bash
ubuntu@admin:~$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
[...]
ubuntu:x:1000:1000:,,,:/home/ubuntu:/bin/bash
ubuntu@admin:~$  

#字段描述
 登录用户名 
 用户密码  #设置为x，是加密后的密码形式
 用户账户的UID（数字形式） 
 用户账户的组ID（GID）（数字形式） 
 用户账户的文本描述（称为备注字段） 
 用户HOME目录的位置 
 用户的默认shell
```

root用户账户是Linux系统的管理员，固定分配给它的UID是 0 。就像上例中显示的，Linux系统会为各种各样的功能创建不同的用户账户，而这些账户并不是真的用户。这些账户叫作系统账户，是系统上运行的各种服务进程访问资源用的特殊账户。所有运行在后台的服务都需要用一个系统用户账户登录到Linux系统上。 

在安全成为一个大问题之前，这些服务经常会用root账户登录。遗憾的是，如果有非授权的用户攻陷了这些服务中的一个，他立刻就能作为root用户进入系统。为了防止发生这种情况，现在运行在Linux服务器后台的几乎所有的服务都是用自己的账户登录。这样的话，即使有人攻入了某个服务，也无法访问整个系统。 

Linux为系统账户预留了500以下的UID值。有些服务甚至要用特定的UID才能正常工作。为普通用户创建账户时，大多数Linux系统会从500开始，将第一个可用UID分配给这个账户（并非所有的Linux发行版都是这样）。 

#### 5.1.2 /etc/shadow文件

/etc/shadow文件对Linux系统密码管理提供了更多的控制。只有root用户才能访问/etc/shadow文件，这让它比起/etc/passwd安全许多。 /etc/shadow文件为系统上的每个用户账户都保存了一条记录。记录就像下面这样： 

```bash
rich:$1$.FfcK0ns$f1UgiyHQ25wrB/hykCn020:11627:0:99999:7:::

#字段描述
 与/etc/passwd文件中的登录名字段对应的登录名 
 加密后的密码 
 自上次修改密码后过去的天数密码（自1970年1月1日开始计算） 
 多少天后才能更改密码 
 多少天后必须更改密码 
 密码过期前提前多少天提醒用户更改密码 
 密码过期后多少天禁用用户账户 
 用户账户被禁用的日期（用自1970年1月1日到当天的天数表示） 
 预留字段给将来使用 
```

使用shadow密码系统后，Linux系统可以更好地控制用户密码。它可以控制用户多久更改一次密码，以及什么时候禁用该用户账户，如果密码未更新的话。 

#### 5.1.3 添加新用户

用来向Linux系统添加新用户的主要工具是 useradd 。这个命令简单快捷，可以一次性创建新用户账户及设置用户HOME目录结构。 useradd 命令使用系统的默认值以及命令行参数来设置用户账户。

系统默认值被设置在/etc/default/useradd文件中。可以使用加入了 -D 选项的 useradd命令查看所用Linux系统中的这些默认值。

```bash
# /usr/sbin/useradd -D 
GROUP=100 
HOME=/home 
INACTIVE=-1 
EXPIRE= 
SHELL=/bin/bash 
SKEL=/etc/skel 
CREATE_MAIL_SPOOL=yes 
# 
#字段说明：
新用户会被添加到GID为 100 的公共组； 
 新用户的HOME目录将会位于/home/loginname； 
 新用户账户密码在过期后不会被禁用； 
 新用户账户未被设置过期日期； 
 新用户账户将bash shell作为默认shell； 
 系统会将/etc/skel目录下的内容复制到用户的HOME目录下； 
 系统为该用户账户在mail目录下创建一个用于接收邮件的文件。 
```

倒数第二个值很有意思。 useradd 命令允许管理员创建一份默认的HOME目录配置，然后把它作为创建新用户HOME目录的模板。这样就能自动在每个新用户的HOME目录里放置默认的系统文件。在Ubuntu Linux系统上，/etc/skel目录有下列文件： 

```bash
$ ls -al /etc/skel 
total 32 
drwxr-xr-x   2 root root  4096 2010-04-29 08:26 . 
drwxr-xr-x 135 root root 12288 2010-09-23 18:49 .. 
-rw-r--r--   1 root root   220 2010-04-18 21:51 .bash_logout 
-rw-r--r--   1 root root  3103 2010-04-18 21:51 .bashrc 
-rw-r--r--   1 root root   179 2010-03-26 08:31 examples.desktop 
-rw-r--r--   1 root root   675 2010-04-18 21:51 .profile 
$ 
```

根据第4章的内容，你应该能知道这些文件是做什么的。它们是bash  shell环境的标准启动文件。系统会自动将这些默认文件复制到你创建的每个用户的HOME目录。 

可以用默认系统参数创建一个新用户账户，然后检查一下新用户的HOME目录。 

```bash
# useradd -m test 
# ls -al /home/test 
total 24 
drwxr-xr-x 2 test test 4096 2010-09-23 19:01 . 
drwxr-xr-x 4 root root 4096 2010-09-23 19:01 .. 
-rw-r--r-- 1 test test  220 2010-04-18 21:51 .bash_logout 
-rw-r--r-- 1 test test 3103 2010-04-18 21:51 .bashrc 
-rw-r--r-- 1 test test  179 2010-03-26 08:31 examples.desktop 
-rw-r--r-- 1 test test  675 2010-04-18 21:51 .profile 
# 
```

默认情况下， useradd 命令不会创建HOME目录，但是 -m 命令行选项会使其创建HOME目录。你能在此例中看到， useradd 命令创建了新HOME目录，并将/etc/skel目录中的文件复制了过来。 

要想在创建用户时改变默认值或默认行为，可以使用命令行参数。下面列出了这些参数。

```bash
参数  描述
-c comment  给新用户添加备注 
-d home_dir  为主目录指定一个名字（如果不想用登录名作为主目录名的话） 
-e expire_date  用YYYY-MM-DD格式指定一个账户过期的日期 
-f inactive_days  指定这个账户密码过期后多少天这个账户被禁用；0表示密码一过期就立即禁用，1表示
禁用这个功能 
-g initial_group  指定用户登录组的GID或组名 
-G group ...  指定用户除登录组之外所属的一个或多个附加组 
-k  必须和-m一起使用，将/etc/skel目录的内容复制到用户的HOME目录 
-m  创建用户的HOME目录 
-M  不创建用户的HOME目录（当默认设置里要求创建时才使用这个选项） 
-n  创建一个与用户登录名同名的新组
-r  创建系统账户 
-p passwd  为用户账户指定默认密码 
-s shell  指定默认的登录shell 
-u uid  为账户指定唯一的UID 
```

 你会发现，在创建新用户账户时使用命令行参数可以更改系统指定的默认值。但如果总需要修改某个值的话，最好还是修改一下系统的默认值。 

可以在 -D 选项后跟上一个指定的值来修改系统默认的新用户设置。参数如下所示。

```bash
参数  描述
-b default_home  更改默认的创建用户HOME目录的位置 
-e expiration_date  更改默认的新账户的过期日期 
-f inactive  更改默认的新用户从密码过期到账户被禁用的天数 
-g group  更改默认的组名称或GID 
-s shell  更改默认的登录shell
```

 更改默认值非常简单： 

```bash
# useradd -D -s /bin/tsch 
# useradd -D 
GROUP=100 
HOME=/home 
INACTIVE=-1 
EXPIRE= 
SHELL=/bin/tsch 
SKEL=/etc/skel 
CREATE_MAIL_SPOOL=yes 
# 
```

#### 5.1.4 删除用户

如果你想从系统中删除用户， userdel 可以满足这个需求。默认情况下， userdel 命令会只删除/etc/passwd文件中的用户信息，而不会删除系统中属于该账户的任何文件。

如果加上 -r 参数， userdel 会删除用户的HOME目录以及邮件目录。然而，系统上仍可能存有已删除用户的其他文件。这在有些环境中会造成问题。 

```bash
# /usr/sbin/userdel -r test 
# ls -al /home/test 
ls: cannot access /home/test: No such file or directory 
# 
```

加了 -r 参数后，用户先前的那个/home/test目录已经不存在了。 

> 在有大量用户的环境中使用 -r 参数时要特别小心。你永远不知道用户是否在其HOME目录下存放了其他用户或其他程序要使用的重要文件。记住，在删除用户的HOME目录之前一定要检查清楚！

#### 5.1.5 修改用户

```bash
命令  描述
usermod  修改用户账户的字段，还可以指定主要组以及附加组的所属关系 
passwd  修改已有用户的密码 
chpasswd  从文件中读取登录名密码对，并更新密码 
chage  修改密码的过期日期 
chfn  修改用户账户的备注信息 
chsh  修改用户账户的默认登录shell 
```

每种工具都提供了特定的功能来修改用户账户信息。下面的几节将具体介绍这些工具。 

**1.  usermod  **

usermod 命令是用户账户修改工具中最强大的一个。它能用来修改/etc/passwd文件中的大部分字段，只需用与想修改的字段对应的命令行参数就可以了。参数大部分跟 useradd 命令的参数一样（比如， -c 修改备注字段， -e 修改过期日期， -g 修改默认的登录组）。除此之外，还有另外一些可能派上用场的选项。 

- -l 修改用户账户的登录名。 
- -L 锁定账户，使用户无法登录。 
- -p 修改账户的密码。 
- -U 解除锁定，使用户能够登录。

-L 选项尤其实用。它可以将账户锁定，使用户无法登录，同时无需删除账户和用户的数据。要让账户恢复正常，只要用 -U 选项就行了。

**2.  passwd和chpasswd**

改变用户密码的一个简便方法就是用 passwd 命令（**只能修改自己的密码**）。

```bash
# passwd test 
Changing password for user test. 
New UNIX password: 
Retype new UNIX password: 
passwd: all authentication tokens updated successfully. 
# 
```

如果只用 passwd 命令，它会改你自己的密码。系统上的任何用户都能改自己的密码，但只有root用户才有权限改别人的密码。

-e 选项能强制用户下次登录时修改密码。你可以先给用户设置一个简单的密码，之后再强制在下次登录时改成他们能记住的更复杂的密码。 

如果需要为系统中的大量用户修改密码， chpasswd 命令可以事半功倍。 chpasswd 命令能从标准输入自动读取登录名和密码对（由冒号分割）列表，给密码加密，然后为用户账户设置。你也可以用重定向命令来将含有 userid:passwd 对的文件重定向给该命令。 

```bash
# chpasswd < users.txt 
# 从文件读取，批量修改
```

**3.  chsh、chfn和chage**

chsh 、 chfn 和 chage 工具专门用来修改特定的账户信息。 

- **chsh** 命令用来快速修改默认的用户登录shell。使用时必须用shell的全路径名作为参数，不能只用shell名。

  ```bash
  #  chsh -s /bin/csh test 
  Changing shell for test. 
  Shell changed. 
  # 
  ```

- **chfn** 命令提供了在/etc/passwd文件的备注字段中存储信息的标准方法。 chfn 命令会将用于Unix的 finger 命令的信息存进备注字段，而不是简单地存入一些随机文本（比如名字或昵称之类的），或是将备注字段留空。 finger 命令可以非常方便地查看Linux系统上的用户信息。 

  ```bash
  # finger rich 
  Login: rich                             Name: Rich Blum 
  Directory: /home/rich                   Shell: /bin/bash 
  On since Thu Sep 20 18:03 (EDT) on pts/0 from 192.168.1.2 
  No mail. 
  No Plan. 
  # 
  ```

  > 出于安全性考虑，很多Linux系统管理员会在系统上禁用 finger 命令，不少Linux发行版甚至都没有默认安装该命令。

  如果在使用 chfn 命令时没有参数，它会向你询问要将哪些适合的内容加进备注字段。

  ```bash
  # chfn test 
  Changing finger information for test. 
  Name []: Ima Test 
  Office []: Director of Technology 
  Office Phone []: (123)555-1234 
  Home Phone []: (123)555-9876
  
  Finger information changed. 
  # finger test 
  Login: test                             Name: Ima Test 
  Directory: /home/test                   Shell: /bin/csh 
  Office: Director of Technology          Office Phone: (123)555-1234
  Home Phone: (123)555-9876 
  Never logged in. 
  No mail. 
  No Plan. 
  # 
  ```

  查看/etc/passwd文件中的记录，你会看到下面这样的结果。

  ```bash
  # grep test /etc/passwd 
  test:x:504:504:Ima Test,Director of Technology,(123)555- 
  1234,(123)555-9876:/home/test:/bin/csh 
  # 
  ```

  所有的指纹信息现在都存在/etc/passwd文件中了。 最后， chage 命令用来帮助管理用户账户的有效期。你需要对每个值设置多个参数，

  ```bash
  -d  设置上次修改密码到现在的天数 
  -E  设置密码过期的日期 
  -I  设置密码过期到锁定账户的天数 
  -m  设置修改密码之间最少要多少天 
  -W  设置密码过期前多久开始出现提醒信息
  ```

- **chage** 命令的日期值可以用下面两种方式中的任意一种：

   YYYY-MM-DD格式的日期 

   代表从1970年1月1日起到该日期天数的数值 

  chage 命令中有个好用的功能是设置账户的过期日期。有了它，你就能创建在特定日期自动过期的临时用户，再也不需要记住删除用户了！过期的账户跟锁定的账户很相似：账户仍然存在，但用户无法用它登录。

### 5.2 使用Linux组

#### 5.2.1 /etc/group文件

与用户账户类似，组信息也保存在系统的一个文件中。/etc/group文件包含系统上用到的每个组的信息。下面是一些来自Linux系统上/etc/group文件中的典型例子。

```bash
root:x:0:root 
bin:x:1:root,bin,daemon 
daemon:x:2:root,bin,daemon 
sys:x:3:root,bin,adm 
adm:x:4:root,adm,daemon 
rich:x:500: 
mama:x:501: 
katie:x:502: 
jessica:x:503: 
mysql:x:27: 
test:x:504: 
```

和UID一样，GID在分配时也采用了特定的格式。系统账户用的组通常会分配低于500的GID值，而用户组的GID则会从500开始分配。/etc/group文件有4个字段：

- 组名 
- 组密码 
- GID 
- 属于该组的用户列表

组密码允许非组内成员通过它临时成为该组成员。这个功能并不很普遍，但确实存在。**千万不能通过直接修改/etc/group文件来添加用户到一个组，要用 usermod 命令**。在添加用户到不同的组之前，首先得创建组。 

#### 5.2.2 创建组

groupadd 命令可在系统上创建新组。

```bash
# /usr/sbin/groupadd shared 
# tail /etc/group 
haldaemon:x:68: 
xfs:x:43: 
gdm:x:42: 
rich:x:500: 
mama:x:501: 
katie:x:502: 
jessica:x:503: 
mysql:x:27: 
test:x:504: 
shared:x:505: #创建的shared组
# 
```

在创建新组时，默认没有用户被分配到该组。 groupadd 命令没有提供将用户添加到组中的选项，但可以用 usermod 命令来弥补这一点。 

```bash
# /usr/sbin/usermod -G shared rich  
# /usr/sbin/usermod -G shared test 
# tail /etc/group 
haldaemon:x:68: 
xfs:x:43: 
gdm:x:42: 
rich:x:500: 
mama:x:501: 
katie:x:502: 
jessica:x:503: 
mysql:x:27: 
test:x:504: 
shared:x:505:rich, test 
# 
```

shared组现在有两个成员： test 和 rich 。 usermod 命令的 -G 选项会把这个新组添加到该用户账户的组列表里。

> 如果更改了已登录系统账户所属的用户组，该用户必须登出系统后再登录，组关系的更改才能生效。

**警告**  为用户账户分配组时要格外小心。如果加了 -g 选项，指定的组名会替换掉该账户的默认组。 -G 选项则将该组添加到用户的属组的列表里，不会影响默认组。 

#### 5.2.3 修改组

在/etc/group文件中可以看到，需要修改的组信息并不多。 groupmod 命令可以修改已有组的GID（加 -g 选项）或组名（加 -n 选项）。

```bash
# /usr/sbin/groupmod -n sharing shared  
# tail /etc/group 
haldaemon:x:68: 
xfs:x:43: 
gdm:x:42: 
rich:x:500: 
mama:x:501: 
katie:x:502: 
jessica:x:503: 
mysql:x:27: 
test:x:504: 
sharing:x:505:test,rich 
# 
```

修改组名时，GID和组成员不会变，只有组名改变。由于所有的安全权限都是基于GID的，你可以随意改变组名而不会影响文件的安全性。 

### 5.3 理解文件权限

#### 5.3.1 使用文件权限符

```bash
$ ls –l  
total 68 
-rw-rw-r-- 1 rich rich   50 2010-09-13 07:49 file1.gz 
-rw-rw-r-- 1 rich rich   23 2010-09-13 07:50 file2 
-rw-rw-r-- 1 rich rich   48 2010-09-13 07:56 file3 
-rw-rw-r-- 1 rich rich   34 2010-09-13 08:59 file4 
-rwxrwxr-x 1 rich rich 4882 2010-09-18 13:58 myprog 
-rw-rw-r-- 1 rich rich  237 2010-09-18 13:58 myprog.c 
drwxrwxr-x 2 rich rich 4096 2010-09-03 15:12 test1 
drwxrwxr-x 2 rich rich 4096 2010-09-03 15:12 test2 
$ 
 - 代表文件 
 d 代表目录 
 l 代表链接 
 c 代表字符型设备 
 b 代表块设备 
 n 代表网络设备 
之后有3组三字符的编码。每一组定义了3种访问权限： 
 r 代表对象是可读的 
 w 代表对象是可写的 
 x 代表对象是可执行的 若没有某种权限，在该权限位会出现单破折线。这3组权限分别对应对象的3个安全级别： 
 对象的属主 
 对象的属组 
 系统其他用户 
```

讨论这个问题的最简单的办法就是找个例子，然后逐个分析文件权限。 

```bash
-rwxrwxr-x 1 rich rich 4882 2010-09-18 13:58 myprog
```

文件myprog有下面3组权限。 

- rwx ：文件的属主（设为登录名rich）。 
- rwx ：文件的属组（设为组名rich）。 
- r-x ：系统上其他人。 

这些权限说明登录名为rich的用户可以读取、写入以及执行这个文件（可以看作有全部权限）。类似地，rich组的成员也可以读取、写入和执行这个文件。然而不属于rich组的其他用户只能读取和执行这个文件： w 被单破折线取代了，说明这个安全级别没有写入权限。

#### 5.3.2  默认文件权限

你可能会问这些文件权限从何而来，答案是 umask 。 umask 命令用来设置所创建文件和目录的默认权限。

```bash
$ touch newfile 
$ ls -al newfile 
-rw-r--r--    1 rich     rich            0 Sep 20 19:16 newfile 
$ 
```

touch 命令用分配给我的用户账户的默认权限创建了这个文件。 umask 命令可以显示和设置这个默认权限。 

```bash
$ umask 
0022 
$ 
```

遗憾的是， umask 命令设置没那么简单明了，想弄明白其工作原理就更混乱了。第一位代表了一项特别的安全特性，叫作粘着位（sticky bit）。

后面的3位表示文件或目录对应的 umask 八进制值。要理解 umask 是怎么工作的，得先理解八进制模式的安全性设置。

 八进制模式的安全性设置先获取这3个 rwx 权限的值，然后将其转换成3位二进制值，用一个八进制值来表示。在这个二进制表示中，每个位置代表一个二进制位。因此，如果读权限是唯一置位的权限，权限值就是 r-- ，转换成二进制值就是 100 ，代表的八进制值是 4 。下面列出了可能会遇到的组合。 

```bash
---  000  0  没有任何权限 
--x  001  1  只有执行权限 
-w-  010  2  只有写入权限 
-wx  011  3  有写入和执行权限 
r--  100  4  只有读取权限 
r-x  101  5  有读取和执行权限 
rw-  110  6  有读取和写入权限 
rwx  111  7  有全部权限 
```

八进制模式先取得权限的八进制值，然后再把这三组安全级别（属主、属组和其他用户）的八进制值顺序列出。因此，八进制模式的值 664 代表属主和属组成员都有读取和写入的权限，而其他用户都只有读取权限。 

了解八进制模式权限是怎么工作的之后， umask 值反而更叫人困惑了。我的Linux系统上默认的八进制的 umask 值是 0022 ，而我所创建的文件的八进制权限却是 644 ，这是如何得来的呢？ umask 值只是个掩码。它会屏蔽掉不想授予该安全级别的权限。接下来我们还得再多进行一些八进制运算才能搞明白来龙去脉。 

要把 umask 值从对象的全权限值中减掉。对文件来说，全权限的值是 666 （所有用户都有读和写的权限）；而对目录来说，则是 777 （所有用户都有读、写、执行权限）。 所以在上例中，文件一开始的权限是 666 ，减去 umask 值 022 之后，剩下的文件权限就成了 644 。 

在大多数Linux发行版中， umask 值通常会设置在/etc/profile启动文件中（参见第6章），不过有一些是设置在/etc/login.defs文件中的（如Ubuntu）。可以用 umask 命令为默认 umask 设置指定一个新值。 

```bash
$ umask 026 
$ touch newfile2 
$ ls -l newfile2 
-rw-r-----    1 rich     rich            0 Sep 20 19:46 newfile2 
$ 
```

在把 umask 值设成 026 后，默认的文件权限变成了 640 ，因此新文件现在对组成员来说是只读的，而系统里的其他成员则没有任何权限。

```bash
umask 值同样会作用在创建目录上。 
$ mkdir newdir 
$ ls -l 
drwxr-x--x    2 rich     rich         4096 Sep 20 20:11 newdir/ 
$ 
```

由于目录的默认权限是 777 ， umask 作用后生成的目录权限不同于生成的文件权限。 umask值 026 会从 777 中减去，留下来 751 作为目录权限设置。 

### 5.4  改变安全性设置 

如果你已经创建了一个目录或文件，需要改变它的安全性设置，在Linux系统上有一些工具能够完成这项任务。本节将告诉你如何更改文件和目录的已有权限、默认文件属主以及默认属组。 

#### 5.4.1 改变权限

chmod 命令用来改变文件和目录的安全性设置。该命令的格式如下：

```bash
chmod options mode file
```

mode 参数可以使用八进制模式或符号模式进行安全性设置。八进制模式设置非常直观，直接用期望赋予文件的标准3位八进制权限码即可。 

```bash
$ chmod 760 newfile 
$ ls -l newfile 
-rwxrw----    1 rich     rich            0 Sep 20 19:16 newfile 
$ 
```

八进制文件权限会自动应用到指定的文件上。符号模式的权限就没这么简单了。与通常用到的3组三字符权限字符不同， chmod 命令采用了另一种方法。下面是在符号模式下指定权限的格式。

```bash
[ugoa…][[+-=][rwxXstugo…] 
```

非常有意义，不是吗？第一组字符定义了权限作用的对象： 

- u 代表用户 

- g 代表组 

- o 代表其他 

- a 代表上述所有

下一步，后面跟着的符号表示你是想在现有权限基础上增加权限（+），还是在现有权限基础上移除权限（-），或是将权限设置成后面的值（=）。 

最后，第三个符号代表作用到设置上的权限。你会发现，这个值要比通常的 rwx 多。额外的设置有以下几项。 

```bash
X ：如果对象是目录或者它已有执行权限，赋予执行权限。 
s ：运行时重新设置UID或GID。 
t ：保留文件或目录。 
u ：将权限设置为跟属主一样。 
g ：将权限设置为跟属组一样。 
o ：将权限设置为跟其他用户一样。
```

像这样使用这些权限。 

```bash
 $ chmod o+r newfile 
 $ ls -lF newfile 
 -rwxrw-r--    1 rich     rich            0 Sep 20 19:16 newfile* 
 $ 
```

不管其他用户在这一安全级别之前都有什么权限， o+r 都给这一级别添加读取权限。

```bash
$ chmod u-x newfile 
$ ls -lF newfile 
-rw-rw-r--    1 rich     rich            0 Sep 20 19:16 newfile 
$ 
```

u-x 移除了属主已有的执行权限。注意 ls 命令的 -F 选项，它能够在具有执行权限的文件名后加一个星号。 

options 为 chmod 命令提供了另外一些功能。 -R 选项可以让权限的改变递归地作用到文件和子目录。你可以使用通配符指定多个文件，然后利用一条命令将权限更改应用到这些文件上。 

#### 5.4.2  改变所属关系 

chown 命令用来改变文件的属主，chgrp 命令用来改变文件的默认属组。 

chown 命令的格式如下。 

```bash
chown options owner[.group] file  
```

可用登录名或UID来指定文件的新属主。

```bash
# chown dan newfile 
# ls -l newfile 
-rw-rw-r--    1 dan      rich            0 Sep 20 19:16 newfile 
# 
```

非常简单。 chown 命令也支持同时改变文件的属主和属组。

```bash
# chown dan.shared newfile 
# ls -l newfile 
-rw-rw-r--    1 dan      shared             0 Sep 20 19:16 newfile 
# 
```

如果你不嫌麻烦，可以只改变一个目录的默认属组。 

```bash
# chown .rich newfile 
# ls -l newfile 
-rw-rw-r--    1 dan      rich            0 Sep 20 19:16 newfile 
# 
```

最后，如果你的Linux系统采用和用户登录名匹配的组名，可以只用一个条目就改变二者。

```bash
# chown test. newfile 
# ls -l newfile 
-rw-rw-r--    1 test    test             0 Sep 20 19:16 newfile 
# 
```

chown 命令采用一些不同的选项参数。 -R 选项配合通配符可以递归地改变子目录和文件的所属关系。 -h 选项可以改变该文件的所有符号链接文件的所属关系。

> 只有root用户能够改变文件的属主。任何属主都可以改变文件的属组，但前提是属主必须是原属组和目标属组的成员。 

chgrp 命令可以更改文件或目录的默认属组。 

```bash
$ chgrp shared newfile 
$ ls -l newfile 
-rw-rw-r--    1 rich     shared          0 Sep 20 19:16 newfile 
$ 
```

用户账户必须是这个文件的属主，除了能够更换属组之外，还得是新组的成员。现在shared组的任意一个成员都可以写这个文件了。这是Linux系统共享文件的一个途径。然而，在系统中给一组用户共享文件也会变得很复杂。下一节会介绍如何实现。 

### 5.5  共享文件

创建新文件时，Linux会用你默认的UID和GID给文件分配权限。想让其他人也能访问文件，要么改变其他用户所在安全组的访问权限，要么就给文件分配一个包含其他用户的新默认属组。

Linux还为每个文件和目录存储了3个额外的信息位。

```bash
设置用户ID（SUID）：当文件被用户使用时，程序会以文件属主的权限运行。 
设置组ID（SGID）：对文件来说，程序会以文件属组的权限运行；对目录来说，目录中创建的新文件会以目录的默认属组作为默认属组。 
粘着位：进程结束后文件还驻留（粘着）在内存中。
```

SGID位对文件共享非常重要。启用SGID位后，你可以强制在一个共享目录下创建的新文件都属于该目录的属组，这个组也就成为了每个用户的属组。

SGID可通过 chmod 命令设置。它会加到标准3位八进制值之前（组成4位八进制值），或者在符号模式下用符号 s 。 
如果你用的是八进制模式，你需要知道这些位的位置：

```bash
000  0  所有位都清零 
001  1  粘着位置位 
010  2  SGID位置位 
011  3  SGID位和粘着位都置位 
100  4  SUID位置位 
101  5  SUID位和粘着位都置位 
110  6  SUID位和SGID位都置位 
111  7  所有位都置位 
```

因此，要创建一个共享目录，使目录里的新文件都能沿用目录的属组，只需将该目录的SGID位置位。

```bash
$ mkdir testdir 
$ ls -l 
drwxrwxr-x    2 rich     rich         4096 Sep 20 23:12 testdir/ 
$ chgrp shared testdir 
$ chmod g+s testdir 
$ ls -l 
drwxrwsr-x    2 rich     shared       4096 Sep 20 23:12 testdir/ 
$ umask 002 
$ cd testdir 
$ touch testfile 
$ ls -l 
total 0 
-rw-rw-r--    1 rich     shared          0 Sep 20 23:13 testfile 
$ 
```

首先，用 mkdir 命令来创建希望共享的目录。然后通过 chgrp 命令将目录的默认属组改为包含所有需要共享文件的用户的组（你必须是该组的成员）。最后，将目录的SGID位置位，以保证目录中新建文件都用shared作为默认属组。 

为了让这个环境能正常工作，所有组成员都需把他们的 umask 值设置成文件对属组成员可写。在前面的例子中， umask 改成了 002 ，所以文件对属组是可写的。 做完了这些，组成员就能到共享目录下创建新文件了。跟期望的一样，新文件会沿用目录的属组，而不是用户的默认属组。现在shared组的所有用户都能访问这个文件了。 