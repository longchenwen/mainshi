# mysql
## 事物四大特性:
	  1.原子性
	  2.一致性
	  3.隔离性
	  4.持久性
	 产生:脏读,不可重复读,幻读
## 存储引擎:
	1.InnoDB
		InnoDB:InnoDB具有事务，支持4个事务隔离级别，回滚，崩溃修复能力和多版本并发的事务安全，包括ACID。
		如果应用中需要执行大量的INSERT或UPDATE操作，则应该使用InnoDB，这样可以提高多用户并发操作的性能
	2.Myisam
		MyISAM管理非事务表。它提供高速存储和检索，以及全文搜索能力。如果应用中需要执行大量的SELECT查询，那么MyISAM是更好的选择
## mysql事物的隔离级别:
	1.读未提交(read-uncommitted)
	2.不可重复读(read-committed)
	3.可重复读(repeatable-read)默认
	2.串行化(serializable)
## [mysql数据库的优化](https://www.cnblogs.com/xuchenliang/p/6844093.html)
	1.数据库表的优化
	2.数据库部署
	3.数据库性能优化
		1.储存引擎的选择
		2.系统内核优化,因为部署在linux系统,对linux内核适当的优化
		3.硬件配置:尽量加大物理内存
	4.数据库架构扩展
		集群:主要思想是分解单台数据库负载,突破磁盘I/O性能,热数据放入到缓存中
		降低磁盘I/O访问率
		1.主从复制和读写分离(代码层次实现读写分离,MySQL Proxy实现读写分离):
			问题:解决两台主数据库数据不一致的情况,负载均衡器:Ngnix
		2.增加缓存:
		3.分库:
		4.分表:垂直分表,水平分表
		5.分区:
	5.数据库维护
## 索引
	1.什么事索引:索引是一种特殊的文件(InnoDB数据表上的索引是表空间的一个组成部分)，
	它们包含着对数据表里所有记录的引用指针。
	更通俗的说，索引就相当于目录。当你在用新华字典时，帮你把目录撕掉了，
	你查询某个字开头的成语只能从第一页翻到第一千页。累！把目录还给你，则能快速定位！
	2.索引的优缺点:
		2.1.优点:可以大大加快数据的检索速度，这也是创建索引的最主要的原因,
		且通过使用索引，可以在查询的过程中，使用优化隐藏器，提高系统的性能
		2.2.缺点:索引需要额外的维护成本；因为索引文件是单独存在的文件,
		对数据的增加,修改,删除,都会产生额外的对索引文件的操作,这些操作需要消耗额外的IO,会降低增/改/删的执行效率。
## 创建索引三种方式:
	1.第一种方式:在执行create table时使用 index 索引name
	2.第二种方式:使用alter table 命令去增加索引:
		2.1. alter table table_name add index index_name(column_list),添加普通索引,索引值可以多次出现
		2.2. alter table table_name add unique(column_list),唯一索引,索引的值必须是唯一的(null除外,null可能出现多次)
		2.3. alter table table_name add primary key (column_list),主键索引,添加一个主键,唯一且不能为Null
		2.4. alter table table_name add fulltext index_name(column_list),全文索引
		备注:其中table_name是要增加索引的表名，column_list指出对哪些列进行索引，多列时各列之间用逗号分隔。
		索引名index_name可自己命名，缺省时，MySQL将根据第一个索引列赋一个名称。另外，ALTER TABLE允许在单个语句中更改多个表，因此可以在同时创建多个索引
	3.第三种方法:使用CREATE INDEX命令创建,CREATE INDEX可对表增加普通索引或UNIQUE索引。（但是，不能创建PRIMARY KEY索引）
		3.1 create index index_name on table_name(column_list)
		3.2 create unique index_name on table_name(column_list)
