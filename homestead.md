# Laravel Homestead

- [介绍](#introduction)
- [内建软件](#included-software)
- [安装与设定](#installation-and-setup)
- [日常使用](#daily-usage)
- [端口](#ports)

<a name="introduction"></a>
## 介绍

Laravel 致力于让 PHP 开发体验更愉快，也包含您的本地开发环境。[vagrant](http://vagrantup.com) 一个简单、优雅的方式来管理与供应虚拟机器。

Laravel Homestead 是一个官方预载的 Vagrant “封装包”，提供您一个完美的开发环境，不需要您在您的本机端安装 PHP、网页服务器或任何服务器软件。不用担心搞乱您的系统！Vagrant 封装包完全搞定。如果有什么地方损坏了，您只要删掉重来即可。

Homestead 可以在任何 Windows, Mac 或 Linux 上面运行，里面包含了 Nginx 网页服务器、PHP 5.5、MySQL、Postgres、Redis、Memcached 还有所有您开发 Laravel 应用程序所需的软件。

Homestead 创建且测试于 Vagrant 1.6 上。

<a name="included-software"></a>
## 内建软件

- Ubuntu 14.04
- PHP 5.5
- Nginx
- MySQL
- Postgres
- Node (With Bower, Grunt, and Gulp)
- Redis
- Memcached
- Beanstalkd
- [Laravel Envoy](/docs/ssh#envoy-task-runner)
- Fabric + HipChat Extension

<a name="installation-and-setup"></a>
## 安装与设定

### 安装 VirtualBox 与 Vagrant

在启动您的 Homestead 环境之前，您必须先安装 [VirtualBox](https://www.virtualbox.org/wiki/Downloads) 和 [Vagrant](http://www.vagrantup.com/downloads.html). 两套软件在各平台都有提供易用的视觉化安装软件。

### 增加 Vagrant 封装包

当 VirtualBox 和 Vagrant 安装完成后，您可以在终端机以下列命令将 'laravel/homestead' 封装包安装进您的 Vagrant 安装软件中。下载封装包会花您一点时间，看您的网路速度决定：

	vagrant box add laravel/homestead

### 复制 Homestead 软件源

一旦封装包已经被加进您的 Vagrant 安装软件后，您必须克隆置于一个集中的 `Homestead` 目录中，目录中放置所有您的 Laravel 项目，如此 homestead 封装包就可以运行您所有的 Laravel（和 PHP）项目。

	git clone https://github.com/laravel/homestead.git Homestead

### 设定您的 SSH 金钥

使用钱需要编辑下 `Homestead.yaml` 文件。您可以在文件中设定您的 SSH 公开金钥，以及您主要机器与 Homestead 虚拟机器之间的共享目录。

您没有 SSH 金钥? 在 Mac 和 Linux 下，您可以利用下面的命令来创建一个 SSH 金钥组:

	ssh-keygen -t rsa -C "your@email.com"

在 Windows 下，您需要安装 [Git](http://git-scm.com/) 并且使用包含在 Git 里的 `Git Bash` 来执行上述的命令。另外您也可以使用 [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) 和 [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)。

一旦您创建了一个 SSH 金钥，记得在您的 `Homestead.yaml` 文件中的 `authorize` 属性指明金钥路径。

### 设定您的共享文件夹

`Homestead.yaml` 文件中的 `folders` 属性列出所有您想跟您的 Homestead 环境共享的文件夹列表。这些文件夹中的文件若有更动，他们将会同步在您的本机与 Homestead 环境里。您可以将您需要的共享文件夹都设定进去。

### 设定您的 Nginx 站点

对 Nginx 不熟悉? 没关系。`sites` 属性允许您简单的对应一个 `网域` 到一个您 homestead 环境中的目录。一个例子的站点设定被在 `Homestead.yaml` 文件中。同样的，您可以加任何您需要的站点到您的 Homestead 环境中。Homestead 可以作为您进行中项目的一个方便虚拟化环境。

### Bash Aliases

如果要增加 Bash aliases 到您的 Homestead 封装包中，只要加到 Homestead 目录根目录下的 `aliases` 文件中。

### 启动 Vagrant 封装包

当您根据您的情况编辑完 `Homestead.yaml` 后，在终端机里，从 Homestead 目录里执行 `vagrant up` 命令。Vagrant 将会将虚拟机器开机，并且自动设定您的共享目录和 Nginx 站点。

为了能访问您的 Nginx 站点，别忘记在本机的 `hosts` 文件中将"网域"加进去。`hosts` 文件会将您的本地网域的站点请求重定向跳转至您的 Homestead 环境中。在 Mac 和 Linux，该文件放在 `/etc/hosts`。在 Windows 环境中，它被放置在 `C:\Windows\System32\drivers\etc\hosts`。您要加进去的内容类似如下：

	127.0.0.1  homestead.app

一旦您将网域加进您的 `hosts` 文件中，您本机可以从端口 8000 通过您的浏览器获取到您的站点。

	http://homestead.app:8000

继续读下去，您会学到如何链接到您的数据库。

<a name="daily-usage"></a>
## 日常使用

### 通过 SSH 连接

要通过 SSH 连接您的 Homestead 环境，您需要使用您设定在 `Homestead.yaml` 文件中的 SSH 金钥连接 `127.0.0.1` 的端口 2222。您也可以轻易的从您的 `Homestead` 目录下执行 `vagrant ssh` 命令来连接。

如果您想要更简便，您可以将下面的 alias 加到您的 `~/.bash_aliases` 或是 `~/.bash_profile` 中：

	alias vm='ssh vagrant@127.0.0.1 -p 2222'

### 链接您的数据库

在 `Homestead` 封装包中，MySQL 与 Postgres 两套数据库都已预装其中。为了更简便，Laravel 的 `local` 数据库设定已经默认将其设定完成。

如果您想要从您的本机上通过 Navicat 或者是 Sequel Pro 连接您的 MySQL 或者 Postgres 数据库，您可以连接 `127.0.0.1` 的 端口 33060 (MySQL) 或 54320 (Postgres)。而帐号密码分别是 `homestead` / `secret`。

> **附注:** 您应该使用这些非标准的端口从外部来链接到虚拟机里的数据库, 而在虚拟机里面使用默认的端口连接本地的服务, 如 3306 and 5432 .

### 增加更多的站点

一旦您的 Homestead 环境上架且运行后，您可能会需要为您的 Laravel 应用程序增加够多的 Nginx 站点。您可以在单一个 Homestead 环境中运行非常多 Laravel 安装软件。两个方式可以达到：第一，您可以在您的 `Homestead.yaml` 文件中增加站点然后执行 `vagrant provision`。

另外，您也可以使用放在您的 Homestead 环境中的 `serve` 命令。您需要 SSH 进入您的 Homestead 环境中，并执行下列命令：

	serve domain.app /home/vagrant/Code/path/to/public/directory

> **附注:** 在执行 `serve` 命令过后，别忘记将新的站点加进您本机的 `hosts` 文件中。

<a name="ports"></a>
## 连接端口

以下的本地端口将会被重定向跳转至您的 Homestead 环境：

- **SSH:** 2222 -> Forwards To 22
- **HTTP:** 8000 -> Forwards To 80
- **MySQL:** 33060 -> Forwards To 3306
- **Postgres:** 54320 -> Forwards To 5432