
## 如何使用基础数据

美国证券交易委员会 (SEC) 要求美国发行人，即上市公司和证券，包括互惠基金，除提交各种其他监管备案要求。

自 1990 年代初期以来，SEC 通过其电子数据收集、分析和检索 (EDGAR) 系统提供了这些文件。它们构成了股票和其他证券（例如公司信贷）基本面分析的主要数据来源，其中价值取决于发行人的业务前景和财务状况。

#### 使用 XBRL 标记自动处理

自 SEC 引入 XBRL 以来，对监管文件的自动分析变得更加容易，XBRL 是一种免费、开放的全球标准，用于业务报告的电子表示和交换。 XBRL 基于 XML；它依赖于 [taxonomies](https://www.sec.gov/dera/data/edgar-log-file-data-set.html) 来定义报告元素的含义并映射到突出显示电子版报告中的相应信息。其中一种分类法代表美国公认会计原则 (GAAP)。

SEC 在 2005 年引入了自愿 XBRL 申报以应对会计丑闻，然后自 2009 年起要求所有申报者采用这种格式，并继续将强制性范围扩大到其他监管申报。 SEC 维护着一个网站，该网站列出了影响不同文件内容的当前分类法，可用于提取特定项目。

有几种途径可以跟踪和访问向美国证券交易委员会报告的基本数据：
- 作为 [EDGAR 公共传播服务]((https://www.sec.gov/oit/announcement/public-dissemination-service-system-contact.html)) (PDS) 的一部分，接受的文件的电子提要是收费。
- SEC 每 10 分钟更新一次 [RSS 提要](https://www.sec.gov/structureddata/rss-feeds-submitted-filings)，其中列出了结构化披露提交。
- 有公共[索引文件](https://www.sec.gov/edgar/searchedgar/accessing-edgar-data.htm) 用于通过 FTP 检索所有文件以进行自动处理。
- 财务报表（和附注）数据集包含来自所有财务报表和附注的经过解析的 XBRL 数据。

SEC 还通过 SEC.gov 发布包含 EDGAR 文件的 [互联网搜索流量](https://www.sec.gov/dera/data/edgar-log-file-data-set.html) 的日志文件，尽管六个月的延迟。


#### 构建基本数据时间序列

[财务报表和注释](https://www.sec.gov/dera/data/financial-statement-and-notes-data-set.html) 数据集中的数据范围包括从主要财务报表（资产负债表、损益表、现金流量、权益变动和综合收益）以及这些报表的脚注。最早可获得 2009 年的数据。


文件夹 [03_sec_edgar](03_sec_edgar) 包含笔记本 [edgar_xbrl](03_sec_edgar/edgar_xbrl.ipynb)，用于下载和解析 XBRL 格式的 EDGAR 数据，并通过结合财务报表和价格数据创建市盈率等基本指标。

### 其他基础数据源

- [耶鲁法学院宏观资源汇编](https://library.law.yale.edu/news/75-sources-economic-data-statistics-reports-and-commentary)
- [资本智商](www.capitaliq.com)
- [Compustat](www.compustat.com)
- [MSCI Barra](www.mscibarra.com)
- [诺斯菲尔德信息服务](www.northinfo.com)
- [量化服务组](www.qsg.com)