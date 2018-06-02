---
title: XML 解析和约束
tags: XML dom4j DTD Schema
---

> extendsible markup language 可扩展标记语言 和 **它的样子** 和 **使用(解析)** 和 **约束**

<!--more-->
## 一. XML 作用
- 保存数据 --- 少量数据
- 做配置文件 --- tomcat的server.xml
- 数据传输载体 --- 服务端将很多报错信息封装到 xml 文件中,返回给客户端解析

## 二. XML 长什么样
XML 本质上是一个树状结构
```xml
<?xml version="1.0" encoding="UTF-8"?>
<stus>
    <stu>
        <name>fan</name>
        <age>52</age>
    </stu>
</stus>
```

## 三. XML 组成元素
> 文档声明 / 标签元素 / 属性 / 注释 / CDATA 区

### 1. 文档声明
```xml
<!--解析这个xml的时候，使用什么版本的解析器解析; 使用什么编码来翻译; 该文档会依赖关联其他文档 -->
<?xml version="1.0" encoding="UTF-8" standalone="no" ?>
```

### 2. 标签元素
> 根标签 / 空标签 / 简单标签 / 复杂标签

- 根标签是文档声明后的第一个标签
- 空标签一般配合**属性**使用, 如: `<age/>`
- 复杂标签是指标签中嵌套了其它标签
- 简单标签只包含普通文字

### 3. CDATA 区
> 相当于转义字符, 将 xml 的关键字和非法字符替换为实体引用

- 非法字符  
严格地讲，在 XML 中仅有字符 "<"和"&" 是非法的。省略号、引号和大于号是合法的，但是把它们替换为实体引用是个好的习惯  
`< 对应  &lt;  & 对应  &amp;`  

- CDATA 处理标签  
如果标签文本中含有标签或者关键字, 不想让 XML 解析器解析, 可以用 CDATA 包裹起来.(CDATA 并不常见, **通常在服务器给客户端返回html数据的时候**)  

```xml
<des><![CDATA[<a href="http://www.bing.com">必应</a>]]></des>
```

## 四. XML 使用 -- 解析
> 其实就是获取元素里面的**文本数据**或者**属性数据**。

### 1. XML 解析的方式
> 解析方式有很多种, 但是常用的有两种: **DOM** 和 **SAX**

![](/assets/img/blog/2018/2018-06-02-xml-parse-2-type.png)  

针对以上两种解析方式, 会使用到哪些工具?  
- jaxp(比较繁琐)
- jdom
- dom4j(广泛使用)

### 2. dom4j 使用
> dom4j 既可以用 DOM 解析, 也可以用 SAX 解析

dom4j **获得元素** 的基本用法:  

```bash
    element.element("stu") : 返回该元素下的第一个stu元素
    element.elements(); 返回该元素下的所有子元素。
```

dom4j中的 `SAXReader` 是增强版的的`SAX`解析方式 , 可进行增删操作(原生 `SAX` 不可以)  

```java
try {
            // 1. 创建sax读取对象
            SAXReader reader = new SAXReader();
            // 2. 指定解析的xml源
            Document document = reader.read(new File("src/xmltest/stus.xml"));

            // 3. 得到元素、
            // 得到根元素
            Element rootElement = document.getRootElement();
            System.out.println(rootElement.element("stu").element("name").getText());

            //获取根元素下面的所有 stu 子元素
            List<Element> elements = rootElement.elements();

            // 遍历所有的stu元素
            for (Element element : elements){
                // 获取stu元素下面的name元素
                String name = element.element("name").getText();
                String age = element.element("age").getText();
                System.out.println("name: "+name + ", age: "+age);
            }
        } catch (DocumentException e) {
            e.printStackTrace();
        }
```
> - 上述代码摘自 dom4j.jar 的 docs 文档下的 example, 可以帮助新手快速开始使用dom4j,
  - 查阅 dom4j api 文档, 获取更多详细使用方法

### 3. dom4j + Xpath 快速定位某一元素
dom4j 在获得元素值的时候, 需要先获得父元素, 然后通过父元素获得子元素, 再通过子元素获得孙元素 .....  
这个过程要求从根标签开始, 逐层获取内嵌标签元素, 如果一个标签嵌套的层级很深, 可以想象为了取出这个标签值, 需要用 `element.element("grandfather").element("father")......` 逐个取出.  

怎样快速定位某一被**深嵌套**的元素?  

dom4j里面支持Xpath的写法。 xpath其实是xml的路径语言，支持我们在解析xml的时候，能够快速的定位到具体的某一个元素。  

`XPath` 语法规则和作用很像`正则表达式`;  

**使用 XPath 语法前, 必须导入 jaxen-1.1-beta-6.jar 依赖(该依赖在 dom4j/lib 目录下),**

```java
            System.out.println("=======使用 XPath 快速定位元素=========");

            //要想使用Xpath， 还得添加支持的jar 获取的是第一个 只返回一个。
            Element nameElement = (Element) rootElement.selectSingleNode("//name");
            System.out.println(nameElement.getText());

            System.out.println("----------------");

            //获取文档里面的所有name元素
            List<Element> list = rootElement.selectNodes("//name");
            for (Element element : list) {
                System.out.println(element.getText());
            }
```
其它 `XPath` 快速查找方法, 参见 `XPathTutorial.chm` 手册文档

