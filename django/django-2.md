# Django 進階

* models
* post value and save with DB
* Build an API

## models

```
from django.db import models

class Result(models.Model):
    StdID = models.CharField(max_length=15) # 學號
    Score = models.IntegerField() # 得分

    def __str__(self):
        return self.StdID

```

## post value and save with DB

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

```
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

## Build an API
