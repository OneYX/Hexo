title: JSP中的EL表达式详细介绍
date: 2015-12-26 02:57:37
tags: JSP EL
---
## JSP中的EL表达式详细介绍

### 一、JSP EL语言定义

>&emsp;&emsp;EL 提供了在 JSP 脚本编制元素范围外使用运行时表达式的功能。脚本编制元素是指页面中能够用于在 JSP 文件中嵌入 Java 代码的元素。它们通常用于对象操作以及执行那些影响所生成内容的计算。JSP 2.0 将 EL 表达式添加为一种脚本编制元素。

<!-- more -->

### 二、JSP EL简介

#### 1、语法结构

`${expression}`
     
#### 2、[ ]与.运算符



- EL 提供“.“和“[ ]“两种运算符来存取数据。
当要存取的属性名称中包含一些特殊字符，如.或?等并非字母或数字的符号，就一定要使用“[ ]“。例如：
${user.My-Name}应当改为${user["My-Name"] }
- 如果要动态取值时，就可以用“[ ]“来做，而“.“无法做到动态取值。例如：
${sessionScope.user[data]}中data 是一个变量
         
#### 3、变量

 EL存取变量数据的方法很简单，例如：${username}。它的意思是取出某一范围中名称为username的变量。
 因为我们并没有指定哪一个范围的username，所以它会依序从Page、Request、Session、Application范围查找。
 假如途中找到username，就直接回传，不再继续找下去，但是假如全部的范围都没有找到时，就回传null。
 属性范围在EL中的名称

|范围|名称|
| ------- | ---------- |
| Page          | PageScope         |
| Request       | RequestScope      |
| Session       | SessionScope      |
| Application   | ApplicationScope  |

### 三、JSP EL 中的有效表达式

有效表达式可以包含文字、操作符、变量（对象引用）和函数调用。我们将分别了解这些有效表达式中的每一种：
       
#### 1、文字

JSP 表达式语言定义可在表达式中使用的以下文字：
    
|文字|文字的值|
| ------------ | ----- |
|Boolean|true 和 false|
|Integer|与 Java 类似。可以包含任何正数或负数，例如 24、-45、567|
|Floating Point|与 Java 类似。可以包含任何正的或负的浮点数，例如 -1.8E-45、4.567|
|String|任何由单引号或双引号限定的字符串。对于单引号、双引号和反斜杠，使用反斜杠字符作为转义序列。必须注意，如果在字符串两端使用双引号，则单引号不需要转义。
|Null|	null|
    
#### 2、操作符

JSP 表达式语言提供以下操作符，其中大部分是 Java 中常用的操作符：
    
|术  语|	定义|
| -------------------- |---------|
|算术型|+、-（二元）、*、/、div、%、mod、-（一元）|
|逻辑型|and、&&、or、｜｜、!、not|
|关系型|==、eq、!=、ne、、gt、<=、le、>=、ge。可以与其他值进行比较，或与布尔型、字符串型、整型或浮点型文字进行比较。|
|空|空操作符是前缀操作，可用于确定值是否为空。|
|条件型|	A ?B :C。根据 A 赋值的结果来赋值 B 或 C。|
 
#### 3、隐式对象

JSP 表达式语言定义了一组隐式对象，其中许多对象在 JSP scriplet 和表达式中可用：

|名称|描述|
|-------|-------------------|
| `pageContext` | JSP 页的上下文。它可以用于访问 JSP 隐式对象，如请求、响应、会话、输出、servletContext 等。例如，`${pageContext.response}` 为页面的响应对象赋值。|
    
此外，还提供几个隐式对象，允许对以下对象进行简易访问：

