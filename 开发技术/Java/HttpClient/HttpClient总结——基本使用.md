# HttpClient总结一之基本使用

最近工作中是做了一个handoop的hdfs系统的文件浏览器的功能，是利用webhdfs提供的rest api来访问hdfs来与hdfs进行交互的，其中大量使用HttpClient，之前一直很忙，没什么时间来总结，今天闲下来了，可以来好好总结一下这个东西了。

## 1.HttpClient简介

http协议可以说是现在Internet上面最重要，使用最多的协议之一了，越来越多的java应用需要使用http协议来访问网络资源，特别是现在rest api的流行，HttpClient 是 Apache Jakarta Common 下的子项目，用来提供高效的、最新的、功能丰富的支持 HTTP 协议的客户端编程工具包，并且它支持 HTTP 协议最新的版本和建议。HttpClient 已经应用在很多的项目中，比如 Apache Jakarta 上很著名的另外两个开源项目 Cactus 和 HTMLUnit 都使用了 HttpClient，很多的java的爬虫也是通过HttpClient实现的，研究HttpClient对我们来说是非常重要的。

## 2.HttpClient不是浏览器     

很多人觉得既然HttpClient是一个HTTP客户端编程工具，很多人把他当做浏览器来理解，但是其实HttpClient不是浏览器，它是一个HTTP通信库，因此它只提供一个通用浏览器应用程序所期望的功能子集，最根本的区别是HttpClient中没有用户界面，浏览器需要一个渲染引擎来显示页面，并解释用户输入，例如鼠标点击显示页面上的某处，有一个布局引擎，计算如何显示HTML页面，包括级联样式表和图像。javascript解释器运行嵌入HTML页面或从HTML页面引用的javascript代码。来自用户界面的事件被传递到javascript解释器进行处理。除此之外，还有用于插件的接口，可以处理Applet，嵌入式媒体对象（如pdf文件，Quicktime电影和Flash动画）或ActiveX控件（可以执行任何操作）。HttpClient只能以编程的方式通过其API用于传输和接受HTTP消息。HttpClient也是完全内容不可知的。

另一个主要区别是对错误输入或HTTP标准违规的容忍。 需要允许无效的用户输入，以使浏览器用户友好。 还需要对从服务器检索的畸形文档的容忍度，以及在执行协议时服务器行为的缺陷，使尽可能多的用户可访问的网站。 然而，HttpClient努力在默认情况下尽可能接近并遵守HTTP标准规范和相关标准。 它还提供了一些手段来放松规范所施加的一些限制，这些限制允许或要求与不兼容的HTTP源或代理服务器兼容。

## 3.HttpClient入门使用

注意这个版本主要是基于HttpClient4.5.2版本的来讲解的，也是现在最新的版本，之所以要提供版本说明的是因为HttpClient 3版本和HttpClient 4版本差别还是很多大的，基本HttpClient里面的接口都变了，你把HttpClient 3版本的代码拿到HttpClient 4上面都运行不起来，会报错的。所以这儿一定要注意，好了废话不多说了，开始。

### 3.1.在pom.xml加入对httpclient的必需的jar包的依赖

```xml
<!--httpclient的接口基本都在这儿-->
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.5.2</version>
</dependency>
<!--httpclient缓存-->
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient-cache</artifactId>
    <version>4.5</version>
</dependency>
<!--http的mime类型都在这里面-->
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpmime</artifactId>
    <version>4.3.2</version>
</dependency>
```

**注意：**常见的MIME类型（通用型）：

| 名称               | 文件名     | 类型                     |
| ------------------ | ---------- | ------------------------ |
| 超文本标记语言文本 | .html      | text/html                |
| xml文档            | .xml       | text/xml                 |
| XHTML文档          | .xhtml     | application/xhtml+xml    |
| 普通文本           | .txt       | text/plain               |
| RTF文本            | .rtf       | application/rtf          |
| PDF文档            | .pdf       | application/pdf          |
| Microsoft Word文件 | .word      | application/msword       |
| PNG图像            | .png       | image/png                |
| GIF图形            | .gif       | image/gif                |
| JPEG图形           | .jpeg,.jpg | image/jpeg               |
| au声音文件         | .au        | audio/basic              |
| MIDI音乐文件       | .mid,.midi | audio/midi,audio/x-midi  |
| RealAudio音乐文件  | .ra, .ram  | audio/x-pn-realaudio     |
| MPEG文件           | .mpg,.mpeg | video/mpeg               |
| AVI文件            | .avi       | video/x-msvideo          |
| GZIP文件           | .gz        | application/x-gzip       |
| TAR文件            | .tar       | application/x-tar        |
| 任意的二进制数据   |            | application/octet-stream |

