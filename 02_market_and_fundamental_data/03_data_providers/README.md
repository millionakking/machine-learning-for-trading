# 02 API 获取市场数据

使用 Python 通过 API 访问市场数据有多种选择。

## pandas 数据阅读器

笔记本 [01_pandas_datareader_demo](01_pandas_datareader_demo.ipynb) 介绍了 pandas 库中内置的一些资源。
- `pandas` 库允许使用 read_html 函数访问网站上显示的数据
- 相关的 `pandas-datareader` 库通过标准接口提供对各种数据提供者的 API 端点的访问

## yfinance：雅虎！金融市场和基本数据

notebook [yfinance_demo](02_yfinance_demo.ipynb) 展示了如何使用 yfinance 从 Yahoo! 下载各种数据。金融。该库通过使用 Pythonic API 以可靠、高效的方式从网站上抓取数据来解决历史数据 API 的弃用问题。

## LOBSTER 分时数据

笔记本 [03_lobster_itch_data](03_lobster_itch_data.ipynb) 演示了 LOBSTER（限价订单簿系统 - 高效重构器）提供的订单簿数据的使用，[在线](https://lobsterdata.com/info/WhatIsLOBSTER.php ) 限价订单簿数据工具，旨在提供简单易用、高质量的限价订单簿数据。

自 2013 年以来，LOBSTER 充当学术界的数据提供商，提供对整个纳斯达克交易股票领域的重建限价订单簿数据的访问权限。最近，它开始提供商业服务。

## Qandl

笔记本 [03_quandl_demo](03_quandl_demo.ipynb) 展示了 Quandl 如何使用非常简单的 API 来提供其免费和付费数据。有关详细信息，请参阅 [文档](https://www.quandl.com/tools/api)。

## zipline & Qantopian

笔记本 [包含笔记本 [zipline_data](05_zipline_data.ipynb) 简要介绍了我们将在本书中使用的回测库 `zipline`，并展示了如何在运行回测时访问股价数据。有关安装，请参阅说明 [此处](../../installation)。

