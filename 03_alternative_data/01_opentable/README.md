## 抓取 OpenTable 数据

替代数据的典型来源是评论网站，如 Glassdoor 或 Yelp，它们使用员工评论或客人评论传达内部见解。这些数据为 ML 模型提供了有价值的输入，这些模型旨在预测企业的前景或直接预测其市场价值以获得交易信号。

数据需要从 HTML 源中提取，排除任何法律障碍。为了说明 Python 提供的 Web 抓取工具，我们将从 OpenTable 检索有关餐厅预订的信息。这种性质的数据可用于按地理、房地产价格或连锁餐厅收入来预测经济活动。

### 构建餐厅预订数据集

> 注意：与所有其他示例不同，使用 Selenium 的代码被编写为在主机上运行，​​而不是使用 Docker 映像，因为它依赖于浏览器。该代码仅在 Ubuntu 和 Mac 上进行了测试。

使用浏览器自动化工具 [Selenium](https://www.seleniumhq.org/)，您可以快速构建纽约市 10,000 多家餐馆的数据集，然后您可以定期更新该数据集以跟踪时间序列。

要设置 Selenium，请运行
```bash
./selenium_setup.sh
```
具有适当的权限，即在运行 `chmod+x selenium_setup.sh` 之后。

脚本 [opentable_selenium] (opentable_selenium.py) 说明了如何抓取和存储数据。只需运行下列指令
```python
python opentable_selenium.py
```

由于网站经常更改，此代码随时可能不能用。

### 更进一步——Scrapy 和 splash

Scrapy 是一个强大的库，用于构建跟踪链接、检索内容并以结构化方式存储解析结果的机器人。结合 headless browser splash，它还可以解释 JavaScript，成为 Selenium 的高效替代品。

您可以在 01_opentable 目录中使用 `scrapy crawl opentable` 命令运行爬虫，结果将记录到 spider.log。




