# Codesmith6.5连接Mysql环境配置

1. 首先需要将MySql.Data.dll复制到codesmith安装目录下v6.5/bin文件夹下，注意dll的版本。
2. 其次因为codesmith6.5采用的是.net4.0的配置文件，找到`C:\Windows\Microsoft.Net\Framework\v4.0.30319\Config\machine`

在其中的DbProviderFactories节点下添加

```xml
<add name="MySQL Data Provider" invariant="MySql.Data.MySqlClient" description=".Net Framework Data Provider for MySQL" type="MySql.Data.MySqlClient.MySqlClientFactory, MySql.Data, Version=6.2.4.0, Culture=neutral, PublicKeyToken=c5687fc88969c44d" />
```

注意其中的版本号要与上面复制到codesmith下面mysql.data.dll版本号一致！

重启codesmith，问题解决

