json序列化时，默认会遇到中文会转换成unicode，如果想保留中文怎么办
```python
import json
a = json.dumps({"ddf":"您好"}, ensure_ascii=False)
```

Numpy, Pandas
与嵌套Python列表相比，Numpy数组的优势
```
1. Python的列表是有效的通用容器。它们支持（相当）高效的插入，删除，附加和连接，并且Python的列表理解使它们易于构造和操作。
2. 它们有一定的局限性：它们不支持“向量化”操作，例如逐元素加法和乘法，并且它们可以包含不同类型的对象这一事实意味着Python必须存储每个元素的类型信息，并且在操作时必须执行类型调度代码在每个元素上。
3. NumPy不仅效率更高；这也更加方便。您可以免费获得许多矢量和矩阵运算，有时这可以避免不必要的工作。而且它们也得到有效实施。
4. NumPy数组更快，您可以使用NumPy，FFT，卷积，快速搜索，基本统计信息，线性代数，直方图等内置大量内容。
```
Pandas和Numpy的区别
```
NumPy 适合进行数值计算和数组操作，而 Pandas 则适合进行数据处理和分析。
1. 数据结构：
    - NumPy 主要提供了多维数组对象（`ndarray`），可以进行快速的向量化操作。
    - Pandas 提供了两种主要的数据结构：`Series`（一维标记数组）和`DataFrame`（二维表格型数据结构），可以更方便地处理和分析数据。
2. 功能：
    - NumPy 提供了大量的数学函数和操作，可以进行数组运算、线性代数、随机数生成等操作。
    - Pandas 主要用于数据处理和分析，提供了丰富的数据操作和处理功能，包括数据清洗、重塑、合并、分组、聚合等。
3. 索引：
    - NumPy 的数组是基于整数索引的，类似于 Python 的列表。
    - Pandas 的 `Series` 和 `DataFrame` 可以使用标签索引，可以通过标签访问和操作数据，也可以通过整数位置访问数据。
4. 适用场景：
    - NumPy 适合用于数值计算和科学计算，特别是对于大规模数据的数组操作。
    - Pandas 适合用于数据处理和数据分析，特别是对于结构化数据的处理和分析。
5. 性能：
    - NumPy 的数组操作是基于 C 语言实现的，因此通常比 Python 的原生列表操作要快。
    - Pandas 是在 NumPy 的基础上进行的扩展，性能上大致相当，但对于某些操作，Pandas 可能会更慢一些，因为它提供了更多的功能和灵活性。
```

Numpy和SciPy
```
1. 在理想情况下，NumPy除了数组数据类型和最基本的操作外，将不包含任何内容：索引，排序，重塑，基本的元素函数等。
2. 所有数字代码都将驻留在SciPy中。但是，NumPy的重要目标之一是兼容性，因此NumPy尝试保留其前任任一个所支持的所有功能。
3. 因此，NumPy包含一些线性代数函数，即使这些函数更恰当地属于SciPy。无论如何，SciPy都包含线性代数模块的更多全功能版本，以及许多其他数值算法。
4. 如果您正在使用python进行科学计算，则可能应该同时安装NumPy和SciPy。大多数新功能属于SciPy，而不是NumPy。
```


