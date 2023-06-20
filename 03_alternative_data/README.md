# 交易的替代数据

本章解释个人、业务流程和传感器如何生成替代数据。它还提供了一个框架，用于导航和评估用于投资目的的替代数据的激增供应。

它演示了使用 Python 对通过网络抓取获得的数据进行采集、预处理和存储的工作流程，为 ML 的应用奠定了基础。它以提供来源、提供者和应用程序的示例作为结尾。

## 内容

1. [替代数据革命](#the-alternative-data-revolution)
    * [资源](#resources)
2. [替代数据来源](#sources-of-alternative-data)
3. [评估替代数据集的标准](#criteria-for-evaluating-alternative-datasets)
    * [资源](#resources-2)
4. [替代数据市场](#the-market-for-alternative-data)
5. [使用替代数据](#working-with-alternative-data)
    * [代码示例：Open Table Web Scraping](#code-sample-open-table-web-scraping)
    * [代码示例：SeekingAlpha 收益成绩单](#code-example-seekingAlpha-earnings-transcripts)
    * [Python 库和文档](#python-libraries--documentation)

## 替代数据革命

对于算法交易，如果新数据源能获取对传统来源来说无法获得的信息，或者能更快地提供访问，则新数据源具备信息优势。跟随全球趋势，投资行业正迅速扩展到市场和基本面数据以外的其他来源，以通过信息优势获得阿尔法。预计到 2020 年，每年在数据、技术能力和相关人才方面的支出将从目前的 30 亿美元增加 12.8%。

今天，投资者可以实时访问宏观数据或公司特定数据，而这些数据在历史上只能以低得多的频率获得。新数据源的用例包括：
- 一组具有代表性的商品和服务的在线价格数据可用于衡量通货膨胀
- 商店访问或购买的数量允许实时估计公司或行业特定的销售或经济活动
- 卫星图像可以在其他地方获得这些信息之前揭示农业产量或矿山或石油钻井平台的活动

### 资源

- [2020 年的数字宇宙](https://www.emc.com/collat​​eral/analyst-reports/idc-the-digital-universe-in-2020.pdf)
- [大数据：创新、竞争和生产力的下一个前沿](https://www.mckinsey.com/business-functions/digital-mckinsey/our-insights/big-data-the-next-frontier-for -创新), 麦肯锡 2011
- [麦肯锡谈人工智能](https://www.mckinsey.com/featured-insights/artificial-intelligence)

## 替代数据的来源

替代数据集由许多来源生成，但可以在较高层次上分类为主要由以下来源生成：
- 在社交媒体上发帖、评论产品或使用搜索引擎的个人
- 记录商业交易的企业，特别是信用卡支付，或作为中介捕获供应链活动
- 传感器，除其他外，通过卫星或安全摄像头等图像或通过手机信号塔等移动模式捕捉经济活动

随着新数据源的出现以及以前标记为“替代”的来源成为主流的一部分，替代数据的性质继续迅速发展。例如，波罗的海干散货指数 (BDI) 汇集了数百家航运公司的数据，以估算干散货船的供需情况，现在可以在彭博终端上获得。

替代数据源在决定算法交易策略的价值或信号内容的关键方面有所不同。

## 评估替代数据集的标准

替代数据的最终目标是在竞争性搜索产生 alpha（即正的、不相关的投资回报）​​的交易信号时提供信息优势。在实践中，从替代数据集中提取的信号可以独立使用，也可以与其他信号结合使用，作为定量策略的一部分。

### 资源

- [大数据和人工智能战略](http://valuesimplex.com/articles/JPM.pdf)，Kolanovic, M. 和 Krishnamachari, R.，摩根大通，2017 年 5 月

## 另类数据市场

2018 年，投资行业预计将在数据服务上花费 20 亿至 30 亿美元，预计这一数字与其他行业一样每年以两位数的速度增长。该支出包括替代数据的获取、相关技术的投资以及合格人才的聘用。

- [替代数据](https://alternativedata.org/)

## 使用另类数据

本节说明如何使用网络抓取获取替代数据，首先针对 OpenTable 餐厅数据，然后转向由 Seeking Alpha 托管的收益电话会议记录。

- [使用谷歌趋势量化金融市场的交易行为](https://www.nature.com/articles/srep01684)，Preis、Moat 和 Stanley，Nature，2013
- [量化 StockTwits 语义术语在金融市场中的交易行为：决策树算法的有效应用](https://www.sciencedirect.com/science/article/pii/S0957417415005473)，Al Nasseri 等，Expert Systems with Applications , 2015

### 代码示例：打开表 Web 抓取

> 注意：与所有其他示例不同，使用 Selenium 的代码被编写为在主机上运行，​​而不是使用 Docker 映像，因为它依赖于浏览器。该代码仅在 Ubuntu 和 Mac 上进行了测试。

此子文件夹 [01_opentable](01_opentable) 包含使用 Scrapy 和 Selenium 脚本的脚本 [opentable_selenium](01_opentable/opentable_selenium.py)。

- [如何在每个浏览器中查看网页的源代码](https://www.lifewire.com/view-web-source-code-4151702)

### 示例代码：SeekingAlpha 收益记录

> 更新：不幸的是，seekingalpha 已经更新了他们的网站，使用了验证码，因此不再可以按照此处描述的方式进行自动下载。

> 注意：与所有其他示例不同，代码编写为在主机上运行而不是使用 Docker 映像，因为它依赖于浏览器。该代码仅在 Ubuntu 和 Mac 上进行了测试。

子文件夹 [02_earnings_calls](02_earnings_calls) 包含脚本 [sa_selenium](02_earnings_calls/sa_selenium.py)，用于从 [SeekingAlpha](www.seekingalpha.com) 网站抓取收益电话记录。

## Python 库和文档
- requests [文档](http://docs.python-requests.org/en/master/)
- BeautifulSoup [文档](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
- Selenium [文档](https://www.seleniumhq.org/)
- Scrapy [文档](https://scapy.readthedocs.io/en/latest/)

