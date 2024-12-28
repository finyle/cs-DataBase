### 0. 线程安全的前缀树： Trie (cow, thread safe, put get del), code format(make lint); memory leak
#### src/primer: 实现线程安全的支持 get put del 的前缀树， 基于 std <mutex> <shared_mutex>
```
src/include/primer/p0_trie.h: 实现trienode的插入删除， 和trie线程安全是的插入删除查找l
TrieNode::insert, remove
Trie::insert, remove, get, 基于wlock,rlock 分别实现trie的有锁并发插入，删除，查找 concurrency
```

### 1. 内存管理器： 两个部分： LRU, BufferPool; 缓存页置换 + 内存缓冲池
#### src/buffer
1. LRUKReplacer(evit, setEvitable, remove, size, recordAccess) 
2. BufferPoolManager(fetchPage, unpinPage, flushPage, newPage, delPage, flushAllPages) 
3. Basic&Read&WritePageGuard 页面替换算法， 并行io优化
#### 优化： 高效的页面替换算法， 并行io处理
```
src/buffer/lru_k_replacer: evit, setEvitable, remove, size, recordAccess
src/buffer/buffer_pool_manager: new, fetch, unpin, flush, flushAll, deltePgImp
```

### 2. B+树索引： B+树创建(数据结构：叶节点， 非叶子节点)；+ 节点插入，删除，查找
#### src/storage/index, page
2. B+Tree page(internel page, leaf page);  insert,search(single value); del; leaf scan; concurrent index 死锁检测， segfault
#### 优化： 并发索引操作： 死锁检测； 段错误排查
```
src/storage/index/b_plus_tree(索引部分): insert() remove() indexIterate() b+树索引的掺入，删除，索引迭代查找
src/storage/page/table_page(页表结构): init() insert() delete() markDeleted() rollbackDelete() search() 页表的crud及回滚
src/storage/table/tuple: 页表结构的定义
src/storage/disk/disk_manager: 数据落盘：调用iostream写入数据文件(持久化)
```

### 3. SQL执行： CRUD等基础的方法执行， 聚合查询，连接查询，排序，分页； + 查询优化： 基于索引的查询优化， 多连接走索引， 索引减枝， 索引更新
#### src/execution & catalog & optimizer & planner
3. method executor(scan, insert, update, del); aggregation&join; sort limit executor, topN optimization 基于index的查询， 多连接查询，剪枝优化 column pruning， system catalog, 索引更新
#### 优化
```
执行顺序： plan -> optimizer -> executor
src/catalog: column,schema,table_generator: 定义命令行中的输出内容
src/executor: sql 语句的具体执行(crud, 聚合， 过滤，) 
src/optimizer: 查询优化，merge， order, sort
src/planner: sql 语句执行难入口entry
```

### 4. 并发控制： 表锁，行锁； 死锁检测， 并发查询(事务)
#### src/concurrency 
4. lockManger(table lock, row lock), deadlcok detection(addEdge, hasCycle), ConcurretQuery(commit, abort); 查询下推预测， index pruning
#### 优化： 查询下推预测； 索引剪枝
```
src/concurrency/lock_manager: 表锁/行锁， 基于图的死锁检测(环检测)
src/concurrency/TransactionManager: 事务的实现， begin, abort, commit 
```

# ##########################################################################
#### set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -static-libasan")
#### tools/shell: set disable_tty: true

