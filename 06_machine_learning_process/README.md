# 机器学习工作流程

本章从本书的第 2 部分开始，我们将说明如何使用一系列有监督和无监督的机器学习 (ML) 模型进行交易。在使用各种 Python 库演示相关应用程序之前，我们将解释每个模型的假设和用例。我们将在第 2-4 部分中介绍的模型类别包括：

- 用于横截面、时间序列和面板数据的回归和分类的线性模型
- 广义加性模型，包括基于树的非线性模型，例如决策树
- 集成模型，包括随机森林和梯度提升机
- 用于降维和聚类的无监督线性和非线性方法
- 神经网络模型，包括循环和卷积架构
- 强化学习模型

我们将把这些模型应用于本书第一部分介绍的市场、基本面和另类数据源。我们将在迄今为止涵盖的材料的基础上，演示如何将这些模型嵌入到将模型信号转化为交易的交易策略中，如何优化投资组合，以及如何评估策略绩效。

许多这些模型及其应用程序有几个共同点。本章涵盖了这些常见方面，以便我们可以在后续章节中专注于特定于模型的用法。它们包括通过优化目标函数或损失函数从数据中学习函数关系的总体目标。它们还包括密切相关的模型性能测量方法。

我们区分了无监督学习和监督学习，并概述了算法交易的用例。我们对比了监督回归和分类问题，将监督学习用于输入和输出数据之间关系的统计推断及其用于预测未来输出的用途。我们还说明了预测误差是如何由模型的偏差或方差引起的，或者是由于数据中的高信噪比引起的。最重要的是，我们提供了诊断错误来源（如过度拟合）并提高模型性能的方法。

如果您已经非常熟悉 ML，请随意跳到前面，直接深入学习如何使用 ML 模型为算法交易策略生成和组合 alpha 因子。

## 内容

