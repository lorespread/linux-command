**系统环境：CentOS Linux release 7.8.2003 (Core)**

**mysql 版本：Server version: 8.0.21 MySQL Community Server - GPL**

**注意：基础常用的软件使用 root 用户进行了安装**

**安装 mysql 8.x 步骤**

### 1 缺少 wget，安装 wget

```shell
yum install -y wget
```

### 2 加载源并进行安装

**其中 yum 安装的 rpm 地址：**[https://dev.mysql.com/downloads/repo/yum/](https://dev.mysql.com/downloads/repo/yum/)

```shell
[root@ra-local-oa ~]# wget https://dev.mysql.com/get/mysql80-community-release-el7-11.noarch.rpm
--2024-04-11 22:44:49--  https://dev.mysql.com/get/mysql80-community-release-el7-11.noarch.rpm
正在解析主机 dev.mysql.com (dev.mysql.com)... 23.5.250.172, 2600:1406:5600:783::2e31, 2600:1406:5600:78e::2e31
正在连接 dev.mysql.com (dev.mysql.com)|23.5.250.172|:443... 已连接。
已发出 HTTP 请求，正在等待回应... 302 Moved Temporarily
位置：https://repo.mysql.com//mysql80-community-release-el7-11.noarch.rpm [跟随至新的 URL]
--2024-04-11 22:44:50--  https://repo.mysql.com//mysql80-community-release-el7-11.noarch.rpm
正在解析主机 repo.mysql.com (repo.mysql.com)... 23.56.114.117, 2600:1406:5600:681::1d68, 2600:1406:5600:680::1d68
正在连接 repo.mysql.com (repo.mysql.com)|23.56.114.117|:443... 已连接。
已发出 HTTP 请求，正在等待回应... 200 OK
长度：14064 (14K) [application/x-redhat-package-manager]
正在保存至：“mysql80-community-release-el7-11.noarch.rpm”

100%[========================================================================================>] 14,064      --.-K/s 用时 0s

2024-04-11 22:44:52 (208 MB/s) - 已保存“mysql80-community-release-el7-11.noarch.rpm” [14064/14064])
```

#### 导入 RPM-GPG-KEY

```shell
rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
```

#### 验证 RPM-GPG-KEY

```
[root@ra-local-oa ~]# rpm --checksig mysql80-community-release-el7-11.noarch.rpm
mysql80-community-release-el7-11.noarch.rpm: rsa sha1 (md5) pgp md5 确定
```

### 3 查看源是否安装成功

**执行成功后会在/etc/yum.repos.d/目录下生成两个 repo 文件 mysql-community.repo 及 mysql-community-source.repo**

**并且通过 yum repolist 可以看到 mysql 相关资源**

```shell
[root@ra-local-oa yum.repos.d]# yum repolist enabled | grep "mysql.*-community.*"
mysql-connectors-community/x86_64       MySQL Connectors Community           242
mysql-tools-community/x86_64            MySQL Tools Community                104
mysql80-community/x86_64                MySQL 8.0 Community Server           465
```

### 4 查看当年源中包含的 mysql 版本

```shell
[root@ra-local-oa yum.repos.d]# yum repolist all | grep mysql
mysql-cluster-7.5-community/x86_64                  MySQL Cluster 7 禁用
mysql-cluster-7.5-community-source                  MySQL Cluster 7 禁用
mysql-cluster-7.6-community/x86_64                  MySQL Cluster 7 禁用
mysql-cluster-7.6-community-source                  MySQL Cluster 7 禁用
mysql-cluster-8.0-community/x86_64                  MySQL Cluster 8 禁用
mysql-cluster-8.0-community-debuginfo/x86_64        MySQL Cluster 8 禁用
mysql-cluster-8.0-community-source                  MySQL Cluster 8 禁用
mysql-cluster-innovation-community/x86_64           MySQL Cluster I 禁用
mysql-cluster-innovation-community-debuginfo/x86_64 MySQL Cluster I 禁用
mysql-cluster-innovation-community-source           MySQL Cluster I 禁用
mysql-connectors-community/x86_64                   MySQL Connector 启用：242
mysql-connectors-community-debuginfo/x86_64         MySQL Connector 禁用
mysql-connectors-community-source                   MySQL Connector 禁用
mysql-innovation-community/x86_64                   MySQL Innovatio 禁用
mysql-innovation-community-debuginfo/x86_64         MySQL Innovatio 禁用
mysql-innovation-community-source                   MySQL Innovatio 禁用
mysql-tools-community/x86_64                        MySQL Tools Com 启用：104
mysql-tools-community-debuginfo/x86_64              MySQL Tools Com 禁用
mysql-tools-community-source                        MySQL Tools Com 禁用
mysql-tools-innovation-community/x86_64             MySQL Tools Inn 禁用
mysql-tools-innovation-community-debuginfo/x86_64   MySQL Tools Inn 禁用
mysql-tools-innovation-community-source             MySQL Tools Inn 禁用
mysql-tools-preview/x86_64                          MySQL Tools Pre 禁用
mysql-tools-preview-source                          MySQL Tools Pre 禁用
mysql57-community/x86_64                            MySQL 5.7 Commu 禁用
mysql57-community-source                            MySQL 5.7 Commu 禁用
mysql80-community/x86_64                            MySQL 8.0 Commu 启用：465
mysql80-community-debuginfo/x86_64                  MySQL 8.0 Commu 禁用
mysql80-community-source                            MySQL 8.0 Commu 禁用
```

### 5 安装 mysql

```python
[root@ra-local-oa yum.repos.d]# yum install -y mysql-community-server
已加载插件：fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.aliyun.com
 * extras: mirrors.163.com
 * updates: mirrors.aliyun.com
正在解决依赖关系
--> 正在检查事务
---> 软件包 mysql-community-server.x86_64.0.8.0.36-1.el7 将被 安装
--> 正在处理依赖关系 mysql-community-common(x86-64) = 8.0.36-1.el7，它被软件包 mysql-community-server-8.0.36-1.el7.x86_64 需要
--> 正在处理依赖关系 mysql-community-icu-data-files = 8.0.36-1.el7，它被软件包 mysql-community-server-8.0.36-1.el7.x86_64 需要
--> 正在处理依赖关系 mysql-community-client(x86-64) >= 8.0.11，它被软件包 mysql-community-server-8.0.36-1.el7.x86_64 需要
--> 正在处理依赖关系 /usr/bin/perl，它被软件包 mysql-community-server-8.0.36-1.el7.x86_64 需要
--> 正在处理依赖关系 net-tools，它被软件包 mysql-community-server-8.0.36-1.el7.x86_64 需要
--> 正在处理依赖关系 perl(Getopt::Long)，它被软件包 mysql-community-server-8.0.36-1.el7.x86_64 需要
--> 正在处理依赖关系 perl(strict)，它被软件包 mysql-community-server-8.0.36-1.el7.x86_64 需要
--> 正在检查事务
---> 软件包 mysql-community-client.x86_64.0.8.0.36-1.el7 将被 安装
--> 正在处理依赖关系 mysql-community-client-plugins = 8.0.36-1.el7，它被软件包 mysql-community-client-8.0.36-1.el7.x86_64 需要
--> 正在处理依赖关系 mysql-community-libs(x86-64) >= 8.0.11，它被软件包 mysql-community-client-8.0.36-1.el7.x86_64 需要
---> 软件包 mysql-community-common.x86_64.0.8.0.36-1.el7 将被 安装
---> 软件包 mysql-community-icu-data-files.x86_64.0.8.0.36-1.el7 将被 安装
---> 软件包 net-tools.x86_64.0.2.0-0.25.20131004git.el7 将被 安装
---> 软件包 perl.x86_64.4.5.16.3-299.el7_9 将被 安装
--> 正在处理依赖关系 perl-libs = 4:5.16.3-299.el7_9，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl(Socket) >= 1.3，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl(Scalar::Util) >= 1.10，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl-macros，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl-libs，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl(threads::shared)，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl(threads)，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl(constant)，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl(Time::Local)，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl(Time::HiRes)，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl(Storable)，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl(Socket)，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl(Scalar::Util)，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl(Pod::Simple::XHTML)，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl(Pod::Simple::Search)，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl(Filter::Util::Call)，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl(File::Temp)，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl(File::Spec::Unix)，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl(File::Spec::Functions)，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl(File::Spec)，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl(File::Path)，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl(Exporter)，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl(Cwd)，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 perl(Carp)，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
--> 正在处理依赖关系 libperl.so()(64bit)，它被软件包 4:perl-5.16.3-299.el7_9.x86_64 需要
---> 软件包 perl-Getopt-Long.noarch.0.2.40-3.el7 将被 安装
--> 正在处理依赖关系 perl(Pod::Usage) >= 1.14，它被软件包 perl-Getopt-Long-2.40-3.el7.noarch 需要
--> 正在处理依赖关系 perl(Text::ParseWords)，它被软件包 perl-Getopt-Long-2.40-3.el7.noarch 需要
--> 正在检查事务
---> 软件包 mariadb-libs.x86_64.1.5.5.68-1.el7 将被 取代
--> 正在处理依赖关系 libmysqlclient.so.18()(64bit)，它被软件包 2:postfix-2.10.1-9.el7.x86_64 需要
--> 正在处理依赖关系 libmysqlclient.so.18(libmysqlclient_18)(64bit)，它被软件包 2:postfix-2.10.1-9.el7.x86_64 需要
---> 软件包 mysql-community-client-plugins.x86_64.0.8.0.36-1.el7 将被 安装
---> 软件包 mysql-community-libs.x86_64.0.8.0.36-1.el7 将被 舍弃
---> 软件包 perl-Carp.noarch.0.1.26-244.el7 将被 安装
---> 软件包 perl-Exporter.noarch.0.5.68-3.el7 将被 安装
---> 软件包 perl-File-Path.noarch.0.2.09-2.el7 将被 安装
---> 软件包 perl-File-Temp.noarch.0.0.23.01-3.el7 将被 安装
---> 软件包 perl-Filter.x86_64.0.1.49-3.el7 将被 安装
---> 软件包 perl-PathTools.x86_64.0.3.40-5.el7 将被 安装
---> 软件包 perl-Pod-Simple.noarch.1.3.28-4.el7 将被 安装
--> 正在处理依赖关系 perl(Pod::Escapes) >= 1.04，它被软件包 1:perl-Pod-Simple-3.28-4.el7.noarch 需要
--> 正在处理依赖关系 perl(Encode)，它被软件包 1:perl-Pod-Simple-3.28-4.el7.noarch 需要
---> 软件包 perl-Pod-Usage.noarch.0.1.63-3.el7 将被 安装
--> 正在处理依赖关系 perl(Pod::Text) >= 3.15，它被软件包 perl-Pod-Usage-1.63-3.el7.noarch 需要
--> 正在处理依赖关系 perl-Pod-Perldoc，它被软件包 perl-Pod-Usage-1.63-3.el7.noarch 需要
---> 软件包 perl-Scalar-List-Utils.x86_64.0.1.27-248.el7 将被 安装
---> 软件包 perl-Socket.x86_64.0.2.010-5.el7 将被 安装
---> 软件包 perl-Storable.x86_64.0.2.45-3.el7 将被 安装
---> 软件包 perl-Text-ParseWords.noarch.0.3.29-4.el7 将被 安装
---> 软件包 perl-Time-HiRes.x86_64.4.1.9725-3.el7 将被 安装
---> 软件包 perl-Time-Local.noarch.0.1.2300-2.el7 将被 安装
---> 软件包 perl-constant.noarch.0.1.27-2.el7 将被 安装
---> 软件包 perl-libs.x86_64.4.5.16.3-299.el7_9 将被 安装
---> 软件包 perl-macros.x86_64.4.5.16.3-299.el7_9 将被 安装
---> 软件包 perl-threads.x86_64.0.1.87-4.el7 将被 安装
---> 软件包 perl-threads-shared.x86_64.0.1.43-6.el7 将被 安装
--> 正在检查事务
---> 软件包 mysql-community-libs-compat.x86_64.0.8.0.36-1.el7 将被 舍弃
---> 软件包 perl-Encode.x86_64.0.2.51-7.el7 将被 安装
---> 软件包 perl-Pod-Escapes.noarch.1.1.04-299.el7_9 将被 安装
---> 软件包 perl-Pod-Perldoc.noarch.0.3.20-4.el7 将被 安装
--> 正在处理依赖关系 perl(parent)，它被软件包 perl-Pod-Perldoc-3.20-4.el7.noarch 需要
--> 正在处理依赖关系 perl(HTTP::Tiny)，它被软件包 perl-Pod-Perldoc-3.20-4.el7.noarch 需要
---> 软件包 perl-podlators.noarch.0.2.5.1-3.el7 将被 安装
--> 正在检查事务
---> 软件包 perl-HTTP-Tiny.noarch.0.0.033-3.el7 将被 安装
---> 软件包 perl-parent.noarch.1.0.225-244.el7 将被 安装
--> 解决依赖关系完成

依赖关系解决

==================================================================================================================================
 Package                                  架构             版本                                 源                           大小
==================================================================================================================================
正在安装:
 mysql-community-libs                     x86_64           8.0.36-1.el7                         mysql80-community           1.5 M
      替换  mariadb-libs.x86_64 1:5.5.68-1.el7
 mysql-community-libs-compat              x86_64           8.0.36-1.el7                         mysql80-community           669 k
      替换  mariadb-libs.x86_64 1:5.5.68-1.el7
 mysql-community-server                   x86_64           8.0.36-1.el7                         mysql80-community            64 M
为依赖而安装:
 mysql-community-client                   x86_64           8.0.36-1.el7                         mysql80-community            16 M
 mysql-community-client-plugins           x86_64           8.0.36-1.el7                         mysql80-community           3.5 M
 mysql-community-common                   x86_64           8.0.36-1.el7                         mysql80-community           665 k
 mysql-community-icu-data-files           x86_64           8.0.36-1.el7                         mysql80-community           2.2 M
 net-tools                                x86_64           2.0-0.25.20131004git.el7             base                        306 k
 perl                                     x86_64           4:5.16.3-299.el7_9                   updates                     8.0 M
 perl-Carp                                noarch           1.26-244.el7                         base                         19 k
 perl-Encode                              x86_64           2.51-7.el7                           base                        1.5 M
 perl-Exporter                            noarch           5.68-3.el7                           base                         28 k
 perl-File-Path                           noarch           2.09-2.el7                           base                         26 k
 perl-File-Temp                           noarch           0.23.01-3.el7                        base                         56 k
 perl-Filter                              x86_64           1.49-3.el7                           base                         76 k
 perl-Getopt-Long                         noarch           2.40-3.el7                           base                         56 k
 perl-HTTP-Tiny                           noarch           0.033-3.el7                          base                         38 k
 perl-PathTools                           x86_64           3.40-5.el7                           base                         82 k
 perl-Pod-Escapes                         noarch           1:1.04-299.el7_9                     updates                      52 k
 perl-Pod-Perldoc                         noarch           3.20-4.el7                           base                         87 k
 perl-Pod-Simple                          noarch           1:3.28-4.el7                         base                        216 k
 perl-Pod-Usage                           noarch           1.63-3.el7                           base                         27 k
 perl-Scalar-List-Utils                   x86_64           1.27-248.el7                         base                         36 k
 perl-Socket                              x86_64           2.010-5.el7                          base                         49 k
 perl-Storable                            x86_64           2.45-3.el7                           base                         77 k
 perl-Text-ParseWords                     noarch           3.29-4.el7                           base                         14 k
 perl-Time-HiRes                          x86_64           4:1.9725-3.el7                       base                         45 k
 perl-Time-Local                          noarch           1.2300-2.el7                         base                         24 k
 perl-constant                            noarch           1.27-2.el7                           base                         19 k
 perl-libs                                x86_64           4:5.16.3-299.el7_9                   updates                     690 k
 perl-macros                              x86_64           4:5.16.3-299.el7_9                   updates                      44 k
 perl-parent                              noarch           1:0.225-244.el7                      base                         12 k
 perl-podlators                           noarch           2.5.1-3.el7                          base                        112 k
 perl-threads                             x86_64           1.87-4.el7                           base                         49 k
 perl-threads-shared                      x86_64           1.43-6.el7                           base                         39 k

事务概要
==================================================================================================================================
安装  3 软件包 (+32 依赖软件包)

总计：100 M
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
警告：RPM 数据库已被非 yum 程序修改。
  正在安装    : mysql-community-client-plugins-8.0.36-1.el7.x86_64                                                           1/36
  正在安装    : mysql-community-common-8.0.36-1.el7.x86_64                                                                   2/36
  正在安装    : mysql-community-libs-8.0.36-1.el7.x86_64                                                                     3/36
  正在安装    : mysql-community-client-8.0.36-1.el7.x86_64                                                                   4/36
  正在安装    : 1:perl-parent-0.225-244.el7.noarch                                                                           5/36
  正在安装    : perl-HTTP-Tiny-0.033-3.el7.noarch                                                                            6/36
  正在安装    : perl-podlators-2.5.1-3.el7.noarch                                                                            7/36
  正在安装    : perl-Pod-Perldoc-3.20-4.el7.noarch                                                                           8/36
  正在安装    : 1:perl-Pod-Escapes-1.04-299.el7_9.noarch                                                                     9/36
  正在安装    : perl-Encode-2.51-7.el7.x86_64                                                                               10/36
  正在安装    : perl-Text-ParseWords-3.29-4.el7.noarch                                                                      11/36
  正在安装    : perl-Pod-Usage-1.63-3.el7.noarch                                                                            12/36
  正在安装    : 4:perl-macros-5.16.3-299.el7_9.x86_64                                                                       13/36
  正在安装    : perl-Storable-2.45-3.el7.x86_64                                                                             14/36
  正在安装    : perl-Exporter-5.68-3.el7.noarch                                                                             15/36
  正在安装    : perl-constant-1.27-2.el7.noarch                                                                             16/36
  正在安装    : perl-Socket-2.010-5.el7.x86_64                                                                              17/36
  正在安装    : perl-Time-Local-1.2300-2.el7.noarch                                                                         18/36
  正在安装    : perl-Carp-1.26-244.el7.noarch                                                                               19/36
  正在安装    : perl-PathTools-3.40-5.el7.x86_64                                                                            20/36
  正在安装    : perl-Scalar-List-Utils-1.27-248.el7.x86_64                                                                  21/36
  正在安装    : 1:perl-Pod-Simple-3.28-4.el7.noarch                                                                         22/36
  正在安装    : perl-File-Temp-0.23.01-3.el7.noarch                                                                         23/36
  正在安装    : perl-File-Path-2.09-2.el7.noarch                                                                            24/36
  正在安装    : perl-threads-shared-1.43-6.el7.x86_64                                                                       25/36
  正在安装    : perl-threads-1.87-4.el7.x86_64                                                                              26/36
  正在安装    : 4:perl-Time-HiRes-1.9725-3.el7.x86_64                                                                       27/36
  正在安装    : perl-Filter-1.49-3.el7.x86_64                                                                               28/36
  正在安装    : 4:perl-libs-5.16.3-299.el7_9.x86_64                                                                         29/36
  正在安装    : perl-Getopt-Long-2.40-3.el7.noarch                                                                          30/36
  正在安装    : 4:perl-5.16.3-299.el7_9.x86_64                                                                              31/36
  正在安装    : mysql-community-icu-data-files-8.0.36-1.el7.x86_64                                                          32/36
  正在安装    : net-tools-2.0-0.25.20131004git.el7.x86_64                                                                   33/36
  正在安装    : mysql-community-server-8.0.36-1.el7.x86_64                                                                  34/36
  正在安装    : mysql-community-libs-compat-8.0.36-1.el7.x86_64                                                             35/36
  正在删除    : 1:mariadb-libs-5.5.68-1.el7.x86_64                                                                          36/36
  验证中      : perl-HTTP-Tiny-0.033-3.el7.noarch                                                                            1/36
  验证中      : mysql-community-server-8.0.36-1.el7.x86_64                                                                   2/36
  验证中      : perl-threads-shared-1.43-6.el7.x86_64                                                                        3/36
  验证中      : perl-Storable-2.45-3.el7.x86_64                                                                              4/36
  验证中      : perl-Exporter-5.68-3.el7.noarch                                                                              5/36
  验证中      : perl-constant-1.27-2.el7.noarch                                                                              6/36
  验证中      : perl-PathTools-3.40-5.el7.x86_64                                                                             7/36
  验证中      : 4:perl-macros-5.16.3-299.el7_9.x86_64                                                                        8/36
  验证中      : 1:perl-parent-0.225-244.el7.noarch                                                                           9/36
  验证中      : perl-Socket-2.010-5.el7.x86_64                                                                              10/36
  验证中      : mysql-community-client-8.0.36-1.el7.x86_64                                                                  11/36
  验证中      : perl-File-Temp-0.23.01-3.el7.noarch                                                                         12/36
  验证中      : net-tools-2.0-0.25.20131004git.el7.x86_64                                                                   13/36
  验证中      : 1:perl-Pod-Simple-3.28-4.el7.noarch                                                                         14/36
  验证中      : perl-Time-Local-1.2300-2.el7.noarch                                                                         15/36
  验证中      : 1:perl-Pod-Escapes-1.04-299.el7_9.noarch                                                                    16/36
  验证中      : perl-Carp-1.26-244.el7.noarch                                                                               17/36
  验证中      : mysql-community-common-8.0.36-1.el7.x86_64                                                                  18/36
  验证中      : mysql-community-libs-8.0.36-1.el7.x86_64                                                                    19/36
  验证中      : perl-Scalar-List-Utils-1.27-248.el7.x86_64                                                                  20/36
  验证中      : mysql-community-icu-data-files-8.0.36-1.el7.x86_64                                                          21/36
  验证中      : perl-Pod-Usage-1.63-3.el7.noarch                                                                            22/36
  验证中      : perl-Encode-2.51-7.el7.x86_64                                                                               23/36
  验证中      : perl-Pod-Perldoc-3.20-4.el7.noarch                                                                          24/36
  验证中      : perl-podlators-2.5.1-3.el7.noarch                                                                           25/36
  验证中      : mysql-community-client-plugins-8.0.36-1.el7.x86_64                                                          26/36
  验证中      : 4:perl-5.16.3-299.el7_9.x86_64                                                                              27/36
  验证中      : perl-File-Path-2.09-2.el7.noarch                                                                            28/36
  验证中      : perl-threads-1.87-4.el7.x86_64                                                                              29/36
  验证中      : 4:perl-Time-HiRes-1.9725-3.el7.x86_64                                                                       30/36
  验证中      : perl-Filter-1.49-3.el7.x86_64                                                                               31/36
  验证中      : perl-Getopt-Long-2.40-3.el7.noarch                                                                          32/36
  验证中      : perl-Text-ParseWords-3.29-4.el7.noarch                                                                      33/36
  验证中      : mysql-community-libs-compat-8.0.36-1.el7.x86_64                                                             34/36
  验证中      : 4:perl-libs-5.16.3-299.el7_9.x86_64                                                                         35/36
  验证中      : 1:mariadb-libs-5.5.68-1.el7.x86_64                                                                          36/36

已安装:
  mysql-community-libs.x86_64 0:8.0.36-1.el7                    mysql-community-libs-compat.x86_64 0:8.0.36-1.el7
  mysql-community-server.x86_64 0:8.0.36-1.el7

作为依赖被安装:
  mysql-community-client.x86_64 0:8.0.36-1.el7                mysql-community-client-plugins.x86_64 0:8.0.36-1.el7
  mysql-community-common.x86_64 0:8.0.36-1.el7                mysql-community-icu-data-files.x86_64 0:8.0.36-1.el7
  net-tools.x86_64 0:2.0-0.25.20131004git.el7                 perl.x86_64 4:5.16.3-299.el7_9
  perl-Carp.noarch 0:1.26-244.el7                             perl-Encode.x86_64 0:2.51-7.el7
  perl-Exporter.noarch 0:5.68-3.el7                           perl-File-Path.noarch 0:2.09-2.el7
  perl-File-Temp.noarch 0:0.23.01-3.el7                       perl-Filter.x86_64 0:1.49-3.el7
  perl-Getopt-Long.noarch 0:2.40-3.el7                        perl-HTTP-Tiny.noarch 0:0.033-3.el7
  perl-PathTools.x86_64 0:3.40-5.el7                          perl-Pod-Escapes.noarch 1:1.04-299.el7_9
  perl-Pod-Perldoc.noarch 0:3.20-4.el7                        perl-Pod-Simple.noarch 1:3.28-4.el7
  perl-Pod-Usage.noarch 0:1.63-3.el7                          perl-Scalar-List-Utils.x86_64 0:1.27-248.el7
  perl-Socket.x86_64 0:2.010-5.el7                            perl-Storable.x86_64 0:2.45-3.el7
  perl-Text-ParseWords.noarch 0:3.29-4.el7                    perl-Time-HiRes.x86_64 4:1.9725-3.el7
  perl-Time-Local.noarch 0:1.2300-2.el7                       perl-constant.noarch 0:1.27-2.el7
  perl-libs.x86_64 4:5.16.3-299.el7_9                         perl-macros.x86_64 4:5.16.3-299.el7_9
  perl-parent.noarch 1:0.225-244.el7                          perl-podlators.noarch 0:2.5.1-3.el7
  perl-threads.x86_64 0:1.87-4.el7                            perl-threads-shared.x86_64 0:1.43-6.el7

替代:
  mariadb-libs.x86_64 1:5.5.68-1.el7

完毕！
```

### 6 启动并查看状态

```shell
[root@ra-local-oa yum.repos.d]# systemctl start mysqld
[root@ra-local-oa yum.repos.d]# systemctl status mysqld
● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: active (running) since 四 2024-04-11 23:16:07 HKT; 10s ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
  Process: 2495 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
 Main PID: 2566 (mysqld)
   Status: "Server is operational"
   CGroup: /system.slice/mysqld.service
           └─2566 /usr/sbin/mysqld

4 月 11 23:16:00 ra-local-oa systemd[1]: Starting MySQL Server...
4 月 11 23:16:07 ra-local-oa systemd[1]: Started MySQL Server.
```

### 7 查看初始密码

```shell
[root@ra-local-oa log]# grep 'temporary password' /var/log/mysqld.log
2024-04-11T15:16:03.034901Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: ipocW3N186>P
```

然后使用初始密码可以登录数据库

```shell
[root@ra-local-oa log]# mysql -uroot -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 8.0.36

Copyright (c) 2000, 2024, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

### 8 修改密码

#### 先更新密码

现在 mysql8 的策略进行了调整，操作数据库之前，必须先要重置密码

```sql
mysql> show databases;
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
mysql> SHOW VARIABLES LIKE 'validate_password%';
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'dhzy2021#';
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements

所以必须把密码设置严格一些

mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'Grewall3558#';
Query OK, 0 rows affected (0.01 sec)
```

根据报错信息，知道目前的密码必须大小写数字加上特殊字符

然后在调整密码设置策略和设置更简洁的密码

#### 重新设置简洁的密码

```sql
mysql> SHOW VARIABLES LIKE 'validate_password%';
+-------------------------------------------------+--------+
| Variable_name                                   | Value  |
+-------------------------------------------------+--------+
| validate_password.changed_characters_percentage | 0      |
| validate_password.check_user_name               | ON     |
| validate_password.dictionary_file               |        |
| validate_password.length                        | 8      |
| validate_password.mixed_case_count              | 1      |
| validate_password.number_count                  | 1      |
| validate_password.policy                        | MEDIUM |
| validate_password.special_char_count            | 1      |
+-------------------------------------------------+--------+
8 rows in set (0.01 sec)

mysql> set global validate_password.check_user_name=OFF;
Query OK, 0 rows affected (0.00 sec)

mysql> set global validate_password.policy=LOW;
Query OK, 0 rows affected (0.00 sec)

mysql> SHOW VARIABLES LIKE 'validate_password%';
+-------------------------------------------------+-------+
| Variable_name                                   | Value |
+-------------------------------------------------+-------+
| validate_password.changed_characters_percentage | 0     |
| validate_password.check_user_name               | OFF   |
| validate_password.dictionary_file               |       |
| validate_password.length                        | 8     |
| validate_password.mixed_case_count              | 1     |
| validate_password.number_count                  | 1     |
| validate_password.policy                        | LOW   |
| validate_password.special_char_count            | 1     |
+-------------------------------------------------+-------+
8 rows in set (0.00 sec)

mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
mysql> set global validate_password.length=1;
Query OK, 0 rows affected (0.00 sec)

mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';
Query OK, 0 rows affected (0.01 sec)

mysql>
```

### 9 数据库操作

例如创建数据库

```shell
mysql> CREATE DATABASE raoa CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
Query OK, 1 row affected (0.00 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| raoa               |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

mysql>
```
