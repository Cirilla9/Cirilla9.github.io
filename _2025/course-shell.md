---
layout: lecture
title: "课程概览与 shell"
date: 2025-01-13
ready: true
sync: true
syncdate: 2021-04-24
video:
  aspect: 56.25
  id: Z56Jmr9Z34Q
solution:
    ready: true
    url: course-shell-solution
---
# 主题 1: The Shell

Shell 可以看作一个执行程序的程序，具体而言，它通过 *fork() & exec()* 调用执行程序。

```console
missing:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
missing:~$ which echo
/bin/echo
missing:~$ /bin/echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

它通过`$PATH`环境变量来寻找命令，这也是Linux很多程序安装的位置。

## 在程序间创建连接

在 Shell 中，程序有两个主要的流：它们的输入流和输出流。

当程序尝试读取信息时，它们会从输入流中进行读取，当程序打印信息时，它们会将信息输出到输出流中。
通常，一个程序的输入输出流都是终端。也就是，键盘作为输入，显示器作为输出。

我们也可以重定向这些流！最简单的重定向是 `< file` 和 `> file`。这两个命令可以将程序的输入输出流分别重定向到文件：

```console
missing:~$ echo hello > hello.txt
missing:~$ cat hello.txt
hello
missing:~$ cat < hello.txt
hello
missing:~$ cat < hello.txt > hello2.txt
missing:~$ cat hello2.txt
hello
```

可以使用 `>>` 来向一个文件追加内容。

`|` 允许我们将一个程序的输出和另外一个程序的输入连接起来：

```console
missing:~$ ls -l / | tail -n1
drwxr-xr-x 1 root  root  4096 Jun 20  2019 var
missing:~$ curl --head --silent google.com | grep --ignore-case content-length | cut --delimiter=' ' -f2
219
```

## 课后练习

将以下内容一行一行地写入 `semester` 文件，可以使用单引号''包含`#!/bin/sh`。

```bash
#!/bin/sh 
curl --head --silent https://missing.csail.mit.edu
```

使用 `chmod` 命令改变权限，使 `./semester` 能够成功执行，不要使用 `sh semester` 来执行该程序。Shell 是如何知晓这个文件需要使用 `sh` 来解析呢？

> 参考：[Shebang](https://en.wikipedia.org/wiki/Shebang_(Unix))

使用 `|` 和 `>` ，将 `semester` 文件输出的最后更改日期信息，写入主目录下的 `last-modified.txt` 的文件中。

## 实现

```bash
zz@8001:~/semester/Shell$ echo '#!/bin/sh' > semester 
zz@8001:~/semester/Shell$ echo 'curl --head --silent https://missing.csail.mit.edu' >> semester 
zz@8001:~/semester/Shell$ cat semester 
#!/bin/sh
curl --head --silent https://missing.csail.mit.edu
zz@8001:~/semester/Shell$ ./semester | grep last-modified > out.txt
zz@8001:~/semester/Shell$ cat out.txt 
last-modified: Sat, 19 Apr 2025 16:35:21 GMT
zz@8001:~/semester/Shell$ 
```
