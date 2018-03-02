---
title: maven
date: 2017-09-01 09:33:36
tags:
---

### 概述

* maven是基于项目对象模型（POM），可以通过一小段信息来管理项目的构建、报告和文档的软件项目管理工具。

``` bash
bin目录包含mvn的运行脚本
boot目录包含一个类加载器的框架,maven使用该目录下的jar包来加载自己的类库
conf是配置文件的目录
lib目录下包含maven所有的类库和第三方类库
```

### maven常用命令

``` bash
mvn:
-v 查看maven版本
compile 编译
test 测试
package 打包
clean 删除target
install 安装jar包到本地仓库中
```

### maven 基础知识

``` bash
目录骨架：
1.mvn archetype:generate 按照提示进行选择
2.mvn archetype:generate -DgroupId=组织名，公司网址的反写+项目名
-DartifactId=项目名-模块名
-Dversion=版本号
-Dpackage=代码所存在的包

maven生命周期：
1.clean 清理项目
pre-clean 执行清理前的工作
clean 清理上一次构建生成的所有文件
post-clean 执行清理后的文件

2.default 构建项目(核心)
compile,test,package,install
运行package命令时，会依次执行compile,test,package。
运行test命令时,会依次执行compile,test。

3.site 生成项目站点
pre-site 在生成项目站点前要完成的工作
site 生成项目的站点文档
post-site 在生成项目站点后要完成的工作
site-deploy 发布生成的站点到服务器上
```

### pom.xml常用元素介绍

``` bash
1.project:根元素，包含了pom的约束信息
2.modelVersion：固定版本，指定了当前pom的版本
3.坐标的信息：
    groupId<反写的公司网址+项目名>
    artifactId<项目名+模块名>
    version<大版本号.分支版本号.小版本号> (snapshot快照，alpha内部测试，beta公测，release稳定，GA正式发布)
    packaging<打包方式：默认是jar，还有war,zip,pom>
4.name: 项目描述名
5.url： 项目的地址
6.description：项目的描述
7.developers：开发人员列表
8.licenses：许可证信息（开源框架）
9.organization：组织信息
10.依赖列表：
    <dependencies>
        <dependency>
            <groupId></groupId>
            <artifactId></artifactId>
            <version></version>
            <type></type>
            <scope></scope>
            <!--设置依赖是否可选（false）-->
            <optional></optional>
            <!--排除依赖传递列表-->
            <exclusions>
                <exclusion></exclusion>
            </exclusions>
        </dependency>
    </dependencies>
11.依赖管理(不会被运行)(定义在父模块中，供子模块使用)
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId></groupId>
                <artifactId></artifactId>
                <version></version>
                <type></type>
                <scope></scope>
                <!--设置依赖是否可选（false）-->
                <optional></optional>
                <!--排除依赖传递列表-->
                <exclusions>
                    <exclusion></exclusion>
                </exclusions>
            </dependency>
        </dependencies>
    </dependencyManagement>
12.通常为我们的构建行为提供相应的支持
<build>
    <!--插件列表-->
    <plugins>
        <plugin>
            <groupId></groupId>
            <artifactId></artifactId>
            <version></version>
        </plugin>
    </plugins>
</build>
13.parent:通常用于子模块对父模块的pom的继承
14.用来聚合运行多个maven项
    <modules>
        <module></module>
    </modules>
```