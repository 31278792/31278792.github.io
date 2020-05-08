---
title: Ajax
date: 2020-04-25 10:50:36
tags:
	- Ajax
	- 异步操作
categories: 前端
password: 31278792
---
### Ajax
**Ajax全称：Asynchronous Javascript And XML(异步 JS和XML)，它能使用JS实现异步访问服务器。众所周知，服务器给客户端传过去的是整个html完整页面，而Ajax局部刷新，服务器不再响应整个页面，只是传递数据，数据交换可以是txt、xml、json**

<!--more-->

#### 异步交互与同步交互
>同步：
>>例如发送一个请求，需要等待伺服器响应结束，才能发送第二个请求，等待时间给用户的直观感受就是"卡"
>>刷新是刷新整个页面
>异步：
>>例如同上发一个请求后，无需等待伺服器的响应，就能开始发送第二个请求
>>可以使用js结束伺服器的响应，然后使用js的局部刷新

![同步与异步](/images/ajax_pic.png "同步交互与异步交互")

#### Ajax优缺点
>优点：
>>异步交互：增强用户体验
>>性能：因为伺服器无需重复加载整个页面，只需要响应部分内容，减轻伺服器压力
>缺点；
>>场景：局限性，不是所有场景适用
>>无端增多了对伺服器的访问次数，给伺服器带来了压力

#### Ajax使用

##### Ajax发送异步请求
```
//四步操作
	//第一步（得到XMLHttpRequest）
	//创建XMLHttpRequest对象函数
	function createXMLHttpRequest(){
		try{
			return new XMLHttpRequest();//大多数浏览器支持它创建XMLHttpRequest对象
		}catch(e){
			try{
				return new ActiveXObject("Msxml2.XMLHTTP");//IE6.0使用其创建XMLHttpRequest对象
			}catch(e){
				try{
					return new ActiveXObject("Microsoft.XMLHTTP");//IE5.5及更早版本使用其创建XMLHttpRequest对象
				}catch(e){
					alert("您这浏览器是自制的吧，这都无法满足您");
					throw e;
				}
			}
		}
	
	}
	
	//第二步（打开与服务器的连接）
	//使用xmlHttp.open(请求方式,请求URL,请求是否为异步);打开连接
	//请求方式:可以使用GET或POST
	//请求URL:指定服务器端资源，例如：/cn/com/resource
	//请求是否为异步:为true发送异步请求，否则为同步请求
	xmlHttp.open("POST","/cn/com/resource",true);
	xmlHttp.setRequestHeader("Content-Type","application/x-www-form-urlencoded");/*这是设置Content-Type请求头，POST需要这一步，GET不用*/

	//第三步（发送请求）
	xmlHttp.send(null);//如果不给参数null可能造成部分浏览器不能发送，如果是GET请求，则必须给出参数null
	xmlHttp.send("username=zhangsan&password=123");//发送请求是指定请求体，发送请求需要带有参数一般使用POST
	
	//第四步（）
	//在xmlHttp对象的一个事件上注册监听器：onreadystatechange
	//xmlHttp一共有5个状态：
	//0状态：刚创建,还没有调用open方法;
	//1状态：请求开始，调用open方法，但还没有调用send方法
	//2状态：调用完了send方法
	//3状态：服务器已经开始响应，但不表示响应结束了
	//4状态；服务器响应结束！（这是我们最关心的状态）
	//得到xmlHttp的状态:
	var state = xmlHttp.readyState;
	//得到服务器响应的状态码：
	var status = xmlHttp.status;//例如为404，500
	//得到服务器响应内容：
	var content = xmlHttp.responseText;//得到文本式内容
	var content = xmlHttp.responseXML;//得到xml格式内容（即Document对象）
	
	xmlHttp.onreadystatechange = function(){//xmlHttp的五种状态都会调用该方法
		if(xmlHttp.readyState==4&&xmlHttp.status==200){//双重判断，是否为4状态，状态码是否为200
			//获取响应内容
			var text = xmlHttp.responseText;
		}
	};
```

#### XStream

##### 作用
**可以把javaBean序列化（即转化）为xml**

##### XStream的jar包
>核心JAR包：XStream-1.4.7.jar
>必须依赖包：xpp3_min-1.1.4c(XML Pull Parser,一款速度很快的xml解析器)

