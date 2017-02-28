# namenode分析

## 文件系统目录树管理
HDFS的目录和文件在内存中是一棵树的形式存储的，这个目录树的结构是由NameNode维护的，NameNode会修改这个树形结构以对外添加和删除文件等操作功能。
文件系统目录树上的节点还保存了HDFS文件与数据块的对应关系，我们知道HDFS中的每个文件都是被拆分成若干数据块冗余存放的，文件与数据块的对应关系
也是由NameNode维护的。

## 数据块以及数据节点管理
HDFS中的数据块时冗余备份在集群中的数据节点上的，所以NameNode还要维护数据块与数据节点之间的对应关系。这里的对应关系包括两个部分：
* 数据块保存在哪些数据节点上
* 一个数据节点保存了哪些数据块

## 租约管理
Hadoop 2.3.0 版本新增了集中式缓存管理功能，允许用户将一些文件和目录保存在HDFS的缓存中。
HDFS的集中式缓存是分布在Datanode上的堆外内存组成的，并且由Namenode统一管理。

## FSNamesystem
Namenode涉及很多HDFS的处理逻辑，例如读文件、写文件、追加写文件等， Namenode的FSNamesystem 类是管理这些逻辑的门面类。

## Namenode的启动和停止
Hadoop2.x 实现中提供了HA功能，HA集群中会存在两种状态的Namenode。Active Namenode作为服务节点， Standby Namenode作为热备节点。
Namenode在启动时会先进入安全模式，在安全模式中Namenode不会接受客户端对命名精简的修改，
Namenode成功启动后会离开安全模式进入standby状态。
