# CentOS7编译安装OpenSSL3.2  

## 下载Openssl源码包  

```
[root@localhost ~]# wget https://www.openssl.org/source/openssl-3.2.1.tar.gz
```

## 解压安装  

```
[root@localhost ~]# tar -xvzf openssl-3.2.1.tar.gz
[root@localhost ~]# cd openssl-3.2.1/
[root@localhost openssl-3.1.0]# ./config --prefix=/usr/local/openssl3.2
```  

** 如果报错为: 缺少IPC/Cmd.pm模块  

```
[root@localhost openssl-3.1.0]# ./config  --prefix=/usr/local/openssl
Can't locate IPC/Cmd.pm in @INC (@INC contains: /root/Downloads/openssl-3.1.0/util/perl /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 . /root/Downloads/openssl-3.1.0/external/perl/Text-Template-1.56/lib) at /root/Downloads/openssl-3.1.0/util/perl/OpenSSL/config.pm line 18.
BEGIN failed--compilation aborted at /root/Downloads/openssl-3.1.0/util/perl/OpenSSL/config.pm line 18.
Compilation failed in require at /root/Downloads/openssl-3.1.0/Configure line 23.
BEGIN failed--compilation aborted at /root/Downloads/openssl-3.1.0/Configure line 23.
```  

** 解决方法：  

```
yum install -y perl-CPAN
perl -MCPAN -e shell
cpan[1]> install IPC/Cmd.pm 
```

** 具体步骤为下：  

```
[root@localhost openssl-3.1.0]# yum install -y perl-CPAN
[root@localhost openssl-3.1.0]# perl -MCPAN -e shell
CPAN.pm requires configuration, but most of it can be done automatically.
If you answer 'no' below, you will enter an interactive dialog for each
configuration option instead.

Would you like to configure as much as possible automatically? [yes] yes

 

Warning: You do not have write permission for Perl library directories.

To install modules, you need to configure a local Perl library directory or
escalate your privileges.  CPAN can help you by bootstrapping the local::lib
module or by configuring itself to use 'sudo' (if available).  You may also
resolve this problem manually if you need to customize your setup.

What approach do you want?  (Choose 'local::lib', 'sudo' or 'manual')
```  

安装缺省的包：  
```
 cpan[1]> install IPC/Cmd.pm 
..............................................................DONE
Fetching with HTTP::Tiny:
http://www.cpan.org/modules/03modlist.data.gz
Reading '/root/.cpan/sources/modules/03modlist.data.gz'
DONE
Writing /root/.cpan/Metadata
Running install for module 'IPC::Cmd'
Running make for B/BI/BINGOS/IPC-Cmd-1.04.tar.gz
Fetching with HTTP::Tiny:
http://www.cpan.org/authors/id/B/BI/BINGOS/IPC-Cmd-1.04.tar.gz
Fetching with HTTP::Tiny:
http://www.cpan.org/authors/id/B/BI/BINGOS/CHECKSUMS
Checksum for /root/.cpan/sources/authors/id/B/BI/BINGOS/IPC-Cmd-1.04.tar.gz ok
Scanning cache /root/.cpan/build for sizes
DONE

  CPAN.pm: Building B/BI/BINGOS/IPC-Cmd-1.04.tar.gz

Checking if your kit is complete...
Looks good
Warning: prerequisite Locale::Maketext::Simple 0 not found.
Warning: prerequisite Module::Load::Conditional 0.66 not found.
Warning: prerequisite Params::Check 0.20 not found.
Warning: prerequisite Test::More 0 not found.
Writing Makefile for IPC::Cmd
Could not read metadata file. Falling back to other methods to determine prerequisites
---- Unsatisfied dependencies detected during ----
----        BINGOS/IPC-Cmd-1.04.tar.gz        ----
    Test::More [requires]
    Locale::Maketext::Simple [requires]
    Module::Load::Conditional [requires]
    Params::Check [requires]
```  

安装完成后， 执行exit退出  

## 重新编译配置：  

```
[root@localhost openssl-3.1.0]# ./config  --prefix=/usr/local/openssl3.2
[root@localhost openssl-3.1.0]# make -j 4
[root@localhost openssl-3.1.0]# make install
```  

libssl.so.3文件在/usr/local/openssl3.2/lib64目录下面，需要配置到共享库中：  

```
[root@localhost ~]# vim /etc/ld.so.conf
include ld.so.conf.d/*.conf
/usr/local/openssl3.2/lib64
```  

加载生效：  
`[root@localhost ~]# ldconfig`  

做软连接：  

```
[root@localhost ~]# ln -s /usr/local/openssl3.2/bin/openssl /usr/bin/openssl3.2
[root@localhost ~]# ln -s /usr/local/openssl3.2/include/openssl/  /usr/include/openssl3.2
```  

查看Openssl版本：  

```
[root@localhost ~]# openssl3.2 version
OpenSSL 3.2.1 30 Jan 2024 (Library: OpenSSL 3.2.1 30 Jan 2024)
```  


REF: https://www.cnblogs.com/haoee/p/17391596.html  


