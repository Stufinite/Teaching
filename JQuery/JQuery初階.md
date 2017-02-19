# JQuery 初階

* 基本JQuery語法

## 1. click

`$().click(function(){ });`

`$()`放html標記，`function(){ }`接的是當click觸發時要做的事。

`<html>
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
</html>`
把上面貼進html點按鈕看看結果

## 2. text

`$().text()`

`$()`放html標記，`()`接的是要取代的文字。

`<html>
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
</html>`
把上面貼進html點按鈕看看結果

## 3. append

`$().append()`

`$()`放html標記，`()`接的是要新增的文字。
效果和text()差不多，不同在取代和續接

`<html>
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
</html>`
把上面貼進html點按鈕看看結果
