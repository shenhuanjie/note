# Java web中使用POI实现excel文件的导入和导出

## 文件导出

```java
package com.shenhuanjie.apache.poi.excel.demo;

import org.apache.poi.hssf.usermodel.HSSFRow;
import org.apache.poi.hssf.usermodel.HSSFSheet;
import org.apache.poi.hssf.usermodel.HSSFWorkbook;

import java.io.IOException;

public class exportXls {
    //导入excle表
    public String exportXls() throws IOException {
        //1.查询所有需要的数据
        List<Subarea> list = subareaService.findAll();
        //2.使用POI将数据写进excel表中
        //2.1在内存中创建一个excel文件
        HSSFWorkbook workbook = new HSSFWorkbook();
        //2.2创建一个标签页
        HSSFSheet sheet = workbook.createSheet("分区数据");
        //2.3创建标题行
        HSSFRow headRow = sheet.createRow(0);
        headRow.createCell(0).setCellValue("分区编号");
        headRow.createCell(1).setCellValue("开始编号");
        headRow.createCell(2).setCellValue("结束编号");
        headRow.createCell(3).setCellValue("位置信息");
        headRow.createCell(4).setCellValue("省市区");
        //2.4将需要的数据放入excel表中
        for (Subarea subarea : list) {
            HSSFRow dataRow = sheet.createRow(sheet.getLastRowNum()+1);
            headRow.createCell(0).setCellValue(subarea.getId());
            headRow.createCell(1).setCellValue(subarea.getStartnum());
            headRow.createCell(2).setCellValue(subarea.getEndnum());
            headRow.createCell(3).setCellValue(subarea.getPosition());
            headRow.createCell(4).setCellValue(subarea.getRegion().getName());
        }

        //3.输出流进行文件下载
        String filename = "分区信息表.xls";
        String mimeType = ServletActionContext.getServletContext().getMimeType(filename);
        ServletOutputStream outs = ServletActionContext.getResponse().getOutputStream();


        //获取客户端的浏览器类型
        String agent = ServletActionContext.getRequest().getHeader("User-Agent");
        filename =  FileUtils.encodeDownloadFilename(filename, agent);
        ServletActionContext.getResponse().setHeader("content-disposition",
                "attachment;filename"+filename);

        workbook.write(outs);
        return NONE;
    }
}
```