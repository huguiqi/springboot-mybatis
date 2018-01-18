# spring-boot-mybatis

## spring boot + Mybatis分页插件-PageHelper的使用

[参考作者源码地址](https://gitee.com/free/Mybatis_PageHelper)
[PageHelper插件github地址](https://github.com/mybatis-book/book)
[用法](https://github.com/pagehelper/Mybatis-PageHelper/blob/master/wikis/zh/HowToUse.md)

本项目有分页的六种写法，其中参数方式和参数对象方式，需要在spring的配置文件中配置：
    
    pagehelper.params=pageNum=pageNum;pageSize=pageSize;
    pagehelper.support-methods-arguments=true
    


## 使用MyBatis Generator插件

[官方使用文档](http://mbg.cndocs.ml/quickstart.html)

[配置详细说明](http://blog.csdn.net/isea533/article/details/42102297)

[配置介绍](http://blog.csdn.net/isea533/article/details/42102297)



pom.xml:

    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
    
        <groupId>com.example</groupId>
        <artifactId>springboot-mybatis</artifactId>
        <version>0.0.1-SNAPSHOT</version>
        <packaging>jar</packaging>
    
        <name>springboot-mybatis</name>
        <description>Demo project for Spring Boot</description>
    
        <parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>1.5.6.RELEASE</version>
            <relativePath/> <!-- lookup parent from repository -->
        </parent>
    
        <properties>
            <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
            <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
            <java.version>1.8</java.version>
            <oracle.version>11.2.0.3</oracle.version>
            <mybatis-generator-core.version>1.3.6</mybatis-generator-core.version>
        </properties>
    
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-aop</artifactId>
            </dependency>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
            </dependency>
    
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>1.3.0</version>
            </dependency>
    
            <dependency>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-core</artifactId>
                <version>${mybatis-generator-core.version}</version>
            </dependency>
    
            <dependency>
                <groupId>com.github.pagehelper</groupId>
                <artifactId>pagehelper-spring-boot-starter</artifactId>
                <version>1.2.3</version>
            </dependency>
    
            <dependency>
                <groupId>com.h2database</groupId>
                <artifactId>h2</artifactId>
                <scope>runtime</scope>
            </dependency>
    
            <!-- Use Oracle -->
            <dependency>
                <groupId>com.oracle</groupId>
                <artifactId>ojdbc6</artifactId>
                <version>${oracle.version}</version>
            </dependency>
    
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <optional>true</optional>
            </dependency>
    
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>fastjson</artifactId>
                <version>1.2.30</version>
            </dependency>
    
            <dependency>
                <groupId>commons-beanutils</groupId>
                <artifactId>commons-beanutils</artifactId>
                <version>1.9.2</version>
            </dependency>
    
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-test</artifactId>
                <scope>test</scope>
            </dependency>
    
    
    
        </dependencies>
    
        <build>
            <plugins>
                <plugin>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-maven-plugin</artifactId>
                </plugin>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-surefire-plugin</artifactId>
                    <version>2.18.1</version>
                    <configuration>
                        <skipTests>true</skipTests> 
                    </configuration>
                </plugin>
                <plugin>
                    <groupId>org.mybatis.generator</groupId>
                    <artifactId>mybatis-generator-maven-plugin</artifactId>
                    <version>1.3.6</version>
                    <configuration>
                    					<configurationFile>${basedir}/src/main/resources/plugin/generatorConfig.xml</configurationFile>
                    					<contexts>OracleTables</contexts>
                    					<overwrite>true</overwrite>
                    					<verbose>true</verbose>
                    				</configuration>
                </plugin>
            </plugins>
        </build>
    
    </project>



## 在resources目录下添加generatorConfig.xml

generatorConfig.xml:


    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE generatorConfiguration
            PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
            "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
    
    <generatorConfiguration>
        <classPathEntry
                location="C:\Users\guiqi\.m2\repository\com\oracle\ojdbc6\11.2.0.3\ojdbc6-11.2.0.3.jar"/>
    
        <context id="OracleTables" targetRuntime="MyBatis3Simple" defaultModelType="flat">
    
            <jdbcConnection driverClass="oracle.jdbc.driver.OracleDriver"
                            connectionURL="jdbc:oracle:thin:@//localhost:1521/dbTest" userId="test"
                            password="test">
                <property name="remarksReporting" value="true"></property>
            </jdbcConnection>
    
            <javaTypeResolver >
                <property name="forceBigDecimals" value="false" />
            </javaTypeResolver>
    
            <javaModelGenerator targetPackage="com.example.demo.bean" targetProject="src/main/java">
                <property name="enableSubPackages" value="true" />
                <property name="trimStrings" value="true" />
            </javaModelGenerator>
    
            <sqlMapGenerator targetPackage="com.example.demo.mapper"  targetProject="src/main/resources">
                <property name="enableSubPackages" value="true" />
            </sqlMapGenerator>
    
            <javaClientGenerator type="XMLMAPPER" targetPackage="com.example.demo.mapper"  targetProject="src/main/java">
                <property name="enableSubPackages" value="true" />
            </javaClientGenerator>
    
            <table tableName="T_USER" domainObjectName="User">
                <generatedKey column="id" sqlStatement="select SEQ_USER.nextval from dual"/>
            </table>
    
        </context>
    </generatorConfiguration>
    

## 运行MybatisGenerator生成mapper和model类

命令行执行：
    
    mvn mybatis-generator:generate
    
就会生成mapper和model类.

如果你想把MBG绑定在mvn install生命周期里，则只需要在插件后面加上`execution`属性：

    <plugin>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-maven-plugin</artifactId>
    <version>1.3.6</version>
    <executions>
        <execution>
            <id>Generate MyBatis Artifacts</id>
            <goals>
                <goal>generate</goal>
            </goals>
        </execution>
    </executions>
    </plugin>


如果你的generatorConfig.xml不在resource目录下，而是在resource下新建了一个`plugin`目录，则如下配置：

    <plugin>
        <groupId>org.mybatis.generator</groupId>
        <artifactId>mybatis-generator-maven-plugin</artifactId>
        <version>1.3.6</version>
        <executions>
            <execution>
                <id>Generate MyBatis Artifacts</id>
                <goals>
                    <goal>generate</goal>
                </goals>
            </execution>
        </executions>
        <configuration>
            <configurationFile>${basedir}/src/main/resources/plugin/generatorConfig.xml</configurationFile>
            <contexts>OracleTables</contexts>
            <overwrite>true</overwrite>
            <verbose>true</verbose>
        </configuration>
    </plugin>
    
指定generatorConfig.xml的路径。
    
如果不想通过mvn运行，则可以在项目中手动执行：

新建一个执行类：

    package com.example.demo.common;
    
    /**
     * Created by guiqi on 2018/1/18.
     */
    
    import java.io.IOException;
    import java.io.InputStream;
    import java.net.URISyntaxException;
    import java.sql.SQLException;
    import java.util.ArrayList;
    import java.util.List;
    
    import org.mybatis.generator.api.MyBatisGenerator;
    import org.mybatis.generator.config.Configuration;
    import org.mybatis.generator.config.xml.ConfigurationParser;
    import org.mybatis.generator.exception.InvalidConfigurationException;
    import org.mybatis.generator.exception.XMLParserException;
    import org.mybatis.generator.internal.DefaultShellCallback;
    
    public class MybatisGeneratorStartUp {
        public static void main(String[] args)
                throws URISyntaxException {
            try {
                List<String> warnings = new ArrayList();
                boolean overwrite = true;
                ClassLoader classloader = Thread.currentThread().getContextClassLoader();
                InputStream is = classloader.getResourceAsStream("plugin/generatorConfig.xml");
                ConfigurationParser cp = new ConfigurationParser(warnings);
                Configuration config = cp.parseConfiguration(is);
                DefaultShellCallback callback = new DefaultShellCallback(overwrite);
                MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
                myBatisGenerator.generate(null);
            } catch (SQLException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            } catch (InterruptedException e) {
                e.printStackTrace();
            } catch (InvalidConfigurationException e) {
                e.printStackTrace();
            } catch (XMLParserException e) {
                e.printStackTrace();
            }
        }
    }


run就行了。。。。。。。


## 生成中文注释

对字段注释配置需要加：

    <commentGenerator>
        <!--是否取消注释-->
        <property name="suppressAllComments" value="true"/>
        <!-- 是否生成注释代时间戳 -->
        <property name="suppressDate" value="true"/>
    </commentGenerator>
    
默认的实现类是：`DefaultCommentGenerator`    

默认的这个实现类是不支持数据表字段的注释描述的，需要我们重写去实现，重写其实很简单，实现`CommentGenerator`接口，就可以了，为了方便，`我就直接继承DefaultCommentGenerator`这个默认注释实现类了。

只需重写 
`void addFieldComment(Field var1, IntrospectedTable var2, IntrospectedColumn var3);`

这个方法，将列对象中的remark取出，然后再调用`addJavaDocLine`方法，就可以将注释加到字段上。

MyCommentGenerator:

    package com.example.demo.common;
    
    import org.mybatis.generator.api.IntrospectedColumn;
    import org.mybatis.generator.api.IntrospectedTable;
    import org.mybatis.generator.api.dom.java.Field;
    import org.mybatis.generator.internal.DefaultCommentGenerator;
    
    public class MyCommentGenerator extends DefaultCommentGenerator {
        public MyCommentGenerator() {
            super();
        }
    
        @Override
        public void addFieldComment(Field field, IntrospectedTable introspectedTable, IntrospectedColumn introspectedColumn) {
            if (introspectedColumn.getRemarks() != null && !introspectedColumn.getRemarks().equals("")) {
                field.addJavaDocLine("/**");
                field.addJavaDocLine(" * " + introspectedColumn.getRemarks());
                addJavadocTag(field, false);
                field.addJavaDocLine(" */");
            }
        }
    
    }


然后在generatorConfig.xml中配置：

    <commentGenerator type="com.example.demo.common.MyCommentGenerator">
                <!--是否取消注释-->
                <property name="suppressAllComments" value="true"/>
                <!-- 是否生成注释代时间戳 -->
                <property name="suppressDate" value="false"/>
    </commentGenerator>
    
**PS:**

**`commentGenerator` 标签一定要按顺序放置，元素类型为 "context" 的内容必须匹配 `(property*,plugin*,commentGenerator?,(connectionFactory|jdbcConnection),javaTypeResolver?,javaModelGenerator,sqlMapGenerator?,javaClientGenerator?,table+)`，也就是说，在前面没有property和plugin标签时，commentGenerator标签要放在首位。**

