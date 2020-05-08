---
title: dom4j
date: 2020-04-21 21:34:01
tags: 
	- XML
	- 标记语言
	- XML解析
categories: JavaWeb
password: 31278792
---
### dom4j
**dom4j组织提供了针对xml解析的解析器dom4j,其是由jdom组织分裂出来后出现的,其优于jdom。dom4j不是javase的一部分**

<!--more-->

#### dom4j使用
*使用前需要在项目中导入dom4j的jar包*
xml文件如下:
```
<?xml version="1.0" encoding="utf-8"?>
<person>
	<p1 id1="01">
		<name>张三</name>
		<age>14</age>
	</p1>
	<p1>
		<name>李四</name>
		<age>45</age>
	</p1>
</person>
```

##### dom4j实现查询xml中元素操作
```
//查询所有name元素里的值
	public static void selectName() throws Exception{
		//1.创建解析器
		SAXReader saxReader = new SAXReader();
		//2.得到document
		Document document = saxReader.read("src/person.xml");
		//3.得到根节点
		Element root = document.getRootElement();
		//4.得到p1
		List<Element> list = root.elements("p1");
		//5.遍历List
		for(Element temp:list){
			//这里temp得到每一个p1元素
			Element name1 = temp.element("name");//得到p1下面的name元素
			//得到name里面的值
			String s = name1.getText();
			System.out.println(s);
		}
	}

//查询第一个name元素的值
	public static void selectSin() throws Exception{
		//1.创建解析器
		SAXReader saxReader = new SAXReader();
		//2.得到document
		Document document = saxReader.read("src/person.xml");
		//3.得到根节点
		Element root = document.getRootElement();
		//4.得到p1
		Element p1 = root.element("p1");
		//5.得到p1下第一个name
		Element name1 = p1.element("name");
		//6.得到name的值
		String s1 = name1.getText();
		System.out.println(s1);
	}
	
//查询第二个name元素的值	
	public static void selectSecond() throws Exception{
		//1.创建解析器
		SAXReader saxReader = new SAXReader();
		//2.得到document
		Document document = saxReader.read("src/person.xml");
		//3.得到根节点
		Element root = document.getRootElement();
		//4.得到所有p1
		List<Element> list = root.elements("p1");
		//5.得到第二个p1
		Element p2 = list.get(1);
		//6.得到p1下面name
		Element name2 = p2.element("name");
		//7.得到name的值
		String s2 = name2.getText();
		System.out.println(s2);
	}
```

##### dom4j实现添加xml中元素操作
```
//在第一个p1标签末尾添加一个sex标签
	public static addSex() throws Exception{
		//1.创建解析器
		SAXReader saxReader = new SAXReader();
		//2.得到document
		Document document = saxReader.read("src/person.xml");
		//3.得到根节点
		Element root = document.getRootElement();
		//4.获取第一个p1
		Element p1 = root.element("p1");
		//5.p1下面添加sex元素
		Element sex1 = p1.addElement("sex");
		//6.在sex里添加内容
		sex1.setText("男");
		//7.回写xml
		OutputFormat format = OutputFormat.createPrettyPrint();//获取缩进格式
		XMLWriter writer = new XMLWriter(new FileOutputStream("src/person.xml"),format);
		writer.write(document);
		writer.close();
} 
```

##### 在特定位置实现元素添加
```
//在第一个p1标签中age标签前添加一个<school>cqgcxy.edu.cn</school>
	public static void addAgeBefore() throws Exception{
		//1.创建解析器
		SAXReader saxReader = new SAXReader();
		//2.得到document
		Document document = saxReader.read("src/person.xml");
		//3.得到根节点
		Element root = document.getRootElement();		
		//4.获取第一个p1
		Element p1 = root.element("p1");
		//5.获取p1下面的所有元素
		List<Element> list = p1.elements();
		//6.创建要添加元素
		Element school = DocumentHelper.createElement("school");
		//7.给标签添加内容
		school.setText("eqgcxy.edu.cn");
		//8.在特定位置添加元素
		list.add(1,school);
		//9.回写xml
		OutputFormat format = OutputFormat.createPrettyPrint();//获取缩进格式
		XMLWriter writer = new XMLWriter(new FileOutputStream("src/person.xml"),format);
		writer.write(document);
		writer.close();
	}
```

##### dom4j实现修改xml中元素操作
```
//修改第一个p1中age元素内容
	public static void fixAge() throws Exception{
		//1.创建解析器
		SAXReader saxReader = new SAXReader();
		//2.得到document
		Document document = saxReader.read("src/person.xml");
		//3.得到根节点
		Element root = document.getRootElement();	
		//4.得到第一个p1
		Element p1 = root.element("p1");
		//5.得到age
		Element age = p1.element("age");
		//6.修改age的值
		age.setText("250");
		//7.回写xml
		OutputFormat format = OutputFormat.createPrettyPrint();//获取缩进格式
		XMLWriter writer = new XMLWriter(new FileOutputStream("src/person.xml"),format);
		writer.write(document);
		writer.close();
	}
```

