## 0. Trie (cow, thread safe, put get del), code foomat(make lint); memory leak
### src/primer: 实现线程安全的支持 get put del 的前缀树， 基于 std <mutex> <shared_mutex>


## 1. 内存管理器： 两个部分： LRU, BufferPool; 缓存页置换 + 内存缓冲池
### src/buffer
1. LRUKReplacer(evit, setEvitable, remove, size, recordAccess) BufferPoolManager(fetchPage, unpinPage, flushPage, newPage, delPage, flushAllPages) Basic&Read&WritePageGuard 页面替换算法， 并行io优化
### 优化： 高效的页面替换算法， 并行io处理


## 2. B+树索引： B+树创建(数据结构：叶节点， 非叶子节点)；+  节点插入，删除，查找
### src/storage/index, page
2. B+Tree page(internel page, leaf page);  insert,search(single value); del; leaf scan; concurrent index 死锁检测， segfault
### 优化： 并发索引操作： 死锁检测； 段错误排查

## 3. SQL执行： CRUD等基础的方法执行， 聚合查询，连接查询，排序，分页； + 查询优化： 基于索引的查询优化， 多连接走索引， 索引减枝， 索引更新
### src/execution & catalog & optimizer & planner
3. method executor(scan, insert, update, del); aggregation&join; sort limit executor, topN optimization 基于index的查询， 多连接查询，剪枝优化 column pruning， system catalog, 索引更新
### 优化

## 4. 并发控制： 表锁，行锁； 死锁检测， 并发查询(事务)
### src/concurrency & 
4. lockManger(table lock, row lock), deadlcok detection(addEdge, hasCycle), ConcurretQuery(commit, abort); 查询下推预测， index pruning
### 优化： 查询下推预测； 索引剪枝




