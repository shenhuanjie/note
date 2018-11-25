# 使用maven引入Apache poi jar包

maven构建的项目-->pom.xml文件

* eclipse提供Dependencies直接添加依赖jar包的工具：直接搜索poi以及poi-ooxml即可,maven会自动依赖需要的jar包:
  1. poi提供microsoft office旧版本支持,eg .xls Excel
  2. poi-ooxml提供microsoft office新版本支持,eg .xlsx Excel
* 或者手动修改pom.xml,在添加jar包依赖的地方加入

```xml
<dependencies>
    <dependency>
        <groupId>org.apache.poi</groupId>
        <artifactId>poi</artifactId>
        <version>3.10-FINAL</version>
    </dependency>
    <dependency>
        <groupId>org.apache.poi</groupId>
        <artifactId>poi-ooxml</artifactId>
        <version>3.10-FINAL</version>
    </dependency>
</dependencies>
```

