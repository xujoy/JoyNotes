1.修改密码

```mysql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new password';
```

2.保存权限复制

```bash
cp  -arp /var/lib/mysql
```

3.创建唯一性约束

```sql
ALTER TABLE `Table_Name` ADD unique(`Cloume_Name`)
```

4.创建关联性约束

```sql
ALTER TABLE `Table_Name` ADD unique KEY(`Cloume_Name1`,`Cloume_Name2`)
```

5.创建索引

```sql
ALTER TABLE table_name ADD INDEX index_name (column_list)
```


