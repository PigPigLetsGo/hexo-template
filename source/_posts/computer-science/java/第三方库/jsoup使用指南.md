---
title: jsoup
categories:
   - [计算机学科,java,第三方库]
tags:
   - 计算机学科
   - java
   - 第三方库
   - 爬虫
---

# 一、jsoup概述

  jsoup 是一款基于 Java 的HTML解析器，它提供了一套非常省力的API，不但能直接解析某个URL地址、HTML文本内容，而且还能通过类似于DOM、CSS或者jQuery的方法来操作数据，所以 jsoup 也可以被当做爬虫工具使用。

------

# 二、相关概念简介

-  `Document` ：文档对象。每份HTML页面都是一个文档对象，Document 是 jsoup 体系中最顶层的结构。
-  `Element`：元素对象。一个 Document 中可以着包含着多个 Element 对象，可以使用 Element 对象来遍历节点提取数据或者直接操作HTML。
-  `Elements`：元素对象集合，类似于`List<Element>`。
-  `Node`：节点对象。标签名称、属性等都是节点对象，节点对象用来存储数据。
-  **类继承关系**：Document 继承自 Element ，Element 继承自 Node。
-  **一般执行流程**：先获取 Document 对象，然后获取 Element 对象，最后再通过 Node 对象获取数据。 

![image-20240308180038864](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240308180038864.png)

------

# 三、获取文档（Document）

>  获得文档对象 Document 一共有4种方法，分别对应不同的获取方式。

正式开始之前，我们需要导入有关 jar 包。

```xml
xml复制代码<dependency>
    <groupId>org.jsoup</groupId>
    <artifactId>jsoup</artifactId>
    <version>1.15.1</version>
</dependency>
```

\--

#### 3.1）从URL中加载文档对象（常用）

使用 `Jsoup.connect(String url).get()`方法获取（只支持 http 和 https 协议）:

```java
java复制代码Document doc = Jsoup.connect("http://csdn.com/").get();

String title = doc.title();
System.out.println(title);
```

`connect(String url)`方法创建一个新的 Connection并通过`.get()`或者`.post()`方法获得数据。如果从该URL获取HTML时发生错误，便会抛出 IOException，应适当处理。

`Connection` 接口还提供一个方法链来解决特殊请求，我们可以在发送请求时带上请求的头部参数，具体如下：

```java
java复制代码Document doc = Jsoup.connect("http://csdn.com")
  .data("query", "Java")
  .userAgent("Mozilla")
  .cookie("auth", "token")
  .timeout(8000)
  .post();
  System.out.println(doc);
```

\--

#### 3.2）从本地文件中加载文档对象

可以使用静态的`Jsoup.parse(File in, String charsetName)` 方法从文件中加载文档。其中`in`表示路径，`charsetName`表示编码方式，示例代码：

```java
java复制代码File input = new File("/tmp/input.html");
Document doc = Jsoup.parse(input, "UTF-8");
System.out.println(doc);
```

\--

#### 3.3）从字符串文本中加载文档对象

使用静态的`Jsoup.parse(String html)` 方法可以从字符串文本中获得文档对象 Document ，示例代码：

```java
java复制代码String html = "<html><head><title>First parse</title></head>"
  + "<body><p>Parsed HTML into a doc.</p></body></html>";

Document doc = Jsoup.parse(html);
System.out.println(doc);
```

\--

#### 3.4）从<body>片断中获取文档对象

使用`Jsoup.parseBodyFragment(String html)`方法.

```java
java复制代码String html = "<p>Lorem ipsum.</p>";
Document doc = Jsoup.parseBodyFragment(html);
// doc 此时为：<body> <p>Lorem ipsum.</p></body>

Element body = doc.body();
System.out.println(body);
```

`parseBodyFragment` 方法创建一个新的文档，并插入解析过的HTML到`body`元素中。假如你使用正常的 `Jsoup.parse(String html)` 方法，通常也能得到相同的结果，但是明确将用户输入作为 body 片段处理是个更好的方式。

`Document.body()` 方法能够取得文档body元素的所有子元素，与 `doc.getElementsByTag("body")`相同。

------

# 四、选择元素（Element）

>  解析文档对象并获取数据一共有 2 种方式，分别为 DOM方式、CSS选择器方式，我们可以选择一种自己喜欢的方式去获取数据，效果一样。

#### 4.1）DOM方式

