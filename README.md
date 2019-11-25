![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 获取已安装的SQL实例列表
#### Get List Of SQL Instances Installed
**发布-日期: 2016年10月14日 (评论)**

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
这是一些显示服务器上安装的所有实例的快速SQL逻辑。这很简单，但在围绕多实例SQL Server创建更多自动进程时仍然非常有用。


## English
Here’s some quick SQL logic to show you all the instances that are installed on a server. This is simple enough, but still super helpful when creating more automatic processes around a multi-instance SQL Server.

---
## Logic
```SQL
use master;
 set nocount on
 declare @sql_instances table
 (
 [id] int identity(1,1)
 , [rootkey] nvarchar(255)
 , [sql_instances] nvarchar(255)
 , [value] nvarchar(255)
 )
 
insert into @sql_instances ([rootkey], [sql_instances], [value])
 execute xp_regread
 @rootkey = 'hkey_local_machine'
 , @key = 'softwaremicrosoftmicrosoft sql server'
 , @value_name = 'installedinstances'
 
select
 'SQL_Instances_Installed' = upper([sql_instances])
 
from
 @sql_instances


```

![#](images/get-list-of-sql-instances-installed-a.png?raw=true "#")

另外，如果需要检查SQL Instnance名称中是否有数值，你可以用这个：

Also… If you needed to check to see if there were numeric values in the SQL Instnance name; You could use this:
```SQL
select
    right([sql_instances], 2)
,   case   
        when right([sql_instances], 2) not like '%[0-9]%' then 'This is NOT numeric'
        else right([sql_instances], 2)
    end as [sql_instances]
from
    @sql_instances

```

On occasion you may run into this error based on key values (missing or otherwise):
RegOpenKeyEx() returned error 2, ‘The system cannot find the file specified.’
Msg 22001, Level 1, State 1

有时你可能会遇到关于键值（缺少或其他情况）的错误：
RegOpenKeyEx() returned error 2, ‘The system cannot find the file specified.’
Msg 22001, Level 1, State 1

Since this error occurs it will not populate the temp table, and therefore will not return any instance names, but no worries as the xp_regread extended stored procedure is not the only approach you can make when reading the instance names from the registry. 

由于发生此错误，它不会填充临时表，因此不会返回任何实例名称，但不必担心，因为xp_regread扩展存储过程不是从注册表中读取实例名称时可以进行的唯一方法。

You can also try this little number called xp_instance_regenumvalues. With a slight modification you can produce the same results and get back some valuable information including the Instant path as this is evaluated from the ‘key’ within enumvalues.

你也可以尝试这个名为xp_instance_regenumvalues的小数字。稍作修改，你就可以产生相同的结果，并获得一些有价值的信息，包括Instant路径，因为这是从enumvalues中的“key”评估的。

```SQL
declare @sql_instances  table
(
    [rootkey]   varchar(255)
,   [value]     varchar(255)
)
 
insert into @sql_instances 
exec master.dbo.xp_instance_regenumvalues 
    @rootkey    = N'HKEY_LOCAL_MACHINE'
,   @key        = N'SOFTWARE\\Microsoft\\Microsoft SQL Server\\Instance Names\\SQL';
 
select
    'Instance_Name' = upper([rootkey])
,   'Instance_Path' = upper([value])
from
    @sql_instances


```



[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

