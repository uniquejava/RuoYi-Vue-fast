## 唯一的model类 -- GenTable

selectDbTable(...) - 数据库中所有的表 =》/tool/gen/db/list, 通过native SQL得到

selectGenTable(...) - 导入的表（i.e: 准备生成代码的表） =》/tool/gen/list, 从自定义的表gen_table取到

## 核心代码

### 查询数据库 GET /tool/gen/db/list

GenController.java

```sql
select a.relname as table_name, b.description as table_comment
from pg_class a
         left join (select * from pg_description where objsubid = 0) b on a.oid = b.objoid
where a.relname in (select tablename from pg_tables where schemaname = (select current_schema()))
  AND a.relname NOT LIKE 'qrtz_%'
  AND a.relname NOT LIKE 'gen_%'
```

### 导入表 POST /tool/gen/importTable

## 预览 GET /tool/gen/preview/{tableId}

## 生成代码 GET /tool/gen/genCode/{tableName}

见com.ruoyi.project.tool.gen.service.GenTableServiceImpl.previewCode

取模板列表：com.ruoyi.project.tool.gen.util.VelocityUtils.getTemplateList

给模板中的变量设置值：com.ruoyi.project.tool.gen.util.VelocityUtils.prepareContext
