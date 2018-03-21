---
title: linux网络管理
date: 2018-03-21 15:28:45
categories:
  - linux
---
# linux网络管理

工作中常用的linux网络管理命令

## 下载命令 - wget

$ wget (选项) (参数)

### 选项：

- -a<日志文件>：在指定的日志文件中记录资料的执行过程；
- -A<后缀名>：指定要下载文件的后缀名，多个后缀名之间使用逗号进行分隔；
- -b：进行后台的方式运行wget；
- -B<连接地址>：设置参考的连接地址的基地地址；
- -c：继续执行上次中断的任务；
- -C<标志>：设置服务器数据块功能标志on为激活，off为关闭，默认值为on；
- -d：调试模式运行指令；
- -D<域名列表>：设置顺着的域名列表，域名之间用“，”分隔；
- -e<指令>：作为文件“.wgetrc”中的一部分执行指定的指令；
- -h：显示指令帮助信息；
- -i<文件>：从指定文件获取要下载的URL地址；
- -l<目录列表>：设置顺着的目录列表，多个目录用“，”分隔；
- -L：仅顺着关联的连接；
- -r：递归下载方式；
- -nc：文件存在时，下载文件不覆盖原有文件；
- -nv：下载时只显示更新和出错信息，不显示指令的详细执行过程；
- -q：不显示指令执行过程；
- -nh：不查询主机名称；
- -v：显示详细执行过程；
- -V：显示版本信息；
- --passive-ftp：使用被动模式PASV连接FTP服务器；
- --follow-ftp：从HTML文件中下载FTP连接文件。

### 参数：

URL：下载指定的URL地址。

### 实例：

- 使用wget下载单个文件：$ wget http://www.linuxde.net/testfile.zip
- 下载并以不同的文件名保存：$ wget -O wordpress.zip http://www.linuxde.net/download.aspx?id=1080
  wget默认会以最后一个符合/的后面的字符来命令，对于动态链接的下载通常文件名会不正确。
- wget限速下载：$ wget --limit-rate=300k http://www.linuxde.net/testfile.zip
  当你执行wget的时候，它默认会占用全部可能的宽带下载。但是当你准备下载一个大文件，而你还需要下载其它文件时就有必要限速了。
- 使用wget断点续传：$ wget -c http://www.linuxde.net/testfile.zip
  使用wget -c重新启动下载中断的文件，对于我们下载大文件时突然由于网络等原因中断非常有帮助，我们可以继续接着下载而不是重新下载一个文件。需要继续中断的下载时可以使用-c参数。
- 使用wget后台下载：$ wget -b http://www.linuxde.net/testfile.zip
  对于下载非常大的文件的时候，我们可以使用参数-b进行后台下载，你可以使用以下命令来察看下载进度：$ tail -f wget-log
- 测试下载链接：$ wget --spider URL