将HTML解析成一个`Document`之后，就可以使用类似于DOM的方法进行操作。

```java
java复制代码// 获取csdn首页所有的链接
Document doc       = Jsoup.connect("http://csdn.com").get();

Elements elements  = doc.getElementsByTag("body");
Elements contents  = elements.first().getElementsByTag("a");

for (Element content : contents) {
    String linkHref = content.attr("href");
    String linkText = content.text();
    System.out.print(linkText+"\t");
    System.out.println(linkHref);
}
```

**说明**

`Elements`这个对象提供了一系列类似于DOM的方法来查找元素，抽取并处理其中的数据。具体如下：

**4.1.1）查找元素**

-  `getElementById(String id)`：通过id来查找元素
-  `getElementsByTag(String tag)`：通过标签来查找元素
-  `getElementsByClass(String className)`：通过类选择器来查找元素
-  `getElementsByAttribute(String key)` ：通过属性名称来查找元素，例如查找带有href元素的标签。
-  `siblingElements()`：获取兄弟元素。如果元素没有兄弟元素，则返回一个空列表。
-  `firstElementSibling()`：获取第一个兄弟元素。
-  `lastElementSibling()`：获取最后一个兄弟元素。
-  `nextElementSibling()`：获取下一个兄弟元素。
-  `previousElementSibling()`：获取上一个兄弟元素。
-  `parent()`：获取此节点的父节点。
-  `children()`：获取此节点的所有子节点。
-  `child(int index)`：获取此节点的指定子节点。

**4.1.2）获取元素数据**

-  `attr(String key)`：获取单个属性值
-  `attributes()`：获取所有属性值
-  `attr(String key, String value)`：设置属性值
-  `text()`：获取文本内容
-  `text(String value)`：设置文本内容
-  `html()`：获取元素内的HTML内容
-  `html(String value)`：设置元素内的HTML内容
-  `outerHtml()`：获取元素外HTML内容
-  `data()`：获取数据内容（例如：script和style标签)
-  `id()`：获得id值（例：`<p id="goods">衣服</p>`）
-  `className()`：获得第一个类选择器值
-  `classNames()`：获得所有的类选择器值
-  `tag()`：获取元素标签
-  `tagName()`：获取元素标签名（如：<p>、<div>等）

**4.1.3）操作HTML文本**

-  `append(String html)`：在末尾追加HTML文本
-  `prepend(String html)`：在开头追加HTML文本
-  `html(String value)`：在匹配元素内部添加HTML文本。

\--

#### 4.2）CSS选择器方式

可以使用类似于CSS选择器的语法来查找和操作元素，常用的方法为`select(String selector)`。

```java
java复制代码Document doc = Jsoup.connect("http://csdn.com").get();

// 获取带有 href 属性的 a 元素
Elements elements = doc.select("a[href]");

for (Element content : elements) {
    String linkHref = content.attr("href");
    String linkText = content.text();
    System.out.print(linkText + "\t");
    System.out.println(linkHref);
}
```

**4.2.1）说明**

`select()`方法在`Document`、`Element`或`Elements`对象中都可以使用，而且是上下文相关的，因此可实现指定元素的过滤，或者采用链式访问。

`select()` 方法将返回一个`Elements`集合，并提供一组方法来抽取和处理结果。

**4.2.2）`select(String selector)`方法参数简介**

-  `tagname`: 通过标签查找元素，例如通过`"a"`来查找`<a>`标签。
-  `#id`: 通过ID查找元素，比如通过`#logo`查找`<p id="logo">`。
-  `.class`: 通过class名称查找元素，比如通过`.titile`查找`<p class="titile">`。
-  `ns|tag`: 通过标签在命名空间查找元素，比如使用 `fb|name` 来查找 `<fb:name>` 。
-  `[attribute]`: 利用属性查找元素，比如通过`[href]`查找`<a href="...">`。
-  `[^attribute]`: 利用属性名前缀来查找元素，比如：可以用`[^data-]` 来查找带有HTML5 dataset属性的元素。
-  `[attribute=value]`: 利用属性值来查找元素，比如：`[width=500]`。
-  `[attribute^=value]`, `[attribute$=value]`, `[attribute*=value]`: 利用匹配属性值开头、结尾或包含属性值来查找元素，比如通过`[href*=/path/]`来查找`<a href="a/path/c.html">`。
-  `[attribute~=regex]`: 利用属性值匹配**正则表达式**来查找元素，比如通过 `img[src~=(?i)\.(png|jpe?g)]`来匹配所有的png或者jpg、jpeg格式的图片。
-  `*`: 通配符，匹配所有元素。

