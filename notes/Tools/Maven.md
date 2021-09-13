* [Maven](#maven)
* [maven作用](#maven作用)
* [maven使用方式](#maven使用方式)
* [maven的安装](#maven的安装)
* [maven的核心概念](#maven的核心概念)
    * [约定的目录结构](#约定的目录结构)
    * [坐标](#坐标)
    * [依赖](#依赖)
    * [仓库](#仓库)
    * [maven的命令](#maven的命令)
    * [maven的声明周期和插件](#maven的声明周期和插件)
* [依赖范围](#依赖范围)

# Maven

maven:是一个项目的构建工具

# maven作用

1.管理依赖：jar管理、下载、版本

2.构建项目，完成代码编译、测试、打包、部署

# maven使用方式

1.独立使用maven:使用maven的各种命令，完成代码的编译、测试、打包等。

2.结合开发工具使用，一般在idea中使用maven：简单，快捷，不需要记命令。

# maven的安装

1.获取安装包，zip文件

2.解压缩文件，到一个目录，非中文目录

3.配置环境变量，M2_HOME,他的值是maven的安装目录

4.在path中加入%M2_HOME%\bin

5.测试maven的安装，使用mvn -v，查看maven的版本信息

# maven的核心概念

## 1.约定的目录结构

> Hello
>
> ----|src
>
> ----|----|java主程序java文件
>
> ----|----|resources配置文件
>
> ----|pow.xml
>
> ----|----|java测试程序java代码
>
> ----|----|resources测试使用的配置文件

## 2.坐标(gav)

1. groupId:组织编码，域名倒写
2. artifactId:项目名称
3. version:自定义版本号

## 3.依赖（dependency）：maven管理依赖

使用依赖把jar导入到你的项目中

## 4.仓库

* 本地仓库，可以在maven安装目录/conf/setting.xml指定，<localRepository>非中文路径</localRepository>不要有空格
* 中央仓库：最权威的，所有的资源都在这里
* 中央仓库的镜像：分担压力的
* 私服：公司的局域网内部使用的

## 5.maven的命令

maven通过命令完成项目的构建

> mvn clean：清理
>
> mvn compile:编译src/main/java目录中的程序，把java编译为class文件，并放到target/classes目录中；同时会拷贝到target/classes目录中
>
> mvn test-compile:编译src/main/test目录下的java程序，拷贝到target/test-class目录中
>
> mvn test:可以进行单元测试，使用junit测试src/main/java目录中的程序是否符合要求
>
> mvn package打包:把程序中src/main/下面的java编译后的class和resources中的配置文件放入到一个压缩文件中
>
> mvn install:把jar,war安装到本机的仓库中

## 6.maven的生命周期和插件

生命周期：项目的构建过程 清理，编译，测试，报告，打包，安装，部署

插件用来清理，编译，测试，报告，打包程序

# 依赖范围

1.compile:默认，在构建的编译，测试，打包，部署需要

2.test：只在测试阶段需要

3.provided:在部署时，有服务器提供，项目本身不需要自带