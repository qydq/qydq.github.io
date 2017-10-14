---
title:  "Android中Gradle自动构建总结"
date:   2016-09-13 14:30
categories: _posts
---

迁移源地址为：<a href="http://bgwan.blog.163.com/blog/static/23930101620168137540684/">《Android中Gradle自动构建总结》</a>

Begin

android中都是使用的gradle，那么如果你如果又写前端，又写后台，那我觉得使用的版本项目构建和管理的工具还是应该统一，倦了maven的繁杂，试玩一下传说中的下一代构建工具。

Gradle和Maven都是项目自动构建工具，编译源代码只是整个过程的一个方面，更重要的是，你要把你的软件发布到生产环境中来产生商业价值，所以，你要运行测试，构建分布、分析代码质量、甚至为不同目标环境提供不同版本，然后部署。整个过程进行自动化操作是很有必要的。
整个过程可以分成以下几个步骤：

编译源代码
运行单元测试和集成测试
执行静态代码分析、生成分析报告
创建发布版本
部署到目标环境
部署传递过程
执行冒烟测试和自动功能测试
如果你手工去执行每一个步骤无疑效率比较低而且容易出错，有了自动化构建你只需要自定义你的构建逻辑，剩下的事情交给工具去完成。


作者：EZLippi
链接：https://www.zhihu.com/question/29338218/answer/51293828
来源：知乎
著作权归作者所有，转载请联系作者获得授权。
eclipse中pom.xml的配置文件》》》
	<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>${slf4j-version}</version>
		</dependency> -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>3.1.0</version>
		</dependency>
		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-lang3</artifactId>
			<version>3.3</version>
		</dependency>
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>fastjson</artifactId>
			<version>1.2.1</version>
		</dependency>
		<!-- 数据源配置 -->
		<dependency>
			<groupId>com.mchange</groupId>
			<artifactId>c3p0</artifactId>
			<version>0.9.5.1</version>
		</dependency>
		<!--数据库相关, mysql, mybatis -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.37</version>
		</dependency>

然后我将其转换成Gradle脚本，结果是惊人的：

dependencies {
    compile('org.springframework:spring-core:2.5.6')
    compile('org.springframework:spring-beans:2.5.6')
    compile('org.springframework:spring-context:2.5.6')
    compile('com.google.code.kaptcha:kaptcha:2.3:jdk15')
    testCompile('junit:junit:4.7')
}
注意配置从原来的28行缩减至7行！这还不算我省略的一些父POM配置。依赖的groupId、artifactId、 version，scope甚至是classfier，一点都不少。较之于Maven或者Ant的XML配置脚本，Gradle使用的Grovvy脚本杀伤力太大了，爱美之心，人皆有之，相比于七旬老妇松松垮垮的皱纹，大家肯定都喜欢少女紧致的脸蛋，XML就是那老妇的皱纹。

Gradle给我最大的有点是两点。其一是简洁，基于Groovy的紧凑脚本实在让人爱不释手，在表述意图方面也没有什么不清晰的地方。其二是灵活，各种在Maven中难以下手的事情，在Gradle就是小菜一碟，比如修改现有的构建生命周期，几行配置就完成了，同样的事情，在Maven中你必须编写一个插件，那对于一个刚入门的用户来说，没个一两天几乎是不可能完成的任务。

、所以这个是我逐渐开始摒弃maven的一个原因。但也不是maven没有用。
maven管理的jcenter的中心库的代码，其实对于android开发来说还是比较值得探究的。

end