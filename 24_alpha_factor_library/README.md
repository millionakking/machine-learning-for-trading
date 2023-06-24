# 附录 - Alpha 因子库

在整本书中，我们强调了功能的智能设计（包括适当的预处理和去噪）通常如何产生有效的策略。
本附录综合了一些关于特征工程的经验教训，并提供了关于这个重要主题的更多信息。

第 4 章根据它们所代表的潜在风险以及投资者将获得高于市场回报的回报对因素进行分类。
这些类别包括价值与增长、质量和情绪，以及波动性、动量和流动性。
在整本书中，我们使用了大量指标来捕捉这些风险因素。
本附录对这些示例进行了扩展，并收集了流行的指标，因此您可以将其用作自己策略开发的参考或灵感。
它还向您展示了如何计算它们，并包括一些评估这些指标的步骤。

为此，我们关注 TA-Lib 实施的广泛指标（参见[第 4 章](04_alpha_factor_research)）和 WorldQuant 的 [101 Formulaic Alphas](https://arxiv.org/pdf/1601.00991.pdf) 论文(Kakushadze 2016)，它呈现了生产中使用的真实量化交易因子，平均持有期为 0.6-6.4 天。

本章涵盖：
- 如何使用 TA-Lib 和 NumPy/pandas 计算几十个技术指标，
- 创建上述论文中描述的公式化 alpha，以及
- 使用从等级相关性和互信息到特征重要性、SHAP 值和 Alphalens 的各种指标评估结果的预测质量。

## 内容

1. [指标动物园](#the-indicator-zoo)
2. [代码示例：TA-Lib 中实现的常见 alpha 因子](#code-example-common-alpha-factors-implemented-in-ta-lib)
3. [代码示例：WorldQuant 对公式化 alpha 的探索](#code-example-worldquants-quest-for-formulaic-alphas)
4. [代码示例：双变量和多变量因子评估](#code-example-bivariate-and-multivariate-factor-evaluation)

## 指标动物园

第 4 章，[金融特征工程：如何研究 Alpha 因子](../04_alpha_factor_research)，总结了学术界和从业者长期以来为识别有助于可靠预测资产回报的信息或变量所做的努力
这项研究从单因素资本资产定价模型转向“[新因素](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.407.3913&rep=rep1&type=pdf) “（Cochrane 2011）。

这个因子动物园包含数百个公司特征和证券价格指标，自 1970 年以来，在异常文献中，它们被作为股票回报的统计显着预测因子（参见 [Green、Hand 和 Zhang](https://academic.oup.com/ rfs/article-abstract/30/12/4389/3091648), 2017)。
- 笔记本 [indicator_zoo](00_indicator_zoo.ipynb) 列出了许多示例。

## 代码示例：TA-Lib 中实现的常见 alpha 因子

TA-Lib 库广泛用于交易软件开发人员对金融市场数据进行技术分析。它包括来自多个类别的 150 多个流行指标，范围从重叠研究（包括移动平均线和布林线）到统计函数（例如线性回归）。

**功能组**|**# 指标**
:-----:|:-----:
重叠研究|17
动量指标|30
成交量指标|3
波动率指标|3
价格变换|4
周期指标|5
数学运算符|11
数学变换|15
统计功能|9

笔记本 [common_alpha_factors](02_common_alpha_factors.ipynb) 包含相关代码示例。

## 代码示例：WorldQuant 对公式化 alpha 的追求


我们在第 1 章“交易机器学习：从想法到执行”(../01_machine_learning_for_trading) 中介绍了 [WorldQuant](https://www.worldquant.com/home/)，作为众包趋势的一部分投资策略。
WorldQuant 维护着一个虚拟研究中心，全世界的宽客们在这里竞相寻找阿尔法。
这些阿尔法是计算表达式形式的交易信号，有助于预测价格走势，就像上一节中描述的常见因素一样。
   
这些公式化的阿尔法将从数据中提取信号的机制转化为代码，并且可以单独开发和测试，目的是将其信息集成到更广泛的自动化策略中（[Tulchinsky 2019](https://onlinelibrary.wiley.com/doi /abs/10.1002/9781119571278.ch1)。
正如本书中反复提到的，在大型数据集中挖掘信号很容易出现多重测试偏差和错误发现。
不管这些重要的注意事项如何，这种方法代表了上一节中介绍的更传统功能的现代替代方案。

[Kakushadze (2016) 提出了此类 alpha 的 [101 个示例](https://arxiv.org/pdf/1601.00991.pdf)，其中 80% 在当时的现实世界交易系统中使用。它定义了一系列对横截面或时间序列数据进行操作并且可以组合的函数，例如以嵌套形式。

笔记本 [101_formulaic_alphas](03_101_formulaic_alphas.ipynb) 包含相关代码。

## 代码示例：双变量和多变量因子评估


为了评估众多因素，我们依靠本书中介绍的各种绩效衡量标准，包括以下内容：
- 因素信号内容相对于一日远期回报的双变量测量
- 训练梯度提升模型的特征重要性的多变量度量，以使用所有因素预测一日远期收益
- 使用 Alphalens 根据因子分位数投资组合的财务绩效

笔记本 [factor_evaluation](04_factor_evaluation.ipynb) 和 [alphalens_analysis](05_alphalens_analysis.ipynb) 包含相关的代码示例。



