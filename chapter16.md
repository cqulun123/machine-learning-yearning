## Chapter 16、Cleaning up mislabeled dev and test set examples

**清理贴错标签的开发和测试集样本**

在错误分析期间，你可能会注意到开发集中的一些样本被错误标记（mislabeled）。当我说“mislabeled”时，我的意思是即使在算法遇到它之前，图片已经被打标人员贴上了错误的标签。即，样本 (x,y) 中的类别标签y的值不正确。例如，也许一些不是猫的图片被错贴标签为包含猫，反之亦然。如果你推测一小部分错误标注的图片很重要，那么添加一个类别以跟踪这一小部分错误标注的样本：

![16](http://oow6unnib.bkt.clouddn.com/myl-c16-0.jpg)

你应该纠正开发集中的标签吗？记住，开发集的目的是为了帮你快速评估算法，以便你可以判断算法A或B谁更好。如果被错误标注的开发集的一小部分妨碍你做出这些判断的能力，那么花时间去修正错误标注的开发集标签是值得的。

例如，假设你的分类器表现如下：

- 开发集的整体准确率……90%（10%整体错误率）
- 贴错标签样本导致的错误……0.6%（开发集错误的6%）
- 其他原因导致的错误……9.4%（开发集错误的94%）

这里，相对于你可能正在改进的9.4%的错误，由于错误标注导致的0.6%的不准确率可能没有那么重要。手动修正开发集中错误标注的图像并没有什么坏处，但这样做并不是关键：不知道系统是否有10%或9.4%的整体错误可能没什么问题。

假设你不断改进cat分类器并达到以下性能：

- 开发集整体准确率……98.0%（2.0%整体错误率）
- 贴错标签样本导致的错误……0.6%（开发集错误的30%）
- 其他原因导致的错误……1.4%（开发集错误的70%）

30%的错误是由于错误标注的开发集图像造成的，这将会为您的准确率估计增加显著的错误。现在去提高开发集中标签的质量是有价值的。处理错误标注的样本将帮助您算出分类器的错误是接近1.4%还是2%——这是一个相对显著的差异。

开始容忍一些错误标记的开发集样本并不罕见，随后随着系统的改进改变主意，以便于一小部分错误标记的样本相对于总的错误集增长。

最后一章解释了如何通过算法的提升来改进错误类别，例如Dog、Great Cat和Blurry。本章您将学习到，你也可以在错误标记的类别上工作——通过改善数据标签。

无论你采用什么方法来修正开发集标签，记得也将其用于测试集标签，以便开发集和测试集继续服从统一分布。将开发集和测试集固定在一起可以避免我们在第六章中讨论的问题（你的团队优化了开发集的性能，只是到后来才意识到他们在根据不同的测试集进行不同的标准判断）。

如果你决定提升标签的质量，那么轻考虑仔细检查系统错误分类的样本的标签，以及正确分类样本的标签。在一个样本中，原始标签和学习算法有可能都是错的。如果只是修正系统已经错误分类的样本的标签，可能会在评估中引入误差。如果你有1000个开发集样本，并且分类器准确率为98%，那么检查错误分类的20个样本比检查正确分类的所有980个样本要容易的多。因为在实践中只检查错误分类的样本比较容易，所以偏差会蔓延到一些开发集中。如果你只开发产品和应用感兴趣，那么这种偏差是可以接受的，但如果你计划在学术研究论文中使用该结果，或者需要一个完全无偏差地测量测试集的准确率，这将会是一个问题。