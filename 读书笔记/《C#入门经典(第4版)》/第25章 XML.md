# 第25章 XML

Extensible Markup Language（可扩展标记语言，XML）近几年得到了广泛的关注。XML并不是新技术，肯定不是Microsoft为了在.NET环境中使用而开发的，但Microsoft在XML的开发初期就认识到了它的重要性，所以，XML在.NET中执行大量的任务，包括描述应用程序的配置、在Web服务之间传输信息等。

XML是一种以简单文本格式存储数据的方式，这意味着它可以被任何计算机读取。在前面章节介绍Web编程技术时可以看到，XML是在Internet上传输数据的绝佳格式。甚至对于人来说，它也很容易理解！

XML的细节非常复杂，因此在此不介绍其所有的细节。但其基本格式非常简单，在大多数情况下，不需要了解XML的详细知识，因为VS通常会处理这些工作——我们基本上不必手动编写XML文档。前面已经说过，XML在.NET领域非常重要，因为它是传输数据的默认格式，所以理解其基本知识至关重要。

**本章的主要内容：**

* XML的结构和元素
* XML模式
* 在应用程序中使用XML

> **注意：**
> 
> 如果需要全面理解XML，请参阅Beginning XML，Fourth Edition一书（由Wrox Press出版，2007）。

### 25.2.1 XML文档对象模型

XML文档对象模型（Document Object Model，DOM）是一组以非常直观的方式访问和处理XML的类。DOM不是读取XML数据的最快方式，但只要理解了类和XML文档中元素之间的关系，DOM就很容易使用。

构成DOM的类在名称空间System.Xml中。在这个名称空间中有几个类和子名称空间。但本章只介绍几个易于操作XML的类。要学习和使用的类如表25-1中所示。

| 类名         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| XmlNode      | 这个类表示文档树中的一个节点，它是本章许多类的基类。如果这个节点表示XML文档的根，就可以从它导航到文档的任意位置 |
| XmlDocument  | XmlDocument类扩展了XmlNode类，但常常是使用XML的第一个对象，因为这个类用于加载和保存磁盘或其他地方的数据 |
| XmlElement   | XmlELement类表示XML文档中的一个元素。XmlElement派生于XmlLinkedNode，XmlLinkedNode派生于XmlNode |
| XmlAttribute | 这个类表示一个属性，与XmlDocument类一样，它也派生于XmlNode类 |
| XmlText      | 表示开标记和闭标记之间的文本                                 |
| XmlComment   | 表示一种特殊类型的节点，这种节点不是文档的一部分，但为读者提供文档各部分的信息 |
| XmlNodeList  | XmlNodeList类表示一个节点集合                                |

## 25.3 小结

本章学习了Extensible Markup Language（XML），XML是一种存储和提取数据的文本格式。我们讨论了需要遵循的规则，以确保XML文档是格式良好的。还论述了如何根据XSD和XDR模式验证它们。

学习了XML的基础知识后，探讨了如何使用C#和Visual Studio在代码中利用XML，最后阐述了如何使用XPath在XML中进行查询。

本章学习了：

* 如何读写XML
* 应用于格式良好的XML的规则
* 如何根据两种模式XSD和XDR验证XML文档
* 如何通过.NET在程序中使用XML
* 如何使用XPath查询搜索XML文档

第26章将学习如何在.NET中使用ADO.NET访问数据库。