# MongoDB 教學
## [參考](http://rasca0027.logdown.com/posts/252512-python-mongodb-pymongo-teaching)

## 安裝：
`pip install pymongo`

## 連接資料庫：

mongoDB預設是沒有密碼的，所以uri設定為None即可  
uri是 `Uniform Resource Identifier` 翻譯叫做 `統一資源識別元`  
url是uri的一個子集合  
簡單來說就是描述資源的位置  

mongoDB的概念與relational的資料庫不同  
table在NoSQL叫做collections  
row叫做Document  
Document就可以處存陣列、字典等等有結構性的資料格式  
比較彈性，但是資料庫就不會自動幫我們建立索引  
要自己建，理由很簡單，因為每一筆資料(Document)不一定都有一樣的欄位  
所以沒辦法自動建索引  

與relational資料庫相比  
insert快  select慢
[實驗數據](https://github.com/webcaetano/mongo-mysql)

```
import pymongo
from pymongo import MongoClient
# mongoDB預設是沒有密碼的，所以uri設定為None即可
uri = None
# 有設密碼才要這樣寫
# uri = "mongodb://USERNAME:password@host?authSource=source"

client = MongoClient(uri)
db = client['資料庫名稱']
collect = db['collections名稱']
```

## insert

```
collect.insert({"name":"張泰瑋", "女朋友":[1,2,3,4,5,6,7,8,9]})
collect.insert({"name":"科餅停", "女朋友":[]})
```

## select

```
cursor = collect.find({"name":"張泰瑋"}).limit(1)
if cursor.count() > 0:
    # count>0代表有查到東西
    print(list(cursor)[0])
```

## update
`upsert`參數是指:update這筆資料時，有就update沒有就insert

```
collect.update({'name':"張泰瑋"}, {'$set': {"女朋友":["沒有了"]}}, upsert=True)

# {'_id':False} 代表不要回傳mongoDB內建的id，也可以用其他欄位設定1和-1指定要或不要顯示
cursor = collect.find({"name":"張泰瑋"}, {'_id':False}).limit(1)

if cursor.count() > 0:
    print(list(cursor)[0])
```

## remove

```
全部刪除
collect.remove({})

指定刪除
collect.remove({"name":"張泰瑋"})
```
