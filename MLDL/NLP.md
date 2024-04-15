# 一、英文NLP处理
## 1. 简单分词、词性还原、停用词过滤


## 2. 特征提取
### TF-IDF
TF-IDF、信息增益、卡方检验、互信息、N-Gram等
## 3. 文本标签向量化
## 4. 选择合适的算法模型进行训练


NLTK(Natural Language Toolkit)是一个Python模块，提供了多种语料库（Corpora）和词典（Lexicon）资源，比如WordNet等，以及一系列基本的自然语言处理工具集，包括：分句，标记解析（Tokenization），词干提取（Stemming），词性标注（POS Tagging）和句法分析（Syntactic Parsing）等，是对英文文本数据进行处理的常用工具。

```python
import nltk
from nltk.tokenize import sent_tokenize
from nltk.tokenize import word_tokenize
from nltk.corpus import brown
 
brown.categories()
s = '近日，中国短道速滑队队员@武大靖,在直播中歪嘴喝水的画面走红,此后他本人还亲自教学。于是，短道速滑国家队的成员们相继挑战,还出了一人炫三瓶的升级版。网友：终于找到进短道速滑队的方法！'
s1 = 'Along with the development of society , more and more problems are brought to our attention , one of the most serious problems is involution and lying flat . Involution means that when social resources cannot meet the needs of everyone, people compete to obtain more resources. An important feature of involution is internal competition , Internal competition is becoming increasing prevalent at an amazing rate. '
englishTokens = word_tokenize(s1)
chineseTokens = word_tokenize(s)
# 分句和分词
print("英文分句", sent_tokenize(s1))
print("英文分词", englishTokens)
print("中文分句", sent_tokenize(s))
print("中文分词", chineseTokens)
 
# 词性标注
# 分词之后才可以进行词性标注
englishTags = nltk.pos_tag(englishTokens)
chineseTags = nltk.pos_tag(chineseTokens)
print("英文词性标注", englishTags)
print("中文词性标注", chineseTags)
 
# 情感分析
#compound表示复杂程度,neu表示中性,neg表示负面情绪,pos表示正面情绪
from nltk.sentiment.vader import SentimentIntensityAnalyzer
s2 = ['This is a good book', 'This is a bad book']
s3 = ['这是一本好书', '这是一本糟糕的书']
# 创建分类器
sid = SentimentIntensityAnalyzer()
#英文情感分析
for sentence in s2:
    print(sentence)
    print("情感得分", sid.polarity_scores(sentence))
#中文情感分析
for sentence in s3:
    print(sentence)
    print("情感得分", sid.polarity_scores(sentence))
```

