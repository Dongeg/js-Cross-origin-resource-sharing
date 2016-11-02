# js-Cross-origin-resource-sharing
js跨域资源共享

js跨域访问的方法

1.jsonp

因为<code><script src="xxx.com"></script></code>不受同源策略的影响，所以可以用它来跨域，称为jsonp

<h3>使用方法</h3>

本文后端代码如下

```php
<?php  
  
//服务端返回JSON数据  
$arr=array('a'=>1,'b'=>2,'c'=>3,'d'=>4,'e'=>5);  
$result=json_encode($arr);  
//echo $_GET['callback'].'("Hello,World!")';  
//echo $_GET['callback']."($result)";  
//动态执行回调函数  
$callback=$_GET['callback'];  
echo $callback."($result)"; //字符串拼接  返回 jsonpCallback(a:1,b:2,etc)

```
1.原生使用
```
<meta content="text/html; charset=utf-8" http-equiv="Content-Type" />  
<script type="text/javascript">  
    function jsonpCallback(result) {  
        alert(result.a);  
        alert(result.b);  
        alert(result.c);  
        for(var i in result) {  
            alert(i+":"+result[i]);//循环输出a:1,b:2,etc.  
        }  
    }  
</script>  
<script type="text/javascript" src="http://crossdomain.com/services.php?callback=jsonpCallback"></script>  
```
2.用jquery
$.getJSON
```js
    $.getJSON("http://crossdomain.com/services.php?callback=?",  
    function(result) {  
        for(var i in result) {  
            alert(i+":"+result[i]);//循环输出a:1,b:2,etc.  
        }  
    }); 

```
$.ajax
```
    $.ajax({  
        url:"http://crossdomain.com/services.php",  
        dataType:'jsonp',  
        data:'',  
        jsonp:'callback',  
        success:function(result) {  
            for(var i in result) {  
                alert(i+":"+result[i]);//循环输出a:1,b:2,etc.  
            }  
        },  
        timeout:3000  
    }); 
```
$.get
```
$.get('http://crossdomain.com/services.php?callback=?', {name: encodeURIComponent('tester')}, function (json) { 
  for(var i in json) alert(i+":"+json[i]); 
}, 'jsonp'); 
```

<a href="http://justcoding.iteye.com/blog/1366102/">参考链接</a>
