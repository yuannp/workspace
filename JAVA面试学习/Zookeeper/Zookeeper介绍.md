#### 1、什么是Zookeeper？

`ZooKeeper`是一个**高可用的分布式数据管理与系统协调框架**（**高可用的分布式协调管理系统**）；基于对`Paxos`算法的实现，保证了分布式环境中数据的强一致性。

#### 2、Zookeeper 用途？

`Zookeeper` 可以被用作配置管理、命名服务、分布式同步、集群管理等方面的协调服务。

#### 3、Zookeeper 数据模型？

`Zookeeper` 的数据模型类似于文件系统，主要包括节点和节点路径。每个节点都有一个唯一的路径，并且可以存储数据。

#### 4、Zookeeper 节点类型？

`Zookeeper` 中有三种类型的节点：持久节点（`Persistent`）、短暂节点（`Ephemeral`）和顺序节点（`Sequential`）。其中，持久节点在创建后<u>一直存在</u>，短暂节点在创建它的<u>客户端连接断开时被删除</u>，顺序节点在节点名称的<u>末尾附加自动增加的数字</u>。

#### 5、Zookeeper 角色？

`Zookeeper` 集群中每个节点都可以成为` Leader `或 `Follower`。`Leader `负责协调写操作，而 `Follower `接收并复制 `Leader `节点的写操作。

#### 6、Zookeeper 集群通信？ 

`Zookeeper` 集群中的节点通过 `TCP` 协议进行通信，并使用“原子广播”机制来确保消息的一致性和可靠性。

#### 7、Zookeeper 监听器？

 `Zookeeper` 可以设置 `Watcher` 监听器，在节点发生变化时触发通知。`Watcher `监听器可以在客户端连接断开或重新连接时自动删除，需要再次设置。

#### 8、Zookeeper 保证数据的一致性？ 

`Zookeeper `通过 `ZAB` 协议（`ZooKeeper Atomic Broadcast`）来实现数据的一致性和可靠性。该协议将写操作广播到所有节点，并使用多数投票机制来确保事务的原子性和一致性。(强一致性)

