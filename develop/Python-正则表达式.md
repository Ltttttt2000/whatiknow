
```
import re
s1 = "Python's features"
s2 = re.match(r'(.)on(.?).', s1, re.M|re.I)
print(s2.group(1))
```

**正则表达式**
```
贪婪匹配：趋向于最大长度匹配
非贪婪：能匹配到就好
```
## 正则
**match 和search**
```
match()函数只检测 RE 是不是在 string 的开始位置匹配
search()会扫描整个 string 查找匹配
也就是说 match()只有在 0 位置匹配成功的话才有返回，如果不是开始位置匹配成功的话，match()就返回 none。
```
用 Python 匹配 HTML g tag 的时候，<._> 和 <._?> 有什么区别？
```
<.*>是贪婪匹配，会从第一个“<”开始匹配，直到最后一个“>”中间所有的字符都会匹配到，中间可能会包含“<>”。

<.*?>是非贪婪匹配，从第一个“<”开始往后，遇到第一个“>”结束匹配，这中间的字符串都会匹配到，但是  不会有“<>”。
```
正则
```
正则匹配不是以4和7结尾的手机号
re.match("1\d{9}[0-3,5-6,8-9],s)
search和match
search匹配到第一个匹配到的数据 re.search().group()
re.findall(r'\d',s) 满足正则都匹配
re.match(s, ss).group()
pattern = re.compile(r'[\u4e00-\u9fa5]+') 匹配中文
result = pattern.findall(title)
url
res.findall(r"https://.*?\.jpg",s)[0]
res = re.search(r"https://.*?\.jpg",s)
res.group()
```

```python
# lc 130 图论
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        m, n = len(board), len(board[0])
        def dfs(i, j):
            if i<0 or j<0 or i>=m or j>=n or board[i][j] != 'O':
                return
            board[i][j] = '-'
            dfs(i-1, j)
            dfs(i, j-1)
            dfs(i+1, j)
            dfs(i, j+1)    # 这里不需要标记访问过的，因为访问过的早已经变成了'-'
        # 遍历边界上的'O'
        for i in range(m):
            if board[i][0] == 'O':
                dfs(i, 0)
            if board[i][n-1] == 'O':
                dfs(i, n-1)
        for j in range(n):
            if board[0][j] == 'O':
                dfs(0, j)
            if board[m-1][j] == 'O':
                dfs(m-1, j)
        
        # 边界和边界相连的'O'已经全部换成了'-'
        for i in range(m):
            for j in range(n):
                if board[i][j] == 'O':
                    board[i][j] = 'X'
                if board[i][j] == '-':
                    board[i][j] = 'O'

```

search match findall
```python
match
search
findall
s = 'dfasf123fd123'

re.match('123',s) # None 从头开始匹配
ret = re.search('123',s) # 任意位置进行匹配 查找到第一个 返回正则表达式对象 span(4,7)
ret.group()
re.findall('123',s) # 所有的都被找到，生成的是一个列表
```