## 五. XML 约束
### 1. 约束的作用
一般的`.xml`文件很自由, 可以用来存储任何格式的文本数据, 但有时候, 我们需要对存储的数据做限定, 比如, 一个`.xml`中标签的id属性不可重复 / 在某个父标签下的子标签只能为特定的指定标签, 不能出现其它标签  

XML 约束就像是一种规范, 给原生的xml文件制定了规则, 告诉 xml 文件哪些标签不可以存, 哪些标签可以存, 以及,怎样存.  

### 2. 约束文件长什么样?
> 常见的约束文件有两种: DTD 和 Schema

DTD 是很早以前就出现的, 语法自成一派(XML 解析器无法直接解析), 可读性较差; 为了取代 DTD, 推出了Schema(Schema就是一个特殊的xml文件, 可以直接用 xml 解析器解析)  

DTD 文件的写法:  
```xml
<?xml version="1.0"?>
<!--第一行写根标签-->
<!ELEMENT stus (stu)>
<!--括号表示该标签下还有其它标签-->
<!ELEMENT stu     (name,age)>
<!--#PCDATA 表示要被解析-->
<!ELEMENT name    (#PCDATA)>
<!ELEMENT age     (#PCDATA)>
```

Schema 文件的写法:  
```xml
<?xml version="1.0" encoding="utf-8"?>
<!--xmlns : xsd 也是一个 xml , 命名空间指定 w3c 的在线 xml 约束-->
<!--targetNamespace : 目标命名空间: 该文件中所有元素都与这个目标空间绑定-->
<!--elementFormDefault : 元素格式化情况-->
<schema xmlns="http://www.w3.org/2001/XMLSchema"
        targetNamespace="http://www.example.org/teacher"
        elementFormDefault="qualified">

    <element name="teachers">
        <complexType>
            <!--表示 teachers 标签下有无数个 teacher-->
            <sequence maxOccurs="unbounded">
                <element name="teacher">
                    <!--表示 teacher 是一个复杂元素-->
                    <complexType>
                        <sequence>
                            <element name="name" type="string"></element>
                            <element name="age" type="int"></element>
                        </sequence>
                    </complexType>
                </element>
            </sequence>
        </complexType>
    </element>
</schema>
```
写`.xsd`文件时, 先从简单元素写起, 然后将简单元素内嵌到复杂元素中

### 3. xml 约束文件的使用
> 怎样在 xml 文件中引入约束文件? 就像在 html 文件中引入对应的 css 文件一样

#### 3.1 引入 DTD 约束
> 三种引入方式: 在线  / 单独本地文件 / 内嵌本地文件

- 在线
```xml
<!-- 文档类型  根标签名字 网络上的dtd   dtd的名称   dtd的路径-->
<!DOCTYPE stus PUBLIC "//UNKNOWN/" "unknown.dtd">
```

- 单独本地文件
```xml
<!-- 引入本地的DTD  ： 根标签名字 引入本地的DTD  dtd的位置 -->
<!DOCTYPE stus SYSTEM "stus.dtd">
```

- 内嵌本地文件
```xml
<!DOCTYPE stus [
   		<!ELEMENT stus (stu)>
   		<!ELEMENT stu (name,age)>
   		<!ELEMENT name (#PCDATA)>
   		<!ELEMENT age (#PCDATA)>
   	]>

   	<stus>
   		<stu>
   			<name>张三</name>
   			<age>18</age>
   		</stu>
   	</stus>
```

#### 3.2 引入 Schema 约束
实例文档(Schema约束的引用写在实例文档的根标签中):  
```xml
<?xml version="1.0" encoding="UTF-8"?>
		<!-- xmlns:xsi : 这里必须是这样的写法，也就是这个值已经固定了。
		xmlns : 这里是名称空间，也固定了，写的是约束文件里面的 目标名称空间
		xsi:schemaLocation : 有两段： 前半段是名称空间，也是目标空间的值 ， 后面是约束文档的路径。
		 -->
		<teachers
			xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
			xmlns="http://www.example.org/teacher"
			xsi:schemaLocation="http://www.example.org/teacher teacher.xsd"
		>
			<teacher>
				<name>zhangsan</name>
				<age>19</age>
			</teacher>
			<teacher>
				<name>lisi</name>
				<age>29</age>
			</teacher>
			<teacher>
				<name>lisi</name>
				<age>29</age>
			</teacher>
		</teachers>
```

#### 3.3 关于命名空间
> 命名空间的作用在于告诉 xml 文件, 该使用哪个 xml 约束

一个文件只可以引入一个 dtd 约束, 但可以引入多个 schema 约束, 当引入多个 Schema 约束时, 需要在命名空间处给该`目标名称空间`取一个别名, 防止多个 schema 文件对同一个元素进行不同约束时, 告诉 xml 解析器, 该使用哪个约束文件的约束条件.  

```xml
  <!-- 假设引入 aa 和 bb 两个约束文件 -->
<teachers
			xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
			xmlns:aa="http://www.example.org/teacher"
      xmlns:bb="http://www.example2.org/teacher"
			xsi:schemaLocation="http://www.example.org/teacher teacher.xsd"
		>

    <teacher>
        <!-- aa 和 bb 都对 name 进行约束, 此处显式指定使用 aa 文件的约束方式 -->
				<aa:name>zhangsan</name>
				<age>19</age>
			</teacher>
```

## 六. 参考手册
- W3C 在线 XML 手册(包含 xml dtd schema)
- dom4j 解压后 docs/index.html 下的使用手册
- dom4j API 手册
- Xpach 手册

卒.
