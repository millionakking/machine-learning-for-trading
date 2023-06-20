# 市场和基本面数据：来源和技术

数据一直是交易的重要驱动力，交易员长期以来一直努力通过获取优质信息来获得优势。这些努力至少可以追溯到谣言，即罗斯柴尔德家族根据鸽子携带英国在滑铁卢取得胜利的预先消息从债券购买中获得了丰厚的收益。

如今，对更快数据访问的投资以领先的**高频交易** (HFT) 公司组成的 Go West 财团的形式出现，该财团将芝加哥商品交易所 (CME) 与东京连接起来。随着交易员竞相利用套利机会，CME 和纽约 BATS 交易所之间的往返延迟已降至接近 8 毫秒的理论极限。与此同时，监管机构和交易所已经开始引入减速带来减缓交易，以限制信息获取不均对竞争的不利影响。

传统上，投资者主要依赖**公开的市场和基本面数据**。创建或获取私有数据集（例如通过专有调查）的努力是有限的。传统策略侧重于股票基本面并根据报告的财务数据建立财务模型，可能结合行业或宏观数据来预测每股收益和股票价格。或者，他们利用技术分析，使用根据价格和交易量信息计算出的指标从市场数据中提取信号。

**机器学习 (ML) 算法** 有望比人类定义的规则和启发式方法更有效地利用市场和基本面数据，尤其是在与下一章的主题——替代数据相结合时。我们将说明如何将从线性模型到递归神经网络 (RNN) 的 ML 算法应用于市场和基本数据并生成可交易信号。

本章介绍市场和基本面数据来源，并解释它们如何反映产生它们的环境。 **交易环境**的细节不仅对市场数据的正确解释很重要，而且对您的策略的设计和执行以及现实回测模拟的实施也很重要。我们还说明了如何使用 Python 访问和处理来自各种来源的交易和财务报表数据。
 
## 内容