1. [机器学习数据的工作原理](#how-machine-learning-from-data-works)
    * [关键挑战：为给定任务找到正确的算法](#the-key-challenge-finding-the-right-algorithm-for-the-given-task)
    * [监督学习：通过示例教授任务]（#supervised-learning-teaching-a-task-by-example）
    * [无监督学习：探索数据以识别有用的模式](#unsupervised-learning-exploring-data-to-identify-useful-patterns)
        - [交易策略用例：从风险管理到文本处理](#use-cases-for-trading-strategies-from-risk-management-to-text-processing)
    * [强化学习：边做边学，一次一步](#reinforcement-learning-learning-by-doing-one-step-at-a-time)
2. [机器学习工作流程](#the-machine-learning-workflow)
    * [代码示例：具有 K 最近邻的 ML 工作流](#code-example-ml-workflow-with-k-nearest-neighbors)
3. [框架问题：目标和指标](#frame-the-problem-goals--metrics)
4. [收集和准备数据](#collect--prepare-the-data)
5. [如何探索、提取和设计特征](#how-to-explore-extract-and-engineer-features)
    * [代码示例：互信息](#code-example-mutual-information)
6. [选择机器学习算法](#select-an-ml-algorithm)
7. [设计和调整模型](#design-and-tune-the-model)
    * [代码示例：偏差方差权衡]（#code-example-bias-variance-trade-off）
8. [如何使用交叉验证进行模型选择](#how-to-use-cross-validation-for-model-selection)
    * [代码示例：如何在 Python 中实现交叉验证](#code-example-how-to-implement-cross-validation-in-python)
9. [使用 scikit-learn 进行参数调整](#parameter-tuning-with-scikit-learn)
    * [代码示例：使用 yellowbricks 的学习和验证曲线](#code-example-learning-and-validation-curves-with-yellowbricks)
    * [代码示例：使用 GridSearchCV 和管道进行参数调整](#code-example-parameter-tuning-using-gridsearchcv-and-pipeline)
10. [金融交叉验证的挑战](#challenges-with-cross-validation-in-finance)
    * [清除、禁运和组合 CV](#purging-embargoing-and-combinatorial-cv)


## 机器学习数据的工作原理

ML 的许多定义都围绕着自动检测数据中有意义的模式。两个突出的例子包括：
- AI 先驱 Arthur Samuelson 在 1959 年将 ML 定义为计算机科学的一个子领域，它赋予计算机学习能力而无需明确编程。
- 该领域的现任领导者之一汤姆·米切尔 (Tom Mitchell) 在 1998 年更具体地确定了一个适定的学习问题：计算机程序从有关任务的经验中学习，并衡量任务的性能是否随着经验（米切尔，1997）。

经验以训练数据的形式呈现给算法。与之前构建解决问题的机器的尝试的主要区别在于，算法用来做出决策的规则是从数据中学习的，而不是像 1980 年代流行的专家系统那样由人类编程。 

涵盖广泛算法和一般应用的推荐教科书包括
- [统计学习简介](http://faculty.marshall.usc.edu/gareth-james/ISL/)，James 等人 (2013)
- [统计学习的要素：数据挖掘、推理和预测](https://web.stanford.edu/~hastie/ElemStatLearn/)，Hastie、Tibshirani 和 Friedman (2009)
- [模式识别和机器学习](https://www.microsoft.com/en-us/research/uploads/prod/2006/01/Bishop-Pattern-Recognition-and-Machine-Learning-2006.pdf),主教 (2006)
- [机器学习](http://www.cs.cmu.edu/~tom/mlbook.html)，米切尔 (1997)。

### 关键挑战：为给定任务找到正确的算法

自动化学习的主要挑战是在将模型的学习泛化到新数据时识别训练数据中有意义的模式。模型可以识别大量的潜在模式，而训练数据仅构成算法在未来执行任务时可能遇到的更大集合现象的样本。

### 监督学习：通过示例教授任务

监督学习是最常用的 ML 类型。我们将在本书的大部分章节中专门介绍此类应用程序。监督一词意味着存在指导学习过程的结果变量——也就是说，它教会算法手头任务的正确解决方案。监督学习旨在从反映这种关系的单个样本中捕获功能性输入-输出关系，并通过对新数据做出有效陈述来应用其学习。

### 无监督学习：探索数据以识别有用的模式

在解决无监督学习问题时，我们只观察特征而没有测量结果。无监督算法不是预测未来结果或推断变量之间的关系，而是旨在识别输入中的结构，该结构允许对数据中包含的信息进行新的表示。

#### 交易策略用例：从风险管理到文本处理

我们将在后面的章节中介绍无监督学习的大量交易用例：
- 将具有相似风险和收益特征的证券组合在一起（参见[第 13 章中的分层风险平价](../13_unsupervised_learning/04_hierarchical_risk_parity)
- 使用[主成分分析](../13_unsupervised_learning/01_linear_dimensionality_reduction) 或自动编码器（[第 20 章](../20_autoencoders_for_conditional_risk_factors) 寻找少量风险因素驱动大量证券的表现
- 识别包含这些文档最重要方面的文档主体（例如，收益电话会议记录）中的潜在主题（[第 15 章](../15_topic_modeling)）

### 强化学习：边做边学，一步一个脚印

强化学习 (RL) 是第三种 ML。它以需要根据环境提供的信息在每个时间步选择一个动作的代理为中心。代理可以是自动驾驶汽车、玩棋盘游戏或视频游戏的程序，或者在某个证券市场上运行的交易策略。

您可以在 [Sutton and Barto](http://www.incompleteideas.net/book/the-book-2nd.html)，2018 年找到精彩的介绍。

## 机器学习工作流程

开发 ML 解决方案需要一种系统的方法，以在高效进行的同时最大限度地提高成功的机会。使过程透明和可复制以促进协作、维护和后续改进也很重要。

整个过程是迭代的，不同阶段的努力会因项目而异。尽管如此，这个过程一般应该包括以下步骤：

1. 框定问题，确定目标指标，定义成功
2. 获取、清理和验证数据
3. 理解你的数据并生成信息特征
4. 选择一种或多种适合您数据的机器学习算法
5. 训练、测试和调整您的模型
6. 用你的模型解决原来的问题

### 代码示例：K 最近邻 ML 工作流

笔记本 [machine_learning_workflow](01_machine_learning_workflow.ipynb) 包含几个示例，使用简单的房价数据集说明机器学习工作流程。

- sklearn [文档](http://scikit-learn.org/stable/documentation.html)
- k 最近邻 [教程](https://www.datacamp.com/community/tutorials/k-nearest-neighbor-classification-scikit-learn) 和 [可视化](http://vision.stanford.edu/教学/cs231n-demos/knn/)

## 构建问题：目标和指标

任何机器学习练习的起点都是它旨在解决的最终用例。有时，这个目标将是统计推断，以便识别变量之间的关联甚至因果关系。然而，最常见的目标是直接预测结果以产生交易信号。

## 收集和准备数据

我们在 [第 2 章](../02_market_and_fundamental_data) 中讨论了市场和基本数据的来源，并在 [第 3 章](../03_alternative_data) 中讨论了替代数据。在后面的章节中说明各种模型的应用时，我们将继续使用这些来源的各种示例。

## 如何探索、提取和设计特征

了解个体变量的分布以及结果和特征之间的关系是选择合适算法的基础。这通常从散点图等可视化开始，如配套笔记本中所示（并显示在下图中），但也包括从线性指标（例如相关性）到非线性统计（例如 Spearman 等级相关性）的数值评估我们引入信息系数时遇到的系数。它还包括信息论措施，例如互信息

### 代码示例：互信息

notebook [mutual_information](02_mutual_information.ipynb) 将信息论应用于我们在notebook [feature_engineering](../04_alpha_factor_research/00_data/feature_engineering.ipynb)中创建的财务数据，在[Alpha Factors – Research and Evaluation]一章中（ (../04_alpha_factor_research)。

## 选择一个机器学习算法

本书的其余部分将介绍几个模型系列，从对输入和输出变量之间函数关系的性质做出相当强假设的线性模型到几乎不做假设的深度神经网络。

## 设计和调整模型

ML 过程包括根据模型泛化误差的估计来诊断和管理模型复杂性的步骤。无偏估计需要统计上合理且有效的程序，以及与输出变量类型一致的误差度量，这也决定了我们是在处理回归、分类还是排序问题。

### 代码示例：偏差方差权衡

ML 模型在预测新输入数据的结果时所犯的错误可以分解为可减少和不可减少的部分。不可约部分是由于未测量的数据中的随机变化（噪声）引起的，例如相关但缺失的变量或自然变化。

笔记本 [bias_variance](03_bias_variance.ipynb) 通过使用越来越复杂的多项式逼近余弦函数并测量样本内误差来演示过度拟合。它抽取 10 个带有一些附加噪声 (n = 30) 的随机样本来学习不同复杂度的多项式。每次，模型都会预测新的数据点，我们会捕获这些预测的均方误差，以及这些误差的标准差。它继续通过尝试学习九阶余弦函数的泰勒级数近似值并添加一些噪声来说明过度拟合与欠拟合的影响。在下图中，我们绘制了真实函数的随机样本，并拟合了欠拟合、过拟合并提供近似正确程度的灵活性的多项式。

## 如何使用交叉验证进行模型选择

当多个候选模型（即算法）可用于您的用例时，选择其中一个的行为称为模型选择问题。模型选择旨在确定在给定新数据的情况下将产生最低预测误差的模型。

### 代码示例：如何在 Python 中实现交叉验证

脚本 [cross_validation](04_cross_validation.py) 通过展示具有十个观察值的模拟数据集的索引如何分配给训练集和测试集，说明了将数据拆分为训练集和测试集的各种选项。
 
## 使用 scikit-learn 调整参数

模型选择通常涉及使用不同算法（例如线性回归和随机森林）或不同配置对模型的样本外性能进行重复交叉验证。不同的配置可能涉及超参数的更改或不同变量的包含或排除。

### 代码示例：使用 yellowbricks 的学习和验证曲线

笔记本 [machine_learning_workflow](01_machine_learning_workflow.ipynb)) 演示了学习和验证的使用说明了各种模型选择技术的使用。

- Yellowbrick：机器学习可视化 [文档](http://www.scikit-yb.org/en/latest/)

### 代码示例：使用 GridSearchCV 和管道进行参数调整

由于超参数调整是机器学习工作流程的关键组成部分，因此有一些工具可以自动执行此过程。 sklearn 库包括一个 GridSearchCV 接口，该接口并行交叉验证所有参数组合，捕获结果，并使用在完整数据集交叉验证期间表现最佳的参数设置自动训练模型。

在实践中，训练集和验证集通常需要在交叉验证之前进行一些处理。 Scikit-learn 提供的管道还可以在 GridSearchCV 促进的自动超参数调整中自动执行任何必要的特征处理步骤。

随附的 machine_learning_workflow.ipynb 笔记本中的实施示例可查看这些工具的实际应用。

笔记本 [machine_learning_workflow](01_machine_learning_workflow.ipynb)) 也演示了这些工具的使用。

## 金融交叉验证的挑战

到目前为止讨论的交叉验证方法的一个关键假设是可用于训练的样本的独立和相同 (iid) 分布。
对于财务数据，情况往往并非如此。相反，由于序列相关和时变标准差，金融数据既不独立也不同分布，也称为异方差性

### 清除、禁运和组合 CV

对于财务数据，标签通常来自重叠的数据点，因为回报是根据多个时期的价格计算的。在交易策略的上下文中，模型预测的结果可能暗示在资产中建立头寸，可能只有在评估此决策时才知道——例如，当头寸被平仓时。

由此产生的风险包括信息从测试泄漏到训练集中，可能导致性能人为膨胀，需要通过确保所有数据都是及时的——即在当时真正可用和已知的——来解决这个问题它用作模型的输入。 Marcos Lopez de Prado 在 [Advances in Financial Machine Learning](https://www.amazon.com/Advances-Financial-Machine-Learning-Marcos/dp/1119482089) 中提出了几种方法来应对金融数据的这些挑战交叉验证：

- 清除：消除在验证集中预测时间点数据点之后进行评估的训练数据点，以避免前瞻性偏差。
- Embargoing：进一步剔除测试期之后的训练样本。