---
title: XML入门
date: 2020-03-30 20:53:48
tags: 
	- XML
	- 标记语言
categories: JavaWeb
password: 31278792
---
## XML(Extensible Markup Language)

### XML是什么
**XML是W3C组织发布的技术，XML类似于HTML，大家知道HTML是一种超文本标记语言，其实XML也是一种标记语言，根据全称翻译它是可扩展标记语言。这里的可扩展是它的特性，HTML的标签是固定的，而XML可以自己定义标签，可以定义中文标签。**

### XML版本
目前有两个版本一个1.0，一个1.1，一般使用1.0，因为1.1无法向下兼容。

### XML做什么
**HTML是用于显示数据，XML也可以显示数据，但它的主要功能并不是显示数据，设计它的初衷是为了存储数据，类似于一个小型数据库**

### XML怎么用

#### 1.不同系统之间传输数据，利于系统的维护

<!--more-->

#### 2.用来表示生活中有关系的数据
示例：
```
<?xml version="1.0" encoding="UTF-8"?>
<中国>
	<帝都>
		<朝阳区></朝阳区>
		<海淀区></海淀区>
	</帝都>
	<四川>
		<成都></成都>
		<绵阳></绵阳>
	</四川>
	<湖北>
		<武汉></武汉>
		<荆州></荆州>
	</湖北>
</中国>
```
#### 3.经常用在配置文件中
比如：存储你的数据库用户名和密码，如果要修改数据库信息就不用该源代码，直接对配置文件进行修改，这样增加了可扩展性和可维护性

### XML的语法

####　xml的文档声明
1)创建一个后缀名为.xml文件
2)文件头部(第一行第一列)必须有文档声明
`<?xml version="1.0" encoding="gbk"?>`
3)属性：
>version:xml的版本 1.0/1.1
>encoding:xml的编码 gbk、utf-8、iso8859-1
>standalone:是否需要依赖其他文件 yes/no
4)中文乱码问题
保存文件编码格式与自身设置编码一致，可以解决乱码问题

#### 定义元素（标签）

##### 1)有开始必须要有结束`<aa></aa>`，如果标签之间没有内容可以只写`</aa>`

##### 2)标签可以嵌套

##### 3)一个xml中有且只有一个根标签
**注意：xml中空格和换行都会被当成内容来解析，譬如两个代码表示内容是不同的**
```
<网址>crazyyzw.top</网址>

<网址>
	crazyyzw.top
</网址>
```
##### 4)XML命名规则
>XML区分大小写
>不能以数字和_开头
>不能以XML自身开头，反例:~<xmla></xmal>~
>标签名字之间不能包含空格
>标签名字之间不能包含冒号

#### 定义属性
>一个标签可以有多个属性
>属性不允许重复 反例：~<dog name="wangwang" name='aoaoao'></dog>~
>属性值可以用单引号，也可以用双引号
>属性命名规范与标签一致

#### 注释
写法：`<!--这是一个注释-->`
**注释不能嵌套**

#### 特殊字符
XML内容中用到的字符转义：
>小于号转义字符为`&lt;`
>大于号转义字符为`&gt;`
>'&'转义字符为`&amp;`
>双引号转义字符为`&quot;`
>单引号转义字符为`&apos;`

#### CDATA区
如果多个字符需要转义，只有放在CDATA区就不需要转义老
写法：
```
<![CDATA[所加内容]]>
<![CDATA[<B>if(a<b && a>c && c>f){}</B>]]>
```

#### PI指令(处理指令)
可以在xml中设置样式
引入方式：
```
<?xml-stylesheet type="text/css" href="reset.css"?>//对中文标签不起作用
```

#### XML的约束
目的使标签内元素更加规范
xml常见两种约束技术：dtd约束和schema约束

### DTD

