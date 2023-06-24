# 书中使用的数据源

我们将使用来自市场、基本面和替代来源的免费历史数据。第 2 章，市场和基本数据和第 3 章，金融替代数据涵盖了这些数据源的特征和访问，并介绍了我们将在整本书中使用的主要提供者。

我们将获取和使用的一些示例数据源包括：
- Quandl 超过 3,000 只美国股票的每日价格和其他数据点
- 纳斯达克 100 只股票的 Algoseek 分钟条形图交易和报价数据
- 日本股票和美国 ETF 和股票的 Stooq 每日价格数据
- 雅虎财经每日价格数据和美股基本面
- 纳斯达克 ITCH 订单簿数据
- 电子数据收集、分析和检索 (EDGAR) SEC 备案
- 来自 Seeking Alpha 的财报电话会议记录
- 来自美联储和其他机构的各种宏观基本数据
- 来自路透社等的财经新闻数据。
- 推特情绪数据
- Yelp 商业评论情绪数据

## 如何获取数据

有几个notebook可以指导您完成数据采购过程：
- notebook [create_datasets](create_datasets.ipynb) 包含有关下载 **Quandl Wiki 股票价格** 以及我们在整本书中使用的其他一些来源的信息，例如 S&P500 基准和美国股票元数据。
- notebook [create_stooq_data](create_stooq_data.ipynb) 演示了如何从 STOOQ 下载日本股票和美国股票以及 ETF 的历史价格。
  > 请注意，从 2020 年 12 月 10 日开始，STOOQ 将禁用自动下载并需要验证码，这样下载和解压缩 zip 文件的代码将不再有效；请导航到他们的网站进行手动下载。
- notebook [create_yelp_review_data](create_yelp_review_data.ipynb) 将文本数据与其他数值特征相结合，用于对 Yelp 用户评论进行情绪分析。
- notebook [glove_word_vectors](glove_word_vectors.ipynb) 下载预训练的词向量。
- notebook [twitter_sentiment](twitter_sentiment.ipynb) 下载并提取推特数据以进行情绪分析。

此外，相关目录和笔记本中提供了获取特定应用数据源的说明。