## 3.2.抓取网页的内容并打印到控制台的demo

```java
package com.shenhuanjie.web.crawler.httpClicentDemo;

import org.apache.http.HttpEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;

import java.io.IOException;
import java.io.InputStream;

public class HttpGetNewSample {
    public static void main(String[] args) {
        String url = "http://www.baidu.com";

        // 使用默认配置的HttpClient
        CloseableHttpClient client = HttpClients.createDefault();
        // 使用Get方法
        HttpGet httpGet = new HttpGet(url);
        InputStream inputStream = null;
        CloseableHttpResponse response = null;

        try {
            // 执行请求，获取响应
            response = client.execute(httpGet);
            // 请求是否成功，打印http状态码
            System.out.println(response.getStatusLine().getStatusCode());
            // 获取响应的实体内容，即所要抓取的网页内容
            HttpEntity entity = response.getEntity();

            //打印到控制台
            //方法一：使用EntityUtils
            if (entity != null) {
                System.out.println(EntityUtils.toString(entity, "utf-8"));
            }
            EntityUtils.consume(entity);
            //方法二  :使用inputStream
           /* if (entity != null) {
                inputStream = entity.getContent();

                BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
                String line = "";
                while ((line = bufferedReader.readLine()) != null) {
                    System.out.println(line);

                }
            }*/
        } catch (IOException e) {
            e.printStackTrace();
        } finally {

            if (response != null) {
                try {
                    response.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (inputStream != null) {
                try {
                    inputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

## 3.3.HttpClient编写程序流程总结

其实从上面我们就可以总结出使用HttpClient其实分为几个步骤

**1.创建HttpClient对象**

这儿使用的是org.apache.http.impl.client.CloseableHttpClient，他是HttpClient接口的一个实例，创建该对象的最简单方法是`CloseableHttpClient client = HttpClients.createDefault();`

HttpClients是创建CloseableHttpClient的工厂，采用默认的配置来创建实例，一般情况下我们就用这个默认的实例就足够，后面我们可以去看下怎么定制自己需求配置的来创建HttpClient接口的实例。如果你去看这个函数的源代码，你可以看到org.apache.http.client.CookieStore，org.apache.http.client.config.RequestConfig等等都是采用默认的。后面我们会专门有篇博客探讨怎么根据自己的需求定制httpclient。

**2.创建某种请求方法的实例**。

创建某种请求的实例，并指定请求的url，如果是get请求，创建对象HttpGet，如果是post  请求，创建对象HttpPost。类型的还有 HttpHead, HttpPost, HttpPut, HttpDelete,  HttpTrace, 还有 HttpOptions。分别对应HEAD、POST  PUT、DELETE、TRACE、OPTIONS方法，每个方法是做什么的如下表：

| 方法    | 描述                                               | 是否包含主体 |
| ------- | -------------------------------------------------- | ------------ |
| GET     | 从服务器获取一份文档                               | 否           |
| HEAD    | 只从服务器获取文档的首部                           | 否           |
| POST    | 向服务器发送需要处理的数据                         | 是           |
| PUT     | 将请求的主体部分存储在服务器上                     | 是           |
| TRACE   | 对可能经过代理服务器传送到服务器上去的报文进行追踪 | 否           |
| OPTIONS | 决定可以在服务器上执行哪些方法                     | 否           |
| DELETE  | 从服务器上删除一份文档                             | 否           |

可以看得到在Http协议中，只有post方法和put方法的请求里面有实体

**3.如果有请求参数的话**

Get方法直接写在url后面，例如：

```java
HttpGet httpget = new HttpGet("http://www.google.com/search?hl=zhCN&q=httpclient&btnG=Google+Search&aq=f&oq=");
```

或者使用setParameter来设置参数

```java
// 如果有请求参数的话，使用setParameter来设置参数
String uri = new URIBuilder()
        .setScheme("http")
        .setHost("www.baidu.com")
        .setPath("/s")
        .setParameter("wd", "HttpClient")
        .toString();
HttpGet httpGet = new HttpGet(uri);
System.out.println(httpget.getURI());
```