## Numpy
数组（Arrays）：NumPy的核心是多维数组对象（numpy.ndarray）。数组是相同数据类型元素的集合，可以是一维、二维或多维。
```python
''' 创建数组'''
numpy.array()
# 根据 start 与 stop 指定的范围以及 step 设定的步长，生成一个 ndarray
numpy.arange(start, stop, step, dtype)
# 创建一个一维数组，数组是一个等差数列构成的；
numpy.linspace 
# 创建一个于等比数列
numpy.logspace 函数用于

''' 获取数组中的元素 : 数组索引和切片'''
# 使用整数索引选择单个元素
arr_1d = np.array([1, 2, 3, 4, 5])
print(arr_1d[2])  # 输出: 3

# 对于二维数组，使用逗号分隔的索引元组
arr_2d = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
print(arr_2d[1, 2])  # 输出: 6

arr_1d = np.array([1, 2, 3, 4, 5])
arr_1d[1:4]  # 输出: [2, 3, 4]
arr_2d = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
print(arr_2d[:2, 1:])  # 输出: [[2, 3], [5, 6]]

# 布尔索引
arr = np.array([1, 2, 3, 4, 5])
mask = arr > 2
print(arr[mask])  # 输出: [3, 4, 5]

# 通过numpy库定义arr数组，arr = np.arange(9).reshape(3,3)，那么下列选项中哪个可以交换第一列和第二列位置
arr = np.arange(9).reshape(3,3)
arr[[1,0,2], :] # 交换前两行的位置
arr[:, [1,0,2]] # 交换前两列的位置
arr[::-1] # 反转二维数组的行
arr[:, ::-1] # 反转二维数组的列

'''形状操作：shape, reshape '''

'''数组合并和分割'''
# 拼接函数
np.concatenate(), np.append(), np.stack(), np.vstack()..
# 分割函数
np.split() # 实现水平切割或者垂直切割，split必须要均等分
np.array_split() # 实现水平切割或者垂直切割（强制切割），指定切割后的数目实现近似均匀切割
np.hsplit() # 可以沿横轴(纵向)拆分原array，可以实现均匀切割或者指定位置切割


'''
数组运算
逐元素运算：支持对整个数组进行逐元素运算。对数组的逐元素运算，提高运算效率。广播（Broadcasting）：NumPy中的广播机制允许不同形状的数组在一起进行运算。
'''
numpy.dot() # 进行矩阵乘法。
numpy.sin() 
numpy.exp()

'''聚合'''
numpy.min()、numpy.max()
np.mean()
nanmean() # 计算忽略NaN值的数组平均值。如果数组具有NaN值，我们可以找出不受NaN值影响的均值；
np.average(a,weights=w) # 还可以计算加权平均值；
std() # 计算矩阵或者数组的标准差；
numpy.reciprocal()# 对数组中的每个元素取倒数，并以数组的形式将它们返回；
numpy.power(a,b)# 将 a 数组中的元素作为底数，把 b 数组中与 a 相对应的元素作幂 ，最后以数组形式返回两者的计算结果；
numpy.mod() # 返回两个数组相对应位置上元素相除后的余数；
numpy.multiply()# 数组乘法运算，返回两个数组相乘的结果

'''排序'''
numpy.sort() # 对输入数组执行排序，并返回一个排序好的数组副本
numpy.argsort() # 返回排序后的元素索引数组

'''随机数np.random'''
numpy.random.randn # 返回一个指定形状的数组，数组中的值服从标准正态分布（均值为0，标准差为1）
np.random.uniform # 从一个均匀分布的区域中随机采样
np.random.rand # 返回一个或一组服从“0~1”均匀分布的随机样本值，随机样本取值范围是\[0,1)
np.random.randint # 函数从给定的区域中随机选取设定数量的整数

'''线性代数: numpy.linalg模块提供线性代数操作，如矩阵求逆、行列式等。'''

'''文件操作'''
numpy.loadtxt() numpy.savetxt() # 读写文本文件
numpy.load() numpy.save() # 读写二进制文件

'''数组操作'''
np.intersect1d(a,b) # 用来获取数组a和数组b之间的公共项
np.setdiff1d(a,b) # 从a数组中删除存在于b数组中的项
np.where(a == b) # a和b元素相匹配的位置
np.lexsort((a,b)) # 是一种排序算法，按键序列对数组进行排序，它返回一个已排序的索引数组

numpy.where() # 返回值是满足了给定条件的元素索引值；
numpy.nonzero() #该函数从数组中查找非零元素的索引位置；
numpy.zeros(shape, dtype=float, order='C')
np.zeros((4,2))# 里面是4*2的矩阵结构 np.ones((2,2))
```

