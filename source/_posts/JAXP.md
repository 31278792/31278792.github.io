---
title: JAXP
date: 2020-04-17 22:19:32
tags: 
	- XML
	- 标记语言
	- XML解析
categories: JavaWeb
password: 31278792
---
### JAXP

#### xml的解析(*将用到JAVA代码*)***重点内容***

##### 简介
xml为标记下语言，JS使用dom解析标记型文档，根据html的层级结构，在内存中分配一个树型结构，把html中的标签、属性、对象都封装成对象，例如：document对象、element对象、属性对象、文本对象、Node节点对象

**xml的解析方式有两种：dom和sax**

<!--more-->
![解析图](/images/xml_pic.png "xml的解析")
*上图是dom的解析过程，该方式好处是很方便实现数据的增删改操作，但是如果文件过大，容易造成内存溢出。至于sax解析采用事件驱动，边读边解析，从上至下一行一行解析，解析到某一对象，将对象名称返回，不会造成内存溢出，查询操作方便，但不能实现增删改操作*
**总结：**
   **-dom方式解析：根据xml的层级结构在内存中分配一个树状结构，把xml标签，属性和文本都封装成对象**
   **-使用dom弊端：如果文件过大，容易造成溢出**
   **-sax方式解析：采用事件驱动，边读边解析，从上至下一行一行解析，解析到某一对象，将对象名称返回**
   **-使用sax弊端：不能实现增删改操作**

##### 解析器
**不同的公司和组织提供了针对dom和sax方式的解析器，通过api方式提供，sun公司提供了针对dom和sax的解析器jaxp，dom4j组织提供的解析器就叫dom4j，同样还有jdom组织的解析器jdom**

##### JAXP的API
**jaxp解析器在jdk的javax.xml.parsers包里面，包里四个类主要针对dom和sax解析使用，针对dom有DocumentBuilder（解析器类）和DocumentBuilderFactory（解析器工厂）两个类，而sax有SAXParser和SAXParserFactory**

#### jaxp使用
xml文件内容如下：
```
<?xml version="1.0" encoding="utf-8"?>
<person>
	<p1>
		<name>张三</name>
		<age>14</age>
	</p1>
	<p1>
		<name>李四</name>
		<age>45</age>
	</p1>
</person>
```

##### 查询操作
```
	public void selectAll() throws Exception{
	//1.创建解析器工厂
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();
	//2.根据工厂创建解析器
		DocumentBuilder builder = builderFactory.newDocumentBuilder();
	//3.解析xml返回的document
		Document document=builder.parse("src/person.xml");//括号内填写你xml文件所在位置
	//4.得到所有元素
		NodeList list=document.getElementsByTagName("name");//这里得到的是上面xml中name标签
	//5.遍历集合
		for(int i = 0; i<list.getLength();i++){
			Node name = list.item(i);//得到每一个name元素
			//得到name元素里面的值
			String s = name1.getTextContent();
			System.out.println(s);
		}
	}
	public void selectSin() throws Exception{
	//1.同上
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();
	//2.同上	
		DocumentBuilder builder = builderFactory.newDocumentBuilder();
	//3.同上	
		Document document=builder.parse("src/person.xml");
	//4.同上
		NodeList list=document.getElementsByTagName("name");
	//5.得到第一个name元素
		Node name1 = list.item(0);
	//6.得到name里面的值
		String s = name1.getTextContent();
		System.out.println(s);
	}
```

##### 增加操作
```
	public void add() throws Exception(){
	//1.同上
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();
	//2.同上	
		DocumentBuilder builder = builderFactory.newDocumentBuilder();
	//3.同上	
		Document document=builder.parse("src/person.xml");
	//4.同上
		NodeList list=document.getElementsByTagName("p1");
	//5.得到第一个p1
		Node p1 = list.item(0);
	//6.创建标签
		Element sex1 = document.createElement("sex");
	//7.创建文本
		Text text1 = document.createTextNode("男");
	//8.把文本添加到sex1下面
		sex1.appendChild(text1);
	//9.把sex1添加到p1下面
		p1.appendChild(sex1);
	//10.回写xml
		TransformerFactory transformerFactory = TransformerFactory.newInstance();
		Transformer transformer = transformerFactory.newTransformer();
		transformer.transform(new DOMSourse(document),new StreamResult("src/person.xml"));
	}
```

