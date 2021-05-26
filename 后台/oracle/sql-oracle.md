#### 基本操作



#### 分页查询

```sql
/* 
	tableName 为表名，rownum为行数
*/
select * from (select rownum rn,t.* from tableName t) where rn<=100 and rn >=10; 
```

#### order by

```sql
 /* Oracle 中的 select 查询出来的结果默认是不排序的，如果需要结果按照 插入顺序排序，可以用 rownum 关键字查询 */
select rownum rn, t.* from blog t order by rn desc
```

