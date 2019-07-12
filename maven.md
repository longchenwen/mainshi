# maven的学习
## 1.mvn clean compile:
	    1.1 clean告诉maven清理输出目录target/
	    1.2 compile告诉maven编译项目主代码

2.mvn clean package:打包

3.mvn clean install:让maven项目直接引用jar,这样安装到本地仓库了(执行install 之前会先执行package)

4.maven 属性:(maven内置属性)
	4.1 内置属性: ${basedir} 表示项目根目录 和 ${version} 表示项目版本
	4.2 POM属性: 例子->${project.artifactId} 就是对应<project><artifactId>元素的属性,一下是常用属性:
		4.2.1 ${project.build.sourceDirectory}: 项目的主源码目录,默认为src/main/java
