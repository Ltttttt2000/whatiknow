https://zhuanlan.zhihu.com/p/432736582


输入以0开头的文本型数字时需要前面加入’
  

SUMIF：对选中范围内符合指定条件的值求和。=SUMIF(range, criteria, sum_range)
Range：条件区域，用于条件判断的区域 criteria：求和条件，由数字、逻辑表达式等组成的判定条件 sum_range：实际求和区域

SUMIFS(sum_range, criteria_range1, criteria1, criteria_range2, criteria2, ...)
AVERAGE(B10:B13)
AVERAGEIF(range(条件判断的区域）, criteria, [average_range])
=averageifs(average_range,criteria_range1,criteria1,criteria_range2,criteria2,...)
MAX（）。MIN（）

ROW函数是用来确定光标的当前行位置或者指定单元格行位置的函数 =ROW(C14) 求C4单元格所在的行的位置。
COLUMN函数是用来确定光标的当前列位置或者指定单元格列位置的函数。
RANDBETWEEN（bottom,top）随机生成[bottom,top]之间的一个随机整数
RAND() 生成[0,1)的随机实数
COUNT函数给定数据集合或者单元格区域中数据的个数进行计数，COUNT函数只能对数字数据进行统计，对于空单元格、逻辑值或者文本数据将不统计。= COUNT（range）
COUNTA函数是计算区域内非空单元格的个数。
COUNTBLANK函数是计算区域内空白单元格的个数。

逻辑函数

IF函数是条件判断函数：如果指定条件的计算结果为 TRUE，IF函数将返回某个值；如果该条件的计算结果为 FALSE，则返回另一个值。IF(test,value_if_true,value_if_false)
=IFS(条件1,值1,条件2,值2……条件N,值N)。
AND函数是指所有参数的逻辑值为真时，返回TRUE；只要有一个参数的逻辑值为假，即返回 FALSE。
OR函数是指任何一个参数逻辑值为 TRUE，即返回 TRUE；所有参数的逻辑值为 FALSE，才返回 FALSE

=YEAR(O26) 从日期中提取年； MONTH；DAY；
=DATE(year, month, day) 输入制定参数生成日期
DATEDIF函数是计算两日期之差，返回两个日期之间的年\月\日间隔数。DATEDIF(start_date,end_date,unit)
Unit 为所需信息的返回类型：Y" 时间段中的整年数；"M" 时间段中的整月数；"D" 时间段中的天数；"MD" 起始日期与结束日期的同月间隔天数；"YD" 起始日期与结束日期的同年间隔天数；"YM" 起始日期与结束日期的同年间隔月数。
WEEKDAY函数是返回某日期的星期数。
WEEKDAY(serial_number日期，return_type) 返回类型一般选2

文本连接符&，把几个内容连接起来，可以是数字、单元格引用、字符等
LEN函数是计算字符串长度的函数。
LEFT函数用于从一个文本字符串的第一个字符开始返回指定个数的字符。=left(text,[num_chars])
RIGHT函数和LEFT函数用法一样，指的是从右边第一个字符开始提取字符
MID函数是从一个字符串中截取出指定数量的字符MID(_text_,_start_num_,_num_chars_)
CONCAT函数用于连接两个或多个内容，比文本连接符号&更高效
  
**SUMIF(C2:C7,{"<10","<6"})*{1,-1} 表示 [6-10)** 
查找函数
**VLOOKUP**
VLOOKUP函数是一个运用非常广的纵向查找函数。
VLOOKUP(要查找的值,要查找的区域,返回数据在查找区域的第几列数,[精确匹配/近似匹配])

反向查找
MATCH函数返回指定数值在指定数组区域中的位置。
MATCH(查找的值, 查找的区域, [1:小于 0:精确匹配 -1:大于])

数据处理
1. 筛选功能ctrl+shif+l
2. 删除重复值选中区域——点击数据——删除重复值——以当前区域