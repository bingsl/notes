* psql备份数据库

```
su - postgres
pg_dump -d map > map.sql 导出sql，用于备份
dropdb map  删除数据库
createdb map  创建数据库
psql -d map -f map.sql  导入sql



windows系统

psql -U postgres 登录到数据库
create database map  创建数据库
psql -U postgres -d map -f map.sql 导入sql
net start postgresql-9.3

```



* psql导出数据到文件

```plsql
COPY (SELECT gid, name FROM l_ccshop3 WHERE name IS NOT null) TO '/var/lib/pgsql/query.csv' (format csv)

copy (select gid,name from l_syb2stop_syb2 where name!='') to '/var/lib/pgsql/test.csv' with csv header; 文件头
```

* psql导出sql

```
pg_dump -U postgres(用户名)  (-t 表名)  数据库名(缺省时同用户名)  > 路径/文件名.sql
[root@centos-linux ~]# pg_dump -U postgres -t map_config map >./test.sql
```

* psql导入sql

```
psql -d newdatabase -U postgres -f mydatabase.sql   // sql 文件在当前路径下
psql -d databaename(数据库名) -U username(用户名) -f < 路径/文件名.sql  // sql 文件不在当前路径下
```

