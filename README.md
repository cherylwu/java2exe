# jar包打包成exe示例

## 说明
针对基于maven的Java项目，通常会打包成jar，
如果要把jar文件包装成exe文件，仅需要在pom.xml配置文件中增加一个插件即可

这里我们用[launch4j](http://launch4j.sourceforge.net/docs.html)

## 环境
- jdk1.8
- maven 3.5.2

## Main方法

逻辑很简单，就是如果命令行有参数，则打印命令行的第一个参数，
无参数则打印“Hell World!”。

```java
package com.hui;

/**
 * Hello world!
 */
public class App {
    public static void main(String[] args) {
        if (null != args && args.length > 0) {
            System.out.println(args[0]);
        } else {
            System.out.println("Hello World!");
        }
    }
}

```

## pom.xml 示例

仅需要在build环节加入以下插件

```xml
<build>
        <plugins>
            <plugin>
                <groupId>com.akathist.maven.plugins.launch4j</groupId>
                <artifactId>launch4j-maven-plugin</artifactId>
                <executions>
                    <execution>
                        <id>l4j-clui</id>
                        <phase>package</phase>
                        <goals><goal>launch4j</goal></goals>
                        <configuration>
                            <headerType>console</headerType>
                            <outfile>target/encc.exe</outfile>
                            <jar>target/java2exe-1.0-SNAPSHOT.jar</jar>
                            <errTitle>encc</errTitle>
                            <classPath>
                                <mainClass>com.hui.App</mainClass>
                                <addDependencies>false</addDependencies>
                                <preCp>anything</preCp>
                            </classPath>
                            <jre>
                                <minVersion>1.5.0</minVersion>
                            </jre>
                            <versionInfo>
                                <fileVersion>1.2.3.4</fileVersion>
                                <txtFileVersion>txt file version?</txtFileVersion>
                                <fileDescription>a description</fileDescription>
                                <copyright>my copyright</copyright>
                                <productVersion>4.3.2.1</productVersion>
                                <txtProductVersion>txt product version</txtProductVersion>
                                <productName>E-N-C-C</productName>
                                <internalName>ccne</internalName>
                                <originalFilename>original.exe</originalFilename>
                            </versionInfo>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <artifactId>maven-assembly-plugin</artifactId>
                <executions>
                    <execution>
                        <id>assembly</id>
                        <phase>package</phase>
                        <goals><goal>single</goal></goals>
                        <configuration>
                            <descriptors>
                                <descriptor>assembly.xml</descriptor>
                            </descriptors>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```

同时在项目根目录新建一个assembly.xml文件

```xml
<assembly xmlns="http://maven.apache.org/plugins/maven-assembly-plugin/assembly/1.1.2"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/plugins/maven-assembly-plugin/assembly/1.1.2 http://maven.apache.org/xsd/assembly-1.1.2.xsd">
    <id>cdc-upgrade</id>
    <formats>
        <format>zip</format>
    </formats>
    <includeBaseDirectory>false</includeBaseDirectory>
    <fileSets>
        <fileSet>
            <directory>${project.build.directory}</directory>
            <outputDirectory>/</outputDirectory>
            <includes>
                <include>*.exe</include>
            </includes>
        </fileSet>
    </fileSets>
</assembly>
```

## 打包
执行
```cmd
mvn clean package 
```
打包以后，可以看到项目target目录下生成了 encc.exe这个文件

在encc.exe所在的目录下执行

```cmd
encc aaa
```

输出结果
```cmd
aaa
```
![](https://images2018.cnblogs.com/blog/682120/201803/682120-20180325002209219-1997595033.png)



## 完整源码
[代码](https://github.com/cherylwu/java2exe)


## 参考文档
[Maven Launch4j Plugin](https://github.com/lukaszlenart/launch4j-maven-plugin/blob/master/src/main/resources/README.adoc)