## Pandas
DataFrame类型数据
```python 
'''数据读取和写入'''
import pandas as pd
df = pd.read_csv('data.csv')
df.to_excel('data.xlsx', index=False)

df.iloc(1,1) # 按照索引值来定位数据元素
df,loc(1,'B') # 按照标签值来定位元素
df.columns()# 提取DataFrame类型数据中的列索引
df.index()# 提取DataFrame类型数据中的行索引
df.head()# 显示data前n行数据
df.info()# 给出样本数据的相关信息概览 ，包括行数，列数，列索引，列非空值个数，列类型，内存占用
df.unique()# 返回NumPy数组ndarray中唯一元素值的列表；
df.value_counts() # 返回唯一元素的值及其在出现的次数；
df.nunique() # 以整数int形式返回唯一元素的数量；
df.count() # 统计非空字符串数量
df.describe() # 方法粗略的观察一些统计量

'''数据修改和删除'''
# 修改列名‘Goodlabel'换为'label'
df.columns=['Name','label'] # 暴力手段修改
df=pd.read_excel("C:/.../工作簿1.xlsx",columns=['Name','label']) # 在读取时更改列名
df.rename(columns={'Goodlabel':’label’},inplace=True)
# 删除
drop()# 删除dataFrame中的行或者列
dropna()# 删除数据中存在空缺值的行或者列。
df.drop(df.index[[2,4]]) or df.drop(['b',’d’])
df.fillna(value) # 用value值来填充 df.fillna(method='ffill') method='ffill'表示向前填充NaN, method='bfill'表示向后填充。
isnull() # 返回表明哪些值是缺失值的布尔值
notnull()# 返回表明哪些值不是缺失值的布尔值
# 采用的用sklearn包中的inpute的SimpleInputer()函数来对缺失值进行填充
df.drop_duplicates(subset=['A','B','C'],keep= ,inplace= )


'''数据选择和过滤'''
df['Name']
df[df['Age']>30]
# 提取出Name列中含有姓名为"Liming"的行( )
df[df['name'].str.startswith('Li')] # 可以获取以特定字符串开头的pandas.Series
df[df['Name'].str.contains('Lim*')] # 用了通配符 先获得一个bool类型的值

'''数据排序'''
df.sort_values(by='Age', ascending=False)
sort_index() # 按照标签排序
sort_values() # 按照值排序
# 两个函数都有一个控制排序顺序的参数ascending，ascending=False时由大到小排序，ascending=True时由小到大排序

'''数据聚合'''
df.groupby('Category').mean()
# groupby是对DataFrame数据进行数据的分组以及分组后地组内运算
df.mean()

'''可视化'''
df.plot(x='Date', y='Value', kind='line')

'''时间序列'''
df['Date'] = pd.to_datetime(df['Date'])
df.resample('M').mean()

'''拼接函数'''
pd.join()
pd.concat()
pd.merge()

"""
去重函数drop_duplicates
subset：表示要进行去重的列名
keep：有三个可选参数，分别是 first、last、False，默认为 first，表示只保留第一次出现的重复项，删除其余重复项，last 表示只保留最后一次出现的重复项，False 则表示删除所有重复项；
inplace：布尔值参数，默认为 False 表示删除重复项后返回一个副本，若为 Ture 则表示直接在原数据上删除重复项；
"""
df.drop_duplicates(subset=['A','B','C'],keep= ,inplace= )

# 重置索引函数为reset_index()去重后行标签不变，如需改变可使用
df.reset_index()重置索引

"""
errors存在三个参数{'ignore'，'raise'，'coerce'}，默认情况下为'raise'。
ignore:只对数字字符串进行转换，其他类型一律不转换
raise: 将非数字字符串转换为数字，数据中如果存在非数字字符串则会返回出错误，时间类型转换为int
coerce: 将数字字符串和bool类型转换为数字,其他均转换为NaN。
downcast =’signed’所有值都将转换为整型。
"""
pd.to_numberic(arg,errors,downcast)
```

**pandas中创建DataFrame**
```
1. 用列表来创建
data = [[一行一列，一行二列], [二行一列，二行二列]]
df = pd.DataFrame(data, columns=[‘列名',’列名'])   # 没有的话默认从0开始定义列名df
2. 用dict创建
data = [{key1:value1},{...}]
df = pd.DataFrame(data) df
3. 用ndarrays创建
data = {'coloum1':[values1], 'coloum2':[values2]}
```

### plt
```python
boxplot()用于绘制箱线图； plot()绘制折线图；  bar()绘制柱状图；  pie()绘制饼状图；绘制直方图的函数是ax.hist()
可视化扩展库matplotlib中的pyplot模块中的legend()函数设置图像标题时
loc参数设置图列位置
prop设置文本字体参数；
fontsize参数设置图例字体大小
frameon控制是否应在图例周围绘制框架；
matplotlib数据库绘制图像的时候，其中plt.savefig('test', dpi=600)中的dpi参数代表每英寸点数不等于像素
plt.subplots(m,n)函数的返回值是一个元组，包括一个图形对象(fig)和所有的axes对象,其中axes 对象数量为m*n，
plt.subplots(m,n)画多个子图时，ax=ax.flatten()操作将ax由n*m的Axes组展平成1*nm的Axes组
subplots() 函数和 subplot() 函数使用方法类似。其不同之处在于，subplots() 既创建了一个包含子图区域的画布，又创建了一个 figure 图形对象，而 subplot()只是创建一个包含子图区域的画布。
ax.set_title()是给ax子图设置标题，当子图存在多个的时候，可以通过ax设置不同的标题；
```