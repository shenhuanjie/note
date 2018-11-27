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

```java
//创建工作表（Sheet)
HSSFSheet sheet = workbook.createSheet("Test");
//创建行，从0开始
HSSFRow row = sheet.createRow(0);
//创建行的单元格，也是从0开始
HSSFCell cell = row.createCell(0);
//设置单元格内容
cell.setCellValue("李志伟");
//设置单元格内容，重载
row.createCell(1).setCellValue(false);
row.createCell(2).setCellValue(new Date());
row.createCell(3).setCellValue(12.345);
```

### 创建文档摘要信息

```java
// 创建文档信息
workbook.createInformationProperties();
// 摘要信息
DocumentSummaryInformation dsi = workbook.getDocumentSummaryInformation();
// 类别
dsi.setCategory("Excel文件");
// 管理者
dsi.setManager("李志伟");
// 公司
dsi.setCompany("——");
// 摘要信息
SummaryInformation si = workbook.getSummaryInformation();
// 主题
si.setSubject("——");
// 标题
si.setTitle("测试文档");
// 作者
si.setAuthor("李志伟");
// 备注
si.setComments("POI测试文档");
```

### 创建批注

```java
// 创建批注
HSSFSheet sheet = workbook.createSheet("Test");
HSSFPatriarch patr = sheet.createDrawingPatriarch();
// 创建批注位置
HSSFClientAnchor anchor = patr.createAnchor(0, 0, 0, 0, 5, 1, 8, 3);
// 创建批注
HSSFComment comment = patr.createCellComment(anchor);
// 设置批注内容
comment.setString(new HSSFRichTextString("这是一个批注段落"));
// 设置批注作者
comment.setAuthor("李志伟");
// 设置批注默认显示
comment.setVisible(true);
HSSFCell cell = sheet.createRow(2).createCell(1);
cell.setCellValue("测试");
// 把批注赋值给单元格
cell.setCellComment(comment);
```

创建批注位置HSSFPatriarch.createAnchor(dx1, dy1, dx2, dy2, col1, row1, col2, row2)方法参数说明：

| 参数 | 说明                     |
| ---- | ------------------------ |
| dx1  | 第1个单元格中x轴的偏移量 |
| dy1  | 第1个单元格中y轴的偏移量 |
| dx2  | 第2个单元格中x轴的偏移量 |
| dy2  | 第2个单元格中y轴的偏移量 |
| col1 | 第1个单元格的列号        |
| row1 | 第1个单元格的行号        |
| col2 | 第2个单元格的列号        |
| row2 | 第2个单元格的行号        |

### 创建页眉和页脚

```java
// 创建工作表（Sheet)
HSSFSheet sheet = workbook.createSheet("Test");

// 得到页眉
HSSFHeader header = sheet.getHeader();
header.setLeft("页眉左边");
header.setRight("页眉右边");
header.setCenter("页眉中间");
// 得到页脚
HSSFFooter footer = sheet.getFooter();
footer.setLeft("页脚左边");
footer.setRight("页脚右边");
footer.setCenter("页脚中间");
```

也可以使用Office自带的标签定义，你可以通过HSSFHeader或HSSFFooter访问到它们，都是静态属性，列表如下：

| 参数                            |      | 说明         |
| ------------------------------- | ---- | ------------ |
| HSSFHeader.tab                  | &A   | 表名         |
| HSSFHeader.file                 | &F   | 文件名       |
| HSSFHeader.startBold            | &B   | 粗体开始     |
| HSSFHeader.endBold              | &B   | 粗体结束     |
| HSSFHeader.startUnderline       | &U   | 下划线开始   |
| HSSFHeader.endUnderline         | &U   | 下划线结束   |
| HSSFHeader.startDoubleUnderline | &E   | 双下划线开始 |
| HSSFHeader.endDoubleUnderline   | &E   | 双下划线结束 |
| HSSFHeader.time                 | &T   | 时间         |
| HSSFHeader.date                 | &D   | 日期         |
| HSSFHeader.numPages             | &N   | 总页面数     |
| HSSFHeader.page                 | &p   | 当前页号     |

## 3. Excel的单元格操作

### 设置格式

