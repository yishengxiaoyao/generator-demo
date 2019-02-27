#MyBatis Generator
MyBatis Generator是用来生成相应的mapper.xml文件、model对象和mapper文件。

## MyBatis Generator XML 文件组成
想要使用MyBatis Generator插件，需要使用在resources或者其他创建一个相应的xml文件，默认名称为generatorConfig.xml。
最简单的xml文件必须包含以下几个标签：
>* jdbcConnection:用来如何连接到目标数据库
>* javaModelGenerator:指定目标目录，在目标目录生成相应的java model对象。
>* sqlMapGenerator: 指定目标目录，在目标目录生成相应的SQL 映射文件。
>* javaClientGenerator(这个可以没有):在目标目录生成相应的接口和类。
>* table:指定相应的表。

在设置generatorConfig.xml文件中的参数时，可以从配置文件中获取，使用<classPathEntry>来设置配置文件的位置，然后使用占位符来获取值。
## 执行MyBatis Generator
### Running MyBatis Generator With Java
需要编写一些代码：
```
List<String> warnings = new ArrayList<>();
ConfigurationParser cp = new ConfigurationParser(warnings);
Configuration config = cp.parseConfiguration(
        this.getClass().getResourceAsStream("/generatorConfig.xml")); //资源文件可以自定义
DefaultShellCallback callback = new DefaultShellCallback(true);
MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
myBatisGenerator.generate(null);
```
### Running MyBatis Generator With Maven
pom.xml文件的配置如下：
```
<build>
    <plugins>
        <plugin>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-maven-plugin</artifactId>
            <version>1.3.7</version>
            <configuration>
                <verbose>true</verbose>
                <configurationFile>src/main/resources/generatorConfig.xml</configurationFile>
            </configuration>
            <executions>
                <execution>
                    <id>Generate MyBatis Artifacts</id>
                    <goals>
                        <goal>generate</goal>
                    </goals>
                </execution>
            </executions>
            <dependencies>
                <dependency>
                    <groupId>mysql</groupId>
                    <artifactId>mysql-connector-java</artifactId>
                    <version>5.1.47</version>
                </dependency>
            </dependencies>
        </plugin>
    </plugins>
</build>
```
在编写完pom.xml文件之后，需要在IDEA的右侧选择plugins选择mybatis-generator下面的mybatis-generator:generator，然后点击运行就可以啦。

在执行生成代码之前，需要在相应的数据库中创建相应的表以及字段。

generatorConfig.xml文件内容如下:
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <classPathEntry location="/Users/renren/.m2/repository/mysql/mysql-connector-java/5.1.47/mysql-connector-java-5.1.47.jar"></classPathEntry>
    <context id="MySQLTables" targetRuntime="MyBatis3">
        <plugin type="org.mybatis.generator.plugins.FluentBuilderMethodsPlugin" />
        <plugin type="org.mybatis.generator.plugins.ToStringPlugin" />
        <plugin type="org.mybatis.generator.plugins.SerializablePlugin" />
        <plugin type="org.mybatis.generator.plugins.RowBoundsPlugin" />

        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/test?useSSL=false"
                        userId="root"
                        password="123456">
        </jdbcConnection>

        <javaModelGenerator targetPackage="com.edu.spring.mybatis.model"
                            targetProject="./src/main/java">
            <property name="enableSubPackages" value="true" />
            <property name="trimStrings" value="true" />
        </javaModelGenerator>

        <sqlMapGenerator targetPackage="com.edu.spring.mybatis.mapper"
                         targetProject="./src/main/resources/mapper">
            <property name="enableSubPackages" value="true" />
        </sqlMapGenerator>

        <javaClientGenerator type="MIXEDMAPPER"
                             targetPackage="com.edu.spring.mybatis.mapper"
                             targetProject="./src/main/java">
            <property name="enableSubPackages" value="true" />
        </javaClientGenerator>

        <table tableName="t_coffee" domainObjectName="Coffee" schema="test" >
            <generatedKey column="id" sqlStatement="CALL IDENTITY()" identity="true" />
            <columnOverride column="price" javaType="org.joda.money.Money" jdbcType="BIGINT"
                            typeHandler="com.edu.spring.mybatis.handler.MoneyTypeHandler"/>
        </table>
    </context>
</generatorConfiguration>
```
更多详细内容，请操作mybatis-generator官方文档[MyBatis Generator](http://www.mybatis.org/generator/index.html)
相关代码: