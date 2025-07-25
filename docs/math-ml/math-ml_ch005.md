# 第一章：介绍

为什么我要学习数学？——这是我每天都会被问到的问题。

其实，你不必学习。但你应该学！

表面上看，先进的数学对于生产环境中的软件工程和机器学习没有什么影响。你不需要手动计算梯度、解线性方程或找特征值。基本和高级算法已经被抽象成库和 API，它们为你完成了所有的繁重工作。

现在，实现一个最先进的深度神经网络几乎等同于在 PyTorch 中实例化一个对象，加载预训练的权重，然后让数据通过模型快速流动。就像所有技术进步一样，这也是一把双刃剑。一方面，加速原型设计和开发的框架让机器学习得以在实践中应用。没有这些框架，我们就无法看到过去十年中深度学习的爆发。

另一方面，高级抽象是我们与底层技术之间的障碍。用户层面的知识仅在我们走熟悉的道路时才足够。（或者直到某些东西出问题了。）

如果你还不相信，那我们来做一个思想实验吧！想象一下，搬到一个新国家，没有语言能力，也不了解当地的生活方式。不过，你有一部智能手机和一个可靠的互联网连接。

如何开始探索呢？

有了 Google Maps 和一张信用卡，你可以在那儿做很多了不起的事情：探索城市，去优秀的餐馆吃饭，享受美好时光。你可以每天购物，而无需说一个字：只需将东西放进篮子里，在收银台刷卡支付。

几个月后，你也会开始学一些语言——比如简单的问候语或自我介绍。你已经有了一个良好的开端！

有一些内置的解决方案可以处理日常任务，它们工作得非常顺利——比如食品订购服务、公共交通等。然而，到了某个时刻，它们会出现故障。例如，你需要联系那个把包裹送到错门的快递员。或者，如果你的租车坏了，你需要寻求帮助。

你可能还想做更多的事情。找一份工作，或者甚至开创自己的事业。为此，你需要有效地与他人沟通。

当你打算在某个地方待几个月时，学习语言是没必要的。然而，如果你打算在那里度过余生，学习语言就是你能做出的最佳投资之一。

现在，把国家换成机器学习，把语言换成数学。

事实上，算法是用数学语言编写的。要精通算法，你必须懂得这门语言。

## 这本书是关于什么的？

> *“知道如何在一个城市中找到路和精通一门学科之间有相似之处；从任何一个点出发，都应该能够到达任何其他的地方。如果能立即找到从一个地方到另一个地方最方便、最迅捷的路径，那就更好了。”*
> 
> *— 乔治·波利亚与加博尔·谢格，在传奇书籍《分析中的问题与定理》的序言中*

上述引用是我最喜欢的之一。对我来说，它意味着知识建立在许多支柱之上。就像一把椅子有四条腿，一个全面的机器学习工程师也有广泛的技能组合，使他们能够在工作中发挥有效作用。我们每个人都专注于一个平衡的技能星座，而数学对许多人来说是一个很好的补充。你可以在没有高级数学的情况下开始机器学习，但在你职业生涯的某个阶段，了解机器学习的数学背景将帮助你将技能提升到一个新的水平。

深度学习的精通之路有两条。一条从实际部分开始，另一条从理论开始。两者都是完全可行的，最终它们会交织在一起。这本书是为那些已经开始走实际应用路线的人准备的，比如数据科学家、机器学习工程师，甚至对这个话题感兴趣的软件开发者。

这本书不是一部 100%纯粹的数学论文。在某些地方，我会做一些简化，以平衡清晰性与数学准确性。我的目标是让你获得“ Eureka!”的瞬间，并帮助你理解更宏大的图景，而不是为你准备数学博士学位。

我读过的大多数机器学习书籍都属于以下两类之一。

1.  专注于实际应用，但在数学概念上不清晰且不精确。

1.  专注于理论，涉及大量的数学，但几乎没有实际应用。

我希望这本书能够兼顾两种方法的优点：既有基本和高级数学概念的扎实介绍，又始终紧扣机器学习的应用。

我的目标不仅仅是涵盖基础知识，而是提供广泛的知识。在我的经验中，要精通一门学科，既需要深入，也需要广泛。仅仅覆盖数学的最基本内容就像走钢丝一样。与其每次遇到数学课题时都像在表演杂技，我希望你能够打下稳固的基础。这样的自信能带你走得更远，并让你在人群中脱颖而出。

在我们的旅程中，我们将遵循一条带领我们穿越的路线图

1.  线性代数，

1.  微积分，

1.  多变量微积分，

1.  概率论。

我们将从线性代数开始我们的旅程。在机器学习中，数据通过向量表示。训练学习算法就等于通过一系列变换来找到数据的更具描述性的表示。

线性代数是研究向量空间及其变换的学科。

简单来说，神经网络就是一个将数据映射到高级表示的函数。线性变换是这些变换的基本构建块。深入理解它们将大有裨益，因为它们在机器学习中无处不在。

虽然线性代数展示了如何描述预测模型，微积分则提供了将这些模型拟合到数据中的工具。当你训练神经网络时，几乎可以肯定是在使用梯度下降法，这是一种源于微积分和微分研究的技术。

除了微分，其“逆运算”也是微积分的核心部分：积分。积分表示一些重要量，如期望值、熵、均方误差等。它们为概率论和统计学奠定了基础。

然而，在进行机器学习时，我们处理的是包含数百万个变量的函数。在更高维度中，情况有所不同。这就是多变量微积分派上用场的地方，其中微分和积分被调整以适应这些空间。

在掌握了线性代数和微积分之后，我们已经准备好描述和训练神经网络。然而，我们仍然缺乏从数据中提取模式的理解。我们如何从实验和观察中得出结论？我们如何描述和发现其中的模式？这些问题通过概率论和统计学得以解答，它们是科学思维的逻辑。在最后一章中，我们扩展了经典的二元逻辑，学习如何处理预测中的不确定性。

## 如何阅读本书

数学遵循定义-定理-证明的结构，这可能一开始很难理解。如果你不熟悉这种思维流程，不用担心，我会在接下来做一个温和的介绍。

本质上，数学是通过数学对象的基本性质来研究抽象对象（例如函数）。与经验观察不同，数学是基于逻辑的，因此具有普遍性。如果我们想要使用逻辑这一强大工具，数学对象需要被精确定义。定义通常会像下面这样以框框的形式呈现。

定义 1.（一个示例定义）

定义通常以这种方式呈现。

给定一个定义，结果通常以“如果 A，则 B”的形式表述，其中 A 为前提，B 为结论。这样的结果称为定理。例如，如果一个函数是可微的，那么它也是连续的；如果一个函数是凸的，那么它有全局最小值；如果我们有一个函数，那么我们可以通过单层神经网络以任意精度近似它。你已经掌握了这种模式。定理是数学的核心。

我们必须提供合理的逻辑推理来接受命题的有效性，这种推理将结论从前提中推导出来。这被称为证明，是数学学习曲线陡峭的原因之一。与其他科学学科不同，数学中的证明是不可争议的陈述，永远铭刻在石上。实际操作中，注意这些框框。

定理 1.（一个示例定理）

设 x 为一个复杂的数学对象。以下两个陈述成立。

（a 如果 A，则 B。

（b）如果 C 和 D，则 E。

证明。证明过程如下。

为了增强学习体验，我会经常把一些有用但不是绝对必要的信息放入备注中。

备注 1。（一个令人兴奋的备注）

数学太棒了。你将因此成为一个更好的工程师。

最有效的学习方式是通过构建事物并将理论付诸实践。在数学中，这就是唯一的学习方式。这意味着你需要仔细阅读文本。不要因为某些东西被写下来就认为它是理所当然的。逐句思考每个句子。把每个论点和计算拆解开来。在阅读证明之前，尽量自己证明定理。

记住这一点，让我们开始吧！系好安全带，路途漫长且充满了曲折与转折。

## 使用的约定

本书中使用了若干文本约定。CodeInText 表示文本中的代码词汇、数据库表名、文件夹名、文件名、文件扩展名、路径名或 URL。例如：“切片通过指定第一个和最后一个元素以及一个可选的步长来实现，语法为 object[first:last:step]。”

代码块设置如下：

```py
from sklearn.datasets import load_iris 
data = load_iris() 
X, y = data["/span>data, data["/span>target 
X[:10]
```

任何命令行输入或输出如下所示：

```py
(3.5, -2.71, 'a string')
```

斜体表示新概念或强调。例如，菜单或对话框中的词汇在文本中通常以这种方式出现。例如：“这是我们第一个非可微函数的例子。”