**4.2.3）参数属性组合使用**

-  `el#id`: 元素+ID，比如： `div#logo`
-  `el.class`: 元素+class，比如： `div.masthead`
-  `el[attr]`: 元素+class，比如 `a[href]`匹配所有带有 href 属性的 a 元素。
-  任意组合，比如：`a[href].highlight`匹配所有带有 href 属性且class="highlight"的 a 元素。
-  `ancestor child`: 查找某个元素下子元素，比如：可以用`.body p` 查找在"body"元素下的所有 `p`元素
-  `parent > child`: 查找某个父元素下的直接子元素，比如：可以用`div.content > p` 查找 `p` 元素，也可以用`body > *` 查找body标签下所有直接子元素
-  `siblingA + siblingB`: 查找在A元素之前第一个同级元素B，比如：`div.head + div`
-  `siblingA ~ siblingX`: 查找A元素之前的同级X元素，比如：`h1 ~ p`
-  `el, el, el`:多个选择器组合，查找匹配任一选择器的唯一元素，例如：`div.masthead, div.logo`

**4.2.4）特殊参数：伪选择器**

-  `:lt(n)`: 查找哪些元素的同级索引值（它的位置在DOM树中是相对于它的父节点）小于n，比如：`td:lt(3)` 表示小于三列的元素
-  `:gt(n)`:查找哪些元素的同级索引值大于`n``，比如`： `div p:gt(2)`表示哪些div中有包含2个以上的p元素
-  `:eq(n)`: 查找哪些元素的同级索引值与`n`相等，比如：`form input:eq(1)`表示包含一个input标签的Form元素
-  `:has(seletor)`: 查找匹配选择器包含元素的元素，比如：`div:has(p)`表示哪些div包含了p元素
-  `:not(selector)`: 查找与选择器不匹配的元素，比如： `div:not(.logo)` 表示不包含 class=logo 元素的所有 div 列表
-  `:contains(text)`: 查找包含给定文本的元素，搜索不区分大不写，比如： `p:contains(jsoup)`
-  `:containsOwn(text)`: 查找直接包含给定文本的元素
-  `:matches(regex)`: 查找哪些元素的文本匹配指定的正则表达式，比如：`div:matches((?i)login)`
-  `:matchesOwn(regex)`: 查找自身包含文本匹配指定正则表达式的元素
-  注意：上述伪选择器索引是从0开始的，也就是说第一个元素索引值为0，第二个元素index为1等

------

# 五、获取数据（Node）

>  在获得文档对象并且指定查找元素后，我们就可以获取元素中的数据。 这些访问器方法都有相应的setter方法来更改数据。

-  `.attr(String key)` ：获得属性的值。
-  `.text()`：获得元素中的文本。
-  `.html()`：获得元素或属性**内部**的HTML内容（不包括本身）。
-  `.outerHtml()`：获得元素或属性**完整**的HTML内容。
-  `.id()`：获得元素id属性值。
-  `className()`：获得元素类选择器值。
-  `.tagName()`：获得元素标签命名。
-  `.hasClass(String className)`：检查这个元素是否含有一个类选择器（不区分大小写）。

```java
java复制代码String   html = "<p><a href='http://csdn.com/'><b>example</b></a> link.</p>";
Document doc  = Jsoup.parse(html);

// 查找第一个<a>元素
Element link = doc.select("a").first();

// 输出：example
String text     = link.text();
// 输出：http://csdn.com/
String href = link.attr("href");
// 输出：<b>example</b>
String aHtml     = link.outerHtml();
// 输出：<a href='http://csdn.com/'><b>example</b></a>
String aOuterHtml     = link.outerHtml();
```

\--

------

# 六、修改数据

>  在解析了一个Document对象之后，你可能想修改其中的某些属性值，并把它输出到前台页面或保存到其他地方，jsoup对此提供了一套非常简便的接口（支持链式写法）。

#### 6.1）设置属性的值

当以下方法针对`Element`对象操作时，只有一个元素会受到影响。当针对`Elements`对象进行操作时，可能会影响到多个元素。

-  `.attr(String key, String value)`：设置标签的属性值。
-  `.addClass(String className)`：增加类选择器选项
-  `.removeClass(String className)`：删除对应的类选择器

