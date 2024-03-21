InterpretableML

- 第一章 前言

machine learning 本质是一套方法，允许计算机从数据中学习，以做出和改进预测

normal programming，常规编程是指所有指令都必须显式的提供给计算机，而“间接编程“是通过提供数据来实现的



---



- 第二章 可解释性

什么是可解释性？非数学的定义：可解释性是人们能够理解决策原因的程度；可解释性是指人们能够一致的预测模型结果的程度。

interpreteble（模型为什么做出这个预测） explainable（对单个实例预测的解释）一下原因推动了对可解释性的需求：

1. 好奇心和学习能力
2. 渴望找到事物存在的意义
3. 安全措施（自动驾驶）
4. 可解释性是检测偏见的调试工具
5. 增加社会认可度
6. 管理社交互动

可解释性方法分类：

1. intrinsic（本质的可解释性）短的决策树、稀疏线性模型，结构简单、可解释的机器学习模型
2. post-hoc （事后解释方法）特征重要性是一种事后解释方法

解释方法的输出：

1. 特征概要统计量 feature summary statistic，比如每个特征提供重要性，成对特征交互强度
2. 特征概要可视化 feature summary visualization
3. 模型内部（例如学习的权重）model internals，比如树模型的分割特征和阈值
4. 数据点 data point（比方说有一个输入 x 正确的分类 label 是y，我们用某些方法改变一部分输入 x，使得 y 错误 y'，然后找到类似的数据点 x'，使得他正确的输入是 y' 然后做 解释

算法透明度，全局整体的模型可解释性

可解释性的三个主要层次：

1. 应用级评估 application level evaluation，将解释放入产品里，由使用的用户进行测试
2. 人员级评估 human level evaluation，非专业级的人员来评估
3. 功能级评估 function level evaluation，用自动化的程序来进行评估

----

- 第三章 数据集介绍

自行车租赁，季节、天气状况、温度、湿度等，预测当天将租用多少辆自行车。

垃圾评论分类

宫颈癌的危险因素，年龄、性伴侣数量、首次性行为、怀孕次数、是否吸烟等，来预测女性是否会患宫颈癌。

-----

- 第四章 可解释的模型

----



- 第五章 模型无关方法

world -> data (sample) -> black box model -> interpretability methods -> inform human

部分依赖图 partial dependence plot PDP 图，显示了一个或两个特征对机器学习模型的预测结果的边际效应。例如应用于线性回归模型时，部分依赖图始终显示线性关系。
$$
\hat f_{x_S}(x_S) = E_{x_C}[\hat f(x_S, x_C)] = \int \hat f(x_S,x_C)d\mathbb P(x_C)
$$

- $x_C$ 是其部分依赖函数应被绘制的特征（被研究特征）
- $x_C$ 是在机器学习模型 $\hat f$ 中使用的其他特征（其他特征）
- 本质其实就是在特征较少的时候画出函数输出随着 $x_C$ 变动时候的曲线

pros

- 直观
- 容易实现

cons

- 对特征数量有限制，二维受限
- 独立性假设（被研究特征和其他特征不相关）
- ![image-20240315103657834](/Users/zhangzhizuo/Library/Application Support/typora-user-images/image-20240315103657834.png)

个体条件期望 Individual Conditional Expectation ICE图，每个实例显示一条线。

![image-20240315103925867](/Users/zhangzhizuo/Library/Application Support/typora-user-images/image-20240315103925867.png)

中心化个体条件期望图 Centered ICE plot

![image-20240315104708697](/Users/zhangzhizuo/Library/Application Support/typora-user-images/image-20240315104708697.png)

累计局部效应









特征交互 feature interaction，一个特征的效应取决于另一个特征的值。

理论，弗里德曼的 H 统计量

- 首先采用双向互度量，它告诉我们模型中的两个特征是否交互以及在何种程度上交互
- 其次，总体交互度量，他告诉我们某个特征在模型中是否与所有其他特征发生交互以及交互程度

如果两个特征不交互
$$
PD_{jk}(x_j,x_k) = PD_j(x_j)+PD_k(x_k)
$$

-  $PD_{jk}(x_j, x_k)$​ 是两个特征的双向部分依赖函数
- $PD_j(x_j)$ 和 $PD_k(x_k)$ 是单个特征的部分依赖函数
- 简单地理解为没有交叉相，是独立相加

同理：
$$
\hat f(x) = PD_j(x_j) + PD_{-j}(x_{-j})
$$
数学上， friedman 和 popescu 为特征 j 和 k 之间的交互作用提出的 H 统计量为：
$$
H_{jk}^2 = \sum_{i=1}^n[PD_{jk}(x_j^{(i)}, x_k^{(i)}) - PD_j(x_j^{(i)}) - PD_k(x_k^{(i)})] / \sum^n_{i=1}PD_{jk}^2(x_j^{(i)}, x_k^{(i)})
$$
可以理解为：交叉相依赖函数所占总体两个依赖函数的比值，其实还是比较好理解的

![image-20240315165417730](/Users/zhangzhizuo/Library/Application Support/typora-user-images/image-20240315165417730.png)

![image-20240315165428028](/Users/zhangzhizuo/Library/Application Support/typora-user-images/image-20240315165428028.png)

优点：

- 交互作用 H 统计量通过部分依赖分解具有理论基础
- H 统计量具有有意义的解释，交互作用定义为由交互作用解释的方差份额
- 由于统计信息是无量纲的，他在各个特征之间都具有可比性
- 使用 H 统计量，可以分析任意更高阶的交互作用

缺点

- 计算量很大
- 不适用所有数据点，估计值是有差异的，不使用足够多的数据会导致不稳定
- 该检验在与模型无关的版本中尚不可用

代替方法：

Hooker 提出的变量交互网络 VIN 是一种将预测函数分解为主要效应和特征交互的方法，然后将特征之间的交互可视化为网络。

Greenwell 等人给予部分依赖性的特征交互测量了两个特征之间的交互。

- 将一个特征固定在不同的固定点上
- 然后测量另外一个特征的依赖函数的方差



置换特征重要性 permutation feature importance，想法其实也很简单，就是置换两个特征（但是感觉这个完全没啥意义，就是置换数值，因为量纲可能不一样，然后置换的话，数值也确定了，我是觉得没啥意义）



全局代理模型

近似模型 approximation model，元模型 metamodel，响应面模型 response surface model，仿真器 emulator 等等，用代理模型去近似原模型的解。

例子，考虑一个回归和分类问题示例。首先训练一个 svm，根据天气和日历信息预测自行车的租量。svm不容易解释，选择 cart 决策树作为可解释模型训练代理。以下是节点分裂信息：

![image-20240316142131536](/Users/zhangzhizuo/Library/Application Support/typora-user-images/image-20240316142131536.png)

![image-20240316142206121](/Users/zhangzhizuo/Library/Application Support/typora-user-images/image-20240316142206121.png)

优点，代理模型方法非常灵活，可以使用“可解释模型”的任何模型，这种方法非常直观和直接，易于实现。

缺点，得出的是有关模型而不是数据的结论，



局部代理  LIME local interpretable model-agnostic explanations LIME，训练局部代理模型 LIME 的方法

- 选择你想要对其黑河预测进行解释的感兴趣实例
- 扰动你的数据集并获得这些新点的黑盒预测
- 根据新样本与目标实例的接近程度对其进行加权
- 在新数据集上训练加全的，可解释的模型
- 通过解释局部模型来解释预测





---



- 第六章 基于样本的解释
- 第七章水晶球















