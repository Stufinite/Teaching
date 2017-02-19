# JQuery 初階

* 基本JQuery語法

## 1. click()

`$().click(function(){ });`

`$()`放html標記，`function(){ }`接的是當click觸發時要做的事。

```
<html>
<head>
<script type="text/javascript" src="/jquery/jquery.js"></script>
<script type="text/javascript">
$(document).ready(function(){
  $("button").click(function(){
    alert("吃屎八醜男");
  });
});
</script>
</head>
<body>
<button>你好帥</button>
</body>
</html>
```
把上面貼進html點按鈕看看結果

## 2. text()

`$().text()`

`$()`放html標記，`()`接的是要取代的文字。

```
<html>
<head>
<script type="text/javascript" src="/jquery/jquery.js"></script>
<script type="text/javascript">
$(document).ready(function(){
  $("button").click(function(){
    $("p").text("不是");
  });
});
</script>
</head>
<body>
<p>你好帥喔!!</p>
<p>你是帥哥嗎??</p>
<button>確認結果</button>
</body>
</html>
```
把上面貼進html點按鈕看看結果

## 3. append()

`$().append()`

`$()`放html標記，`()`接的是要新增的文字。
效果和text()差不多，不同在取代和續接

```
<html>
<head>
<script type="text/javascript" src="/jquery/jquery.js"></script>
<script type="text/javascript">
$(document).ready(function(){
  $("button").click(function(){
    $("p").append("不是你醜男!!");
  });
});
</script>
</head>
<body>
<p>誰是帥哥:</p>

<button>看解答</button>
</body>
</html>
```
把上面貼進html點按鈕看看結果

## 4. each()

`$().each(function(){ });`

`each()`方法類似for的功能，配合陣列使用就可以取到每一個的值。
這裡用html標記來取代陣列作範例:

```
<html>
<head>
<script type="text/javascript" src="/jquery/jquery.js"></script>
<script type="text/javascript">
$(document).ready(function(){
  $("button").click(function(){
    $("li").each(function(){
      alert($(this).text() +":0分")
    });
  });
});
</script>
</head>
<body>
<button>看看女生給你的分數</button>
<ul>
<li>長相</li>
<li>身材</li>
<li>財富</li>
</ul>
</body>
</html>
```
把上面貼進html點按鈕看看結果

## 5. getJSON()

獲得 JSON 數據，並輸出结果

`$.getJSON(" ",function(){ });`

`" "`裡填json路徑或網址，執行的功能放在`{ }`

