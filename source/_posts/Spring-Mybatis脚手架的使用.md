title: Spring+Mybatis脚手架的使用
date: 2015-12-25 22:33:13
tags: Develop
---
### Github地址

> **https://github.com/1994/ssm-scaffold.git**

### 简单说明

这是一个Spring4+Mybatis3的脚手架项目，方便老鸟使用，新手学习。出于个人爱好，我还添加了其他的依赖，以下是全部依赖:
```xml
<properties>
        <junit-version>3.8.1</junit-version>
        <spring-version>4.2.1.RELEASE</spring-version>
        <mybatis-version>3.3.0</mybatis-version>
        <mybatis-spring-version>1.2.3</mybatis-spring-version>
        <druid-version>1.0.15</druid-version>
        <fastjson-version>1.2.7</fastjson-version>
        <mysql-connection-version>5.1.6</mysql-connection-version>
        <mybatis-generator-version>1.3.2</mybatis-generator-version>
        <pagehelper-version>4.0.1</pagehelper-version>
        <slf4j-version>1.7.12</slf4j-version>
        <log4j-version>1.2.17</log4j-version>
    </properties>
```
在一些我觉得很有必要的地方我都加上了中文注释。
### 安装

推荐使用IDEA:
![](http://7xawrk.com1.z0.glb.clouddn.com/15-10-14/8288030.jpg)
clone 完后会看到这样的目录结构
![](http://7xawrk.com1.z0.glb.clouddn.com/15-10-14/31026011.jpg)
### 修改配置文件

项目的配置文件均放在 src/main/resources下
* applicationContext.xml:Spring 配置文件
* generator.properties:Mybatis-generator 配置文件
* generatorConfig.xml:Mybatis-generator 配置文件
* jdbc.properties:jdbc配置文件
* log4j.properties:log4j 配置文件
* mvc-dispatcher-servlet.xml:SpringMVC配置文件

主要需要修改的是

* generator.properties:Mybatis-generator 配置文件
* generatorConfig.xml:Mybatis-generator 配置文件
* jdbc.properties:jdbc配置文件

applicationContext.xml 以下地方需要注意：
``` xml
//Spring注解自动扫描的包
<context:component-scan base-package="dao" />
//Mybatis自动配置Mapper的包，也就是Mybatis生成xxMapper所在的包
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="dao"/>
</bean>
```
如果你修改了相关的package，则上面需要修改，同理在mvc-dispatcher-servlet.xml中也有action:
``` xml
<context:component-scan base-package="action"/>
```
### 开始

准备一张表user表:
``` sql
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user` (
  `userid` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(255) NOT NULL,
  `password` varchar(255) NOT NULL,
  PRIMARY KEY (`userid`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;
```
### 使用mybatis-generator自动生成

默认情况下，IDEA能自动识别出Maven的mybatis generator插件，但我们还需要进行一点修改
在IDEA中，Run->Edit Configurations 修改Maven的选项:
![](http://7xawrk.com1.z0.glb.clouddn.com/15-10-14/90011352.jpg)
添加上一个参数`-e`，用于在控制台打印错误信息。
然后我们在`generatorConfig.xml`中加入这张表：
``` xml
<table tableName="user" enableCountByExample="false"
           enableUpdateByExample="false" enableDeleteByExample="false"
           enableSelectByExample="false" selectByExampleQueryId="false">
</table>
```

`generatorConfig.xml`里还有很多配置，你可以直接使用我的默认配置，往上面添加table即可。
相关配置说明可以看[这篇文章](http://www.jianshu.com/p/e09d2370b796)。
这样我们就能点击Run了，顺利的话就能看到自动生成的代码。

### 编写service

`Myabtis-generator`会自动生成`UserMapper.java`,`User`,`UserMapper.xml`
针对User这张表已经自动生成了如下方法:

``` java
int deleteByPrimaryKey(Integer userid);

int insert(User record);

int insertSelective(User record);

User selectByPrimaryKey(Integer userid);

int updateByPrimaryKeySelective(User record);

int updateByPrimaryKey(User record);
```

如果上面的代码已经能满足你的需求了，那就什么都不用写。如果要增加自己的功能，比如上面没有的查询全部的User，我们所要做的便是修改`UserMapper.xml`,在`UserMapper.java`增加相应接口即可。
`UserMapper.xml`中增加:
``` xml
<select id="selectAll" resultType="entity.User">
    select userid, username, password from user
</select>
```
`UserMapper.java`中增加一个与ID同名的接口:
``` java
List<User> selectAll();
```
编写一个service测试一下，我们只要依赖注意一个UserMapper,就能使用相应的功能了:
``` java
package service;

import com.github.pagehelper.PageHelper;
import dao.UserMapper;
import entity.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;


@Service
public class UserService {

    private UserMapper userMapper;

    @Autowired
    public void setUserMapper(UserMapper userMapper) {
        this.userMapper = userMapper;
    }

    public User getUser(Integer userid){
        return userMapper.selectByPrimaryKey(userid);
    }

    /**
     * @param pageNum  页码
     * @param pageSize 每页显示数量
     */
    public List<User> getUsers(int pageNum, int pageSize){
        PageHelper.startPage(pageNum, pageSize);
        return userMapper.selectAll();
    }
}
```
我们知道，在真实的场景中，一次性获取全部用户显然是不现实的，我们往往要进行分页操作。这里我们用到了`PageHelper`这个Mybatis分页插件,详细的文档说明请看[这里](http://git.oschina.net/free/Mybatis_PageHelper)。

### 编写action

我们写个简单的action测试一下,json方面我使用了`fastjson`,配置在`mvc-dispatcher-servlet.xml`里，默认解决了IE下json提示下载的问题，其他更多的配置请看这里。
``` java
package action;

import entity.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import service.UserService;

import java.util.List;


@Controller
@RequestMapping("/user")
public class DemoAction {

    private UserService userService;

    @Autowired
    public void setUserService(UserService userService) {
        this.userService = userService;
    }

    @RequestMapping("/get")
    @ResponseBody
    public List<User> test() {
        return userService.getUsers(1,10);
    }

}
```

### 结语

当然我这里很多细节没有讲到，仅仅是简单的使用了一下，希望各位有心的读者可以自己动手搭建一下。