# Zipline：摄取自定义分钟数据

此目录中的“python”脚本概述了如何在 Zipline 中提取自定义分钟数据。它基于 Algoseek 分钟柱交易数据，该数据不是免费提供的。
但是，您可以通过从 [Algoseek](https://www.algoseek.com) 慷慨提供的交易和报价数据的免费样本中提取第一个、最高、最低、最后和交易量列来创建类似的数据集在这里](https://www.algoseek.com/ml4t-book-data.html)。

不幸的是，Zipline 的管道 API 不适用于分钟栏数据，因此我们没有在书中使用这个自定义包，但我将这个示例代码留在这里以适应您自己的项目。