##### 使用步骤
```
XStream xstream = new XStream();
String xmlStr = xstream.toXML(javabean);
```

#### JSON

##### JSON是什么
**JS提供的一种数据交换格式！**

##### JSON的语法
示例；
`var person = {"name": "zhangsan", "age": 18, "sex": "male"};`
这JSON对象就跟JS字面式定义对象没什么区别，{}表示对象，里面属性值可以是null、数值、字符串、数组(用[]括起来)、布尔值
*属性名这里必须用双引号括起来，单引号不行*
上例JSON对象如果用双引号括起来成字符串，也可以通过eval函数实现转换
```
var str = "{\"name\": \"zhangsan\", \"age\": 18, \"sex\": \"male\"}";//双引号里面的双引号需要使用转义字符
var person = ever("(" + str + ")");
```

##### JSON与XML比较
**可读性：XML胜出**
**解析难度：JSON简单很多**
**流行度：XML已经流行很多年，但在AJAX领域，JSON更受欢迎**

##### json-lib小工具

###### 是什么?
**它可以把javabean转换为json串**

###### 核心类
JSONObject --父类Map
JSONArray --父类List

###### 使用方式
```
//方式一
JSONObject map = new JSONObject();
map.put("name","zhangsan");
map.put("age",18);
map.put("sex","male");
String s = map.toString();
System.out.println(s);//打印出一个JSON字符串

//方式二，将Person对象转换为JSON对象
Person p = new Person("lisi",21,"male");
JSONObject map = JSONObject.fromObject(p);
System.out.println(map.toString());

//JSONArray同上类似
Person p1 = new Person("zhangsan",50,"famale");
Person p2 = new Person("wangwu",46,"male");
JSONArray list = new JSONArray();
list.add(p1);
list.add(p2);
System.out.println(list.toString());

//若原先有一个List，我们可以把List转化为JSON对象
Person p1 = new Person("zhangsan",50,"famale");
Person p2 = new Person("wangwu",46,"male");
List<Person> list = new ArrayList<Person>();
list.add(p1);
list.add(p2);

System.out.println(JSONArray.fromObject(list).toString());

```

#### 打包AJAX工具类
```
	//创建XMLHttpRequest对象函数
	function createXMLHttpRequest(){
		try{
			return new XMLHttpRequest();//大多数浏览器支持它创建XMLHttpRequest对象
		}catch(e){
			try{
				return new ActiveXObject("Msxml2.XMLHTTP");//IE6.0使用其创建XMLHttpRequest对象
			}catch(e){
				try{
					return new ActiveXObject("Microsoft.XMLHTTP");//IE5.5及更早版本使用其创建XMLHttpRequest对象
				}catch(e){
					alert("您这浏览器是自制的吧，这都无法满足您");
					throw e;
				}
			}
		}
	
	}
	
var ajax = function(option){
	/*传递对象参数，该对象包括以下属性，
	请求方式：method,
	请求的url：url,
	是否异步：asyn,
	请求体：params,
	回调方法：callback,
	服务器响应数据转换成什么类型：type*/
	
	/*
	 *1.得到xmlHttp
	 */
	var xmlHttp = createXMLHttpRequest();
	
	/*
	 *2.打开连接
	 */
	if(!option.method){//默认为GET请求
		option.method = GET;
	}
	if(option.asyn == undefined){//默认为异步处理
		option.asyn = true;
	}
	xmlHttp.open(option.method,url,option.asyn);
	
	/*
	 *3.判断是否为POST
	 */
	if("POST"==option.method){
		xmlHttp.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
	}

	/*
	 *4.发送请求
	 */
	xmlHttp.send(option.params);
	
	/*
	 *5.注册监听
	 */
	xmlHttp.onreadystatechange = funcion(){
		if(xmlHttp.readyState==4&&xmlHttp.status==200){//双重判断
			var data;
			if(!option.type){//如果type没有给值默认为文本类型
				data = xmlHttp.responseText;
			}else if(option.type == "xml"){
				data = xmlHttp.responseXML;
			}else if(option.type == "json"){
				var text = xmlHttp.responseText;
				data = eval("(" + text + ")");
			}else if(option.type == "text"){
				data = xmlHttp.responseText;
			}
			//调用回调方法
			option.callback(data);
		}
	}  
}
export(ajax);
```


