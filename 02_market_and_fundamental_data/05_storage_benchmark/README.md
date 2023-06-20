## 使用 pandas 进行高效的数据存储

notebook [storage_benchmark](storage_benchmark.ipynb) 比较了主要存储格式的效率和性能。

特别是，它比较：
- CSV：逗号分隔的标准平面文本文件格式。
- HDF5：分层数据格式，最初由国家超级计算中心开发，是一种快速且可扩展的数字数据存储格式，可在使用 PyTables 库的 pandas 中使用。
- Parquet：一种二进制、列式存储格式，是 Apache Hadoop 生态系统的一部分，可提供高效的数据压缩和编码，由 Cloudera 和 Twitter 开发。它可通过 pyarrow 库用于 pandas，该库由 pandas 的原作者 Wes McKinney 领导。


它使用可配置为包含数字或文本数据或两者的测试“DataFrame”。对于 HDF5 库，我们测试了固定格式和表格格式。表格格式允许查询并可以附加到。

### 测试结果

简而言之，结果是：
- 对于纯数字数据，HDF5 格式表现最好，表格格式也与 CSV 共享最小的内存占用，为 1.6 GB。固定格式使用两倍的空间，parquet 格式使用 2 GB。
- 对于数字和文本数据的混合，HDF5 利用其相对于 CSV 的读取优势，parquet 明显更快。

该笔记本说明了如何使用 %%timeit 单元魔法配置、测试和收集计时。同时演示了使用这些存储格式所需的相关pandas命令的用法。
