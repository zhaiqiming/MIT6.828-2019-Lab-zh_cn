# 实验：Xv6和Unix程序 

这个实验将会让你熟悉Xv6和它的系统调用 

## 引导Xv6  

你必须使用一个X86 Athena machine；也就是说 uname -a 之后应该显示 i386 GNU/Linux or i686 GNU/Linux or x86_64 GNU/Linux。你可以登陆进一个公共的Athena host，通过使用指令ssh -X athena.dialup.mit.edu  

我们已经为你在Athena machine上建立了相应的编译器和模拟器。你可以通过运行 add -f 6.828 来使用它们。每次登录你都必须运行此命令(或者将它添加到环境变量)。如果你在编译或运行qemu时得到模糊的错误,请首先检查是否将课程资源添加到了环境变量  

获取本Xv6 lab的资源，并检查util分支：  

    $ git clone git://github.com/mit-pdos/xv6-riscv-fall19.git  
    Cloning into 'xv6-riscv'...  
    ...  
    $ cd xv6-riscv-fall19  
    $ git checkout util  

为了使lab更加简单，xv6-riscv-fall19代码仓库和书中的xv6-riscv只有轻微的不同  

在本实验中你使用的文件是分布式的，通过使用Git版本控制系统。你要为你的答案创建一个新的分支（git branch util）。你可以通过[Git用户手册](http://www.kernel.org/pub/software/scm/git/docs/user-manual.html)来学习更多关于Git的知识。[CS-oriented overview of Git](http://eagain.net/articles/git-for-computer-scientists/)或许也是有用的。Git允许您跟踪您对代码的更改。举个例子,如果你完成了一个练习,和想提交你的变更,你可以通过运行以下内容提交你的修改:  

    $ git commit -am 'my solution for util lab exercise 1'  
    Created commit 60d2135: my solution for util lab exercise 1  
     1 files changed, 1 insertions(+), 0 deletions(-)  
    $  

你可以使用git diff命令跟踪你的更改。运行git diff将显示你从上次提交过后更改的代码,“git diff origin/xv6-riscv-fall19”将会显示自初始版本以来改变过的代码。origin/xv6-riscv-fall19就是你下载课程初始代码的分支的名字。  

在Athena上build Xv6：  

    $ make  
    riscv64-linux-gnu-gcc    -c -o kernel/entry.o kernel/entry.S  
    riscv64-linux-gnu-gcc -Wall -Werror -O -fno-omit-frame-pointer -ggdb -MD -mcmodel=medany -ffreestanding -fno-common -nostdlib -mno-relax -I. -fno-stack-protector -fno-pie -no-pie   -c -o kernel/start.o kernel/start.c  
    ...  
    $ make qemu  
    ...  
    mkfs/mkfs fs.img README user/_cat user/_echo user/_forktest user/_grep user/_init user/_kill user/_ln user/_ls user/_mkdir user/_rm user/_sh user/_stressfs user/_usertests user/_wc user/_zombie user/_cow  
    nmeta 46 (boot, super, log blocks 30 inode blocks 13, bitmap blocks 1) blocks 954 total 1000  
    balloc: first 497 blocks have been allocated  
    balloc: write bitmap block at sector 45  
    qemu-system-riscv64 -machine virt -kernel kernel/kernel -m 3G -smp 3 -nographic -drive   file=fs.img,if=none,format=raw,id=x0 -device virtio-blk-device,drive=x0,bus=virtio-mmio-bus.0  
    hart 0 starting  
    hart 2 starting  
    hart 1 starting  
    init: starting sh  
    $  

如果你在命令行输入ls,应该会看到类似于下面的输出:  
    $ ls  
    .              1 1 1024  
    ..             1 1 1024  
    README         2 2 2181  
    cat            2 3 21024  
    echo           2 4 19776  
    forktest       2 5 11456  
    grep           2 6 24512  
    init           2 7 20656  
    kill           2 8 19856  
    ln             2 9 19832  
    ls             2 10 23280  
    mkdir          2 11 19952  
    rm             2 12 19936  
    sh             2 13 38632  
    stressfs       2 14 20912  
    usertests      2 15 106264  
    wc             2 16 22160  
    zombie         2 17 19376  
    cow            2 18 27152  
    console        3 19 0  

这些就是在初始mkfs文件系统中包含的程序文件。你只是运行了其中一个：ls  

如果你是在非Athena机器上工作，你需要根据[Tools page](https://pdos.csail.mit.edu/6.828/2019/tools.html)安装qume和gcc for RISC-V。  

# 提交程序  

你将使用[提交网站](https://6828.scripts.mit.edu/2019/handin.py/)来提交你的作业。在你提交作业或实验前，你需要从网站上申请一个API key。实验代码采用GNU Make规则来简化提交。在你最后的更改提交后,输入 make handin 提交你的实验。  
$ git commit -am "ready to submit my lab"  
[util c2e3c8b] ready to submit my lab  
 2 files changed, 18 insertions(+), 2 deletions(-)  

    $ make handin  
    git archive --prefix=util/ --format=tar HEAD | gzip >     util-handin.tar.gz  
    Get an API key for yourself by visiting https://6828.scripts.mit.edu/    2018/handin.py/  
    Please enter your API key: XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX  
      % Total    % Received % Xferd  Average Speed   Time    Time         Time  Current  
                                 Dload  Upload   Total   Spent        Left  Speed  
    100 50199  100   241  100 49958    414  85824 --:--:-- --:--:--           --:--:-- 85986  
    $  

make handin 将会把API key存在 myapi.key中。如果你需要改变API key，删除这个文件，然后执行 make handin 再生成一个就可以了（myapi.key不能有换行符）  

如果你在 <font color=red>make handin</font> 时有未提交的改变或者有无法追踪的文件，你将会看到类似下面的输出：  

    M hello.c  
    ?? bar.c  
    ?? foo.pyc  
    Untracked files will not be handed in.  Continue? [y/N]  

检查上面的，确保你的答案中需要的文件不在上面以??开头的行中，你可以使用<font color=red>git add filename</font>来使Git能追踪到你的文件。  

如果<font color=red>make handin</font>不能正常工作，你可以使用<font color=red>make tarball</font>来生成tar文件，然后在[web](https://6828.scripts.mit.edu/2019/handin.py/)上提交  

你可以使用<font color=red>make grade</font>来运行评分程序。TAs将使用与你本地相同的评分程序来评分。  

## Sleep  

为Xv6实现Unix Sleep调用；你的Sleep应该能够暂停一段用户指定的ticks时长（tick是内核中定义的一个概念，代表两次时钟中断之间的时间长度）。你的答案应该实现在user/sleep.c  

一些提示：  
-  你可以通过查看/user下的其他程序来学习如何将命令行参数传入程序。如果用户忘记传入参数，sleep程序应该打印error信息  
-  命令行参数作为字符串传递;你可以使用atoi将它转换成一个整数(见user/ ulib.c)  
-  使用系统调用 sleep （见user/usys.S 与 kernel/sysproc.c）  
-  确保 main 调用 exit() 来退出程序  
-  在makefile中将程序添加到UPROGS并通过 make fs.img 来编译程序  
-  阅读Kernighan and Ritchie's book The C programming language (second edition) (K&R) 来学习C语言  

从Xv6 shell运行程序：  

      $ make qemu  
      ...  
      init: starting sh  
      $ sleep 10  
      (nothing happens for a little while)  
      $  

如果您的程序行为如上所示，那么你的答案是正确的  

可选：编写一个 uptime 的调用程序，打印出uptime的值（tips：uptime是一个系统调用） 。  

## pingpong  

编写一个程序，该程序使用UNIX系统调用通过一对管道在两个进程之间``乒乓''一个字节，每个方向一个。父进程通过向byte_fd [1]写入字节来发送，而子进程通过从parent_fd [0]读取来接收字节。子进程从父进程收到一个字节后，通过写入child_fd [1] 响应，然后由父级读取。您的解决方案应该在文件user / pingpong.c中。  

一些提示：  

- 使用 pipe 创建管道  
- 使用 fork 创建一个子进程  
- 使用 read 读取管道，并使用 write 写入管道  

从xv6 shell运行该程序，它将产生以下输出：  

    $ make qemu  
    ...  
    init: starting sh  
    $ pingpong  
    4: received ping  
    3: received pong  
    

如果您的程序行为如上所述，则您的解决方案是正确的。“:”之前的数字是打印输出的进程的进程ID。您可以通过调用系统调用getpid来获取进程ID  

## primes  

利用管道编写一个并发版本的筛素数程序,这个想法来自Unix pipe管道的创始人.[web](http://swtch.com/~rsc/thread/)此页中解释了如何去操作.你的答案应该写在user/primes.c  

你的目标是使用 pipe 和 fork 去创建一个管道。 第一个进程将[2,35]输入进管道。对于每一个质数,您将安排创建一个进程从其左邻居读取在管道并写入其右邻居管道。因为xv6的文件描述符和进程数量是有限制的，所以第一个进程应该在写入35之后停止写入。  

一些提示：  

- 小心的关闭进程不需要的文件描述符，否则你的程序可能在第一个进程到达35之前耗尽xv6的资源  
- 一旦第一个进程到达35，您就应该安排管道干净利落地终止，包括所有子进程（提示：关闭管道的写端时 read 将返回 EOF）。  
- 直接往管道里写入32位 intS 而不是格式化的 ACSII I/O 是最简单的。  
- 你应该根据需求在管道里创建进程  

如果你的输入如下所示，那么你的答案就是正确的：  

    $ make qemu  
    ...  
    init: starting sh  
    $ primes  
    prime 2  
    prime 3  
    prime 5  
    prime 7  
    prime 11  
    prime 13  
    prime 17  
    prime 19  
    prime 23  
    prime 29  
    prime 31  
    $  
    
## find  

编写UNIX查找程序的简单版本：在目录树中查找名称与参数字符串匹配的所有文件。 您的解决方案应该在文件user / find.c中。  

一些提示：  

- 查看user / ls.c以了解如何读取目录  
- 使用递归来进入子目录  
- 不要递归到“.” 和“..”  
- 对文件系统的更改在qemu的运行中持续存在。 要获得干净的文件系统，请先运行 make clean，然后再make qemu  
- 你需要使用C风格字符串。 查看K＆R（C book），例如5.5节  

可选：支持使用正则表达式进行名称匹配。 grep.c 对正则表达式有一些原始支持。  

如果产生以下输出（文件系统包含文件 a/b 时），则您的解决方案是正确的：  

    $ make qemu  
    ...  
    init: starting sh  
    $ mkdir a  
    $ echo > a/b  
    $ find . b  
    ./a/b  
    $  
    
## xargs  

编写UNIX xargs程序的简单版本：从标准输入中读取每行并为每行运行一个命令，并将该行的结果作为参数提供给其他命令。 您的解决方案应该在文件user / xargs.c中。  

下面的例子说明了xarg的行为:  

    $ xargs echo bye  
    hello too  
    bye hello too  
    ctrl-d  
    $  

一些提示：  

使用fork和exec系统调用在输入的每一行上调用命令。 在父级中使用wait等待子级完成命令的运行。  
从标准输入中读取一个字符，直到换行符（'\n'）。  
kernel/param.h 中声明了 MAXARG ，如果您需要声明 argv ,这可能很有用。  
对文件系统的更改在qemu的运行中持续存在。 要获得干净的文件系统，请先运行make clean，然后再进行qemu。  

xargs, find, 和 grep 的结合:  

    $ find . b | xargs grep hello  

以上命令将会在每个拥有文件名为 b 的文件夹下运行 grep hello  

运行 xargstest.sh 脚本来测试你的答案.如果你的答案像接下来输出的一样,那么你的答案就是正确的:  

    $ make qemu  
    ...  
    init: starting sh  
    $ sh < xargstest.sh  
    $ $ $ $ $ $ hello  
    hello  
    hello  
    $ $  

您可能需要修复 find 程序中的错误。 输出中有很多 $ ，因为xv6 shell是很原始的，并且它不会意识到正在处理文件中的命令而不是控制台中的命令，它会为文件中的每个命令打印 $  

本实验到此完成。 在实验室目录中，使用 git commit 提交更改，然后键入 make handin 提交代码。  
