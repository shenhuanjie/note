# Apache POI使用详解

## 1. POI结构与常用类

### POI介绍

Apache POI是Apache软件基金会的开源项目，POI提供API给Java程序对Microsoft Office格式档案读和写的功能。 .NET的开发人员则可以利用NPOI (POI for .NET) 来存取 Microsoft Office文档的功能。

### POI结构说明

| 包名称 | 说明                                               |
| ------ | -------------------------------------------------- |
| HSSF   | 提供读写Microsoft Excel XLS格式档案的功能。        |
| XSSF   | 提供读写Microsoft Excel OOXML XLSX格式档案的功能。 |
| HWPF   | 提供读写Microsoft Word DOC格式档案的功能。         |
| HSLF   | 提供读写Microsoft PowerPoint格式档案的功能。       |
| HDGF   | 提供读Microsoft Visio格式档案的功能。              |
| HPBF   | 提供读Microsoft Publisher格式档案的功能。          |
| HSMF   | 提供读Microsoft Outlook格式档案的功能。            |

### POI常用类说明

| 类名               | 说明                 |
| ------------------ | -------------------- |
| HSSFWorkbook       | Excel的文档对象      |
| HSSFSheet          | Excel的表单          |
| HSSFRow            | Excel的行            |
| HSSFCell           | Excel的格子单元      |
| HSSFFont           | Excel字体            |
| HSSFDataFormat     | 格子单元的日期格式   |
| HSSFHeader         | Excel文档Sheet的页眉 |
| HSSFFooter         | Excel文档Sheet的页脚 |
| HSSFCellStyle      | 格子单元样式         |
| HSSFDateUtil       | 日期                 |
| HSSFPrintSetup     | 打印                 |
| HSSFErrorConstants | 错误信息表           |

## 2. Excel的基本操作

### 创建Workbook和Sheet

```java
package com.shenhuanjie.apache.poi.excel.demo;

import org.apache.poi.hssf.usermodel.HSSFSheet;
import org.apache.poi.hssf.usermodel.HSSFWorkbook;

import java.io.FileOutputStream;
import java.io.IOException;

public class main {
    private static final String ROOT_PATH = "C:\\Users\\shenh\\Documents\\Output\\Excel\\";

    public static void main(String[] args) throws IOException {
        //文件名
        String fileName = "excel-sample.xls";
        //文件路径
        String filePath = ROOT_PATH + fileName;
        //创建Excel文件(Workbook)
        HSSFWorkbook workbook = new HSSFWorkbook();
        //创建工作表(Sheet)
        HSSFSheet sheet = workbook.createSheet();
        //创建工作表(Sheet)
        sheet = workbook.createSheet("Test");
        //删除默认工作表(Sheet)
        workbook.removeSheetAt(0);
        FileOutputStream out = new FileOutputStream(filePath);
        //保存Excel文件
        workbook.write(out);
        //关闭文件流
        out.close();
        System.out.println("OK!");
    }
}
```

### 创建单元格

