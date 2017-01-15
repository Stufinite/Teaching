# Django 進階

* models
* post value and save with DB
* Build an API

## models

```
class SubmitForm(forms.Form):
    # required=False 要詢問!!!!
    Std_ID = forms.CharField(label='Please Enter Your Student ID', max_length=15, required=False)
    Json_Str = forms.CharField(label='Please Enter Your Json Result', max_length=1000, required=False)
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
          <input type="" name="answer" placeholder="請輸入JSON答案">
          <label class="login-field-icon fui-lock" for="login-pass"></label>
          </div>
          
          <!--  <a class="btn btn-primary btn-large btn-block" href="#">login</a>  -->
          <button class="btn btn-primary btn-large btn-block" type="submit" id="submit">繳交</button>
</form>
```

```
# 如果是用POST的方式進來這個function
if request.method == 'POST' and request.POST:
	# 如果是POST，就再產生一個變數接request.POST的東西，並將之與form.py裡面的格式結合
	data = request.POST 
	data=data.dict()
```

## Build an API
