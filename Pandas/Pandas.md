# 一、读取数据

```python
import pandas as pd

fpath = "./datas/ml-latest-small/ratings.csv"

# 读取csv文件
ratings = pd.read_csv(fpath)
# 查看前几行
ratings.head()
# 查看数据的形态，返回的行数和列数
ratings.shape
# 查看列名列表
ratings.columns
# 查看索引列
ratings.index
# 查看每列的数据类型
ratings.dtypes

# 读取txt文件 并且指定分隔符列名
pvuv = pd.read_csv(
    fpath,
    sep="\t",
    header=None,
    names=['pdate', 'pv', 'uv']
)


# 读取excel文件
pvuv = pd.read_excel(fpath)

# 读取mysql数据库
import pymysql
conn = pymysql.connect(
        host='127.0.0.1',
        user='root',
        password='123456',
        database='test',
        charset='utf8'
    )

mysql_page = pd.read_sql("select * from crazyant_pvuv", con=conn)
```

# 二、Pandas数据结构

```python
# Series： 一维
s1 = pd.Series([1,'a',5.2,7])

# 创建一个具有标签索引的Series
s2 = pd.Series([1, 'a', 5.2, 7], index=['d','b','a','c'])

# 使用Python字典创建Series
sdata={'Ohio':35000,'Texas':72000,'Oregon':16000,'Utah':5000}
s3=pd.Series(sdata)

# DataFrame： 多维
data={
        'state':['Ohio','Ohio','Ohio','Nevada','Nevada'],
        'year':[2000,2001,2002,2001,2002],
        'pop':[1.5,1.7,3.6,2.4,2.9]
    }
df = pd.DataFrame(data)
```

# 三、查询数据

```python
df = pd.read_csv("./datas/beijing_tianqi/beijing_tianqi_2018.csv")

# 查询前几行
df.head()
'''
	ymd	bWendu	yWendu	tianqi	fengxiang	fengli	aqi	aqiInfo	aqiLevel
0	2018-01-01	3℃	-6℃	晴~多云	东北风	1-2级	59	良	2
1	2018-01-02	2℃	-5℃	阴~多云	东北风	1-2级	49	优	1
2	2018-01-03	2℃	-5℃	多云	北风	1-2级	28	优	1
3	2018-01-04	0℃	-8℃	阴	东北风	1-2级	28	优	1
4	2018-01-05	3℃	-6℃	多云~晴	西北风	1-2级	50	优	1
'''

# 查询第几行
df.loc[]
# 查询多行
df.loc[0：2]
# 查询多列
df[['year','pop']]
# 替换
df.loc[:,"yWendu"] =df["yWendu"].str.replace("℃","")


# 设定索引为日期，方便按日期筛选
df.set_index('ymd', inplace=True)

# 本次按字符串处理
df.index
'''
Index(['2018-01-01', '2018-01-02', '2018-01-03', '2018-01-04', '2018-01-05',
       '2018-01-06', '2018-01-07', '2018-01-08', '2018-01-09', '2018-01-10',
       ...
       '2018-12-22', '2018-12-23', '2018-12-24', '2018-12-25', '2018-12-26',
       '2018-12-27', '2018-12-28', '2018-12-29', '2018-12-30', '2018-12-31'],
      dtype='object', name='ymd', length=365)
'''

# 查看数据类型
df.dtypes
'''
bWendu        int32
yWendu        int32
tianqi       object
fengxiang    object
fengli       object
aqi           int64
aqiInfo      object
aqiLevel      int64
dtype: object
'''

# 得到单个值
df.loc['2018-01-03', 'bWendu']
'''
2
'''

# 得到一个Series
df.loc['2018-01-03', ['bWendu', 'yWendu']]
'''
bWendu     2
yWendu    -5
Name: 2018-01-03, dtype: object
'''

# 得到Series
df.loc[['2018-01-03','2018-01-04','2018-01-05'], 'bWendu']
'''
ymd
2018-01-03    2
2018-01-04    0
2018-01-05    3
Name: bWendu, dtype: int32
'''

# 得到DataFrame
df.loc[['2018-01-03','2018-01-04','2018-01-05'], ['bWendu', 'yWendu']]
'''
	bWendu	yWendu
ymd		
2018-01-03	2	-5
2018-01-04	0	-8
2018-01-05	3	-6
'''

# 使用数值区间进行范围查询
# 行index按区间
df.loc['2018-01-03':'2018-01-05', 'bWendu']
'''
ymd
2018-01-03    2
2018-01-04    0
2018-01-05    3
Name: bWendu, dtype: int32
'''

# 列index按区间
df.loc['2018-01-03', 'bWendu':'fengxiang']
'''
bWendu        2
yWendu       -5
tianqi       多云
fengxiang    北风
Name: 2018-01-03, dtype: object
'''

# 行和列都按区间查询
df.loc['2018-01-03':'2018-01-05', 'bWendu':'fengxiang']
'''

bWendu	yWendu	tianqi	fengxiang
ymd				
2018-01-03	2	-5	多云	北风
2018-01-04	0	-8	阴	东北风
2018-01-05	3	-6	多云~晴	西北风
'''

# 使用条件表达式查询
# 简单条件查询，最低温度低于-10度的列表
df.loc[df['yWendu']<-10, :]
'''
	ymd	bWendu	yWendu	tianqi	fengxiang	fengli	aqi	aqiInfo	aqiLevel
22	2018-01-23	-4	-12	晴	西北风	3-4级	31	优	1
23	2018-01-24	-4	-11	晴	西南风	1-2级	34	优	1
24	2018-01-25	-3	-11	多云	东北风	1-2级	27	优	1
359	2018-12-26	-2	-11	晴~多云	东北风	2级	26	优	1
360	2018-12-27	-5	-12	多云~晴	西北风	3级	48	优	1
361	2018-12-28	-3	-11	晴	西北风	3级	40	优	1
362	2018-12-29	-3	-12	晴	西北风	2级	29	优	1
363	2018-12-30	-2	-11	晴~多云	东北风	1级	31	优	1
'''

# 查询最高温度小于30度，并且最低温度大于15度，并且是晴天，并且天气为优的数据
df.loc[(df["bWendu"]<=30) & (df["yWendu"]>=15) & (df["tianqi"]=='晴') & (df["aqiLevel"]==1), :]
'''
	ymd	bWendu	yWendu	tianqi	fengxiang	fengli	aqi	aqiInfo	aqiLevel
235	2018-08-24	30	20	晴	北风	1-2级	40	优	1
249	2018-09-07	27	16	晴	西北风	3-4级	22	优	1
'''
```

# 四、新增数据列



```python
import pandas as pd
fpath = "./datas/beijing_tianqi/beijing_tianqi_2018.csv"
df = pd.read_csv(fpath)
df.head()
'''
ymd	bWendu	yWendu	tianqi	fengxiang	fengli	aqi	aqiInfo	aqiLevel
0	2018-01-01	3℃	-6℃	晴~多云	东北风	1-2级	59	良	2
1	2018-01-02	2℃	-5℃	阴~多云	东北风	1-2级	49	优	1
2	2018-01-03	2℃	-5℃	多云	北风	1-2级	28	优	1
3	2018-01-04	0℃	-8℃	阴	东北风	1-2级	28	优	1
4	2018-01-05	3℃	-6℃	多云~晴	西北风	1-2级	50	优	1
'''

# 直接赋值的方法
df.loc[:,"wencha"] = df["bWendu"] - df["yWendu"] # 新增一列

# 替换掉温度的后缀℃
df.loc[:, "bWendu"] = df["bWendu"].str.replace("℃", "").astype('int32')
df.loc[:, "yWendu"] = df["yWendu"].str.replace("℃", "").astype('int32')

# df.apply方法
def get_wendu_type(x):
    if x["bWendu"] > 33:
        return '高温'
    if x["yWendu"] < -10:
        return '低温'
    return '常温'

# 注意需要设置axis==1，这是series的index是columns
df.loc[:, "wendu_type"] = df.apply(get_wendu_type, axis=1)

# 查看温度类型的计数
df['tianqi'].value_counts()

# 新增列
# 可以同时添加多个新的列
df.assign(
    yWendu_huashi = lambda x : x["yWendu"] * 9 / 5 + 32,
    # 摄氏度转华氏度
    bWendu_huashi = lambda x : x["bWendu"] * 9 / 5 + 32
)

# 按条件选择分组分别赋值
# 先创建空列（这是第一种创建新列的方法）
df['wencha_type'] = ''

df.loc[df["bWendu"]-df["yWendu"]>10, "wencha_type"] = "温差大"

df.loc[df["bWendu"]-df["yWendu"]<=10, "wencha_type"] = "温差正常"
```

# 五、数据统计函数

```python
import pandas as pd
fpath = "./datas/beijing_tianqi/beijing_tianqi_2018.csv"
df = pd.read_csv(fpath)

# 查看前3行
df.head(3)
'''
	ymd	bWendu	yWendu	tianqi	fengxiang	fengli	aqi	aqiInfo	aqiLevel
0	2018-01-01	3℃	-6℃	晴~多云	东北风	1-2级	59	良	2
1	2018-01-02	2℃	-5℃	阴~多云	东北风	1-2级	49	优	1
2	2018-01-03	2℃	-5℃	多云	北风	1-2级	28	优	1
'''

# 汇总类统计
# 一下子提取所有数字列统计结果
df.describe()
'''
	bWendu	yWendu	aqi	aqiLevel
count	365.000000	365.000000	365.000000	365.000000
mean	18.665753	8.358904	82.183562	2.090411
std	11.858046	11.755053	51.936159	1.029798
min	-5.000000	-12.000000	21.000000	1.000000
25%	8.000000	-3.000000	46.000000	1.000000
50%	21.000000	8.000000	69.000000	2.000000
75%	29.000000	19.000000	104.000000	3.000000
max	38.000000	27.000000	387.000000	6.000000
'''

# 查看单个Series的数据平均值
df["bWendu"].mean()
18.665753424657535

# 最高温
df["bWendu"].max()
38

# 最低温
df["bWendu"].min()
-5

# 唯一值
df["fengxiang"].unique()
'''
array(['东北风', '北风', '西北风', '西南风', '南风', '东南风', '东风', '西风'], dtype=object)
'''

#nunique（），n代表了个数number，可以直接获取去重数据后的个数
df["fengli"].nunique()
6

# 按值计数
df["fengxiang"].value_counts()
'''
南风     92
西南风    64
北风     54
西北风    51
东南风    46
东北风    38
东风     14
西风      6
Name: fengxiang, dtype: int64
'''

# 相关系数和协方差
# 协方差：衡量同向反向程度，如果协方差为正，说明X，Y同向变化，协方差越大说明同向程度越高；如果协方差为负，说明X，Y反向运动，协方差越小说明反向程度越高。

# 相关系数：衡量相似度程度，当他们的相关系数为1时，说明两个变量变化时的正向相似度最大，当相关系数为－1时，说明两个变量变化的反向相似度最大

# 协方差矩阵：
df.cov()
'''
	bWendu	yWendu	aqi	aqiLevel
bWendu	140.613247	135.529633	47.462622	0.879204
yWendu	135.529633	138.181274	16.186685	0.264165
aqi	47.462622	16.186685	2697.364564	50.749842
aqiLevel	0.879204	0.264165	50.749842	1.060485
'''
# 相关系数矩阵
df.corr()
'''
	bWendu	yWendu	aqi	aqiLevel
bWendu	1.000000	0.972292	0.077067	0.071999
yWendu	0.972292	1.000000	0.026513	0.021822
aqi	0.077067	0.026513	1.000000	0.948883
aqiLevel	0.071999	0.021822	0.948883	1.000000
'''

# 单独查看空气质量和最高温度的相关系数(机器学习重要)
df["aqi"].corr(df["bWendu"])
0.07706705916811077

```

# 六、缺失值的处理

```python
import pandas as pd
# 略过空行 skiprows
studf = pd.read_excel("./datas/student_excel/student_excel.xlsx", skiprows=2)
studf
'''
	Unnamed: 0	姓名	科目	分数
0	NaN	小明	语文	85.0
1	NaN	NaN	数学	80.0
2	NaN	NaN	英语	90.0
3	NaN	NaN	NaN	NaN
4	NaN	小王	语文	85.0
5	NaN	NaN	数学	NaN
6	NaN	NaN	英语	90.0
7	NaN	NaN	NaN	NaN
8	NaN	小刚	语文	85.0
9	NaN	NaN	数学	80.0
10	NaN	NaN	英语	90.0
'''

# 检测空值
studf.isnull()
'''
	Unnamed: 0	姓名	科目	分数
0	True	False	False	False
1	True	True	False	False
2	True	True	False	False
3	True	True	True	True
4	True	False	False	False
5	True	True	False	True
6	True	True	False	False
7	True	True	True	True
8	True	False	False	False
9	True	True	False	False
10	True	True	False	False
'''

# 非空
studf["分数"].notnull()

# 筛选没有空分数的所有行
studf.loc[studf["分数"].notnull(), :]

# 删除掉全是空值的列
studf.dropna(axis="columns", how='all', inplace=True)

# 将分数列为空的填充为0分
studf.fillna({"分数":0},inplace=True)
# 等同于
studf.loc[:, '分数'] = studf['分数'].fillna(0)

# 使用前面的有效值填充
studf.loc[:, '姓名'] = studf['姓名'].fillna(method="ffill")

# 将清洗好的excel保存
studf.to_excel("./datas/student_excel/student_excel_clean.xlsx",index=False)
```

# 七、数据排序

```python
import pandas as pd
fpath = "./datas/beijing_tianqi/beijing_tianqi_2018.csv"
df = pd.read_csv(fpath)
df.head()
'''
	ymd	bWendu	yWendu	tianqi	fengxiang	fengli	aqi	aqiInfo	aqiLevel
0	2018-01-01	3	-6	晴~多云	东北风	1-2级	59	良	2
1	2018-01-02	2	-5	阴~多云	东北风	1-2级	49	优	1
2	2018-01-03	2	-5	多云	北风	1-2级	28	优	1
3	2018-01-04	0	-8	阴	东北风	1-2级	28	优	1
4	2018-01-05	3	-6	多云~晴	西北风	1-2级	50	优	1
'''

# Series的排序
df["aqi"].sort_values(ascending=False)
'''
86     387
72     293
91     287
71     287
317    266
      ... 
301     22
272     22
249     22
281     21
271     21
Name: aqi, Length: 365, dtype: int64
'''

# DataFrame的排序
df.sort_values(by="aqi")
'''
	ymd	bWendu	yWendu	tianqi	fengxiang	fengli	aqi	aqiInfo	aqiLevel
271	2018-09-29	22	11	晴	北风	3-4级	21	优	1
281	2018-10-09	15	4	多云~晴	西北风	4-5级	21	优	1
249	2018-09-07	27	16	晴	西北风	3-4级	22	优	1
272	2018-09-30	19	13	多云	西北风	4-5级	22	优	1
301	2018-10-29	15	3	晴	北风	3-4级	22	优	1
...	...	...	...	...	...	...	...	...	...
317	2018-11-14	13	5	多云	南风	1-2级	266	重度污染	5
71	2018-03-13	17	5	晴~多云	南风	1-2级	287	重度污染	5
91	2018-04-02	26	11	多云	北风	1-2级	287	重度污染	5
72	2018-03-14	15	6	多云~阴	东北风	1-2级	293	重度污染	5
86	2018-03-28	25	9	多云~晴	东风	1-2级	387	严重污染	6
'''

#降序
df.sort_values(by="aqi", ascending=False)

#多列排序
df.sort_values(by=["aqiLevel", "bWendu"])

# 分别指定升序和降序
df.sort_values(by=["aqiLevel", "bWendu"], ascending=[True, False])
```

# 八、字符串处理

```python
import pandas as pd
fpath = "./datas/beijing_tianqi/beijing_tianqi_2018.csv"
df = pd.read_csv(fpath)
df.head()
'''
	ymd	bWendu	yWendu	tianqi	fengxiang	fengli	aqi	aqiInfo	aqiLevel
0	2018-01-01	3℃	-6℃	晴~多云	东北风	1-2级	59	良	2
1	2018-01-02	2℃	-5℃	阴~多云	东北风	1-2级	49	优	1
2	2018-01-03	2℃	-5℃	多云	北风	1-2级	28	优	1
3	2018-01-04	0℃	-8℃	阴	东北风	1-2级	28	优	1
4	2018-01-05	3℃	-6℃	多云~晴	西北风	1-2级	50	优	1
'''

# 字符串替换函数
df["bWendu"].str.replace("℃", "")

# 判断是不是数字
df["bWendu"].str.isnumeric()

# 长度
df["aqi"].str.len()

# 判断是否由xxx开头的
condition = df["ymd"].str.startswith("2018-03")
df[condition].head()

# 先把-变为空气 然后 截取前6
df["ymd"].str.replace("-", "").str.slice(0, 6)
# slice就是切片语法，可以直接用 这个
df["ymd"].str.replace("-", "").str[0:6]

# 方法1：链式replace
df["中文日期"].str.replace("年", "").str.replace("月","").str.replace("日", "")

# 方法2：正则表达式替换
df["中文日期"].str.replace("[年月日]", "")
```

# 九、数据Merge

Pandas的Merge，相当于Sql的Join，将不同的表按key关联到一个表

```python
left = pd.DataFrame({'sno': [11, 12, 13, 14],
                      'name': ['name_a', 'name_b', 'name_c', 'name_d']
                    })
left
"""
	sno	name
0	11	name_a
1	12	name_b
2	13	name_c
3	14	name_d
"""

right = pd.DataFrame({'sno': [11, 12, 13, 14],
                      'age': ['21', '22', '23', '24']
                    })
right
'''
	sno	age
0	11	21
1	12	22
2	13	23
3	14	24
'''

# 一对一关系，结果中有4条
pd.merge(left, right, on='sno')
'''
	sno	name	age
0	11	name_a	21
1	12	name_b	22
2	13	name_c	23
3	14	name_d	24
'''
```

