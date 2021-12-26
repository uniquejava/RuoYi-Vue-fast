DbTable - 数据库中所有的表 =》/tool/gen/db/list

GenTable - 导入的表（准备生成代码的表） =》/tool/gen/list

## 查询数据库 table list

GenController.java

GET /db/list

```sql
select a.relname as table_name, b.description as table_comment
from pg_class a
         left join (select * from pg_description where objsubid = 0) b on a.oid = b.objoid
where a.relname in (select tablename from pg_tables where schemaname = (select current_schema()))
  AND a.relname NOT LIKE 'qrtz_%'
  AND a.relname NOT LIKE 'gen_%'
```

## 导入表

POST /importTable