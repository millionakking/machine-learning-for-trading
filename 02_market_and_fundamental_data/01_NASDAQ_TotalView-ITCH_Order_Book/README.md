# 使用市场数据：NASDAQ_TotalView-ITCH Order Book

虽然 FIX 占据着巨大的市场份额，但交易所也提供本地协议。纳斯达克提供 TotalView ITCH 直接数据馈送协议，允许订户跟踪从放置到执行或取消的单个股票工具订单。

因此，它允许重建跟踪特定证券或金融工具的主动限价买卖订单列表的订单簿。订单簿通过列出每个价位的出价或出价股票数量来揭示全天的市场深度。它还可以识别负责特定买卖订单的市场参与者，除非它是匿名下达的。市场深度是流动性和大量市场订单的潜在价格影响的关键指标。

除了匹配市价单和限价单外，纳斯达克还经营在开盘和收盘时执行大量交易的竞价或交叉盘。随着被动投资的持续增长和交易者寻找机会执行更大的股票，交叉盘变得越来越重要。 TotalView 还发布了纳斯达克开盘和收盘交叉盘和纳斯达克 IPO/暂停交叉盘的净订单失衡指标 (NOII)。

> 此示例需要大量内存，可能超过 16GB（我使用的是 64GB，尚未检查最低要求）。如果您遇到容量限制，请记住，您能够运行代码对于本书中的任何其他内容都不是必需的。首先，它旨在展示您将在机构投资环境中使用什么样的数据，在该环境中，系统将被构建为管理比这个单日示例大得多的数据。

## 解析二进制 ITCH 消息

ITCH v5.0 规范声明了 20 多种与系统事件、股票特征、限价单的下达和修改以及交易执行相关的消息类型。它还包含有关开盘价和收盘价交叉之前的净订单不平衡的信息。

纳斯达克提供了几个月的每日二进制文件样本。本章的 GitHub 存储库包含一个笔记本 build_order_book.ipynb，它说明了如何解析 ITCH 消息的示例文件并为任何给定的报价重建已执行的交易和订单簿。

下表显示了本书（日期为 2018 年 3 月 29 日）中使用的示例文件的最常见消息类型的频率。该代码同时更新为使用 2019 年 3 月 27 日的新样本。

| Message type | Order book impact                                                                  | Number of messages |
|:------------:|------------------------------------------------------------------------------------|-------------------:|
| A            | New unattributed limit order                                                       | 136,522,761        |
| D            | Order canceled                                                                     | 133,811,007        |
| U            | Order canceled and replaced                                                        | 21,941,015         |
| E            | Full or partial execution; possibly multiple messages for the same original order  | 6,687,379          |
| X            | Modified after partial cancellation                                                | 5,088,959          |
| F            | Add attributed order                                                               | 2,718,602          |
| P            | Trade Message (non-cross)                                                          | 1,120,861          |
| C            | Executed in whole or in part at a price different from the initial display price   | 157,442            |
| Q            | Cross Trade Message                                                                | 17,233             |

对于每条消息，规范列出了组件及其各自的长度和数据类型：


| Name                    | Offset  | Length  | Value      | Notes                                                                                |
|-------------------------|---------|---------|------------|--------------------------------------------------------------------------------------|
| Message Type            | 0       | 1       | S          | System Event Message                                                                 |
| Stock Locate            | 1       | 2       | Integer    | Always 0                                                                             |
| Tracking Number         | 3       | 2       | Integer    | Nasdaq internal tracking number                                                      |
| Timestamp               | 5       | 6       | Integer    | Nanoseconds since midnight                                                           |
| Order Reference Number  | 11      | 8       | Integer    | The unique reference number assigned to the new order at the time of receipt.        |
| Buy/Sell Indicator      | 19      | 1       | Alpha      | The type of order being added. B = Buy Order. S = Sell Order.                        |
| Shares                  | 20      | 4       | Integer    | The total number of shares associated with the order being added to the book.        |
| Stock                   | 24      | 8       | Alpha      | Stock symbol, right padded with spaces                                               |
| Price                   | 32      | 4       | Price (4)  | The display price of the new order. Refer to Data Types for field processing notes.  |
| Attribution             | 36      | 4       | Alpha      | Nasdaq Market participant identifier associated with the entered order               |

笔记本 [01_build_itch_order_book](01_parse_itch_order_flow_messages.ipynb)、[02_rebuild_nasdaq_order_book](02_rebuild_nasdaq_order_book.ipynb) 和 [03_normalize_tick_data](03_normalize_tick_data.ipynb) 包含用于
- 下载纳斯达克 Total View 样本报价数据，
- 解析来自二进制源数据的消息
- 重建给定股票的订单簿
- 可视化订单流数据
- 标准化刻度数据

代码已更新为使用日期为 2019 年 3 月 27 日的最新纳斯达克示例文件。

警告：报价数据的大小约为 12GB，在具有 32GB RAM 的 4 核 i7 CPU 上，一些处理步骤可能需要几个小时。

## 正则化分时数据

贸易数据以纳秒为索引，并且非常嘈杂。例如，当交易启动在买入和卖出市场订单之间交替时，买卖反弹会导致价格在买入价和卖出价之间波动。为了提高噪声信号比并改善统计特性，我们需要通过聚合交易活动对报价数据进行重新采样和规范化。

我们通常会收集汇总期间的开盘价（第一价）、最低价、最高价和收盘价（最后一个），以及成交量加权平均价 (VWAP)、交易的股票数量以及与数据相关的时间戳。

笔记本 [03_normalize_tick_data](03_normalize_tick_data.ipynb) 说明了如何使用使用不同聚合方法的时间和体积条来规范化嘈杂的报价。
