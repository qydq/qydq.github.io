---
title:  "SSH 三层架构中struts 实现连接数据库Mysql代码参考"
date:   2015-04-10 18:50:48
categories: _posts
---

迁移源地址为：<a href="http://bgwan.blog.163.com/blog/static/239301016201531035058765/">《SSH 三层架构中struts 实现连接数据库Mysql代码参考》</a>

date：2015.04.10

author：Chee   \ bgwan

declare：Reproduced original sources 转载标明出处

1.index.jsp

<%@ page language="java" import="java.util.*" pageEncoding="utf-8"%>
<%
 String path = request.getContextPath();
 String basePath = request.getScheme() + "://"
   + request.getServerName() + ":" + request.getServerPort()
   + path + "/";
%>
<%@ taglib uri="/struts-tags" prefix="s"%>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
<head>
<base href="<%=basePath%>">

<title>Demo_iterate</title>
<meta http-equiv="pragma" content="no-cache">
<meta http-equiv="cache-control" content="no-cache">
<meta http-equiv="expires" content="0">
<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
<meta http-equiv="description" content="This is my page">
<!--
 <link rel="stylesheet" type="text/css" href="styles.css">
 -->
</head>

<body>
 This is my JSP page.
 <s:form action="user/login">
 <s:textfield name="username" label="用户名"></s:textfield>
 <s:password name="password" label="密码"></s:password>
 <s:checkbox name="book" label="看书"></s:checkbox>
 <s:submit value="提交"></s:submit>
 </s:form>
 <br>---------------------------<br>
 <%
  List<String> list = new ArrayList<String>();
  list.add("Leon");
  list.add("John");
  list.add("Peter");
  list.add("Jeff");
  list.add("Linda");
  request.setAttribute("names", list);
 %>
 <h3>Names:</h3>

  <s:set name="myList" value="{'bgwan','nakama','chee'}"></s:set>
  <s:property value="#myList[2]"/><br>
  <s:property value="#myList"/>
  <br>
  ---------------------------<br><ol>
  <s:iterator value="#request.names" status="statu">
  <s:if test="#statu.odd">
  <li>奇数：<s:property /></li>
  </s:if>
  <s:else>
  <li>偶数：<s:property /></li>
  </s:else>
  </s:iterator>
  </ol>
---------------------------<br>
<s:set name="myMap" value="#{'key1':'apple','key2':'bar'}"/>
<s:property value="#myMap[2]"/><br>
<s:property value="#myMap"/>
<br>---------------------------<br>
<s:if test="'abc' in{'ads','acb','aba','abc'}">
abc在集合
</s:if>
<s:else>abc不在集合</s:else>
<br>---------------------------<br>
<s:iterator var="fruit" value="myList">
<s:property value="fruit"/>
</s:iterator>

</body>
</html>

2.welcome.jsp  同时也是struts基本标签的用法

<%@ page language="java" import="java.util.*" pageEncoding="utf-8"%>
<%@ taglib prefix="s" uri="/struts-tags" %>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <base href="<%=basePath%>">

    <title>My JSP 'welcome.jsp' starting page</title>
 <!--
 <link rel="stylesheet" type="text/css" href="styles.css">
 -->

  </head>

  <body>
    This is my JSP page. <br>
    亲爱的用户欢迎您:
    下面是struts2一般标签的测试内容：<br><hr>

    <p>----111111111--------s:include-----------</p>
    <s:include value="fail.jsp" /><hr>

    <p>---222222222---------s:param-and s:include----------</p>
    <s:include value="fail.jsp">
    <s:param name="author" value="'bgwan'" />
    </s:include><hr>

    <p>-----33333333-------s:property--and push---------</p>
    <s:bean name="com.ascent.po.Usr" id="u">
    param的作用是：为其他标签如include，bean提供参数<br>
    <s:param name="username" value="'baijiangwan'" />
    <s:param name="password" value="'baijiangpassword'" />
    </s:bean>
    <!-- 使用push 把值放入栈顶 -->
    <s:push value="#u">
    <s:property value="username"/>
    <s:property value="password"/><br>下面没有提供value参数：为默认值<b>
    <s:property/></b>
    </s:push><hr>

        <p>--4444444444----------s:set标签-----------</p>
        <s:bean name="com.ascent.po.Usr" id="uu">
        <s:param name="username" value="'nakama'" />
    <s:param name="password" value="'123456'" />
    </s:bean>
    <!-- 讲stack context中的uu放到默认范围内 -->
    <s:set value="#uu" name="usr" scope="request" />
    <s:property value="#request.usr.password" /><hr>

    <p>------555555555------s:text标签-----------</p>
    <s:i18n name="com.ascent.po.Usr">
    <s:text name="main.title------1" />
    </s:i18n><br>
    <s:text name="main.title------2" /><br>
    <s:text name="illll.lage.greetings------3">
    <s:param>Mr Smith</s:param>
    </s:text><hr>

    <p>------6666666666------s:if s:elseif s:else标签-----------</p>
    <s:if test="%{false}">
    <div>cuowu111</div><br></s:if>
    <s:elseif test="%{true}"><div>正确222</div><br></s:elseif>
    <s:else><div>haha333</div></s:else><hr>

    <p>------77777777777------iterator标签-----------</p>
    <%
  List<String> list = new ArrayList<String>();
  list.add("Leon");
  list.add("John");
  list.add("Peter");
  list.add("Jeff");
  list.add("Linda");
  request.setAttribute("names", list);
 %>
 <h3>Names:</h3>

  <s:set name="myList" value="{'bgwan','nakama','chee'}"></s:set>
  <s:property value="#myList[2]"/><br>
  <s:property value="#myList"/>
  <br>
  ---------------------------<br><ol>
  <s:iterator value="#request.names" status="statu">
  <s:if test="#statu.odd">
  <li>奇数：<s:property /></li>
  </s:if>
  <s:else>
  <li>偶数：<s:property /></li>
  </s:else>
  </s:iterator>
  </ol>

  <p>--8888888888----------sort标签-----------</p>
  <s:bean name="com.ascen.util.MyComarator" id="mycomparator" />
  <table border="1" width="200">
  <s:sort comparator="#mycomparator"
  source="{'woaini','wozhendeaini','wozhendezhendeaini'}">
  <s:iterator status="st">
  <tr style="background-color:red;">
  <s:if test="#st.odd"></s:if>
  <td><s:property /></td>
  </tr>
  </s:iterator>
  </s:sort>
  </table><hr>

  <p>----99999999999--------data标签-----------</p>
  <%Date now = new Date();
  pageContext.setAttribute("now", now);%>nice=f format
  <s:date name="#attr.now" format="dd/MM/yyyy" nice="false" /><br>nice=t format
  <s:date name="#attr.now" format="dd/MM/yyyy" nice="ture" /><br>no format nice =t
  <s:date name="#attr.now" nice="ture" /><br>no format  nice=f
  <s:date name="#attr.now" nice="false" /><br><hr>

  <p>----1010101010--------il18标签-----------</p>
  <s:i18n name="properties/Mymessages">
  <s:text name="HelloWorld" />
  </s:i18n><hr>


  <p>-----0000000000000-------url标签-----------</p>
  <s:a href="xxx.action">添加</s:a>
  <s:a href="xxx.action?id=1">编辑</s:a>

  <s:url id="url" action="xxx.acton">
  <s:param name="id" value="1"></s:param>
  </s:url>
  <s:a href="%{url}">删除</s:a>

  <a href="<s:url action="xxx.action">
  <s:param name="id" value="1" />

  </s:url>">删除2</a><hr>
  </body>
