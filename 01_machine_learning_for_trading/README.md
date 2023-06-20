# 交易机器学习：从创意到执行

算法交易依赖于执行算法以自动执行交易策略的部分或全部元素的计算机程序。 **算法**是为实现目标而设计的一系列步骤或规则。它们可以采用多种形式并促进整个投资过程的优化，从创意产生到资产配置、交易执行和风险管理。

**机器学习** (ML) 涉及从数据中学习规则或模式以实现诸如最小化预测误差等目标的算法。本书中的示例将说明 ML 算法如何从数据中提取信息以支持或自动化关键投资活动。这些活动包括观察市场和分析数据以形成对未来的预期并决定下达买入或卖出订单，以及管理由此产生的投资组合以产生相对于风险的有吸引力的回报。

最终，主动投资管理的目标是产生阿尔法，定义为超过用于评估的基准的投资组合回报。 **主动管理的基本法则**假设产生阿尔法的关键是拥有准确的回报预测以及根据这些预测采取行动的能力（Grinold 1989；Grinold 和 Kahn 2000）。

它定义了**信息比率** (IR)，将主动管理的价值表示为投资组合与基准之间的回报差异与这些回报的波动性之比。它进一步将 IR 近似为
- **信息系数** (IC)，衡量预测质量与结果的等级相关性
- **策略广度**的平方根表示为对这些预测的独立投注数量

金融市场中经验丰富的投资者的竞争意味着做出精确预测以产生 alpha 需要卓越的信息，要么通过访问更好的数据，要么通过卓越的处理能力，要么两者兼而有之。这就是 ML 的用武之地：**ML 交易 (ML4T)** 的应用通常旨在更有效地利用迅速多样化的数据范围，以产生更好和更具可操作性的预测，从而提高投资决策的质量和结果。

从历史上看，算法交易曾经被更狭义地定义为交易执行的自动化，以最大限度地减少卖方提供的成本。这本书采用了更全面的视角，因为一般算法的使用，特别是 ML，已经开始影响更广泛的活动，从产生想法和从数据中提取信号到资产配置、头寸调整、测试和评估策略.

本章着眼于导致机器学习成为投资行业竞争优势来源的行业趋势。我们还将研究机器学习在投资过程中的哪些位置以启用算法交易策略。

## 内容

