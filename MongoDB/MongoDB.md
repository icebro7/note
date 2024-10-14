# MongoDB

NoSQL文档型数据库



# 命令

### 1、基本命令

```shell
# 显示当前MongoDB实例中的所有数据库。
show databases
show dbs 

# 切换到数据库<dbname>。
use <dbname> 

# 显示当前使⽤中的数据库名称
db

# 清屏
cls

# 显示当前数据库中的所有集合
show collections

# 删除当前的数据库
db.dropDatabase() 

# 退出mongosh会话
exit
```



### 2、创建/插⼊

```shell
# 在集合中插⼊⼀个新的⽂档
insertOne
db.users.insertOne({name: "⽼杨"})

# 在集合中插⼊多个新的⽂档
insertMany
db.users.insertMany([{name: "李四"}, {name: "王五"}])
```



### 3、查找

```shell
# 查询所有的⽂档
find
db.users.find()

# 查询所有满⾜参数对象<filterObject>中指定过滤条件的数据
find(<filterObject>)
db.users.find({name: "⽼杨"})

# 查询所有满⾜参数对象<filterObject>中指定过滤条件的数据，并且只返回<selectObject>中指定的字段
db.users.find({name: "⽼杨"}, {name: 1, email: 1})

# 与find⽤法相同，找到满⾜过滤条件的对象，但是只返回第⼀条
findOne
db.users.findOne({level: 1})

# 返回满⾜条件的记录的数量
countDocuments
```



### 4、更新

```shell
# 更新满⾜条件的第⼀个⽂档
updateOne

# 更新满⾜条件的所有⽂档。
updateMany 

# 替换满⾜条件的第⼀个⽂档。
replaceOne 

# 通过传⼊的⽂档替换已有⽂档或插⼊⼀个新的⽂档。
save 

# 只更新⽂档中$set指定的字段，不会影响其他字段。
$set 

# ⽤于递增（或递减）⽂档中指定字段值的操作符。
$inc 

# 更新某个字段的名称。
$rename 

# 删除⼀个字段。
$unset 

# 将值加⼊⼀个数组中，不会判断是否有重复的值。
$push 

# 将值从⼀个数组中移除。
$pull 

# 将值加⼊⼀个数组中，会判断是否有重复的值，若重复则不加⼊
$addToSet 
```



### 5、删除

```shell
# 删除满⾜条件的第⼀个⽂档。
deleteOne

# 删除满⾜条件的所有⽂档。
deleteMany
```



### 6、过滤

```shell
# 等于（equal）
$eq 
db.users.find({level:{$in:3}})

# 不等于（not equal）。
$ne

# ⼤于（greater than) /⼤于等于 （greater than or equal to）
$gt / $gte 

# ⼩于（less than) /⼩于等于 （less than or equal to）
$lt / $lte 

# 值在指定值列表中，就返回该⽂档。
$in 

# 值不等于指定值列表中的任何⼀个，就返回该⽂档。
$nin 

# 检查复数条件是否均为真，可以简单理解为“并且”。
$and 
db.users.find({$and:[{level:{$get:3}},{level:{$lte:5}}]})

# 检查复数条件中是否有⼀个为真，可以简单理解为“或者”。
$or 

# 将$not⾥⾯的过滤条件取反。
$not 

# 检查⼀个字段是否存在。
$exists 

# 在不同的字段之间做⽐较。
$expr 
```



### 7、聚合

```shell
# 计数
countDocuments
db.users.countDocuments()

# 计算总和。
$sum 

# 计算平均值。
$avg 

# 获取最⼩值。
$min 

# 获取最⼤值。
$max 

# 将值加⼊⼀个数组中，不会判断是否有重复的值。
$push 

# 获取第⼀个⽂档数据。
$first 

# 获取最后⼀个⽂档数据。
$last 
```















