---
title: maven插件之execution
date: 2019-11-06 17:00:25
tags:
---

参考文档：[Guide to Configuring Plug-ins#Using the \<executions\> Tag](https://maven.apache.org/guides/mini/guide-configuring-plugins.html#Using_the_executions_Tag)

`<executions>`标签可以配置多个`<executioin>`标签
`<execution>`标签通常是对于某个mojo的配置。使该mojo可以参与到构建周期(build lifecycle)的某一阶段(phase)。

### 示例配置
```
<project>
  ...
  <build>
    <plugins>
      <plugin>
        <artifactId>maven-myquery-plugin</artifactId>
        <version>1.0</version>
        <executions>
          <execution>
            <id>execution1</id>
            <phase>test</phase>
            <configuration>
              <url>http://www.foo.com/query</url>
              <timeout>10</timeout>
              <options>
                <option>one</option>
                <option>two</option>
                <option>three</option>
              </options>
            </configuration>
            <goals>
              <goal>query</goal>
            </goals>
          </execution>
          <execution>
            <id>execution2</id>
            <configuration>
              <url>http://www.bar.com/query</url>
              <timeout>15</timeout>
              <options>
                <option>four</option>
                <option>five</option>
                <option>six</option>
              </options>
            </configuration>
            <goals>
              <goal>query</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
  ...
</project>
```
### 被动触发
如果`execution`标签的`<phase>`标签有被指定，则会在指定的阶段(phase)执行的时候触发

>Maven的生命周期是抽象的，其实际行为都是由插件来完成，如package阶段的任务可能就会由maven-jar-plugin完成

>Maven的生命周期是抽象的，这意味着生命周期本身不做任何实际的工作，在Maven的设计中，实际的任务（如编译源代码）都交由插件来完成。

>生命周期抽象了构建的各个步骤，定义了它们的次序，但没有提供具体实现。Maven设计了插件机制。每个构建步骤都可以绑定一个或者多个插件行为，而且Maven为大多数构建步骤编写并绑定了默认插件。例如实际上编译是由maven-compiler-plugin完成的，而测试是由maven-surefire-plugin完成的。

当`<packaging>`为`jar`时，在默认的构建生命周期中包含以下目标

| phase | plugin:goal |
|:---|:---|
|  process-resources |resources:resources |
| compile | compiler:compile |
|process-test-resources| resources:testResources|
|test-compile|compiler:testCompile|
|test| surefire:test|
|package| jar:jar|
|install|install:install|
|deploy|deploy:deploy|



### 主动触发
自 maven 3.3.1开始，maven支持通过指定execution id的方式直接指行插件目标
```
mvn myqyeryplugin:queryMojo@execution1
```