#### DTD文件创建
>1.创建一个文件后缀名为.dtd
>2.看xml文件中有几个元素，然后在dtd中写几个<!ELEMENT>
>3.判断元素是简单元素还是复杂元素，(*简单元素就是元素没有嵌套子元素*)
>>简单元素:<!ELEMENT 简单元素名称 (#PCDATA)>
>>复杂元素：<!ELEMENT 复杂元素名称 (子元素名称1,...)>,*注意：它的孙子元素不包括在内*

>4.在xml文件中引入dtd文件
>>引入外部dtd文件： `<!DOCTYPE 根元素名称 SYSTEM "dtd文件的路径"><!--引入方式1-->`
>>使用内部dtd文件： `<!DOCTYPE 根元素名称 [dtd文件内容]><!--引入方式2-->`
>>使用网络上的dtd文件： `<!DOCTYPE 根元素名称 PUBLIC "DTD名称" "DTD文档的URL"><!--引入方式3-->`
**xml文件在浏览器中运行不受dtd约束，因为浏览器只会校验xml语法，不会通过dtd文件进行校验，我们需要专门工具实现校验xml的约束**

#### 使用DTD定义元素
##### 简单元素语法
`<!ELEMENT 元素名称 约束>`
示例：
```
<!ELEMENT name (#PCDATA)>//这里的(#PCDATA)是一种约束，约束name中内容为字符串类型，其他约束：EMPTY:元素为空（没有内容）,ANY：表示任意
```
##### 复杂元素语法
`<!ELEMENT 复杂元素名称 (子元素名称1,...)>`
*表示子元素出现次数：*
示例：
```
<!ELEMENT person (name+,age,sex)>//name后+表示person里可以嵌套一个或多个name元素
<!ELEMENT person (name,age?,sex)>//age后?表示person里可以嵌套0个或多个age元素
<!ELEMENT person (name+,age*,sex)>//age后*表示person里可以嵌套任意个数age且可以为0
```
复杂元素定义中()里逗号隔开，xml中需按()内顺序定义元素，如果将逗号换为|表示子元素只能有一个定义出来

#### 使用dtd定义属性
##### 定义属性语法
```
<!ATTLIST 元素名称 
属性名称 属性的类型 属性的约束
>
```
##### 属性的类型
**CDATA:表示xml属性的取值为普通文本字符串**
**枚举(AA|BB|CC):xml属性值只能取其中一个**
**ID:xml属性的值只能有字母和_开头**

##### 属性的约束
**#REQUIRED:属性必须存在**
**#MPLED:属性可有可无**
**#FIXED:表示一个固定的值，示例：**`#FIXED: "ABC"`
**直接值：直接在属性的约束定义属性值，如果属性定义，默认使用直接值**

##### 定义实体
**DTD中定义，在xml中使用**
语法：`<!ENTITY 实体名称 实体内容>`
**xml中引用：**
`&实体名称;`
*注意：最好在内部dtd中定义实体，在外部定义，有些浏览器下，内容可能获取不到*

### Schema
**schema符合xml语法，与dtd不同的是一个xml中允许有多个个schema(通过名称空间进行区分)，schema支持除字符串更多数据类型(包括用户自定义数据类型)，schema约束更加严格，比dtd复杂**

#### 创建Schema文件
>1.创建后缀名为.xsd的XML Schema文件（schema与xml一样必须有一个根节点，而它的根节点就是`<schema></schema>`）
>2.设置schema的属性，xmlns属性表示当前文件是一个约束文件，targetNamespace属性提供引用该schema约束文件的自定义地址，
>3.看xml文件中有几个元素，写几个`<element></element>`
>4.判断xml中元素是简单元素还是复杂元素
>>简单元素：`<element name="简单元素名" type="数据类型"></element>`
>>复杂元素：`<element name="复杂元素名"><complexType>子元素</complexType></element>`

#### 引入Schema文件
**在xml文件的根节点中属性xmlns第一个内容同schema文件后追加`-instance`，第二个是targetNamespace中地址（由于xmlns重名，需要未其中一个设置别名，格式为`:别名`），最后配置targetNamespace中地址 schema的路径**
示例：
```
<person xmlns:xsi="http://www.w3c.org/2001/XMlSchema-instance" xmlns="http://www.zidingyi.cn/20200330 one.xsd">
	<name>二狗</name>
	<age>14</age>
</person>
```

#### Schema使用
>1. <sequence></sequence>:表示元素出现顺序
>2. <all></all>:表示元素只能出现一次
>3. <choice></choice>:表示只能出现里面元素中任意一个
>4. maxOccurs="unbounded":表示元素出现的次数
>5. <any></any>:表示任意元素