</html>

3.fail.jsp

<%@ page language="java" import="java.util.*" pageEncoding="utf-8"%>
<%@ taglib prefix="s" uri="/struts-tags" %>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <base href="<%=basePath%>">

    <title>My JSP 'fail.jsp' starting page</title>
 <!--
 <link rel="stylesheet" type="text/css" href="styles.css">
 -->

  </head>

  <body>
    This is my JSP page. <br>
    对不起 亲爱的 您输入的账号或密码错误:<br>
    这也是一个include标签插入页：
    下面为接受的数据<p>
    ${param.author }
    </p>${param.author }
  </body>
</html>
4.MyComarator.java
package com.ascen.util;

import java.util.Comparator;

public class MyComarator implements Comparator{
 @Override
 public int compare(Object o1, Object o2) {
  // TODO Auto-generated method stub
  return o1.toString().length()-o2.toString().length();
 }

}
5.Usr.java

package com.ascent.po;

public class Usr {

 private String username;
 private String password;
 public String getUsername() {
  return username;
 }
 public void setUsername(String username) {
  this.username = username;
 }
 public String getPassword() {
  return password;
 }
 public void setPassword(String password) {
  this.password = password;
 }

}
6.LoginAction.java

package sunshuntao.loginAction;

import sunshuntao.loginModel.LoginModel;


public class LoginAction {
 private String username;
 private String password;
 public String getUsername() {
  return username;
 }
 public void setUsername(String username) {
  this.username = username;
 }
 public String getPassword() {
  return password;
 }
 public void setPassword(String password) {
  this.password = password;
 }
 public String execute(){
  String result = "fail";
  LoginModel u = new LoginModel();
  boolean b = u.login(username,password);
  if(b){
   result="succ";
  }
  return result;
 }

}

7.LoginModel.java

package sunshuntao.loginModel;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class LoginModel {

 public boolean login(String username, String password) {
  // TODO Auto-generated method stub
  System.out.println("用户名："+username);
  System.out.println("密码："+password);
  boolean r = false;
  try {
   Class.forName("com.mysql.jdbc.Driver");
   String url = "jdbc:mysql://127.0.0.1:3306/jw";
   Connection conn = DriverManager.getConnection
     (url, "root", "123456");
   Statement stat = conn.createStatement();
   String sql = "select *from userinfo where username='" + username
     + " 'and password='" + password + "'";
   ResultSet resu = stat.executeQuery(sql);
   if (resu.next())
    r = true;

  } catch (ClassNotFoundException e) {
   e.printStackTrace();
  } catch (SQLException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return r;
 }
}
8.struts.xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC "-//Apache Software Foundation//DTD Struts Configuration 2.1//EN" "http://struts.apache.org/dtds/struts-2.1.dtd">
<struts>
<package name="mypack" extends="struts-default" namespace="/user">

<action name="login" class="sunshuntao.loginAction.LoginAction">

<result name="fail">/fail.jsp</result>
<result name="succ">/welcome.jsp</result>

</action>
</package>
</struts>

9.web.xml

<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" version="3.1">
  <display-name>Demo_iterator</display-name>
  <filter>
    <filter-name>struts2</filter-name>
    <filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
  <!--有的 没有这个 tomcat启动可能报错 -->
  <init-param>
            <param-name>actionPackages</param-name>
            <param-value>annotation.actions</param-value>
        </init-param>

  </filter>
  <filter-mapping>
    <filter-name>struts2</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
</web-app>
10.数据库创建和链接机制参考相关资料。

备注：项目实现的功能比较多，不太适合初学者。struts 架构 、struts链接数据库mysql、struts一般标签的使用（下一遍日志会记录）、简单的体现了SSH的一种开发模式。