## 本书内容

第一章，向量和向量空间涵盖了向量是什么以及如何使用它们。我们将从具体的例子出发，通过精确的数学定义到实现，理解向量空间和用于高效表示向量的 NumPy 数组。除了基础知识，我们还将学习

第二章，向量空间的几何结构通过研究范数、距离、内积、角度和正交性等概念向前推进，为向量空间的代数定义增添了急需的几何结构。这些不仅是可视化的工具，它们在机器学习中也扮演着至关重要的角色。我们还将遇到我们的第一个算法——Gram-Schmidt 正交化方法，它可以将任何一组向量转换为正交标准基。

在第三章，实践中的线性代数中，我们再次使用 NumPy，实施迄今为止所学的一切。在这里，我们学习如何在实践中使用高性能的 NumPy 数组：操作、广播、函数，最终实现从头开始的 Gram-Schmidt 算法。这也是我们第一次接触到矩阵，线性代数中的工作马。

第四章《线性变换》讲述了矩阵的真正性质；即在向量空间之间的结构保持变换。通过这种方式，原本看似深奥的事物——比如矩阵乘法的定义——突然变得有意义。我们再次从代数结构跃升到几何结构，让我们能够将矩阵视为改变其底层空间的变换。我们还将研究矩阵的一个最重要的描述量：行列式，描述了底层线性变换如何影响空间的体积。

第五章《矩阵与方程》展示了矩阵作为线性方程组的第三种（也是我们最终的）面貌。在这一章中，我们首先学习了如何使用高斯消元法手动求解线性方程组，然后通过我们新学的线性代数知识，对其进行超级强化，得到了强大的 LU 分解。在 LU 分解的帮助下，我们迎难而上，成功实现了大约 70000 倍的行列式计算加速。

第六章介绍了矩阵的两个最重要的描述量：特征值和特征向量。我们为什么需要它们？

因为在第七章《矩阵分解》中，我们能够借助它们达到线性代数的巅峰。首先，我们展示了通过构造基于特征向量的基，实对称矩阵可以被写成对角形式，这就是著名的谱分解定理。反过来，谱分解的巧妙应用导出了奇异值分解，这是线性代数中最重要的结果。

第八章《矩阵与图论》通过研究线性代数与图论之间的丰硕联系，结束了本书的线性代数部分。通过将矩阵表示为图，我们能够展示深刻的结果，比如 Frobenius 标准形，甚至可以讨论图的特征值和特征向量。

在第九章《函数》中，我们将详细研究函数，这是我们迄今为止直觉上使用的一个概念。这一次，我们将直觉用数学的方式精确定义，学习到函数本质上是点与点之间的箭头。

第十章《数字、序列与级数》继续深入探讨数字的概念。从自然数到实数的每一步都代表着一个概念上的飞跃，最终在序列与级数的研究中达到顶峰。

在第十一章《拓扑学、极限与连续性》中，我们几乎达到了最有趣的部分。然而，在微积分中，许多对象、概念和工具通常是通过极限和连续函数来描述的。因此，我们将详细研究这些概念。

第十二章讲述了微积分中最重要的概念：微分。在这一章中，我们学习了函数的导数描述了 1）切线的斜率，2）函数的最佳局部线性逼近。从实际角度来看，我们还研究了导数在各种运算中的行为，最重要的是函数组合，从而得出了反向传播的基础链式法则。

在所有准备工作完成之后，第十三章《优化》介绍了用于训练几乎所有神经网络的算法：梯度下降。为此，我们学习了导数如何描述函数的单调性，以及如何通过一阶和二阶导数来表征局部极值。

第十四章《积分》总结了我们对单变量函数的学习。从直观上讲，积分描述的是函数图形下的（有符号）面积，但经过仔细观察，我们还会发现它实际上是微分的逆运算。在机器学习中（实际上在整个数学领域中），积分描述了各种概率、期望值以及其他基本量。

现在我们已经理解了如何在单变量中进行微积分，第十五章引领我们进入多变量函数的世界，这也是机器学习的领域。在这里，我们有各种各样的函数：标量-向量、向量-标量和向量-向量函数。

在第十六章《导数和梯度》中，我们继续我们的旅程，克服了将微分推广到多变量函数的困难。在这里，我们有三种类型的导数：偏导数、全导数和方向导数；从而得到梯度向量和雅可比矩阵与海森矩阵。

