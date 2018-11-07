# 1. 解析XML格式数据

参考文献：

- 参考文献一：[Carson_HO简书](https://www.jianshu.com/p/e636f4f8487b)
- 参考文献二：[菜鸟教程](http://www.runoob.com/w3cnote/android-tutorial-xml.html)
- 参考文献三：第一行代码

------

![](https://www.runoob.com/wp-content/uploads/2015/09/57964990.jpg)

**与html的区别**：

- html用于显示信息
- XML用于存储&传输信息

![](https://upload-images.jianshu.io/upload_images/944365-08971963f11142a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

## 语法

一个常见的XML文档一般由以下部分组成：

- 文档声明
- 元素（Element）
- 属性（Attribute）

**文档声明**

- 在XML文档的最前面，必须编写一个文档声明，用来声明XML文档的类型

  `<?xml version="1.0" ?>`

- 用`encoding`属性说明文档的字符编码

  `<?xml version="1.0" encoding="UTF-8" ?>`



**元素**

- 一个元素包括了开始标签和结束标签
- 拥有内容的元素：`<video>小黄人</video>`
- 没有内容的元素：`<video></video>`
- 没有内容的元素简写：`<video/>`

```xml
<videos>
    <video>
        <name>小黄人 第01部</name>
         <length>30</length>
    </video>
</videos>
```

- 规范的XML文档最多只有1个根元素，其他元素都是根元素的子孙元素
- **XML中的所有空格和换行，都会当做具体内容处理**



**属性**

- 一个元素可以拥有多个属性
  `<video name="小黄人 第01部" length="30" />`
- 属性值必须用 双引号"" 或者 单引号'' 括住

- 实际上，属性表示的信息也可以用子元素来表示，比如

  ```xml
  <video>
      <name>小黄人 第01部</name>
          <length>30</length>
  </video>
  ```



示例：

```xml
<bookstore>
  <book category="CHILDREN">
     <title lang="en"> Harry Potter </title>
     <author> JK.Rowling</author>
  </book>
<book category="WEB">
     <title lang="en"> woshiPM </title>
     <author>Carson_Ho</author>
  </book>
</bookstore>
```

> 其中，<bookstore>是根元素；<book>是子元素，也是元素类型之一；而<book>中含有属性，即category，属性值是CHILDREN；而元素<author>则拥有文本内容（ JK.Rowling）

- **XML元素命名规则**

1. 不能以数字或标点符号开头
2. 不能包含空格
3. 不能以xml开头

- **CDATA**
  不被解析器解析的文本数据，所有xml文档都会被解析器解析（cdata区段除外）
  `<![CDATA["传输的文本 "]]>`
- **PCDATA**
  被解析的字符数据



## XML树结构

ML文档中的元素会形成一种树结构，从根部开始，然后拓展到每个树叶（节点）

```xml
<？xml version ="1.0" encoding="UTF-8"?>
<简历>
     <基本资料>
     <求职意向>
     <自我评价>
     <其他信息>
     <联系方式>
     <我的作品>
 </简历
```

则树结构为：

![](https://upload-images.jianshu.io/upload_images/944365-b6f9ca5ebe95c00a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/808/format/webp)

XML文件是由节点构成的。它的第一个节点为“根节点”。一个XML文件必须**有且只能有一个根节点**，其他节点都必须是它的子节点。

![](https://upload-images.jianshu.io/upload_images/944365-5201c366b824c081.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/549/format/webp)

this 代表整个XML文件，它的根节点就是 this.firstChild 。 this.firstChild.childNodes 则返回由根节点的所有子节点组成的节点数组。

![](https://upload-images.jianshu.io/upload_images/944365-436068f942114f7c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/547/format/webp)

**每个子节点又可以有自己的子节点**。节点编号由0开始，根节点的第一个子节点为 this.firstChild.childNodes[0]，它的子节点数组就是this.firstChild.childNodes[0].childNodes 。

![](https://upload-images.jianshu.io/upload_images/944365-dfff6e8f4f15a8c9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/549/format/webp)

根节点第一个子节点的第二个子节点 this.firstChild.childNodes[0].childNodes[1]，它返回的是一个XML对象（Object） 。这里需要特别注意，节点标签之间的数据本身也视为一个节点 this.firstChild.childNodes[0].childNodes[1].firstChild ，而不是一个值。

我们解析XML的最终目的当然就是获得数据的值：this.firstChild.childNodes[0].childNodes[1].firstChild.nodeValue 。

> 请注意区分：节点名称（<性别></性别>）和之间的文本内容（男）可以当作是节点，也可以当作是一个值

> 节点：
>  名称：this.firstChild.childNodes[0].childNodes[1]
>  文本内容：this.firstChild.childNodes[0].childNodes[1].firstChild

> 值：
>  名称：this.firstChild.childNodes[0].childNodes[1].nodeValue
>  （节点名称有时也是我们需要的数据）
>  文本内容：this.firstChild.childNodes[0].childNodes[1].nodeName

------



## XML解析

`XML`的解析方式主要分为2大类：

![](https://upload-images.jianshu.io/upload_images/944365-2b14c40543dd16f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/760/format/webp)

### DOM方式

`Document Object Model`，即 文件对象模型，是 **一种 基于树形结构节点 & 文档驱动 的XML解析方法**

![](https://upload-images.jianshu.io/upload_images/944365-5aceda236a0d44c2.png)

**解析示例**：

```java
// 假设需要解析的XML文档如下（subject.xml）

<？xml version ="1.0" encoding="UTF-8"?>`
<code>
<language id="1">
    <name>Java</name>
    <usage>Android</usage>
 </language>
<language id="2">
    <name>Swift#</name>
    <usage>iOS</usage>
 </language>
<language id="3">
    <name>Html5</name>
   <usage>Web</usage>
 </language>
 </code>

// 解析的核心代码

public static List<subject> getSubjectList(InputStream stream)
   { tv = (TextView)findViewById(R.id.tv);
        try {
            //打开xml文件到输入流
            InputStream stream = getAssets().open("subject.xml");
            //得到 DocumentBuilderFactory 对象
            DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();
            //得到DocumentBuilder对象
            DocumentBuilder builder = builderFactory.newDocumentBuilder();
            //建立Document存放整个xml的Document对象数据
            Document document = builder.parse(stream);
            //得到 XML数据的"根节点" 
            Element element = document.getDocumentElement();
            //获取根节点的所有language的节点
            NodeList list = element.getElementsByTagName("language");
             //遍历所有节点
            for (int i= 0;i<=list.getLength();i++){
            //获取lan的所有子元素
                Element language = (Element) list.item(i);
            //获取language的属性（这里即为id）并显示
                tv.append(lan.getAttribute("id")+"\n");
          //获取language的子元素 name 并显示                       tv.append(sub.getElementsByTagName("name").item(0).getTextContent()+"\n");
         //获取language的子元素usage 并显示                    tv.append(sub.getElementsByTagName("usage").item(0).getTextContent()+"\n");
            }
```

![](https://upload-images.jianshu.io/upload_images/944365-60abffe0d9e510a2.png)

### SAX方式

 `Simple API for XML`，**一种 基于事件流驱动、通过接口方法解析 的XML解析方法**

![](https://upload-images.jianshu.io/upload_images/944365-4eebf15fb06a2f07.png)

使用SAX解析XML文档是，通常情况会新建一个类继承自**DefaultHandler**，重写父类的5个方法：

```java
public class MyHandler extends DefaultHandler{ 
    @Override 
    public void startDocument() throws SAXException{ 
    } 
 
    @Override 
    public void startElement(String uri,String localName,String qName, 
                     Attributes attributes) throws SAXException{ 
    } 
 
    @Override 
    public void characters(char[] ch,int start,int length) throws SAXException{ 
    } 
 
    @Override 
    public void endElement(String uri,String localName,String qName) 
              throws SAXException{ 
    } 
 
    @Override 
    public void endDocument() throws SAXException{ 
    } 
}
```

**注**：

- `startDocument()`方法在开始XML解析的时候调用
- `startElement()`方法会在开始解析某个节点的时候调用
- `characters()`方法在获取节点中的内容的时候调用
  - 获取节点内容时，character()方法可能会被调用多次，一些换行符也会被当作内容解析出来，需要针对这种情况做好控制
- `endElement()`方法在完成解析某个节点的时候调用
- `endDocument()`方法会在完成整个XML解析的时候调用



示例：

![](E:\document\photo\first-code\get_dataxml.jpg)

```java
public class ContentHandler extends DefaultHandler {

    private String nodeName;

    private StringBuilder id;

    private StringBuilder name;

    private StringBuilder version;

    @Override
    public void startDocument() throws SAXException {
        id = new StringBuilder();
        name = new StringBuilder();
        version = new StringBuilder();
    }

    @Override
    public void startElement(String uri, String localName, String qName, Attributes attributes) throws SAXException {
        // 记录当前结点名
        nodeName = localName;
    }

    @Override
    public void characters(char[] ch, int start, int length) throws SAXException {
        // 根据当前的结点名判断将内容添加到哪一个StringBuilder对象中
        if ("id".equals(nodeName)) {
            id.append(ch, start, length);
        } else if ("name".equals(nodeName)) {
            name.append(ch, start, length);
        } else if ("version".equals(nodeName)) {
            version.append(ch, start, length);
        }
    }

    @Override
    public void endElement(String uri, String localName, String qName) throws SAXException {
        if ("app".equals(localName)) {
            Log.d("ContentHandler", "id is " + id.toString().trim());
            Log.d("ContentHandler", "name is " + name.toString().trim());
            Log.d("ContentHandler", "version is " + version.toString().trim());
            // 最后要将StringBuilder清空掉
            id.setLength(0);
            name.setLength(0);
            version.setLength(0);
        }
    }

    @Override
    public void endDocument() throws SAXException {
        super.endDocument();
    }

}
```

- 解析函数

  ```java
  private void parseXMLWithSAX(String xmlData) {
      try {
          SAXParserFactory factory = SAXParserFactory.newInstance();
          XMLReader xmlReader = factory.newSAXParser().getXMLReader();
          ContentHandler handler = new ContentHandler();
          // 将ContentHandler的实例设置到XMLReader中
          xmlReader.setContentHandler(handler);
          // 开始执行解析
          xmlReader.parse(new InputSource(new StringReader(xmlData)));
      } catch (Exception e) {
          e.printStackTrace();
      }
  }
  ```

先创建一个SAXParserFactory的对象，在获取XMLReader对象，将ContentHandler的实例设置到XMLReader中，最后调用`parse()`方法开始执行解析

**特点&应用场景**

![](https://upload-images.jianshu.io/upload_images/944365-9a77c8fc23aea6bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

### PULL解析

一种 基于事件流驱动 的`XML`解析方法

![](https://upload-images.jianshu.io/upload_images/944365-297fb5d8c529a53e.png)

示例：

```java
private void parseXMLWithPull(String xmlData) {
    try {
        XmlPullParserFactory factory = XmlPullParserFactory.newInstance();
        XmlPullParser xmlPullParser = factory.newPullParser();
        xmlPullParser.setInput(new StringReader(xmlData));
        int eventType = xmlPullParser.getEventType();
        String id = "";
        String name = "";
        String version = "";
        while (eventType != XmlPullParser.END_DOCUMENT) {
            String nodeName = xmlPullParser.getName();
            switch (eventType) {
                // 开始解析某个结点
                case XmlPullParser.START_TAG: {
                    if ("id".equals(nodeName)) {
                        id = xmlPullParser.nextText();
                    } else if ("name".equals(nodeName)) {
                        name = xmlPullParser.nextText();
                    } else if ("version".equals(nodeName)) {
                        version = xmlPullParser.nextText();
                    }
                    break;
                }
                // 完成解析某个结点
                case XmlPullParser.END_TAG: {
                    if ("app".equals(nodeName)) {
                        Log.d("MainActivity", "id is " + id);
                        Log.d("MainActivity", "name is " + name);
                        Log.d("MainActivity", "version is " + version);
                    }
                    break;
                }
                default:
                    break;
            }
            eventType = xmlPullParser.next();
        }
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

**特点&应用场景**：

![](https://upload-images.jianshu.io/upload_images/944365-2054af8a3573ca46.png)

![](https://upload-images.jianshu.io/upload_images/944365-43d765e1dd96264a.png)

------

# 2. 解析JSON格式数据

**Json是什么**？

**JavaScript Object Natation,** 一种轻量级的数据交换格式, 与XML一样, 广泛被采用的客户端和服务端交互的解决方案！具有良好的可读和便于快速编写的特性。

**JSON**与**XML**的比较：

- JSON和XML的数据可读性基本相同;
- JSON和XML同样拥有丰富的解析手段
- JSON相对于XML来讲，数据的体积小
- JSON与JavaScript的交互更加方便
- JSON对数据的描述性比XML较差
- JSON的速度要远远快于XML

![](https://upload-images.jianshu.io/upload_images/944365-c18275e60ecc9272.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

## 语法

- 1个JSON文件里含多个数据，这些数据 以 `JSON`值 的形式 存在

```JSON
// JSON实例
｛"skill":{
          "web":[
                 {
                  "name":"html",
                  "year":"5"
                 },
                 {
                  "name":"ht",
                  "year":"4"
                 }],
           "database":[
                  {
                  "name":"h",
                  "year":"2"
                 }]
`}}
```

- 1个`JSON`值的内容形式可以是：**”名称 - 值“对、数组 或 对象**，下面将详细说明

![](https://upload-images.jianshu.io/upload_images/944365-c20ecfe01e9bcde3.png)

## JSON解析

![](https://upload-images.jianshu.io/upload_images/944365-627212e897321623.png)

### Android studio自带的org.json解析

- **解析原理**：基于文档驱动
- **解析流程**：把全部文件读入到内存中 ->> 遍历所有数据 ->> 根据需要检索想要的数据

示例：

```java
private void parseJSONWithJSONObject(String jsonData) {
    try {
        JSONArray jsonArray = new JSONArray(jsonData);
        for (int i = 0; i < jsonArray.length(); i++) {
            JSONObject jsonObject = jsonArray.getJSONObject(i);
            String id = jsonObject.getString("id");
            String name = jsonObject.getString("name");
            String version = jsonObject.getString("version");
            Log.d("MainActivity", "id is " + id);
            Log.d("MainActivity", "name is " + name);
            Log.d("MainActivity", "version is " + version);
        }
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

（假设文档定义是一个数组）

首先是将服务器返回的数据传入到一个**JSONArry**对象中，然后循环遍历这个**JSONArray**，从中取出的每一个元素都是**JSONObject**对象，每个JSONObject对象又会包含id、name和version这些数据。只需调用**getString()**方法将这些数据取出，打印即可

### GSON解析

GSON是Google的开源库，

- **解析原理**：基于事件驱动
- **解析流程**：根据所需取的数据 建立1个对应于`JSON`数据的`JavaBean`类，即可通过简单操作解析出所需数据

**具体使用**:

- **创建一个与JSON数据对应的JavaBean类（用作存储需要解析的数据）**

  Gson解析关键是根据JSON数据写出一个对应的JavaBean，规则如下：

  ![](https://upload-images.jianshu.io/upload_images/944365-3e8699e02592ef02.jpg)

  ```java
  /** 
    * 简单转换
    */ 
          // JSON数据1
          String json = "{\"id\":1,\"name\":\"小明\",\"sex\":\"男\",\"age\":18,\"height\":175}";
  
          // 对应的JavaBean类
          public class EntityStudent {
          private int id;
          private String name;
          private String sex;
          private int age;
          private int height;
  
          public void setId(int id){
              this.id = id;
          }
          public void setName(String name){
              this.name = name;
          }
          public void setSex(String sex){
              this.sex = sex;
          }
          public void setAge(int age){
              this.age = age;
          }
          public void setHeight(int height){
              this.height = height;
          }
          public int getId(){
              return id;
          }
          public String getName(){
              return name;
          }
          public String getSex(){
              return sex;
          }
          public int getAge(){
              return age;
          }
          public int getHeight(){
              return  height;
          }
          public void show(){
                      System.out.print("id=" + id + ",");
                      System.out.print("name=" + name+",");
                      System.out.print("sex=" + sex+",");
                      System.out.print("age=" + age+",");
                      System.out.println("height=" + height + ",");
  
          }
          }
  
  /** 
    * 复杂转换
    */ 
          // JSON数据2（具备嵌套）
          {"translation":["车"],
            "basic":
              {
                "phonetic":"kɑː",
                "explains":["n. 汽车；车厢","n. (Car)人名；(土)贾尔；(法、西)卡尔；(塞)察尔"]},
            "query":"car",
            "errorCode":0,
            "web":[{"value":["汽车","车子","小汽车"],"key":"Car"},
                   {"value":["概念车","概念车","概念汽车"],"key":"concept car"},
                   {"value":["碰碰车","碰撞用汽车","碰碰汽车"],"key":"bumper car"}]
          }
  
          // 对应的复杂的JSON数据对应的JavaBean类
          public class student {
              public String[] translation;    //["车"]数组
              public basic basic;             //basic对象里面嵌套着对象，创建一个basic内部类对象
              public  static class basic{     //建立内部类
                  public String phonetic;
                  public String[] explains;
              }
              public String query;
              public int errorCode;
              public List<wb> web;            //web是一个对象数组，创建一个web内部类对象
              public static class wb{         
                      public String[] value;
                      public String key;
                  }
  
              public void show(){
                  //输出数组
                  for (int i = 0;i<translation.length;i++)
                  {
                  System.out.println(translation[i]);
                  }
                  //输出内部类对象
                  System.out.println(basic.phonetic);
                  //输出内部类数组
                  for (int i = 0;i<basic.explains.length;i++){
                      System.out.println(basic.explains[i]);
                  }
                  System.out.println(query);
                  System.out.println(errorCode);
                  for (int i = 0;i<web.size();i++){
                      for(int j = 0; j<web.get(i).value.length;j++)
                      {
                          System.out.println(web.get(i).value[j]);
                      }
                      System.out.println(web.get(i).key);
                  }
              }
              }
  ```

- 导入GSON库

- 使用GSON进行解析

  ```java
  public class App {
  
      private String id;
  
      private String name;
  
      private String version;
  
      public String getId() {
          return id;
      }
  
      public void setId(String id) {
          this.id = id;
      }
  
      public String getName() {
          return name;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  
      public String getVersion() {
          return version;
      }
  
      public void setVersion(String version) {
          this.version = version;
      }
  
  }
  ```

  ```java
  private void parseJSONWithGSON(String jsonData) {
      Gson gson = new Gson();
      List<App> appList = gson.fromJson(jsonData, new TypeToken<List<App>>() {}.getType());
      for (App app : appList) {
          Log.d("MainActivity", "id is " + app.getId());
          Log.d("MainActivity", "name is " + app.getName());
          Log.d("MainActivity", "version is " + app.getVersion());
      }
  }
  ```

**注**：

- 若一段JSON格式数据为：`{"name":"Tom","age":20}`，则可以定义一个Person类，加入name和age这两个字段，简单调用下述代码可以将JSON数据解析成一个Person对象

  ```java
  Gson gson = new Gson();
  Person person = gson.fromJson(jsonData, Person.class);
  ```

![](https://upload-images.jianshu.io/upload_images/944365-0c6f7862ce6df961.png)



JSON与XML解析对比：

![](https://upload-images.jianshu.io/upload_images/944365-f185987f8f2ab459.png)