##### dom4j实现删除xml中元素操作
```
//删除第一个p1下school节点
	public static void deleteSchool() throws Exception{
		//1.创建解析器
		SAXReader saxReader = new SAXReader();
		//2.得到document
		Document document = saxReader.read("src/person.xml");
		/*使用工具类替换1、2步
		Document document = Dom4jUtils.getDocument(Dom4jUtils.PATH);*/
		//3.得到根节点
		Element root = document.getRootElement();	
		//4.得到第一个p1
		Element p1 = root.element("p1");
		//5.得到school
		Element school = p1.element("school");
		//6.删除school(通过父节点实现删除)
		p1.remove(school);
		//7.回写xml
		OutputFormat format = OutputFormat.createPrettyPrint();//获取缩进格式
		XMLWriter writer = new XMLWriter(new FileOutputStream("src/person.xml"),format);
		writer.write(document);
		writer.close();
		/*使用工具类替换回写操作
		Dom4jUtils.xmlWriter(PATH,document)*/
	}
```

##### dom4j实现获取xml中元素属性值操作
```
//获取第一个p1属性id1的值
	public static void getValue() throws Exception{
		//1.创建解析器
		SAXReader saxReader = new SAXReader();
		//2.得到document
		Document document = saxReader.read("src/person.xml");
		//3.得到根节点
		Element root = document.getRootElement();
		//4.得到第一个p1
		Element p1 = root.element("p1");
		//5.获取p1的属性id1值
		String value = p1.attributeValue("id1");
		System.out.println(value);
	}
```

##### dom4j工具类
**由于什么的CRUD代码中存在大量重复使用的代码，因此我们可以建立一个工具类来封装这些重复代码，实现调用工具类，来减少代码冗余**
```
public class Dom4jUtils{
	public state final String PATH = "src/person.xml";
	//返回document
	public static Document getDocument(String path){
		try{
			//1.创建解析器
			SAXReader saxReader = new SAXReader();
			//2.得到document
			Document document = saxReader.read(path);
			return document;
		}catch(Exception e){
			e.printStackTrace();
		}
		return null;
	}
	
	//回写xml
	public static void xmlWriter(String path,Document document){
		try{
			OutputFormat format = OutputFormat.createPrettyPrint();//获取缩进格式
			XMLWriter writer = new XMLWriter(new FileOutputStream("src/person.xml"),format);
			writer.write(document);
			writer.close();
		}catch(Exception e){
			e.printStackTrace();
		}
	}
}
```

#### dom4j支持XPATH操作
**可以直接获取某一个元素，不必按照一层一层解析的方式**
##### 语法形式
第一种形式：`/a/b/c`来表示a里面的b里面的c
第二种形式：`//A`表示选择所有A元素
第三种形式：`/AAA/CCC/DDD/*`表示/AAA/CCC/DDD/里面的所有元素
第四种形式：`/*/*/*/`DDD表示选择所有第四层所有DDD元素
第五种形式；`//*`所有元素
第六种形式：`/a/b[1]`选择a标签中第一个b标签，/a/b[last()]选择a标签中最后一个b标签
第七种形式：`//@id`表示选择所有id属性，//BBB[@id]表示选择所有含id属性的BBB元素
第八种形式；`//BBB[@id='b1']`选择含Id属性且值为b1的所有BBB标签,b1如果左右有空格将不会被选取*

##### 如何在dom4j中使用XPATH
**默认的情况下，dom4j不支持XPATH，如果想使用，需要引入XPATH的jar包，在文件中找到jaxen-1.1-beta-6.jar，将其复制进eclipse构建的项目中lib下，右击项目Build Path选择Add to Build Path**
**在dom4j中提供了两个方法支持XPATH,它们分别是：selectNodes("XPATH表达式")和selectSingleNode("XPATH表达式")，第一个方法获取多个节点，第二个则获取单一节点**

```
//查询xml中所有name元素的值
public static void getValue()throws Exception{
	//1.得到document
	Document document = Dom4jUtils.getDocument(Dom4jUtils.PATH);
	//2.得到所有name
	List<Node> list=document.selectNodes("//name");
	//3.遍历集合
	for(Node note:list){//node是每一个name元素
		//获取name元素里面的值
		String s = node.getText();
		System.out.println(s);
	}
}

```

```
//获取第一个p1下面name的值
public static void getValue1()throws Exception{
	//1.得到document
	Document document = Dom4jUtils.getDocument(Dom4jUtils.PATH);
	//2.得到name元素
	Node name1=document.和selectSingleNode("//p1[@id1="01"]/name");
	//3.得到name里面内容
	String s1 = name1.getText();
	System.out.println(s1);
}
```
