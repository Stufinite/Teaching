# Django 進階

* models
* post value and save with DB
* Build an API

## models

### 把這段貼進 `firstapp/models.py`

這就是設定資料庫的 `Schema`

設定他有那些欄位等等

`__str__` 是這個資料庫物件的字串表示型態

```
from django.db import models

class Result(models.Model):
    StdID = models.CharField(max_length=15) # 學號
    Score = models.IntegerField() # 得分

    def __str__(self):
        return self.StdID
```

### After this, update your DATABASE schema 
### 更新到新的 `database schema`

`makemigrations`:當你的models.py有更新的時候，需要執行一次，django會自動產生出一個migrations檔

其實就是一次記錄存檔，記錄你在這個時間點對資料庫的設定是什麼  
你可以向git一樣任意切換不同時間點的設定

`migrate`:(預設)套用到最新的migrations檔，也可以指定不同版本的migrations


```
python manage.py makemigrations
python manage.py migrate

```

## Template

### insert these line into the body of `firstapp/templates/firstapp/index.html`
### 把這段html程式碼貼近 `firstapp/templates/firstapp/index.html` 的 `body` 裡面，貼哪裡都沒差

<body>
```
  <form class="ui form" action="" method="post">
          {% csrf_token %}
          <div class="control-group">
            <input type="" name="studentID" placeholder="請輸入學號">
            <label class="login-field-icon fui-user" for="login-name"></label>
          </div>

      <div class="control-group">
        <input type="" name="score" placeholder="請輸入your score">
        <label class="login-field-icon fui-user" for="login-name"></label>
      </div>
            <button class="btn btn-primary btn-large btn-block" type="submit" id="submit">繳交</button>
  </form>
```  
</body>

## Views.py

### insert these line into `group` function of `firstapp/views.py`
### 把下面這一段貼進去 `group` 的 `views.py`

def group(request):

  貼這些進去views.py

  ```
  from firstapp.models import Result

  # 如果是用POST的方式進來這個function
  if request.method == 'POST' and request.POST:
      # 如果是POST，就再產生一個變數接request.POST的東西，並將之與form.py裡面的格式結合
      data = request.POST 
      data=data.dict()

      print(data)
      print(data['studentID'])
      print(data['score'])

      Result.objects.create(StdID=data['studentID'], Score=data['score'])
  ```

  return render()

## 進到後台看看 database 實際储存的結果吧

### add these line into firstapp/admin.py


```
from firstapp.models import Result
# Register your models here.

admin.site.register(Result)
```

### create superuser to login into 127.0.0.1:8000/admin
### 建立 `django` 的 `superuser` 然後你就可以進到自己網站的後台囉

```
python manage.py createsuperuser
```

### That's it, log into [127.0.0.1:8000/admin](http://127.0.0.1:8000/admin) for a look.

## show database result in template
## 在 `template` 直接顯示 `database` 撈出來的資料~

### update `firstapp/templates/firstapp/index.html`

### 整段都貼進去吧

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  {% for i in allResult %}
    <h1>{{i.StdID}} {{i.Score}}</h1>
  {% endfor %}

  <h1>Welcome~ {{name}}</h1>
  {% for i in times %}
    <h1>{{g}}</h1>
    <button>{{buttonName}}</button>
  {% endfor %}

  <form class="ui form" action="" method="post">
          {% csrf_token %}
          <div class="control-group">
            <input type="" name="studentID" placeholder="請輸入學號">
            <label class="login-field-icon fui-user" for="login-name"></label>
          </div>

      <div class="control-group">
        <input type="" name="score" placeholder="請輸入your score">
        <label class="login-field-icon fui-user" for="login-name"></label>
      </div>
            <button class="btn btn-primary btn-large btn-block" type="submit" id="submit">繳交</button>
  </form>
</body>
</html>
```

### update firstapp/views.py

### 整段都貼進去吧

```
# Create your views here.
def group(request):
  g = '中興資工 107與他的快樂伙伴'
  times = range(10)
  name = request.GET['name']
  buttonName = 'i am button'
  # 如果是用POST的方式進來這個function
  if request.method == 'POST' and request.POST:
      # 如果是POST，就再產生一個變數接request.POST的東西，並將之與form.py裡面的格式結合
      data = request.POST 
      data=data.dict()

      print(data)
      print(data['studentID'])
      print(data['score'])

      Result.objects.create(StdID=data['studentID'], Score=data['score'])
      allResult = Result.objects.all()

  return render(request, 'firstapp/index.html', locals())
```