|术语|	定义|
| ------- | -------- |
| param | 将请求参数名称映射到单个字符串参数值（通过调用`ServletRequest.getParameter (String name)` 获得）。`getParameter(String)`方法返回带有特定名称的参数。表达式 `$(param.name)` 相当于`request.getParameter (name)` 。|
| paramValues |将请求参数名称映射到一个数值数组（通过调用 `ServletRequest.getParameter (String name)` 获得）。它与 `param` 隐式对象非常类似，但它检索一个字符串数组而不是单个值。表达式 `${paramvalues.name)` 相当于 `request.getParamterValues(name)`。|
|header|将请求头名称映射到单个字符串头值（通过调用 `ServletRequest.getHeader(String name)` 获得）。表达式 `${header.name}` 相当于 `request.getHeader(name)`。|
| headerValues |将请求头名称映射到一个数值数组（通过调用`ServletRequest.getHeaders(String)`获得）。它与头隐式对象非常类似。表达式 `${headerValues.name}` 相当于`request.getHeaderValues(name)`。|
| cookie |将 cookie 名称映射到单个 cookie对象。向服务器发出的客户端请求可以获得一个或多个 cookie。表达式 `${cookie.name.value}` 返回带有特定名称的第一个 cookie 值。如果请求包含多个同名的 cookie，则应该使用 `${headerValues.name}`表达式。|
| initParam |	将上下文初始化参数名称映射到单个值（通过调用`ServletContext.getInitparameter(String name)` 获得）。|
    
除了上述两种类型的隐式对象之外，还有些对象允许访问多种范围的变量，如 Web 上下文、会话、请求、页面：
    
|术语|	定义|
|----|  ----|
|pageScope|将页面范围的变量名称映射到其值。例如，EL 表达式可以使用 `${pageScope.objectName}` 访问一个 JSP 中页面范围的对象，还可以使用 `${pageScope.objectName.attributeName}` 访问对象的属性。|
|requestScope|将请求范围的变量名称映射到其值。该对象允许访问请求对象的属性。例如，EL 表达式可以使用`${requestScope.objectName}` 访问一个 JSP 请求范围的对象，还可以使用 `${requestScope.objectName.attributeName}` 访问对象的属性。|
|sessionScope|将会话范围的变量名称映射到其值。该对象允许访问会话对象的属性。例如：`$sessionScope.name}` 
|applicationScope|将应用程序范围的变量名称映射到其值。该隐式对象允许访问应用程序范围的对象。|
     
### 四、特别强调

1. 注意当表达式根据名称引用这些对象之一时，返回的是相应的对象而不是相应的属性。例如：即使现有的 pageContext 属性包含某些其他值，${pageContext} 也返回 PageContext 对象。
2. 注意 <%@ page isELIgnored="true" %> 表示是否禁用EL语言,TRUE表示禁止.FALSE表示不禁止.JSP2.0中默认的启用EL语言。
    
### 五、举例说明

1. 例如，< %=request.getParameter(“username”)% >等价于${ param.username }

2. 例如，但是下面的那句EL语言可以完成如果得到一个username为空，则不显示null,而是不显示值。
  <%=user.getAddr( ) %>等价于${user.addr}。
  
3. 例如：
    <% =request.getAttribute(“userlist”) %>等价于${requestScope.userlist }

4. 例如，原理如上例3。
    ${ sessionScope.userlist } 1
    ${ sessionScope.userlist } 2
    ${ applicationScope.userlist } 3 
    ${ pageScope.userlist } 4
    ${uselist} 含义：执行顺序为4 1 2 3。
    “.”后面的只是一个字符串，并不是真正的内置对象，不能调用对象。
    
5. 例如，
    <%=user.getAddr( ) %>等价于${user.addr}
    第一句前面的user,为一个变量。
    第二句后面user，必须为在某一个范围里的属性。

### 补充：

``` javascript
<%@ taglib prefix="c" http://java.sun.com/jstl/core_rt">http://java.sun.com/jstl/core_rt" %>
FOREACH:
<c:forEach items="${messages}"
var="item"
begin="0"
end="9"
step="1"
varStatus="var">
……
</c:forEach>
OUT:
<c:out value="/${logininfo.username}"/>
```

