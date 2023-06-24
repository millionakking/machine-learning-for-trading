# 投资组合优化和绩效评估

为了在市场条件下实施之前测试策略，我们需要模拟算法进行的交易并验证其性能。策略评估包括针对历史数据进行回测以优化策略的参数，以及进行前向测试以根据新的样本外数据验证样本内性能。目标是避免因根据过去的特定情况调整策略而出现错误。

在投资组合的背景下，正资产回报可以抵消负价格变动。一种资产的正价格变化更有可能抵消另一种资产的损失，两种头寸之间的相关性越低。基于投资组合风险如何取决于头寸的协方差，Harry Markowitz 于 1952 年开发了基于多元化的现代投资组合管理背后的理论。其结果是均值方差优化，它为一组给定的资产选择权重以最小化风险，衡量为给定预期收益的收益标准差。

资本资产定价模型 (CAPM) 引入了风险溢价，以超过无风险投资的预期回报来衡量，作为持有资产的均衡回报。这种回报补偿了对单一风险因素（市场）的敞口，该风险因素是系统性的，而不是资产的特殊性，因此不能分散掉。

随着额外的风险因素和更精细的风险暴露选择的出现，风险管理已经变得更加复杂。凯利准则是一种流行的动态投资组合优化方法，它是随时间推移选择一系列头寸；爱德华·索普 (Edward Thorp) 于 1968 年将其最初在赌博中的应用改编成了股票市场，这一点广为人知。

因此，有多种优化投资组合的方法，包括应用机器学习 (ML) 来了解资产之间的层次关系，并将其持有的资产视为投资组合风险状况的补充或替代品。本章将涵盖以下主题：

## 内容

