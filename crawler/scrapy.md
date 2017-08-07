# Scrapy教學

![scrapylogo](scrapylogo.png)

### 安裝

* Linux:`pip install scrapy`
(若遇到Error: command 'x86_64-linux-gnu-gcc' failed with exit status 1，請先`sudo apt-get install python3-dev`)
* Windows:請參考[大數學堂教學](http://www.largitdata.com/course/67/) 安裝anaconda

直接整理一下怎麼安裝:  
1. 下載[anaconda](https://www.continuum.io/downloads)(如果原本有python安裝好的話，無需跟原本的python同一個版本，我是3.5不過我下載3.6應該是沒有太大衝突，但記得不要加入環境變數)
2. 安裝好打開conda prompt，透過以下程式碼檢測安裝好的環境，確認一下:
```
  $pip -V #檢測哪一個版本所使用的pip  
  $pip list #檢測pip 已經安裝了哪些東西
  $python -V #檢測python的版本
```
3. 去找[twisted的wheel檔](http://www.lfd.uci.edu/~gohlke/pythonlibs/#twisted)，下載後cd到下載檔案的目錄，執行以下程式碼:
```
  $cd Downloads #cd到下載目錄  
  $pip install Twisted-17.5.0-cp36-cp36m-win_amd64.whl #安裝這個檔案(如果不是用conda，.whl是wheel檔，請記得先安裝pip install wheel)
```
4. 直接:
```
  $pip install scrapy
```



### 架構

1. spiders：爬蟲的邏輯，寫在spiders
2. item：爬下來的資料格式，定義在item
3. pipline（進階）：爬下來的所有item，可以經由pipeline將資料彙整存進資料庫

### 示範-IThome

目標：將IThome每篇新聞整理程下方的json格式

![ithome](ithome.png)

```
[
  {
    "description": "www.newcastleinternationaluniversity.com網路釣魚存在已久，但卻仍然是相當有效的攻擊手法，英國知名的新堡大學(New Castle University)最近便遇到有人打著學校招牌，卻要騙取不熟悉學校真正樣貌的國際學生學費與個資的詐騙駭客，緊急出面澄清。\n \n在英國中學生的大學入學考試成績即將發表前，網路上出現了一個企圖偽冒新堡大學的網站，該網站註冊了非學術機構的網域名稱www.newcastleinternationaluniversity.com，儘管與新堡大學官網的域名www.ncl.ac.uk相去甚遠，且網站本身的設計與新堡大學官網亦不同，但卻採用與新堡大學乍看之下頗為類似的校徽，網站內容也堪稱完備、校園位置也使用正牌新堡大學位置，很難讓人察覺是釣魚網站。\n下圖為假造的新堡國際大學網站，左上角的校徽和正牌新堡大學（下）極為相似：\n\n \n正牌的新堡大學網站：\n\n \n儘管對於英國當地學生來說，不至於搞混兩個網站，但對於國際學生而言，卻未必能夠分清正牌新堡大學與偽冒的新堡國際大學間的差異。\n \n冒牌網站會要求學生的個人資料，包括護照資訊，同時還會要求學生提供信用卡資訊作為繳付修讀課程的註冊費之用。\n \n新堡大學透過Twitter發表聲明表示，有注意到企圖仿冒新堡大學品牌的詐騙網站以及其收費行為，並強調該網站與學校毫不相關，呼籲任何人不要提供任何資料給這個網站。\n \n英國媒體初步分析指出，該網站似乎是透過電子郵件，針對英國以外的學生發送。資安專家表示，該網站並非只有表面的釣魚網站，還費心製作不少內容，詐騙效果更高，同時提醒現今的詐騙攻擊的設計、執行，都越來越講究。\n ",
    "price": 0,
    "location": "",
    "channel": "",
    "category": "event",
    "title": "想到英國留學要小心! 歹徒假造知名大學網頁騙走你的學費",
    "link": "http://www.ithome.com.tw/news/115706",
    "type": "it",
    "image": "http://static4.ithome.com.tw/sites/default/files/styles/picture_size_large/public/field/image/new-1.jpg?itok=sNPi7Nd4",
    "time": "2017-07-21"
  },
  ...
  ...
  ...
]
```

### 作法
1. 建立scrapy這個專案：
    1. `scrapy startproject spcrapyDemo`
    2. `scrapy genspider ithome www.ithome.com.tw`
2. 先定義好資料格式 schema
    * spcrapyDemo/spiders/items.py的內容：
      ```
      # -*- coding: utf-8 -*-
      import scrapy
      class TripadvisorItem(scrapy.Item):
          # define the fields for your item here like:
          title = scrapy.Field()
          location = scrapy.Field()
          description = scrapy.Field()
          category = scrapy.Field()
          type = scrapy.Field()
          channel = scrapy.Field()
          time = scrapy.Field()
          price = scrapy.Field()
          image = scrapy.Field()
          link = scrapy.Field()
      ```
3. 寫spiders的邏輯：
    1. 流程：`start_urls` -> `parse` -> `callback function`
        * callback function的作用：以IThome為例  
          首頁是新聞的清單，點進去才會有新聞的詳細資訊  
          parse只是讓你拿到首頁的新聞清單  
          需要自己寫一個callback function才能從首頁鑽進去每篇新聞。
    3. 先定義要爬的網址 start_urls：在這裡就是ithome的網址 [http://www.ithome.com.tw/security](http://www.ithome.com.tw/security)
        * spcrapyDemo/spiders/ithome.py：
        ```
        start_urls = ['http://www.ithome.com.tw/security']
        ```
    4. scrapy會自動requests.get() start_urls裏面所有的網址，然後把response傳回`parse`這個function  
    `response.body`可以拿到網址的html  
    `yield` 是python的語法，產生generator  
    目的是較節省記憶體和時間，每個yield都會鑽進每篇新聞裏面  
    並且透過自己定義的`parse_detail`去爬該篇詳細的新聞資訊  
    整段parse的邏輯如下：`res.select('.item')`可以選取到首頁所有的新聞  
    再使用`scrapy.Request`一一對每一篇新聞做request  
    並且使用`parse_detail`這個函數去解析每篇新聞  
    回傳一個item
        * spcrapyDemo/spiders/ithome.py：
        ```
        def parse(self, response):
    		res = BeautifulSoup(response.body)
    		for i in res.select('.item'):
    			yield scrapy.Request("http://www.ithome.com.tw" + i.select('a')[0]['href'], self.parse_detail)
        ```
    5. parse_detail是每篇新聞，每篇新聞要回傳一個json的object，在scrapy裏面，是用orm的概念，存成物件，再將他轉換成json的型態。所以自定義的callback function，需要回傳item
        * spcrapyDemo/spiders/ithome.py：
        ```
        def parse_detail(self, response):
    		res = BeautifulSoup(response.body)
    		tripItem = TripadvisorItem()
    		tripItem['title'] = res.select('.page-header')[0].text.replace('\n', '')
    		tripItem['location'] = ''
    		tripItem['description'] = functools.reduce(lambda x,y:x+'\n'+y, map(lambda review:review.text, res.select('.even p'))).replace('\n', '', 1)
    		tripItem['category'] = "event"
    		tripItem['type'] = "it"
    		tripItem['channel'] = ""
    		tripItem['time'] = res.select('#block-views-view-news-custom-submitted .created')[0].text
    		tripItem['price'] = 0
    		tripItem['image'] = res.select('.img-wrapper img')[0]['src']
    		tripItem['link'] = response.url
    		return tripItem
        ```
    4. 執行：`scrapy crawl ithome -o xxx.json -t json` 這樣就產生一個叫作`xxx.json`的檔案了
        * [範例程式碼連結](https://github.com/UDICatNCHU/User-Interest-Extraction-API/tree/master/restaurant%2Battractions)
        

    5. Debug：`scrapy crawl ithome --logfile logfile.txt` 這樣就產生一個叫作`logfile.txt`的檔案了，會把原本應該輸出在cmdline的文字直接寫入file，透過print印在cmdline的文字也不會被淹沒在茫茫的log文字之中。
       