* <c:out>将value中的内容输出到当前位置，这里也就是把logininfo对象的username属性值输出到页面当前位置。
* ${……}是JSP2.0 中的Expression Language（EL）的语法。它定义了一个表达式，其中的表达式可以是一个常量（如上），也可以是一个具体的表达语句（如forEach循环体中的情况）。典型案例如下：
&emsp;&emsp; ?&nbsp;${logininfo.username}这表明引用logininfo 对象的username 属性。我们可以通过“.”操作符引用对象的属性，也可以用“[]”引用对象属性，如${logininfo[username]}与${logininfo.username}达到了同样的效果。
* “[&nbsp;]”引用方式的意义在于，如果属性名中出现了特殊字符，如“.”或者“-”，此时就必须使用“[ ]”获取属性值以避免语法上的冲突（系统开发时应尽量避免这一现象的出现）。与之等同的JSP Script大致如下：
&emsp;&emsp;`LoginInfo logininfo = (LoginInfo)session.getAttribute(“logininfo”);`
&emsp;&emsp;`String username = logininfo.getUsername();`
* 可以看到，EL大大节省了编码量。
&emsp;&emsp;这里引出的另外一个问题就是，EL 将从哪里找到logininfo 对象，对于${logininfo.username}这样的表达式而言，首先会从当前页面中寻找之前是否定义了变量logininfo，如果没有找到则依次到Request、Session、Application 范围内寻找，直到找到为止。如果直到最后依然没有找到匹配的变量，则返回null.
* 如果我们需要指定变量的寻找范围，可以在EL表达式中指定搜寻范围：
&emsp;&emsp; `${pageScope.logininfo.username}`
&emsp;&emsp; `${requestScope.logininfo.username}`
&emsp;&emsp; `${sessionScope.logininfo.username}`
&emsp;&emsp; `${applicationScope.logininfo.username}`
* 在Spring 中，所有逻辑处理单元返回的结果数据，都将作为Attribute 被放置到       HttpServletRequest 对象中返回（具体实现可参见Spring 源码中org.springframework.web.servlet.view.InternalResourceView.exposeModelAsRequestAttributes方法的实现代码），也就是说SpringMVC 中，结果数据对象默认都是requestScope。因此，在Spring MVC 中，
以下寻址方法应慎用：
&emsp;&emsp; `${sessionScope.logininfo.username}`
&emsp;&emsp; `${applicationScope.logininfo.username}`
* ?&nbsp;${1＋2}
结果为表达式计算结果，即整数值3。
* ?&nbsp;${i>1}
&emsp;&emsp;如果变量值i>1的话，将返回bool类型true。与上例比较，可以发现EL会自动根据表达式计算结果返回不同的数据类型。
&emsp;&emsp;表达式的写法与java代码中的表达式编写方式大致相同。
* IF / CHOOSE:

``` javascript
<c:if test="${var.index % 2 == 0}">
*
</c:if>
<--判定条件一般为一个EL表达式。-->
<c:if>并没有提供else子句，使用的时候可能有些不便，此时我们可以通过<c:choose>
tag来达到类似的目的：
<c:choose>
<c:when test="${var.index % 2 == 0}">
*
</c:when>
<c:otherwise>
!
</c:otherwise>
</c:choose>
```

类似Java 中的switch 语句，<c:choose>提供了复杂判定条件下的简化处理手法。其
中`<c:when>`子句类似case子句，可以出现多次。上面的代码，在奇数行时输出“*”号，
而偶数行时输出“!”。
经验： 如果EL表达式无法解析：– <%@ page isELIgnored="false" %>

### 一、JSTL

#### 1. EL运算符


* var指定变量，并把EL运算结果赋值给该变量值为true/false；
* scope:指定 var变量的范围；

#### 2. 迭代标签

* 语法：`<c:forEach items=“collection” var=“name” varStatus=“status” begin=“int“ 
end=”int” step=“int” >
//循环体
</c:forEach>`
* 说明:
1)items:是集合，用EL表达式；
2)var:变量名，存放items
3)varStatus: 显示循环状态的变量
&emsp;&emsp;①index:从0开始;
&emsp;&emsp;②count:元素位置,从1开始;
&emsp;&emsp;③first:如果是第一个元素则显示true;
&emsp;&emsp;④last:如果是最后一个元素则显示true;
4)begin:循环的初始值(整型)；
5)end: 循环结束 ;
6)step:步长,循环间隔的数值；