如预期的那样，在多变量中，优化也稍显复杂。这个问题在第十七章《多变量优化》中得到了澄清，在这里我们学习了单变量二阶导数检验的类似方法，并实现了万能的梯度下降的最终形式，完成了我们的微积分学习。

现在我们已经对机器学习有了机械性的理解，第十八章《什么是概率？》向我们展示了如何在不确定性下进行推理和建模。在数学术语中，概率空间是由科尔莫哥洛夫公理定义的，我们还将学习那些让我们能够与概率模型打交道的工具。

第十九章介绍了随机变量和分布，不仅让我们能够将微积分工具引入概率论，还将概率模型压缩为序列或函数。

最后，在第二十章中，我们学习了期望值的概念，通过平均值、方差、协方差和熵来量化概率模型和分布。

## 为了充分利用本书

本书的代码以 Jupyter notebooks 形式提供，托管在 GitHub 上，链接为 [`github.com/cosmic-cortex/mathematics-of-machine-learning-book`](https://github.com/cosmic-cortex/mathematics-of-machine-learning-book)。要运行这些 notebooks，您需要安装所需的包。

安装这些包的最简单方法是使用 Conda。Conda 是一个很棒的 Python 包管理工具。如果您的系统上尚未安装 Conda，可以在此找到安装说明：[`bit.ly/InstallConda`](https://bit.ly/InstallConda)。

请注意，Conda 的许可证可能对商业使用有一些限制。安装 Conda 后，请按照本书仓库 README.md 中的环境安装说明进行操作。

## 下载示例代码文件

本书的代码包托管在 GitHub 上，链接为 [`github.com/cosmic-cortex/mathematics-of-machine-learning-book`](https://github.com/cosmic-cortex/mathematics-of-machine-learning-book)。我们还提供了来自我们丰富书籍和视频目录的其他代码包，链接为 [`github.com/PacktPublishing/`](https://github.com/PacktPublishing/)。快去看看吧！

## 下载彩色图片

我们还提供了一个 PDF 文件，包含本书中使用的带有彩色图片的截图/图表。您可以在此下载：[`packt.link/gbp/9781837027873`](https://packt.link/gbp/9781837027873)。

## 联系我们

我们始终欢迎读者的反馈。一般反馈请发送至 feedback@packtpub.com，并在邮件主题中注明书名。如果您有任何关于本书的疑问，请通过 questions@packtpub.com 与我们联系。勘误：尽管我们已尽力确保内容的准确性，但错误总会发生。如果您发现书中的错误，我们将非常感激您向我们报告。请访问 [`www.packtpub.com/submit-errata`](http://www.packtpub.com/submit-errata)，点击提交勘误并填写表单。盗版：如果您在互联网上发现我们作品的任何非法复制品，我们将非常感激您提供该位置地址或网站名称。请通过 copyright@packtpub.com 联系我们，并附上该资料的链接。如果您有兴趣成为作者：如果您在某个领域有专长，并且对写书或参与书籍贡献感兴趣，请访问 [`authors.packtpub.com`](http://authors.packtpub.com)。

## 分享您的想法

阅读完《机器学习的数学》后，我们很希望听到您的想法！[点击这里直接访问亚马逊评论页面](https://packt.link/r/1837027870)，并分享您的反馈。

您的评价对我们和技术社区都非常重要，它将帮助我们确保提供优质的内容。

## 下载本书的免费 PDF 副本

感谢购买本书！

喜欢随时随地阅读，却无法携带纸质书籍？或者您的电子书购买与您选择的设备不兼容？

不用担心；现在购买每本 Packt 书籍，您都可以免费获得该书的无 DRM 版 PDF。

随时随地阅读，在任何设备上都能使用。直接将您喜爱的技术书籍中的代码复制、粘贴到您的应用程序中。

福利不仅仅如此！您还可以获得独家的折扣、新闻通讯和每天发送到您邮箱的精彩免费内容。

按照以下简单步骤享受这些福利：

1.  扫描二维码或访问以下链接：

    ![PIC](img/file2.png)

    [`packt.link/free-ebook/9781837027873`](https://packt.link/free-ebook/9781837027873)

1.  提交您的购买凭证。

1.  就这么简单！我们会直接将您的免费 PDF 和其他福利发送到您的电子邮件地址。