1. [市场数据反映交易环境](#market-data-reflects-the-trading-environment)
    * [市场微观结构：交易的具体细节](#market-microstructure-the-nuts-and-bolts-of-trading)
2. [使用高频市场数据](#working-with-high-frequency-market-data)
    * [如何使用纳斯达克订单簿数据](#how-to-work-with-nasdaq-order-book-data)
    * [如何传达交易：FIX 协议](#how-trades-are-communicated-the-fix-protocol)
    * [纳斯达克 TotalView-ITCH 数据提要](#the-nasdaq-totalview-itch-data-feed)
        - [代码示例：解析和规范化刻度数据](#code-example-parsing-and-normalizing-tick-data-)
        - [其他资源](#additional-resources)
    * [AlgoSeek 分钟柱：股票报价和交易数据](#algoseek-minute-bars-equity-quote-and-trade-data)
        - [从综合提要到分钟柱](#from-the-consolidated-feed-to-minute-bars)
        - [代码示例：如何处理 AlgoSeek 盘中数据](#code-example-how-to-process-algoseek-intraday-data)
3. [获取市场数据的API](#api-access-to-market-data)
    * [使用 pandas 进行远程数据访问](#remote-data-access-using-pandas)
    * [代码示例](#code-examples)
    * [数据源](#data-sources)
    * [行业新闻](#industry-news)
4. [如何使用基础数据](#how-to-work-with-fundamental-data)
    * [财务报表数据](#financial-statement-data)
    * [使用 XBRL 标记的自动处理](#automated-processing-using-xbrl-markup)
    * [代码示例：构建基本数据时间序列](#code-example-building-a-fundamental-data-time-series)
    * [其他基本数据源](#other-fundamental-data-sources)
5. [使用 pandas 进行高效数据存储](#efficient-data-storage-with-pandas)
    * [代码示例](#code-example)
 
## 市场数据反映交易环境

市场数据是交易者如何直接或通过中介机构在众多市场之一上为金融工具下订单、如何处理这些订单以及如何通过匹配供需来设定价格的产物。因此，数据反映了交易场所的制度环境，包括管理订单、交易执行和价格形成的规章制度。有关全球概览，请参阅 [Harris](https://global.oup.com/ushe/product/trading-and-exchanges-9780195144703?cc=us&lang=en&) (2003) 和 [Jones](https://www0 .gsb.columbia.edu/faculty/cjones/papers/2018.08.31%20US%20Equity%20Market%20Data%20Paper.pdf) (2018) 有关美国市场的详细信息。

算法交易者使用包括 ML 在内的算法来分析买卖订单的流动以及由此产生的交易量和价格统计数据，以提取交易信号，以捕捉对供需动态或某些市场参与者行为等方面的洞察。在我们开始使用由此类环境（即纳斯达克）创建的实际报价数据之前，本节回顾在回测期间影响交易策略模拟的机构特征。

### 市场微观结构：交易的具体细节

市场微观结构研究制度环境如何影响交易过程并塑造价格发现、买卖差价和报价、日内交易行为和交易成本等结果。在算法和电子交易的快速发展的推动下，它是金融研究中发展最快的领域之一。

如今，对冲基金赞助内部分析师跟踪快速变化的复杂细节，确保以最佳市场价格执行，并设计利用市场摩擦的策略。在我们深入研究交易生成的数据之前，本节简要概述了关键概念，即不同的市场和订单类型。

- [交易和交易 - 从业者的市场微观结构](https://global.oup.com/ushe/product/trading-and-exchanges-9780195144703?cc=us&lang=en&)，Larry Harris，牛津大学出版社，2003 年
- [了解美国股票市场数据的市场](https://www0.gsb.columbia.edu/faculty/cjones/papers/2018.08.31%20US%20Equity%20Market%20Data%20Paper.pdf)，查尔斯·琼斯，纽约证券交易所，2018
- [世界交易所联合会](https://www.world-exchanges.org/our-work/statistics)
- [订单驱动市场的经济学](https://www.springer.com/gp/book/9788847017658)，Abergel 等人，2011 年
    - 介绍不同社区（物理学家、经济学家、数学家、金融工程师）在建模和分析订单驱动市场方面的想法和研究。这些研究的主要兴趣是导致价格统计的统计规律性的机制。还介绍了与其他重要问题有关的结果，例如市场影响、交易策略的盈利能力或微观结构效应的数学模型。

## 使用高频市场数据

两类市场数据涵盖在 Reg NMS 下交易的在美国交易所上市的数千家公司：合并源结合了每个交易场所的交易和报价数据，而每个单独的交易所提供专有产品以及该特定场所的额外活动信息。

在本节中，我们将首先展示 NASDAQ 提供的专有订单流数据，这些数据代表实际的订单流、交易和最终价格，因为它们是在逐笔报价的基础上发生的。然后，我们演示如何将这种以不规则间隔到达的连续数据流规范化为固定持续时间的条形图。最后，我们介绍包含综合交易和报价信息的 AlgoSeek 股票分钟柱数据。在每种情况下，我们都会说明如何使用 Python 处理数据，以便您可以将这些来源用于您的交易策略。

### 如何使用纳斯达克订单簿数据

市场数据的主要来源是订单簿，它全天实时更新以反映所有交易活动。交易所通常将此数据作为收费的实时服务提供，但也可能免费提供一些历史数据。

在美国，股票市场提供三级报价，即一级、二级和三级，提供越来越细化的信息和功能：
- 第一级：实时出价和要价信息，可从众多在线资源中获得
- 二级：添加有关特定做市商的出价和要价的信息，以及最近交易的规模和时间，以便更好地了解给定股票的流动性。
- III 级：增加了输入或更改报价、执行订单和确认交易的能力，并且仅供做市商和交易所成员公司使用。访问 III 级报价允许注册经纪人满足最佳执行要求。

交易活动反映在有关市场参与者发送的订单的大量消息中。这些消息通常符合用于实时交换证券交易和市场数据的电子金融信息交换 (FIX) 通信协议或本地交换协议。

- [限价订单簿](https://arxiv.org/pdf/1012.0349.pdf)
- [使用深度学习进行中间价格预测的特征工程](https://arxiv.org/abs/1904.05384)
- [限价订单簿中的价格跳跃预测](https://arxiv.org/pdf/1204.1381.pdf)
- [处理和可视化订单簿数据](https://github.com/0b01/recurrent-autoencoder/blob/master/Visualizing%20order%20book.ipynb) 作者：Ricky Han

### 交易是如何传达的：FIX 协议

交易活动反映在市场参与者发送的有关交易订单的大量消息中。这些消息通常符合用于实时交换证券交易和市场数据的电子金融信息交换 (FIX) 通信协议或本地交换协议。

- [FIX 交易标准](https://www.fixtrading.org/standards/)
- Python：[简单修复](https://github.com/da4089/simplefix)
- C++ 版本：[quickfixengine](http://www.quickfixengine.org/)
- 盈透证券[界面](https://www.interactivebrokers.com/en/index.php?f=4988)

### 纳斯达克 TotalView-ITCH 数据提要

虽然 FIX 占据着巨大的市场份额，但交易所也提供本地协议。纳斯达克提供 TotalView ITCH 直接数据馈送协议，允许订户跟踪从放置到执行或取消的单个股票工具订单。

- ITCH [规范](http://www.nasdaqtrader.com/content/technicalsupport/specifications/dataproducts/NQTVITCHspecification.pdf)
- [示例文件](ftp://emi.nasdaq.com/ITCH/)

#### 代码示例：解析和规范化刻度数据

- 文件夹 [NASDAQ TotalView ITCH Order Book](01_NASDAQ_TotalView-ITCH_Order_Book) 包含笔记本
    - 下载纳斯达克 Total View 样本报价数据，
    - 解析来自二进制源数据的消息
    - 重建给定股票的订单簿
    - 可视化订单流数据
    - 标准化刻度数据
- 二进制数据服务：`struct` [模块](https://docs.python.org/3/library/struct.html)
 
#### 其他资源
 
- 本地交换协议 [世界各地](https://en.wikipedia.org/wiki/List_of_electronic_trading_protocols_
 - [限价订单簿中的高频交易](https://www.math.nyu.edu/faculty/avellane/HighFrequencyTrading.pdf)，Avellaneda 和 Stoikov，《量化金融》，第 1 卷。 8, No. 3, April 2008, 217–224
 - [使用模拟器开发执行算法](http://www.math.ualberta.ca/~cfrei/PIMS/Almgren5.pdf)，Robert Almgren，量化经纪人，2016
 - [回测微观结构策略](https://rickyhan.com/jekyll/update/2019/12/22/how-to-simulate-market-microstructure.html)，Ricky Han，2019
- [最优高频做市](http://stanford.edu/class/msande448/2018/Final/Reports/gr5.pdf)，Fushimi 等人，2018
- [模拟和分析订单簿数据：队列反应模型](https://arxiv.org/pdf/1312.0563.pdf)，Huan 等人，2014
- [如何在限价订单簿中揭示潜在流动性？](https://arxiv.org/pdf/1808.09677.pdf)，Dall’Amico 等人，2018 年

### AlgoSeek 分钟柱：股票报价和交易数据

AlgoSeek 以以前仅供机构投资者使用的质量提供历史盘中数据。 AlgoSeek Equity bar 以用户友好的格式提供非常详细的日内报价和交易数据，旨在简化日内 ML 驱动策略的设计和回测。正如我们将看到的，数据不仅包括 OHLCV 信息，还包括关于买卖价差的信息以及价格上下波动的分时报价等信息。
AlgoSeek 非常友好地提供了 2013 年至 2017 年纳斯达克 100 只股票的分钟条数据样本以供演示之用，并将向本书的读者提供该数据的一个子集。

#### 从综合提要到分钟柱

AlgoSeek 分钟柱基于证券信息处理器 (SIP) 提供的数据，该处理器管理本节开头提到的综合提要。您可以在 https://www.algoseek.com/data-drive.html 找到文档。

报价和交易数据字段
分钟柱数据最多包含 54 个字段。柱线的开盘价、最高价、最低价和收盘价元素有八个字段，即：
- 柱和相应交易的时间戳
- 当前买卖报价和相关交易的价格和规模

还有 14 个数据点包含柱形周期的交易量信息：
- 股票数量和相应的交易
- 等于或低于买价、买价和中点之间、中点、中点和卖价之间、等于或高于卖价以及交叉盘的交易量
- 上涨或下跌交易的股票数量，即当价格上涨或下跌时，以及当价格没有变化时，根据之前的价格变动方向区分

#### 代码示例：如何处理 AlgoSeek 日内数据

目录 [algoseek_intraday](02_algoseek_intraday) 包含有关如何从 AlgoSeek 下载示例数据的说明。

- 此信息将很快提供。

## API 访问市场数据

使用 Python 通过 API 访问市场数据有多种选择。在本章中，我们首先介绍 [`pandas`](https://pandas.pydata.org/) 库中内置的一些源代码。接着简单介绍一下交易平台【Quantopian】（https://www.quantopian.com/posts），数据提供商【Quandl】（https://www.quandl.com/）（12/2018被纳斯达克收购） ) 和我们将在本书后面使用的回溯测试库 [`zipline`](https://github.com/quantopian/zipline)，并列出了几个额外的选项来访问各种类型的市场数据。目录 [data_providers](03_data_providers) 包含几个说明这些选项用法的笔记本。

### 使用 pandas 进行远程数据访问

- read_html [文档](https://pandas.pydata.org/pandas-docs/stable/)
- 标准普尔 500 成分股来自 [维基百科](https://en.wikipedia.org/wiki/List_of_S%26P_500_companies)
- `pandas-datareader`[文档](https://pandas-datareader.readthedocs.io/en/latest/index.html)

### 代码示例

文件夹 [数据提供者](03_data_providers) 包含使用各种数据提供者的示例。
1.使用[pandas DataReader]进行远程数据访问(03_data_providers/01_pandas_datareader_demo.ipynb)
2. 使用[yfinance](03_data_providers/02_yfinance_demo.ipynb)下载行情和基本面数据
3. 从[LOBSTER](03_data_providers/03_lobster_itch_data.ipynb) 解析限价订单报价数据
4. Quandl [API Demo](03_data_providers/04_quandl_demo.ipynb)
5. Zipline [数据访问](03_data_providers/05_zipline_data_demo.ipynb)

### 数据源

- Quandl [docs](https://docs.quandl.com/docs) and Python [API](https://www.quandl.com/tools/python﻿)
- [yfinance](https://github.com/ranaroussi/yfinance)
- [Quantopian](https://www.quantopian.com/posts)
- [Zipline](https://zipline.ml4trading.io/﻿)
- [LOBSTER](https://lobsterdata.com/)
- [The Investor Exchange](https://iextrading.com/﻿)
- [IEX Cloud](https://iexcloud.io/) financial data infrastructure
- [Money.net](https://www.money.net/)
- [Trading Economic](https://tradingeconomics.com/)
- [Barchart](https://www.barchart.com/)
- [Alpha Vantage](https://www.alphavantage.co/﻿)
- [Alpha Trading Labs](https://www.alphatradinglabs.com/)
- [Tiingo](https://www.tiingo.com/) stock market tools

### 行业新闻

- [彭博社和路透社的数据份额输给了较小的竞争对手](https://www.ft.com/content/622855dc-2d31-11e8-9b4b-bc4b9f08f381)，英国《金融时报》，2018 年

## 如何使用基础数据

基本数据与决定证券价值的经济驱动因素有关。数据的性质取决于资产类别：
- 对于股票和企业信贷，它包括企业财务以及行业和经济范围内的数据。
- 对于政府债券，它包括国际宏观数据和外汇。
- 对于大宗商品，它包括特定资产的供需决定因素，例如农作物的天气数据。

我们将关注美国的股票基本面，因为美国的数据更容易获取。全球约有 13,000 多家上市公司生成了 200 万页的年度报告和 30,000 多个小时的财报电话会议。在算法交易中，基本数据和根据这些数据设计的特征可用于直接导出交易信号，例如作为价值指标，并且是预测模型（包括机器学习模型）的基本输入。

###财务报表数据

美国证券交易委员会 (SEC) 要求美国发行人，即上市公司和证券，包括互惠基金，除提交各种其他监管备案要求。

自 1990 年代初期以来，SEC 通过其电子数据收集、分析和检索 (EDGAR) 系统提供了这些文件。它们构成了股票和其他证券（例如公司信贷）基本面分析的主要数据来源，其中价值取决于发行人的业务前景和财务状况。

### 使用 XBRL 标记自动处理

自 SEC 引入 XBRL 以来，对监管文件的自动分析变得更加容易，XBRL 是一种免费、开放的全球标准，用于业务报告的电子表示和交换。 XBRL 基于 XML；它依赖于 [taxonomies](https://www.sec.gov/dera/data/edgar-log-file-data-set.html) 来定义报告元素的含义并映射到突出显示电子版报告中的相应信息。其中一种分类法代表美国公认会计原则 (GAAP)。

SEC 在 2005 年引入了自愿 XBRL 申报以应对会计丑闻，然后自 2009 年起要求所有申报者采用这种格式，并继续将强制性范围扩大到其他监管申报。 SEC 维护着一个网站，该网站列出了影响不同文件内容的当前分类法，可用于提取特定项目。

有几种途径可以跟踪和访问向美国证券交易委员会报告的基本数据：
- 作为 [EDGAR 公共传播服务]((https://www.sec.gov/oit/announcement/public-dissemination-service-system-contact.html)) (PDS) 的一部分，接受的文件的电子提要是收费。
- SEC 每 10 分钟更新一次 [RSS 提要](https://www.sec.gov/structureddata/rss-feeds-submitted-filings)，其中列出了结构化披露提交。
- 有公共[索引文件](https://www.sec.gov/edgar/searchedgar/accessing-edgar-data.htm) 用于通过 FTP 检索所有文件以进行自动处理。
- 财务报表（和附注）数据集包含来自所有财务报表和附注的经过解析的 XBRL 数据。

SEC 还通过 SEC.gov 发布包含 EDGAR 文件的 [互联网搜索流量](https://www.sec.gov/dera/data/edgar-log-file-data-set.html) 的日志文件，尽管六个月的延迟。

### 代码示例：构建基本数据时间序列

[财务报表和注释](https://www.sec.gov/dera/data/financial-statement-and-notes-data-set.html) 数据集中的数据范围包括从主要财务报表（资产负债表、损益表、现金流量、权益变动和综合收益）以及这些报表的脚注。最早可获得 2009 年的数据。

文件夹 [04_sec_edgar](04_sec_edgar) 包含笔记本 [edgar_xbrl](04_sec_edgar/edgar_xbrl.ipynb)，用于下载和解析 XBRL 格式的 EDGAR 数据，并通过结合财务报表和价格数据创建市盈率等基本指标。

### 其他基础数据源

- [Compilation of macro resources by the Yale Law School](https://library.law.yale.edu/news/75-sources-economic-data-statistics-reports-and-commentary)
- [Capital IQ](www.capitaliq.com)
- [Compustat](www.compustat.com)
- [MSCI Barra](www.mscibarra.com)
- [Northfield Information Services](www.northinfo.com)
- [Quantitative Services Group](www.qsg.com)

## 使用 pandas 进行高效的数据存储

我们将在本书中使用许多不同的数据集，比较主要格式的效率和性能是值得的。特别是，我们比较以下内容：

- CSV：逗号分隔的标准平面文本文件格式。
- HDF5：分层数据格式，最初由国家超级计算中心开发，是一种快速且可扩展的数字数据存储格式，可在使用 PyTables 库的 pandas 中使用。
- Parquet：一种二进制、列式存储格式，是 Apache Hadoop 生态系统的一部分，可提供高效的数据压缩和编码，由 Cloudera 和 Twitter 开发。它可通过 pyarrow 库用于 pandas，该库由 pandas 的原作者 Wes McKinney 领导。

### 代码示例

[05_storage_benchmark](05_storage_benchmark)目录下的notebook [storage_benchmark](05_storage_benchmark/storage_benchmark.ipynb)对比了上述库的性能。
