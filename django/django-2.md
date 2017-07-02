# Django 進階

* models 資料庫介紹
* 透過post存到資料庫
* 申請superuser，並且查看後台
* 存取資料庫資料並呈現到template

## 0. 先把建立app
`django-admin startapp firstapp` (window 試試django-admin.py startapp firstapp)

first/first/urls.py:
```
from django.conf.urls import url, include
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^firstapp/',include('firstapp.urls') ),
]
```

建立first/firstapp/urls.py:
```
from django.conf.urls import url, include
from firstapp.views import group

urlpatterns = [
    url(r'^group$', group),
]
```
把之前first的virws.py移動到  
first/firstapp/views.py:
```
from django.shortcuts import render

# Create your views here.
def group(request):
	g = '中興資工 107與他的快樂伙伴'
	times = range(5)
	name = request.GET['name']
	print(locals())
	return render(request, 'index.html', locals())
```

在settings.py新增firstapp在陣列裏面：  
```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    #########新增這個#########
    'firstapp'
    #############
]
```

## 1. models 資料庫介紹
把這段貼進 `firstapp/models.py`  
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

### 更新到新的 `database schema`

`makemigrations`:當你的models.py有更新的時候  
需要執行一次，django會自動產生出一個migrations檔  
其實就是一次記錄存檔，記錄你在這個時間點對資料庫的設定是什麼  
你可以向git一樣任意切換不同時間點的設定

`migrate`:(預設)套用到最新的migrations檔，也可以指定不同版本的migrations


```
python manage.py makemigrations
python manage.py migrate

```

## 2. 透過post存到資料庫

## Template

把這段html程式碼貼近 `templates/index.html` 的 `body` 裡面，貼哪裡都沒差


`<body>`

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

`</body>`

## Views.py

把下面這一段貼進去 `group` 的 `views.py`

`def group(request):`
原本的東西可以留著  
再下面新增就好  
注意縮排
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
  貼到這邊就好  

`  return render()`

## 進到後台看看 database 實際储存的結果吧

把這幾行加到 `firstapp/admin.py`


```
from firstapp.models import Result
# Register your models here.

admin.site.register(Result)
```

## 3. 申請superuser，並且查看後台


```
python manage.py createsuperuser
```
這樣就可以登入 [127.0.0.1:8000/admin](http://127.0.0.1:8000/admin) 看看資料庫的長相了.

### 4. 存取資料庫資料並呈現到template

整段都貼進去 `templates/index.html`



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

整段都貼進去吧

```
from firstapp.models import Result
from django.shortcuts import render
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

  return render(request, 'index.html', locals())
```