```java
// 创建工作表（Sheet）
HSSFSheet sheet = workbook.createSheet("Test");
HSSFRow row = sheet.createRow(0);
// 设置日期格式——使用Excel内嵌的格式
HSSFCell cell = row.createCell(0);
cell.setCellValue(new Date());
HSSFCellStyle style = workbook.createCellStyle();
style.setDataFormat(HSSFDataFormat.getBuiltinFormat("m/d/yy h:mm"));
cell.setCellStyle(style);
// 设置保留2位小数——使用Excel内嵌的格式
cell = row.createCell(1);
cell.setCellValue(12.3456789);
style = workbook.createCellStyle();
style.setDataFormat(HSSFDataFormat.getBuiltinFormat("0.00"));
cell.setCellStyle(style);
// 设置货币格式——使用自定义的格式
cell = row.createCell(2);
cell.setCellValue(12345.6789);
style = workbook.createCellStyle();
style.setDataFormat(workbook.createDataFormat().getFormat("Y#,##0"));
cell.setCellStyle(style);
// 设置百分比格式——使用自定义格式
cell = row.createCell(3);
cell.setCellValue(0.123456789);
style = workbook.createCellStyle();
style.setDataFormat(workbook.createDataFormat().getFormat("0.00%"));
cell.setCellStyle(style);
// 设置中文大写格式--使用自定义的格式
cell = row.createCell(4);
cell.setCellValue(12345);
style = workbook.createCellStyle();
style.setDataFormat(workbook.createDataFormat().getFormat("[DbNum2][$-804]0"));
cell.setCellStyle(style);
// 设置科学计数法格式--使用自定义的格式
cell = row.createCell(5);
cell.setCellValue(12345);
style = workbook.createCellStyle();
style.setDataFormat(workbook.createDataFormat().getFormat("0.00E+00"));
cell.setCellStyle(style);
```

**HSSFDataFormat.getFormat和HSSFDataFormat.getBuiltinFormat的区别：**当使用Excel内嵌的（或者说预定义）的格式时，直接用HSSFDataFormat.getBuiltinFormat静态方法即可。当使用自己定义的格式时，必须先调用HSSFWorkbook.createDataFormat()，因为这时在底层会先找有没有匹配的内嵌FormatRecord，如果没有就会新建一个FormatRecord，所以必须先调用这个方法，然后你就可以用获得的HSSFDataFormat实例的getFormat方法了，当然相对而言这种方式比较麻烦，所以内嵌格式还是用HSSFDataFormat.getBuiltinFormat静态方法更加直接一些。

### 合并单元格

```java
HSSFSheet sheet = workbook.createSheet("Test");
HSSFRow row = sheet.createRow(0);
// 合并列
HSSFCell cell = row.createCell(0);
cell.setCellValue("合并列");
CellRangeAddress region = new CellRangeAddress(0, 0, 0, 5);
sheet.addMergedRegion(region);
// 合并行
cell = row.createCell(6);
cell.setCellValue("合并行");
region = new CellRangeAddress(0, 5, 6, 6);
sheet.addMergedRegion(region);
```

CellRangeAddress对象其实就是表示一个区域，其构造方法如下：CellRangeAddress(firstRow, lastRow, firstCol, lastCol)，参数的说明：

| 参数     | 说明                       |
| -------- | -------------------------- |
| firstRow | 区域中第一个单元格的行号   |
| lastRow  | 区域中最后一个单元格的行号 |
| firstCol | 区域中第一个单元格的列号   |
| lastCol  | 区域中最后一个单元格的列号 |

**提示：**即使你没有用CreateRow和CreateCell创建过行或单元格，也完全可以直接创建区域然后把这一区域合并，Excel的区域合并信息是单独存储的，和RowRecord、ColumnInfoRecord不存在直接关系。

### 单元格对齐

```java
HSSFSheet sheet = workbook.createSheet("Test");
HSSFRow row = sheet.createRow(0);
HSSFCell cell = row.createCell(0);
cell.setCellValue("单元格对齐");
HSSFCellStyle style = workbook.createCellStyle();
// 水平居中
style.setAlignment(HSSFCellStyle.ALIGN_CENTER);
// 垂直居中
style.setVerticalAlignment(HSSFCellStyle.VERTICAL_CENTER);
// 自动换行
style.setWrapText(true);
// 缩进
style.setIndention((short) 5);
// 文本旋转，这里的取值是从 -90 到90，而不是0 - 180
style.setRotation((short) 60);
cell.setCellStyle(style);
```

水平对齐相关参数

## 参考

* [Apache POI使用详解](https://www.cnblogs.com/LiZhiW/p/4313789.html)