# 十、数据合并

```python
df1 = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
                    'B': ['B0', 'B1', 'B2', 'B3'],
                    'C': ['C0', 'C1', 'C2', 'C3'],
                    'D': ['D0', 'D1', 'D2', 'D3'],
                    'E': ['E0', 'E1', 'E2', 'E3']
                   })
df1
'''
	A	B	C	D	E
0	A0	B0	C0	D0	E0
1	A1	B1	C1	D1	E1
2	A2	B2	C2	D2	E2
3	A3	B3	C3	D3	E3
'''
df2 = pd.DataFrame({ 'A': ['A4', 'A5', 'A6', 'A7'],
                     'B': ['B4', 'B5', 'B6', 'B7'],
                     'C': ['C4', 'C5', 'C6', 'C7'],
                     'D': ['D4', 'D5', 'D6', 'D7'],
                     'F': ['F4', 'F5', 'F6', 'F7']
                   })
df2
'''
	A	B	C	D	F
0	A4	B4	C4	D4	F4
1	A5	B5	C5	D5	F5
2	A6	B6	C6	D6	F6
3	A7	B7	C7	D7	F7
'''

# 合并concat 默认加在下面
pd.concat([df1,df2])
'''
	A	B	C	D	E	F
0	A0	B0	C0	D0	E0	NaN
1	A1	B1	C1	D1	E1	NaN
2	A2	B2	C2	D2	E2	NaN
3	A3	B3	C3	D3	E3	NaN
0	A4	B4	C4	D4	NaN	F4
1	A5	B5	C5	D5	NaN	F5
2	A6	B6	C6	D6	NaN	F6
3	A7	B7	C7	D7	NaN	F7
'''

# 使用ignore_index=True可以忽略原来的索引
pd.concat([df1,df2], ignore_index=True)
'''
	A	B	C	D	E	F
0	A0	B0	C0	D0	E0	NaN
1	A1	B1	C1	D1	E1	NaN
2	A2	B2	C2	D2	E2	NaN
3	A3	B3	C3	D3	E3	NaN
4	A4	B4	C4	D4	NaN	F4
5	A5	B5	C5	D5	NaN	F5
6	A6	B6	C6	D6	NaN	F6
7	A7	B7	C7	D7	NaN	F7
'''

# 使用join=inner过滤掉不匹配的列
pd.concat([df1,df2], ignore_index=True, join="inner")
'''
	A	B	C	D
0	A0	B0	C0	D0
1	A1	B1	C1	D1
2	A2	B2	C2	D2
3	A3	B3	C3	D3
4	A4	B4	C4	D4
5	A5	B5	C5	D5
6	A6	B6	C6	D6
7	A7	B7	C7	D7
'''

# 使用axis=1相当于添加新列(横着加就是了)
pd.concat([df1,s1,s2], axis=1)
```

# 十一、分组聚合

```python
df = pd.DataFrame({'A': ['foo', 'bar', 'foo', 'bar', 'foo', 'bar', 'foo', 'foo'],
                   'B': ['one', 'one', 'two', 'three', 'two', 'two', 'one', 'three'],
                   'C': np.random.randn(8),
                   'D': np.random.randn(8)})
df
'''
	A	B	C	D
0	foo	one	0.542903	0.788896
1	bar	one	-0.375789	-0.345869
2	foo	two	-0.903407	0.428031
3	bar	three	-1.564748	0.081163
4	foo	two	-1.093602	0.837348
5	bar	two	-0.202403	0.701301
6	foo	one	-0.665189	-1.505290
7	foo	three	-0.498339	0.534438
'''

# 单个列groupby，查询所有数据列的统计
df.groupby('A').sum()
'''
A	C	D		
bar	-2.142940	0.436595
foo	-2.617633	1.083423
'''

# 多个列groupby，查询所有数据列的统计
df.groupby(['A','B']).mean()
'''
A	B	C	D	
bar	one	-0.375789	-0.345869
three	-1.564748	0.081163
two	-0.202403	0.701301
foo	one	-0.061143	-0.358197
three	-0.498339	0.534438
two	-0.998504	0.632690
'''

# 同时查看多种数据统计
df.groupby('A').agg([np.sum, np.mean, np.std])
'''
	C	D
sum	mean	std	sum	mean	std
A						
bar	-2.142940	-0.714313	0.741583	0.436595	0.145532	0.526544
foo	-2.617633	-0.523527	0.637822	1.083423	0.216685	0.977686
'''

# 不同列使用不同的聚合函数
df.groupby('A').agg({"C":np.sum, "D":np.mean})

# 可以直接matplotlab group_data.plot()
```