1. [如何衡量投资组合绩效](#how-to-measure-portfolio-performance)
    * [（调整后的）夏普比率]（#the-adjusted-sharpe-ratio）
    * [主动管理的基本法则](#the-fundamental-law-of-active-management)
2. [如何管理投资组合风险和回报](#how-to-manage-portfolio-risk--return)
    * [现代投资组合管理的演变](#the-evolution-of-modern-portfolio-management)
    * [均值-方差优化](#mean-variance-optimization)
        - [代码示例：在 Python 中寻找有效边界](#code-examples-finding-the-efficient-frontier-in-python)
    * [均值方差优化的替代方案](#alternatives-to-mean-variance-optimization)
        - [1/N 组合](#the-1n-portfolio)
        - [最小方差投资组合](#the-minimum-variance-portfolio)
        - [Black-Litterman 方法](#the-black-litterman-approach)
        - [如何调整你的赌注——凯利法则](#how-to-size-your-bets--the-kelly-rule)
        - [使用 Python 进行 MV 优化的替代方案](#alternatives-to-mv-optimization-with-python)
    * [分层风险平价](#hierarchical-risk-parity)
3. [使用 `Zipline` 交易和管理投资组合](#trading-and-managing-a-portfolio-with-zipline)
    * [代码示例：通过交易和投资组合优化进行回测](#code-examples-backtests-with-trades-and-portfolio-optimization-)
4. [使用 `pyfolio` 测量回测性能](#measure-backtest-performance-with-pyfolio)
    * [代码示例：来自 `Zipline` 回测的 `pyfolio` 评估]（#code-example-pyfolio-evaluation-from-a-zipline-backtest）

## 如何衡量投资组合的表现

为了评估和比较不同的策略或改进现有策略，我们需要能够反映它们在我们目标方面的表现的指标。在投资和交易中，最常见的目标是**投资组合的回报和风险**。

回报和风险目标意味着权衡：在某些情况下承担更多风险可能会产生更高的回报，但也意味着更大的下行空间。为了比较不同的策略如何应对这种权衡，计算每单位风险回报率的比率非常受欢迎。我们将依次讨论**夏普比率**和**信息比率**（IR）。

###（调整后的）夏普比率

事前夏普比率 (SR) 将投资组合的预期超额投资组合与其标准差衡量的超额回报的波动性进行比较。它将报酬衡量为承担每单位风险的平均超额回报。可以从数据中估算出来。

财务回报通常违反独立同分布假设。 Andrew Lo 对平稳但自相关的回报的分布和时间聚合进行了必要的调整。这很重要，因为投资策略的时间序列属性（例如，均值回归、动量和其他形式的序列相关性）会对 SR 估计量本身产生不小的影响，尤其是在从更高频率对 SR 进行年化时数据。

- [夏普比率统计](https://www.jstor.org/stable/4480405?seq=1#page_scan_tab_contents)，Andrew Lo，金融分析师杂志，2002 年

###主动管理的基本规律

一个奇怪的事实是，我们在 [第 1 章](../01_machine_learning_for_trading) 中提到的由 Jim Simons 创立的表现最佳的量化基金 Renaissance Technologies (RenTec) 产生了与沃伦巴菲特相似的回报，尽管方法截然不同。沃伦巴菲特的投资公司伯克希尔哈撒韦公司在相当长的时间内持有大约 100-150 只股票，而 RenTec 每天可能执行 100,000 笔交易。我们如何比较这些不同的策略？

ML 是关于优化目标函数的。在算法交易中，目标是整体投资组合的回报和风险，通常相对于基准（可能是现金、无风险利率或资产价格指数，如标准普尔 500 指数）。

高信息率 (IR) 意味着相对于所承担的额外风险而言具有吸引力的出色表现。主动管理的基本法则将 IR 分解为信息系数 (IC)，作为衡量预测技能的指标，以及通过独立下注应用此技能的能力。它总结了经常玩（高广度）和玩好（高 IC）的重要性。

IC 测量 alpha 因子与其信号产生的远期回报之间的相关性，并捕捉经理预测技能的准确性。策略的广度由投资者在给定时间段内进行的独立投注数量来衡量，两个值的乘积与 IR 成正比，也称为评估风险（Treynor 和 Black）。

基本定律很重要，因为它突出了表现出色的关键驱动因素：准确的预测和做出独立预测并根据这些预测采取行动的能力都很重要。在实践中，鉴于预测之间的横截面和时间序列相关性，很难估计策略的广度。

- [积极的投资组合管理：产生卓越回报和控制风险的定量方法](https://www.amazon.com/Active-Portfolio-Management-Quantitative-Controlling/dp/0070248826)，Richard Grinold 和 Ronald Kahn，1999
- [如何使用证券分析改进投资组合选择](https://econpapers.repec.org/article/ucpjnlbus/v_3a46_3ay_3a1973_3ai_3a1_3ap_3a66-86.htm)，Jack L Treynor 和 Fischer Black，商业杂志，1973 年
- [投资组合约束和主动管理的基本法则](https://faculty.fuqua.duke.edu/~charvey/Teaching/BA491_2005/Transfer_coefficient.pdf)，Clarke 等人，2002 年

## 如何管理投资组合风险和回报

投资组合管理旨在选择和调整金融工具中的头寸，以实现与基准相关的预期风险回报权衡。作为投资组合经理，在每个时期，您都会选择优化多元化的头寸，以降低风险，同时实现目标回报。在各个时期，这些头寸可能需要重新平衡以考虑价格变动导致的权重变化，以实现或维持目标风险状况。

### 现代投资组合管理的演变

多元化允许我们通过利用不完全相关性如何允许一种资产的收益来弥补另一种资产的损失来降低给定预期回报的风险。 Harry Markowitz 于 1952 年发明了现代投资组合理论 (MPT)，并提供了通过选择适当的投资组合权重来优化多元化的数学工具。
 
### 均值-方差优化

现代投资组合理论求解最佳投资组合权重，以最小化给定预期回报的波动性，或最大化给定波动水平的回报。关键的必要输入是预期资产回报、标准差和协方差矩阵。
- [投资组合选择](https://www.math.ust.hk/~maykwok/courses/ma362/07F/markowitz_JF.pdf), Harry Markowitz, The Journal of Finance, 1952
- [资本资产定价模型：理论与证据](http://mba.tuck.dartmouth.edu/bespeneckbo/default/AFA611-Eckbo%20web%20site/AFA611-S6B-FamaFrench-CAPM-JEP04.pdf)， Eugene F. Fama 和 Kenneth R. French，经济展望杂志，2004 年

#### 代码示例：在 Python 中寻找有效边界

我们可以使用 scipy.optimize.minimize 和资产回报、标准差和协方差矩阵的历史估计来计算有效边界。
- 笔记本 [mean_variance_optimization](04_mean_variance_optimization.ipynb) 用于计算 python 中的有效边界。

### 均值-方差优化的替代方案

均值-方差优化问题的准确输入带来的挑战导致采用了几种实用的替代方法，这些替代方法限制了均值、方差或两者，或者忽略了更具挑战性的回报估计，例如我们讨论的风险平价方法在本节后面。

#### 1/N 投资组合

简单的投资组合提供了有用的基准来衡量产生过度拟合风险的复杂模型的附加值。最简单的策略——等权重投资组合——已被证明是表现最好的策略之一。

#### 最小方差投资组合

另一种选择是全局最小方差 (GMV) 投资组合，它优先考虑风险最小化。它显示在有效边界图中，可以通过使用均值-方差框架最小化投资组合标准差来计算如下。

#### Black-Litterman 方法

Black 和 Litterman (1992) 的全球投资组合优化方法将经济模型与统计学习相结合，并且很受欢迎，因为它可以生成在许多情况下都合理的预期回报估计。
该技术假设市场是 CAPM 均衡模型所暗示的均值-方差投资组合。它建立在这样一个事实的基础上，即观察到的市值可以被视为市场分配给每种证券的最佳权重。市场权重反映了市场价格，而市场价格又体现了市场对未来回报的预期。

- [全球投资组合优化](http://www.sef.hku.hk/tpg/econ6017/2011/black-litterman-1992.pdf), Black, Fischer;利特曼，罗伯特
金融分析师杂志，1992 年

#### 如何调整你的赌注——凯利法则

凯利规则在赌博领域有着悠久的历史，因为它提供了关于在不同（但有利的）赔率的（无限）投注序列中的每一个下注多少以最大化最终财富的指导。它于 1956 年由 Claude Shannon 在贝尔实验室的同事 John Kelly 作为信息率的新解释发表。他对新问答节目 The $64,000 Question 中对候选人的赌注很感兴趣，西海岸的一位观众利用三个小时的延迟来获取有关获胜者的内幕信息。

凯利将香农的信息理论联系起来，以解决在赔率有利但不确定性仍然存在的情况下对长期资本增长的最佳赌注。他的规则最大化对数财富作为每场比赛成功几率的函数，并且包括隐含的破产保护，因为 log(0) 是负无穷大，因此凯利赌徒自然会避免失去一切。

- [信息率的新解释](https://www.princeton.edu/~wbialek/rome/refs/kelly_56.pdf)，John Kelly，1956 年
- [击败庄家：21 人游戏的制胜策略](https://www.amazon.com/Beat-Dealer-Winning-Strategy-Twenty-One/dp/0394703103)，Edward O. Thorp， 1966年
- [击败市场：科学的股票市场系统](https://www.researchgate.net/publication/275756748_Beat_the_Market_A_Scientific_Stock_Market_System)，Edward O. Thorp，1967
- [量化交易：如何建立自己的算法交易业务](https://www.amazon.com/Quantitative-Trading-Build-Algorithmic-Business/dp/0470284889/ref=sr_1_2?s=books&ie=UTF8&qid=1545525861&sr =1-2), Ernie Chan, 2008

#### 使用 Python 进行 MV 优化的替代方案

- notebook [kelly_rule](05_kelly_rule.ipynb) 演示了单个和多个资产案例的应用。
- 后者的结果也包含在笔记本 [mean_variance_optimization](04_mean_variance_optimization.ipynb) 中，以及其他几种替代方法。

### 分层风险平价

这种由 [Marcos Lopez de Prado](http://www.quantresearch.org/) 开发的新方法旨在解决一般二次优化器和马科维茨临界线算法 (CLA) 的三个主要问题，特别是：
- 不稳定，
- 浓度，和
- 表现不佳。

分层风险平价 (HRP) 应用图论和机器学习，根据协方差矩阵中包含的信息构建多元化投资组合。然而，与二次优化器不同，HRP 不需要协方差矩阵的可逆性。事实上，HRP 可以在病态退化甚至奇异的协方差矩阵上计算投资组合——这对于二次优化器来说是一项不可能完成的壮举。蒙特卡洛实验表明，HRP 提供的样本外方差低于 CLA，尽管最小方差是 CLA 的优化目标。与传统的风险平价方法相比，HRP 还可以从样本中生成风险较低的投资组合。我们将在 [第 13 章](../13_unsupervised_learning) 讨论无监督学习（包括层次聚类）在交易中的应用时更详细地讨论 HRP。

- [建立优于样本外的多元化投资组合](https://jpm.pm-research.com/content/42/4/59.short)，Marcos López de Prado，《投资组合管理杂志》42，第 1 期。 4（2016 年）：59-69。
- [基于分层聚类的资产分配](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2840729)，Thomas Raffinot，2016 年

我们演示了如何实施 HRP 并将其与第 13 章[无监督学习](../13_unsupervised_learning) 中的替代方案进行比较，其中我们还介绍了层次聚类。

## 使用 `Zipline` 交易和管理投资组合

开源[zipline](https://zipline.ml4trading.io/index.html)库是由众包量化投资基金[Quantopian](https:// www.quantopian.com/) 以促进算法开发和实时交易。它使算法对交易事件的反应自动化，并为其提供当前和历史时间点数据，避免前瞻性偏差。 [第 8 章 - ML4T 工作流程](../08_strategy_workflow) 更详细、专门介绍了使用 `zipline` 和 `backtrader` 进行回溯测试。

在 [第 4 章](../04_alpha_factor_research) 中，我们引入了 `zipline` 来模拟从尾随横截面市场、基本面和另类数据计算 alpha 因子。现在我们将利用 alpha 因子来导出买卖信号并对其采取行动。

### 代码示例：通过交易和投资组合优化进行回测

本节的代码位于以下两个笔记本中：
- 本节中的笔记本使用 `conda` 环境 `backtest`。请参阅安装 [instructions](../installation/README.md) 下载最新的 Docker 镜像或设置环境的其他方法。
- 笔记本 [backtest_with_trades](01_backtest_with_trades.ipynb) 模拟交易决策，这些决策基于上一章使用 Zipline 的简单 MeanReversion alpha 因子构建投资组合。我们没有明确优化投资组合权重，只是为每个持股分配同等价值的头寸。
- 笔记本 [backtest_with_pf_optimization](02_backtest_with_pf_optimization.ipynb) 演示了如何将 PF 优化用作简单策略回测的一部分。

## 使用 `pyfolio` 测量回测性能

Pyfolio 使用许多标准指标促进样本内和样本外的投资组合绩效和风险分析。它使用多个内置场景生成涵盖回报、头寸和交易分析以及市场压力期间事件风险分析的撕样，还包括贝叶斯样本外绩效分析。

### 代码示例：来自 `Zipline` 回测的 `pyfolio` 评估

笔记本 [pyfolio_demo](03_pyfolio_demo.ipynb) 说明了如何从上一个文件夹中进行的回测中提取 `pyfolio` 输入。然后它继续使用 pyfolio 计算几个性能指标和撕纸

- 此笔记本需要 `conda` 环境 `backtest`。请参阅 [安装说明](../installation/README.md) 以运行最新的 Docker 映像或设置环境的其他方法。