##### 修改操作
```
	public void fix() throws Exception(){
	//1.同上
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();
	//2.同上	
		DocumentBuilder builder = builderFactory.newDocumentBuilder();
	//3.同上	
		Document document=builder.parse("src/person.xml");
	//4.得到sex元素
		Node sex1 = document.getElementsByTagName("sex").item(0);
	//5.修改sex的内容
		sex1.setTextContent("man");
	//6.回写
		TransformerFactory transformerFactory = TransformerFactory.newInstance();
		Transformer transformer = transformerFactory.newTransformer();
		transformer.transform(new DOMSourse(document),new StreamResult("src/person.xml"));
	}
```

##### 删除操作
```
	public void delete() throws Exception(){
	//1.同上
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();
	//2.同上	
		DocumentBuilder builder = builderFactory.newDocumentBuilder();
	//3.同上	
		Document document=builder.parse("src/person.xml");
	//4.得到sex元素
		Node sex1 = NodeList list=document.getElementsByTagName("sex").item(0);
	//5.得到它的父节点p1
		Node p1 = sex1.getParentNode();
	//6.删除sex
		p1.removeChild(sex1);
	//7.回写	
		TransformerFactory transformerFactory = TransformerFactory.newInstance();
		Transformer transformer = transformerFactory.newTransformer();
		transformer.transform(new DOMSourse(document),new StreamResult("src/person.xml"));
	｝
```

##### 遍历节点(*把所有元素名称打印出来*)
```
	public void listElement() throws Exception(){
	//1.同上
		DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();
	//2.同上	
		DocumentBuilder builder = builderFactory.newDocumentBuilder();
	//3.同上	
		Document document=builder.parse("src/person.xml");
	//5.调用递归方法
		list1(document);
	}
	4.编写一个递归方法实现遍历
	public static void list1(Node node){
		if(node.getNodeType == Node.ELEMENT_NODE){//避免将标签内容中不必要的空格和tab打印出来
			System.out.println(node.getNodeName());//获取节点名称
		}
		//得到一层子节点
		NodeList list = node.getChildNodes();
		for(int i=0;i<list.getLength();i++){
			Node temp = list.item(i);
			//继续得到temp的子节点
			list1.(temp);
		}
	}
```

#### JAXP实现sax操作
**由于sax不能执行增删改操作，所以以下是实现它的查询操作**
```
class MyDefault1 extends DefaultHandler{//以下三个方法自动执行
	@Override
	public void startElement(String uri,String localName,String qName,Attributes attributes)throws SAXException{
		super.startElement(uri,localName,qName,attributes);
	}
	@Override
	public void characters(char[] ch,int start,int length)throws SAXException{
		super.characters(ch,start,length);
	}
	@Override
	public void endElement(String uri,String localName,String qName)throws SAXException{
		super.endElement(uri,localName,qName);
	}
}

class MyDefault2 extends DefaultHandler{//获取所有name元素的值
	boolean flag = false;
	@Override
	public void startElement(String uri,String localName,String qName,Attributes attributes)throws SAXException{
		//判断qName是否是name元素
		if("name".equals(qName)){
			flag = true;
		}
	}
	@Override
	public void characters(char[] ch,int start,int length)throws SAXException{
		//当flag值是true时表示解析到name元素
		if(flag == true){
			System.out.println(new String(ch,start,length));
		}
	}
	@Override
	public void endElement(String uri,String localName,String qName)throws SAXException{
		//将flag设置为false,表示name元素结束
		if("name".equals(qName)){
			flag = false;
		}
	}
}
```

```
	public static void main (String[] args){
	//1.创建解析器工厂
		SAXParserFactory saxParserFactory = SAXParserFactory.newInstance();
	//2.创建解析器
		SAXParser saxParser = SAXParserFactory.newSAXParser();
	//3.执行parse方法
		saxParser.parse("src/p1.xml",new MyDefault2());
	}
	
```











