# Django 進階

* models
* post value and save with DB
* Build an API

## models

### put these into firstapp/models.py

```
from django.db import models

class Result(models.Model):
    StdID = models.CharField(max_length=15) # 學號
    Score = models.IntegerField() # 得分

    def __str__(self):
        return self.StdID

```

### After this, update your DATABASE schema 

```
python manage.py makemigrations
python manage.py migrate

```

## Template

### insert these line into the body of firstapp/templates/firstapp/index.html

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

## Views.py

### insert these line into `group` function of firstapp/views.py

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

## See your Database with admin

### add these line into firstapp/admin.py

```
from firstapp.models import Result
# Register your models here.

admin.site.register(Result)
```

### create superuser to login into 127.0.0.1:8000/admin

```
python manage.py createsuperuser
```

### That's it, log into 127.0.0.1:8000/admin for a look.

## show database result in template

### update firstapp/templates/firstapp/index.html

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