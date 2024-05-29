# 实习
爬虫软件原理
用户代理User-Agent
IP->多IP，多服务器
验证码->图像识别技术
Cookie才能访问/登陆->定时采集
JS加密网页内容->调用代码
链接随机化->用脚本形成分页地址

后裔采集器：流程图模式（图形化编程）&智能模式（自动分析网页结构）

匹配规则：xpath, css, regex
定时抓取
IP池：爬虫软件维护IP池，用不同IP发送请求，降低IP封锁概率
打码功能：内置验证码识别器，实现机器打码或手动打码

代理、反爬虫机制
手段：限制访问频率、验证码等
反爬机制
1. 基于身份识别：通过headers字段User-Agent，通过请求参数
2. 基于爬虫行为：请求频率/总请求数量，爬取行为
3. 基于数据加密：对响应中含有的数据进行特殊化处理

xpath和css选择器
xpath：用路径来定位
节点选择：div@
谓词：精准定位//div[@class='example']
轴：允许在文档中沿着结点和属性移动ancestor:div

css选择器：
基本：标签div，类example， ID，#uniqueID,通配符*
组合选择器div.example
属性input[type='text']
伪类：选择元素特殊状态，伪元素：选择特殊部分

```
xpath: 可沿节点轴移动，更灵活的节点选择，支持正则更强大的匹配能力，可选父子兄等更多精细的节点。
css：更简洁直观，适合基本的选择操作，浏览器原声支持性能更好，某些情况下速度更快
```

HTML解析(Hypertext Markup Language)
使用库：BeautifulSoup,lxml解析提取信息parser
将HTML文档解析成文档对象模型DOM(document object model)树形活其他数据结构
标签和属性，元素选择器
HTML解析流程：分词Tokenization->构建节点树Tree construction->样式计算Style Calculation.->异常处理，编码处理，事件处理，性能优化
网络请求库request
正则表达式：提取特定模式信息

# 数据的财富建模
目的：通过分析用户对三种商品的评星及文本评价，给商品做出功能性的改进提出建议，以及预测未来时间的销售战略。
任务：
1. 数据预处理
2. 模型
	1. 分析影响每个商品的因素
	2. 分析用户的评价是否受到其他用户评价的影响
	3. 分析评星是否与某些关键词相关
3. 时间模型？

数据：商品信息、start rating (1-5)、多少用户认为该评价有用、评价内容是否粘贴了商品信息、是否购买了、评价title、评价内容、评价时间‘

start rating 分为三档1-2、3、4-5
将评价的内容打分处理NLTK工具做情感分析SentimentIntensityAnalyzer in NLTK
[-1, 1]
[negative, positive]
每句话获得[{‘neg’:0.0,‘neu’:0.737,‘pos’:0.263,‘compound’:0.3612}]
负面分数、中性分数、正面分数、总分数
we set “like” as a positive emotion and “hate” as a negative emotion

修正评分和情感分析分数（使他们范围相对应）


算TF- IDF+余弦相似度算文本相似度看是否受到其他影响
向量夹角，接近0度代表越相似


LDA模型+时间
pyLDA 


主题模型分析LDA
核心思想：每篇文档都可以看作是多个主题的混合，而每个主题则由一组词构成。
从大量文档中自动提取主题信息。
主题模型是一种无监督学习方法，它能够自动从大量文档中提取主题信息。
主题模型的重要性在于： 
- 主题模型能够帮助我们理解文档集中的主题结构，有助于文档分类、聚类和信息检索。 
- 主题模型能够将高维的文本数据降维到低维的主题空间，便于后续的分析和处理。
分类：
- LSA潜在语义分析Latent Semantic Analysis：基于线性代数，通过奇异值分解SVD降维
- PLSA概率潜在语义分析Probabilistic LSA：基于概率图模型，将文档表示为主题的混合分布，主题表示为词的概率分布。
- LDA潜在狄利克雷分配：生成式概率模型，在PLSA的基础上引入狄利克雷先验分布。
### LDA
LDA（潜在狄利克雷分配）是一种生成式概率模型，它在PLSA的基础上引入狄利克雷先验分布。LDA假设每篇文档由多个主题组成，每个主题由多个词组成。

LDA的计算步骤如下：
1. 初始化文档-主题分布矩阵和主题-词分布矩阵。 
2. 使用吉布斯抽样（Gibbs Sampling）或变分推断（Variational Inference）迭代更新文档-主题分布矩阵和主题-词分布矩阵，直到收敛。
```python
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.decomposition import TruncatedSVD, LatentDirichletAllocation

# 示例文本数据
documents = [
    '我喜欢编程，编程是一门有趣的技术',
    '我喜欢旅游，旅游可以放松心情',
    '编程和旅游都是我的爱好'
]

# 构建词频矩阵
vectorizer = CountVectorizer()
X = vectorizer.fit_transform(documents)

# LSA算法
lsa = TruncatedSVD(n_components=2)
lsa_result = lsa.fit_transform(X)
print('LSA结果：\n', lsa_result)

# LDA算法
lda = LatentDirichletAllocation(n_components=2, random_state=0)
lda_result = lda.fit_transform(X)
print('LDA结果：\n', lda_result)
```

LDA 采用词袋模型。所谓词袋模型，是将一篇文档，我们仅考虑一个词汇是否出现，而不考虑其出现的顺序。在词袋模型中，“我喜欢你”和“你喜欢我”是等价的。
与词袋模型相反的一个模型是n-gram，n-gram考虑了词汇出现的先后顺序。


主题数量人为指定，
得到每个主题下词语的分布概率+文档对应的主题概率
