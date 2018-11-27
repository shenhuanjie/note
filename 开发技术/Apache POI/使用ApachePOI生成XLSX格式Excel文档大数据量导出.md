# 使用ApachePOI生成XLSX格式Excel文档大数据量导出

最近在做使用POI进行大数据量导出,现在把其整理成工具类供大家参考。Apache POI 3.8版本增加了前缀为SXSSF相关的类,主要用于大数据量的写入与读取。

关于ApachePOI导出Excel基本的使用我这里就不详解了,具体参考：

* [Apache POI官方网站](http://poi.apache.org/index.html) 
* [Apache POI使用详解](https://blog.csdn.net/vbirdbest/article/details/72870714)

**关于封装的工具类需要注意：**

 以下代码少**ReportInternalException**大家可以忽略(我们封装的一个异常类) 
* 导出的Excel同时考虑到数据的本身类型,如整数、小数、日期等 。
* 写入数据方式需依次调用方法[writeExcelTitle、writeExcelData、dispose],先完成写入Excel标题与列名,再完成数据写入（或者说基于模板方式写入数据），最终关闭流与资源的释放 。
* 我们使用[styleMap]方法避免重复创建Excel单元格样式（否则受Excel创建样式数量限制） 。
* SXSSFWorkbook基于模板写入数据的时候仍需要借助XSSFWorkbook 。
* SXSSFWorkbook写入数据模式大致为：根据初始化设置的flushRows（内存存储条数）数随着数据写入逐步把数据刷新至硬盘，具体参考官方文档与API介绍。

```  java
XSSFWorkbook tplWorkBook = new XSSFWorkbook(new FileInputStream(file)); 
Workbook writeDataWorkBook = new SXSSFWorkbook(tplWorkBook, flushRows); 
```

**Maven依赖配置**

```xml
<properties>
    <poi.version>3.10-FINAL</poi.version>
</properties>

<dependencies>
    <dependency>
        <groupId>org.apache.poi</groupId>
        <artifactId>poi</artifactId>
        <version>${poi.version}</version>
    </dependency>
    <dependency>
        <groupId>org.apache.poi</groupId>
        <artifactId>poi-ooxml</artifactId>
        <version>${poi.version}</version>
    </dependency>
    <dependency>
        <groupId>org.apache.poi</groupId>
        <artifactId>poi-ooxml-schemas</artifactId>
        <version>${poi.version}</version>
    </dependency>
</dependencies>
```

**代码实现**

