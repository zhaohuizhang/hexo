title: Struts validate验证框架配置使用
tags:
  - struts
  - Validation
id: 97
categories:
  - Struts
date: 2015-04-28 03:20:57
---

1，struts-config.xml文件中配置
<pre>&lt;action path="/login" name="loginForm" scope="request"
    type="org.napu.action.LoginForm"
    validate="true" input="/login.jsp"&gt;
    &lt;forward name="succ" path="/success.jsp"/&gt;
    &lt;forward name="error" path="/error.jsp"/&gt;
&lt;/action&gt;</pre>
validate的值为true。
<pre>&lt;struts-config&gt;
....
&lt;message-resouses  parameter="AppliactionResource.property" /&gt;
&lt;plug-in className="org.apache.struts.validator.ValidatorPlugin"&gt;
    &lt;set-property value="/WEB-INF/validator-rules.xml,/WEB-INF/validation.xml" property="pathnames"&gt;
&lt;/plug-in&gt;
&lt;/struts-config&gt;</pre>
2，在WEB-INF目录中新建validation.xml文件。
<pre>&lt;form-validation&gt;
&lt;global&gt;&lt;/global&gt;
&lt;formset&gt;
&lt;form name="LoginForm"&gt;
&lt;field property="username" depends="required,minlength,maxlength"&gt;
&lt;arg0 key="userform.username"/&gt;
&lt;arg1 name="minlength" key="${var:min}" resource="false"/&gt;
&lt;arg2 name="maxlength" key="${var:max}" resource="true"/&gt;
&lt;var&gt;
&lt;var-name&gt;min&lt;/var-name&gt;
&lt;var-value&gt;6&lt;/var-value&gt;
&lt;/var&gt;
&lt;var&gt;
&lt;var-name&gt;max&lt;/var-name&gt;
&lt;var-value&gt;20&lt;/var-value&gt;
&lt;/var&gt; 
&lt;/field&gt;
&lt;/form&gt;
&lt;/formset&gt;
&lt;/form-validation&gt;</pre>
3，把validator框架使用的消息文本添加到应用的ResourceBundle中，在ApplicationResource.properties中添加内容。

4，将Form的extends ActionForm 改为extends ValidatorForm

5，在JSP页面中添加&lt;html:errors property="key"/&gt;。这里property对应Form中的定义的属性。

&nbsp;

### 自定义的验证

1，创建一个实现规则接口的静态方法的类。

2，在validator-rules.xml中的global元素中添加validator子元素

3，在资源属性文件中添加error信息。

4，在validation.xml中添加配置。