```java
java复制代码Document doc = Jsoup.connect("http://csdn.com").get();

// 复数，Elements
Elements elements = doc.getElementsByClass("text");
// 单数，Element
Element element = elements.first();

// 复数对象，所有 class="text" 的元素都将受到影响
elements.attr("name","goods");
// 单数对象，只有一个元素会受到影响（链式写法）
element.attr("name","shop")
        .addClass("red");
```

#### 6.2）修改元素的HTML内容

可以使用`Element`中的HTML设置方法具体如下：

-  `.html(String value)`：这个方法将先清除元素中的HTML内容，然后用传入的HTML代替。
-  `.prepend(String value)`：在元素**前**添加html内容。
-  `.append(String value)`：在元素**后**添加html内容。
-  `.wrap(String value)`：对元素包裹一个外部HTML内容，将元素置于新增的内容中间。

```java
java复制代码Document doc = Jsoup.connect("http://csdn.com").get();

Element div = doc.select("div").first();
div.html("<p>csdn</p>");
div.prepend("<p>a</p>");
div.append("<p>good</p>");
// 输出：<div"> <p>a</p> <p>csdn</p> <p>good</p> </div>

Element span = doc.select("span").first();
span.wrap("<li><a href='...'></a></li>");
// 输出: <li><a href="..."> <span>csdn</span> </a></li>
```

#### 6.3）修改元素的文本内容

对于传入的文本，如果含有像 `<`, `>` 等这样的字符，将以文本处理，而非HTML。

-  `.text(String text)` ：清除元素内部的HTML内容，然后用提供的文本代替。
-  `.prepend(String first)`：在元素**后**添加文本节点。
-  `Element.append(String last)`：在元素**前**添加文本节点。

```java
java复制代码 // <div></div>
Element div = doc.select("div").first();

div.text(" one "); 
div.prepend(" two ");
div.append(" three ");
// 输出: <div> two one three </div>
```

------

# 七、其他功能

#### 7.1）相对路径转绝对路径

**问题描述**：  你有一个包含相对URLs路径的HTML文档，现在需要将这些相对路径转换成绝对路径的URLs。

**解决方式**：

1. 确保在你解析文档时有指定`base URI`路径。
2. 然后使用 `abs:` 属性前缀来取得包含`base URI`的绝对路径。代码如下：

```java
java复制代码Document doc = Jsoup.connect("http://www.open-open.com").get();
Element link = doc.select("a").first();

// 输出：/
String relHref = link.attr("href");

// 输出：http://www.open-open.com/
String absHref = link.attr("abs:href");
```

**说明**：

在HTML元素中，URLs经常写成相对于文档位置的相对路径，如：`<a href="/download">...</a>`。当你使用 `.attr(String key)` 方法来取得a元素的href属性时，它将直接返回在HTML源码中指定的值。

假如你需要取得一个绝对路径，需要在属性名前加 `abs:` 前缀，这样就可以返回包含根路径的URL地址`attr("abs:href")`。因此在解析HTML文档时，定义base URI非常重要。

如果你不想使用`abs:` 前缀，还有一个方法能够实现同样的功能 `.absUrl(String key)`。

\--

#### 7.2）消除不受信任的HTML (防止XSS攻击)

**问题描述**：

  在某些网站中经常会提供用户评论的功能，但是有些不怀好意的用户，会搞一些脚本到评论内容中，而这些脚本可能会破坏整个页面的行为，更严重的是获取一些机要信息，此时需要清理该HTML，以避免跨站脚本攻击（XSS）。

**解决方式**：  使用`clean()`方法清除恶意代码，但需要指定一个配置的 `Safelist`（旧版本中是`Whitelist`），通常使用`Safelist.basic()`即可。`Safelist`的工作原理是将输入的 HTML 内容单独隔离解析，然后遍历解析树，只允许已知的安全标签和属性输出。

```java
java复制代码String unsafe = 
        "<p><a href='http://csdn.com/' onclick='attack()'>Link</a></p>";
        
// 输出: <p><a href="http://csdn.com/" >Link</a></p>
String safe = Jsoup.clean(unsafe, Safelist.basic());
System.out.println(safe);
```

**说明**：

  jsoup的`Safelist`不仅能够在服务器端对用户输入的HTML进行过滤，只输出一些安全的标签和属性，还可以限制用户可以输入的标签范围。