#### 3. `<c:otherwise>`标签
&emsp;&emsp;例：
&emsp;&emsp;如果user.wealthy值true，则显示user.wealthy is true.
``` javascript
<c:choose>
<c:when test="">
user.generous is true.
</c:when>
<c:when test="">
user.stingy is true.
</c:when>
<c:otherwise>
user.generous and user.stingy are false.
</c:otherwise>
</c:choose>
```

&emsp;&emsp;说明： 
* 只有当条件user.generous返回值是true时，才显示`user.generous is true.`
* 只有当条件user.stingy返回值是true时，才显示`user.stingy is true.`
* 其它所有的情况（即user.generous和user.stingy的值都不为true）全部显示`user.generous and user.stingy are false.`
* 由于JSTL没有形如`if (){…} else {…}`的条件语句，所以这种形式的语句只能用`<c:choose>`、
`<c:when>`和`<c:otherwise>`标签共同来完成了。

#### 4. `<c:forTokens>`标签
说明：

|属性|说明|是否必须|默认值|
|----|---|-------|-----|
|items |进行循环的项目 |是 |无|
|delims |分割符 |是 |无|
|begin |开始条件 |否 |0|
|end |结束条件 |否 |集合中的最后一个项目|
|step |步长 |否 |1|
|var |代表当前项目的变量名 |否 |无|
|varStatus |显示循环状态的变量 |否 |无|

例子:
`<c:forTokens items="a:b:c:d" delims=":" var="token">`
`<c:out value=""/>`
`</c:forTokens>`
这个标签的使用相当于`java.util.StringTokenizer`类。在这里将字符串a:b:c:d以：分开循环四次，token是循环到当前分割到的字符串。

#### 5. <c:redirect>标签
&emsp;&emsp;说明：标签将请求重新定向到另外一个页面，它有以下属性

|属性 |描 述 |是否必须 |缺省值|
|-----|-----|--------|-----|
|url |url地址 |是 |无|
|context |/后跟本地web应用程序的名字 |否 |当前应用程序|

&emsp;&emsp;例子：
`<c:redirect /'>http://www.yourname.com/login.jsp"/>
将请求重新定向到http://www.yourname.com/login.jsp页，相当于response.setRedirect
("http://www.yourname.com/login.jsp");`

#### 6. <c:param>标签
&emsp;&emsp;说明：<c:param>标签用来传递参数给一个重定向或包含页面，它有以下属性

|属性 |描 述 |是否必须 |缺省值|
|-----|-----|--------|-----|
|name |在request参数中设置的变量名 |是 |无|
|value |在request参数中设置的变量值 |否 |无|

&emsp;&emsp;例子：
&emsp;&emsp;`<c:redirect url="login.jsp">`
&emsp;&emsp;&emsp;&emsp;`<c:param value="888"/>`
&emsp;&emsp;`</c:redirect>`
将参数888以id为名字传递到login.jsp页面，相当于login.jsp?id=888

#### 7. `<fmt:>`格式化标签

&emsp;&emsp;说明：需要导入 `<%@ taglib prefix="fmt" http://java.sun.com/jsp/jstl/fmt">http://java.sun.com/jsp/jstl/fmt" %>`
&emsp;&emsp;1）格式化日期`<fmt:formatDate value=“” pattern=“yyyy-MM-dd HH:mm:ss”/>`
&emsp;&emsp;&emsp;&emsp;Value:通过EL表达式或`<%new Date() %> `取的日期值；
&emsp;&emsp;&emsp;&emsp;Pattern:输出的日期格式； 
&emsp;&emsp;2）格式化数字`<fmt:formatNumber value=“${n}” pattern=“###,###,##”/>`
&emsp;&emsp;&emsp;&emsp;Value:通过EL表达式取的数字值；
&emsp;&emsp;&emsp;&emsp;Pattern:输出的数字格式；