1. [机器学习在投资行业的兴起](#the-rise-of-ml-in-the-investment-industry)
    * [从电子交易到高频交易](#from-electronic-to-high-frequency-trading)
    * [因子投资和聪明贝塔基金](#factor-investing-and-smart-beta-funds)
    * [胜过人类的算法先驱](#algorithmic-pioneers-outperform-humans)
        - [机器学习驱动的基金吸引了 1 万亿美元的资产管理规模](#ml-driven-funds-attract-1-trillion-aum)
        - [量子基金的出现](#the-emergence-of-quantamental-funds)
    * [机器学习和替代数据](#ml-and-alternative-data)
2. [设计和执行机器学习驱动的策略](#designing-and-executing-an-ml-driven-strategy)
    * [采购和管理数据](#sourcing-and-managing-data)
    * [从阿尔法因子研究到投资组合管理](#from-alpha-factor-research-to-portfolio-management)
    * [策略回测](#strategy-backtesting)
3. [ML 交易实践：策略和用例](#ml-for-trading-in-practice-strategies-and-use-cases)
    * [算法策略的演变](#the-evolution-of-algorithmic-strategies)
    * [ML 交易用例](#use-cases-of-ml-for-trading)
        - [用于特征提取和洞察的数据挖掘](#data-mining-for-feature-extraction-and-insights)
        - [用于 alpha 因子创建和聚合的监督学习](#supervised-learning-for-alpha-factor-creation-and-aggregation)
        - [资产配置](#asset-allocation)
        - [测试贸易理念](#testing-trade-ideas)
        - [强化学习](#reinforcement-learning)
4. [资源与参考](#resources--references)
    * [学术研究](#academic-research)
    * [行业新闻](#industry-news)
    * [书籍](#books)
        - [机器学习](#machine-learning)
    * [课程](#courses)
    * [机器学习竞赛与交易](#ml-competitions--trading)
    * [Python 库](#python-库)

## 机器学习在投资行业的兴起

投资行业在过去几十年里发生了翻天覆地的变化，并在竞争加剧、技术进步和充满挑战的经济环境中继续发展。本节回顾了塑造整体投资环境的主要趋势，以及算法交易的背景和如何更具体地使用 ML。

将算法交易和 ML 推向当前突出地位的趋势包括：
- 市场微观结构的变化，例如电子交易的普及以及跨资产类别和地域的市场整合
- 根据风险因素风险而非资产类别制定投资策略
- 计算能力、数据生成和管理以及统计方法的革命，包括深度学习的突破
- 算法交易先驱的表现优于人类全权委托投资者

此外，2001 年和 2008 年的金融危机影响了投资者进行多元化投资和风险管理的方式。结果之一是以交易所交易基金 (ETF) 形式出现的低成本被动投资工具的崛起。 2008 年危机引发主要中央银行大规模购买资产后，在低收益率和低波动性的情况下，注重成本的投资者将超过 3.5 万亿美元从主动管理的共同基金转移到被动管理的 ETF。

竞争压力也反映在较低的对冲基金费用上，从传统的 2% 的年管理费和 20% 的利润回吐率分别下降到 2017 年的平均 1.48% 和 17.4%。

### 从电子交易到高频交易

自 1960 年代网络开始将价格传送至计算机终端以来，电子交易在能力、交易量、资产类别覆盖范围和地域方面取得了显着进步。

- [Dark Pool Trading & Finance](https://www.cfainstitute.org/en/advocacy/issues/dark-pools), CFA Institute
- [Dark Pools in Equity Trading: Policy Concerns and Recent Developments](https://crsreports.congress.gov/product/pdf/R/R43739), Congressional Research Service, 2014
- [High Frequency Trading: Overview of Recent Developments](https://fas.org/sgp/crs/misc/R44443.pdf), Congressional Research Service, 2016

### 因子投资和聪明贝塔基金

资产提供的回报是与金融投资相关的不确定性或风险的函数。例如，股权投资意味着承担公司的业务风险，而债券投资意味着承担违约风险。

就特定风险特征预测回报而言，识别和预测这些风险因子的行为成为设计投资策略时的主要焦点。它产生有价值的交易信号，是实现卓越主动管理结果的关键。随着时间的推移，行业对风险因子的理解发生了很大的变化，并影响了 ML 用于算法交易的方式。

解释高于和超出 CAPM 的回报的因子被纳入投资风格，使投资组合倾向于一个或多个因子，并且资产开始迁移到基于因子的投资组合。 2008 年的金融危机所有资产类别的一起崩溃，凸显了资产类别标签如何具有高度误导性，并在投资者不考虑潜在因子风险时会造成错误的多元化感。

在过去的几十年里，量化因子投资已经从基于两种或三种风格的简单方法演变为智能多因子或奇异的 beta 产品。 Smart beta 基金在 2017 年的资产管理规模已突破 1 万亿美元，证明了结合主动和被动管理的混合投资策略的流行。聪明贝塔基金采用被动策略，但会根据一个或多个因子对其进行修改，例如更便宜的股票或根据股息支付筛选它们，以产生更好的回报。这种增长恰逢对传统主动管理人收取高额费用的批评越来越多，以及对他们绩效的更严格审查。

持续发现和成功预测风险因子，无论是单独还是与其他风险因子结合，都会显着影响跨资产类别的未来资产回报，是投资行业 ML 激增的关键驱动因素，并将成为贯穿整个过程的关键主题书。

### 胜过人类的算法先驱

率先采用算法交易的公司的资产管理规模 (AUM) 的业绩记录和增长，在引起投资者兴趣后的整个行业努力复制其成功，对法交易的发展发挥了关键作用。

主要或完全依赖算法决策的系统策略最著名的是由数学家詹姆斯·西蒙斯 (James Simons) 提出的，他于 1982 年创立了 Renaissance Technologies，并将其打造成一流的量化公司。其神秘的奖章基金不对外开放，自 1982 年以来估计年化回报率为 35%。

DE Shaw、Citadel 和 Two Sigma 这三支最著名的量化对冲基金使用基于算法的系统策略，自成立以来,扣除手续费后，在 2017 年首次跻身投资者总收益前 20 名.

#### 机器学习驱动的基金吸引了 1 万亿美元的资产管理规模

摩根士丹利在 2017 年估计，算法策略在过去六年中以每年 15% 的速度增长，在对冲基金、共同基金和智能贝塔 ETF 之间控制着约 1.5 万亿美元。其他报告显示，由于传统对冲基金的资金外流，量化对冲基金行业的资产管理规模即将超过 1 万亿美元，自 2010 年以来几乎翻了一番。相比之下，根据最新的全球对冲基金研究报告，对冲基金行业总资本达到 3.21 万亿美元。

- [Global Algorithmic Trading Market to Surpass US$ 21,685.53 Million by 2026](https://www.bloomberg.com/press-releases/2019-02-05/global-algorithmic-trading-market-to-surpass-us-21-685-53-million-by-2026)
- [The stockmarket is now run by computers, algorithms and passive managers](https://www.economist.com/briefing/2019/10/05/the-stockmarket-is-now-run-by-computers-algorithms-and-passive-managers), Economist, Oct 5, 2019

#### 量化交易基金的出现

主动投资管理中出现了两种截然不同的方法：系统（或量化）投资和自主投资。系统（量化）方法依赖于可重复和数据驱动的算法来识别许多证券的投资机会；相比之下，自主投资的方法涉及对较少数量证券的深入分析。随着基本面经理采取更多数据科学驱动的方法，这两种方法变得越来越相似。

据巴克莱银行称，即使是基本面交易员现在也用量化技术武装自己，量化交易基金占 550 亿美元的系统资产。与特定公司无关，量化基金广泛的运用于证券领域交易模式和动态。巴克莱编制的数据显示，量化分析师掌控的资金目前约占对冲基金总资产的 17%。

### 机器学习和替代数据

长期以来，对冲基金一直通过信息优势和发现新的不相关信号的能力来寻找阿尔法。从历史上看，这包括对购物者或选举或全民投票前的选民进行的专有调查。有时，利用公司内部人士、医生和专家网络来扩大对行业趋势或公司的了解会跨越法律界线：2010 年后对交易员、投资组合经理和分析师使用内部信息的一系列起诉震惊了整个行业。

相比之下，使用 ML 开发传统和替代数据源的信息优势与专家和行业网络或访问公司管理无关，而是与收集大量数据并实时分析它们的能力有关。

三种趋势已经彻底改变了算法交易策略中数据的使用，并可能进一步将投资行业从自主投资风格转变为量化风格：
- 数字数据量呈指数级增长
- 以更低的成本增加计算能力和数据存储容量
- 用于分析复杂数据集的 ML 方法的进展

- [Can We Predict the Financial Markets Based on Google's Search Queries?](https://onlinelibrary.wiley.com/doi/abs/10.1002/for.2446), Perlin, et al, 2016, Journal of Forecasting

## 设计和执行机器学习驱动的策略

ML 可以在交易策略生命周期的多个步骤中增加价值，并依赖于关键基础设施和数据资源。因此，本书旨在探讨 ML 技术如何适应更广泛的策略设计、执行和评估过程。

算法交易策略由 alpha 因子组合驱动，这些 alpha 因子将一个或多个数据源转化为信号，进而预测未来资产回报并触发买入或卖出订单。第 2 章，市场和基本数据和第 3 章，金融替代数据涵盖了数据的来源和管理、原材料和成功交易策略的最重要的驱动因素。

[第 4 章，Alpha 因子研究](../04_alpha_factor_research) 概述了一个方法论上合理的过程来管理随着数据量的增加而增加的错误发现的风险。 [第 5 章，策略评估](../05_strategy_evaluation) 提供交易策略执行和绩效衡量的背景。

以下小节概述了这些步骤，我们将在整本书中深入讨论这些步骤。

### 收集和管理数据

数据可用性在数量、种类和速度方面的显着变化是对 ML 在交易中应用的关键补充，这反过来又推动了行业在获取新数据源方面的支出。然而，数据供应的激增需要仔细选择和管理以发现潜在价值，包括以下步骤：


1. 识别和评估包含不会衰减太快的 alpha 信号的市场、基本面和替代数据源。
2. 部署或访问基于云的可扩展数据基础设施和分析工具，如 Hadoop 或 Spark，以促进快速、灵活的数据访问。
3. 仔细管理和整理数据，通过在时间点基础上将数据调整到所需的频率来避免前瞻性偏差。这意味着数据应仅反映给定时间可用和已知的信息。在扭曲的历史数据上训练的 ML 算法几乎肯定会在实时交易中失败。

我们将在第 2 章“市场和基本数据：来源和技术”和第 3 章“金融替代数据：类别和用例”中详细介绍这些方面。

### 从 alpha 因子研究到投资组合管理

Alpha 因子旨在从数据中提取信号，以预测给定投资领域在交易范围内的资产回报。在评估时，一个因子对每项资产采用单一值，但可以组合一个或多个输入变量。该过程涉及下图中概述的步骤：

交易策略工作流程的研究阶段包括阿尔法因子的设计、评估和组合。 ML 在此过程中发挥着重要作用，因为随着投资者对更简单因子的信号衰减和当今可用的更丰富数据的反应，因子的复杂性增加了。

Alpha 因子发出进入和退出信号，导致买入或卖出订单，订单执行导致投资组合持有。各个头寸的风险概况相互作用以创建特定的投资组合风险概况。投资组合管理涉及头寸权重的优化，以实现所需的投资组合风险并返回与总体投资目标一致的配置文件。这个过程是高度动态的，以纳入不断变化的市场数据。

### 策略回测

将投资理念纳入算法策略需要使用科学方法进行广泛测试，该方法试图根据其在替代样本外市场情景中的表现来觉得是否拒绝该理念。测试可能涉及模拟数据以捕获被认为可能但未反映在历史数据中的场景。

## ML 交易实践：策略和用例

在实践中，我们将 ML 应用于特定策略背景下的交易，以满足特定的业务目标。在本节中，我们简要描述了交易策略是如何演变和多样化的，并概述了 ML 应用程序的真实示例，强调了它们与本书所涵盖内容的关系。

### 算法策略的演变

量化策略在三个浪潮中不断发展并变得更加复杂：

1. 在 20 世纪 80 年代和 90 年代，信号通常来自学术研究，并使用来自市场和基本数据的单一或极少数输入。 AQR 是当今最大的量化对冲基金之一，成立于 1998 年，旨在大规模实施此类策略。这些信号现在已基本商品化并以 ETF 的形式提供，例如基本的均值回归策略。
2. 在 2000 年代，在 Eugene Fama 和 Kenneth French 等人的开创性工作的基础上，基于因子的投资激增。基金使用算法来识别暴露于价值或动量等风险因素的资产，以寻求套利机会。金融危机初期的赎回引发了 2007 年 8 月的量化地震，席卷了基于因子的基金行业。这些策略现在也可以作为只做多头的 smart beta 基金提供，这些基金根据一组给定的风险因素倾斜投资组合。
3. 第三个时代是由对 ML 能力和替代数据的投资驱动的，以生成可重复交易策略的盈利信号。因子衰减是一个主要挑战：新异常的超额回报已被证明从发现到发表下降了四分之一，并且由于竞争和拥挤而在发表后下降了 50% 以上。

如今，交易者在使用算法执行规则时追求一系列不同的目标：
- 旨在实现有利定价的交易执行算法
- 旨在从小幅价格变动中获利的短期交易，例如，由于套利
- 旨在预测其他市场参与者行为的行为策略
- 基于绝对和相对价格和回报预测的交易策略

### 机器学习交易用例

机器学习从广泛的市场、基本面和另类数据中提取信号，并且可以应用于算法交易策略过程的所有步骤。主要应用包括：
- 数据挖掘以识别模式、提取特征并产生洞察力
- 监督学习以产生风险因素或 alpha 并创造交易想法
- 将单个信号聚合成一个策略
- 根据算法学习的风险状况分配资产
- 策略的测试和评估，包括通过使用合成数据
- 使用强化学习对策略进行交互式、自动改进

我们简要地强调了其中的一些应用程序，并确定了我们将在后面的章节中展示它们的用途的地方。

#### 用于特征提取和洞察的数据挖掘

对大型复杂数据集进行经济高效的评估需要大规模检测信号。整本书中有几个例子：
- **信息论**有助于估计候选特征的信号内容，因此有助于为 ML 模型提取最有价值的输入。在第 4 章金融特征工程：如何研究 Alpha 因素中，我们使用互信息来比较监督学习算法预测资产回报的各个特征的潜在值。 De Prado (2018) 的第 18 章估计价格序列的信息内容，以此作为决定替代交易策略的基础。
- **无监督学习**提供了广泛的方法来识别数据中的结构以获得洞察力或帮助解决下游任务。我们提供了几个例子：
    - 在第 13 章，[无监督学习：从数据驱动的风险因素到分层风险平价](../13_unsupervised_learning/README.md)，我们介绍了聚类和降维以从高维数据集生成特征。
    - 在第 15 章，[收益电话和金融新闻的主题建模](../15_topic_modeling/README.md)，我们应用贝叶斯概率模型来总结金融文本数据。
    - 在第 20 章：[条件风险因素的自动编码器](../20_autoencoders_for_conditional_risk_factors)，我们使用深度学习提取以资产特征为条件的非线性风险因素，并根据 [Kelly et.等]（https://www.aqr.com/Insights/Research/Working-Paper/Autoencoder-Asset-Pricing-Models）（2020 年）。
- **模型透明度**：我们强调特定于模型的方法来深入了解各个变量的预测能力，并引入一种称为 SHapley Additive exPlanations (SHAP) 的新颖博弈论方法。我们在第 12 章“提升您的交易策略”和附录中将其应用于具有大量输入变量的梯度提升机。

#### 用于 alpha 因子创建和聚合的监督学习

将 ML 应用于交易最常见的理由是获得对资产基本面、价格变动或市场状况的预测。一个策略可以利用多个相互构建的 ML 算法：

- **下游模型**可以通过整合对单个资产的前景、资本市场预期以及证券之间的相关性的预测，在投资组合层面产生信号。
- 或者，ML 预测可以为**全权交易**提供信息，就像前面概述的量化方法一样。

ML 预测还可以**针对特定风险因素**，例如价值或波动性，或实施技术方法，例如趋势跟踪或均值回归：
- 在第 3 章中，[金融的替代数据：类别和用例](../03_alternative_data/README.md)，我们说明了如何使用基本数据为 ML 驱动的估值模型创建输入。
- 第 14 章，[交易文本数据：情绪分析](../14_working_with_text_data/README.md)，第 15 章，[收益电话和金融新闻的主题建模](../15_topic_modeling/README.md)，以及第 16 章，[提取更好的特征：收益电话和 SEC 文件的词嵌入](../16_word_embeddings/README.md)，我们使用可用于预测公司收入的商业评论的替代数据作为估价活动。
- 在第 9 章，[从波动率预测到统计套利：时间序列模型](../09_time_series_models/README.md)，我们演示了如何预测宏观变量作为市场预期的输入，以及如何预测波动率等风险因素
- 在第 19 章，[用于交易的 RNN：多变量回报序列和文本数据](../19_recurrent_neural_nets/README.md)，我们介绍了循环神经网络，它使用非线性时间序列数据实现了卓越的性能。

#### 资产分配
机器学习已被用于根据计算风险平价分层形式的决策树模型来分配投资组合。因此，风险特征是由资产价格模式而不是资产类别驱动的，并实现了卓越的风险回报特征。

- 第 5 章，[投资组合优化和绩效评估](../05_strategy_evaluation/README.md)，以及第 13 章，[无监督学习：从数据驱动的风险因素到分层风险平价](../13_unsupervised_learning/README. md)，我们说明了层次聚类如何提取比传统资产类别定义更好地反映相关模式的数据驱动风险类别（参见 De Prado，2018 年第 16 章）。


#### 测试交易思路

回测是选择成功的算法交易策略的关键步骤。使用合成数据的交叉验证是一项关键的 ML 技术，当与适当的方法结合使用以纠正多重测试时，它可以生成可靠的样本外结果。财务数据的时间序列性质要求对标准方法进行修改，以避免前瞻性偏差或以其他方式污染用于训练、验证和测试的数据。此外，历史数据的可用性有限导致使用合成数据的替代方法：
我们将演示使用市场、基本面和替代方法测试 ML 模型的各种方法，这些方法可以获得样本外误差的可靠估计。
在第 21 章，[用于合成训练数据的生成对抗网络](../21_gans_for_synthetic_time_series/README.md)，我们介绍了能够生成高质量合成数据的生成对抗网络 (GAN)。

####强化学习

交易发生在竞争激烈的互动市场中。强化学习旨在训练智能体学习基于奖励的策略函数；它通常被认为是金融 ML 中最有前途的领域之一。见，例如Hendricks 和 Wilcox (2014) 以及 Nevmyvaka、Feng 和 Kearns (2006) 的交易执行申请。

- 在第 22 章 [深度强化学习：构建交易代理](../22_deep_reinforcement_learning/README.md) 中，我们介绍了 Q-learning 等关键强化算法，以演示使用 OpenAI 的 Gym 环境训练强化算法以进行交易。

## 资源与参考

### 学术研究

- [The fundamental law of active management](http://jpm.iijournals.com/content/15/3/30), Richard C. Grinold, The Journal of Portfolio Management Spring 1989, 15 (3) 30-37
- [The relationship between return and market value of common stocks](https://www.sciencedirect.com/science/article/pii/0304405X81900180), Rolf Banz,Journal of Financial Economics, March 1981
- [The Arbitrage Pricing Theory: Some Empirical Results](https://onlinelibrary.wiley.com/doi/abs/10.1111/j.1540-6261.1981.tb00444.x), Marc Reinganum, Journal of Finance, 1981
- [The Relationship between Earnings' Yield, Market Value and Return for NYSE Common Stock](https://pdfs.semanticscholar.org/26ab/311756099c8f8c4e528083c9b90ff154f98e.pdf), Sanjoy Basu, Journal of Financial Economics, 1982
- [Bridging the divide in financial market forecasting: machine learners vs. financial economists](http://www.sciencedirect.com/science/article/pii/S0957417416302585), Expert Systems with Applications, 2016 
- [Financial Time Series Forecasting with Deep Learning : A Systematic Literature Review: 2005-2019](http://arxiv.org/abs/1911.13288), arXiv:1911.13288 [cs, q-fin, stat], 2019 
- [Empirical Asset Pricing via Machine Learning](https://doi.org/10.1093/rfs/hhaa009), The Review of Financial Studies, 2020 
- [The Characteristics that Provide Independent Information about Average U.S. Monthly Stock Returns](http://academic.oup.com/rfs/article/30/12/4389/3091648), The Review of Financial Studies, 2017 
- [Characteristics are covariances: A unified model of risk and return](http://www.sciencedirect.com/science/article/pii/S0304405X19301151), Journal of Financial Economics, 2019 
- [Estimation and Inference of Heterogeneous Treatment Effects using Random Forests](https://doi.org/10.1080/01621459.2017.1319839), Journal of the American Statistical Association, 2018 
- [An Empirical Study of Machine Learning Algorithms for Stock Daily Trading Strategy](https://www.hindawi.com/journals/mpe/2019/7816154/), Mathematical Problems in Engineering, 2019 
- [Predicting stock market index using fusion of machine learning techniques](http://www.sciencedirect.com/science/article/pii/S0957417414006551), Expert Systems with Applications, 2015 
- [Predicting stock and stock price index movement using Trend Deterministic Data Preparation and machine learning techniques](http://www.sciencedirect.com/science/article/pii/S0957417414004473), Expert Systems with Applications, 2015 
- [Deep Learning for Limit Order Books](http://arxiv.org/abs/1601.01987), arXiv:1601.01987 [q-fin], 2016 
- [Trading via Image Classification](http://arxiv.org/abs/1907.10046), arXiv:1907.10046 [cs, q-fin], 2019 
- [Algorithmic trading review](http://doi.org/10.1145/2500117), Communications of the ACM, 2013 
- [Assessing the impact of algorithmic trading on markets: A simulation approach](https://www.econstor.eu/handle/10419/43250), , 2008 
- [The Efficient Market Hypothesis and Its Critics](http://www.aeaweb.org/articles?id=10.1257/089533003321164958), Journal of Economic Perspectives, 2003 
- [The Arbitrage Pricing Theory Approach to Strategic Portfolio Planning](https://doi.org/10.2469/faj.v40.n3.14), Financial Analysts Journal, 1984 

### 行业新闻

- [The Rise of the Artificially Intelligent Hedge Fund](https://www.wired.com/2016/01/the-rise-of-the-artificially-intelligent-hedge-fund/#comments), Wired, 25-01-2016
- [Crowd-Sourced Quant Network Allocates Most Ever to Single Algo](https://www.bloomberg.com/news/articles/2018-08-02/crowd-sourced-quant-network-allocates-most-ever-to-single-algo), Bloomberg, 08-02-2018
- [Goldman Sachs’ lessons from the ‘quant quake’](https://www.ft.com/content/fdfd5e78-0283-11e7-aa5b-6bb07f5c8e12), Financial Times, 03-08-2017
- [Lessons from the Quant Quake resonate a decade later](https://www.ft.com/content/a7a04d4c-83ed-11e7-94e2-c5b903247afd), Financial Times, 08-18-2017
- [Smart beta funds pass $1tn in assets](https://www.ft.com/content/bb0d1830-e56b-11e7-8b99-0191e45377ec), Financial Times, 12-27-2017
- [BlackRock bets on algorithms to beat the fund managers](https://www.ft.com/content/e689a67e-2911-11e8-b27e-cc62a39d57a0), Financial Times, 03-20-2018
- [Smart beta: what’s in a name?](https://www.ft.com/content/d1bdabaa-a9f0-11e7-ab66-21cc87a2edde), Financial Times, 11-27-2017
- [Computer-driven hedge funds join industry top performers](https://www.ft.com/content/9981c870-e79a-11e6-967b-c88452263daf), Financial Times, 02-01-2017
- [Quants Rule Alpha’s Hedge Fund 100 List](https://www.institutionalinvestor.com/article/b1505pmf2v2hg3/quants-rule-alphas-hedge-fund-100-list), Institutional Investor, 06-26-2017
- [The Quants Run Wall Street Now](https://www.wsj.com/articles/the-quants-run-wall-street-now-1495389108), Wall Street Journal, 05-21-2017
- ['We Don’t Hire MBAs': The New Hedge Fund Winners Will Crunch The Better Data Sets](https://www.cbinsights.com/research/algorithmic-hedge-fund-trading-winners/), cbinsights, 06-28-2018
- [Artificial Intelligence: Fusing Technology and Human Judgment?](https://blogs.cfainstitute.org/investor/2017/09/25/artificial-intelligence-fusing-technology-and-human-judgment/), CFA Institute, 09-25-2017
- [The Hot New Hedge Fund Flavor Is 'Quantamental'](https://www.bloomberg.com/news/articles/2017-08-25/the-hot-new-hedge-fund-flavor-is-quantamental-quicktake-q-a), Bloomberg, 08-25-2017
- [Robots Are Eating Money Managers’ Lunch](https://www.bloomberg.com/news/articles/2017-06-20/robots-are-eating-money-managers-lunch), Bloomberg, 06-20-2017
- [Rise of Robots: Inside the World's Fastest Growing Hedge Funds](https://www.bloomberg.com/news/articles/2017-06-20/rise-of-robots-inside-the-world-s-fastest-growing-hedge-funds), Bloomberg, 06-20-2017
- [When Silicon Valley came to Wall Street](https://www.ft.com/content/ba5dc7ca-b3ef-11e7-aa26-bb002965bce8), Financial Times, 10-28-2017
- [BlackRock bulks up research into artificial intelligence](https://www.ft.com/content/4f5720ce-1552-11e8-9376-4a6390addb44), Financial Times, 02-19-2018
- [AQR to explore use of ‘big data’ despite past doubts](https://www.ft.com/content/3a8f69f2-df34-11e7-a8a4-0a1e63a52f9c), Financial Times, 12-12-2017
- [Two Sigma rapidly rises to top of quant hedge fund world](https://www.ft.com/content/dcf8077c-b823-11e7-9bfb-4a9c83ffa852), Financial Times, 10-24-2017
- [When Silicon Valley came to Wall Street](https://www.ft.com/content/ba5dc7ca-b3ef-11e7-aa26-bb002965bce8), Financial Times, 10-28-2017
- [Artificial intelligence (AI) in finance - six warnings from a central banker](https://www.bundesbank.de/en/press/speeches/artificial-intelligence--ai--in-finance--six-warnings-from-a-central-banker-711602), Deutsche Bundesbank, 02-27-2018
- [Fintech: Search for a super-algo](https://www.ft.com/content/5eb91614-bee5-11e5-846f-79b0e3d20eaf), Financial Times, 01-20-2016
- [Barron’s Top 100 Hedge Funds](https://www.barrons.com/articles/top-100-hedge-funds-1524873705)
- [How high-frequency trading hit a speed bump](https://www.ft.com/content/d81f96ea-d43c-11e7-a303-9060cb1e5f44), FT, 01-01-2018

### 书籍

- [Advances in Financial Machine Learning](https://www.wiley.com/en-us/Advances+in+Financial+Machine+Learning-p-9781119482086), Marcos Lopez de Prado, 2018
- [Quantresearch](http://www.quantresearch.info/index.html) by Marcos López de Prado
- [Quantitative Trading](http://epchan.blogspot.com/), Ernest Chan
- [Machine Learning in Finance](https://www.springer.com/gp/book/9783030410674), Dixon, Matthew F., Halperin, Igor, Bilokon, Paul, Springer, 2020

#### 机器学习

- [Machine Learning](http://www.cs.cmu.edu/~tom/mlbook.html), Tom Mitchell, McGraw Hill, 1997
- [An Introduction to Statistical Learning](http://www-bcf.usc.edu/~gareth/ISL/), Gareth James et al.
    - Excellent reference for essential machine learning concepts, available free online
- [Bayesian Reasoning and Machine Learning](http://web4.cs.ucl.ac.uk/staff/D.Barber/textbook/091117.pdf), Barber, D., Cambridge University Press, 2012 (updated version available on author's website)

### 课程

- [Algorithmic Trading](http://personal.stevens.edu/~syang14/fe670.htm), Prof. Steve Yang, Stevens Institute of Technology
- [Machine Learning](https://www.coursera.org/learn/machine-learning), Andrew Ng, Coursera
- [Deep Learning Specialization](http://deeplearning.ai/), Andrew Ng
    - Andrew Ng’s introductory deep learning course
- Machine Learning for Trading Specialization, [Coursera](https://www.coursera.org/specializations/machine-learning-trading)
- Machine Learning for Trading, Georgia Tech CS 7646, [Udacity](https://www.udacity.com/course/machine-learning-for-trading--ud501
- Introduction to Machine Learning for Trading, [Quantinsti](https://quantra.quantinsti.com/course/introduction-to-machine-learning-for-trading)

### 机器学习竞赛与交易

- [IEEE Investment Ranking Challenge](https://www.crowdai.org/challenges/ieee-investment-ranking-challenge)
    - [Investment Ranking Challenge : Identifying the best performing stocks based on their semi-annual returns](https://arxiv.org/pdf/1906.08636.pdf)
- [Two Sigma Financial Modeling Challenge](https://www.kaggle.com/c/two-sigma-financial-modeling)
- [Two Sigma: Using News to Predict Stock Movements](https://www.kaggle.com/c/two-sigma-financial-news)
- [The Winton Stock Market Challenge](https://www.kaggle.com/c/the-winton-stock-market-challenge)
- [Algorithmic Trading Challenge](https://www.kaggle.com/c/AlgorithmicTradingChallenge)
   
### Python 库

- matplotlib [docs](https://github.com/matplotlib/matplotlib)
- numpy [docs](https://github.com/numpy/numpy)
- pandas [docs](https://github.com/pydata/pandas)
- scipy [docs](https://github.com/scipy/scipy)
- scikit-learn [docs](https://scikit-learn.org/stable/user_guide.html)
- LightGBM [docs](https://lightgbm.readthedocs.io/en/latest/)
- CatBoost [docs](https://catboost.ai/docs/concepts/about.html)
- TensorFlow [docs](https://www.tensorflow.org/guide)
- PyTorch [docs](https://pytorch.org/docs/stable/index.html)
- Machine Learning Financial Laboratory (mlfinlab) [docs](https://mlfinlab.readthedocs.io/en/latest/)
- seaborn [docs](https://github.com/mwaskom/seaborn)
- statsmodels [docs](https://github.com/statsmodels/statsmodels)
- [Boosting numpy: Why BLAS Matters](http://markus-beuckelmann.de/blog/boosting-numpy-blas.html)



















































