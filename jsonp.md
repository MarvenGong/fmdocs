# JSONP 支持
应许多用户的要求，先支持JSONP的访问，使用方法很简单，在接口地址后面加入callback参数即可，当然，您也可以自定义参数名称，通过jsonp_callback_name参数指定如：
## 使用示例
### 1、默认callback参数为回调函数
~~~javascript
// 自己通过script标签或者iframe请求
// 接口根地址-此处是我本地的调试地址
var mockUrl = 'http://localhost:3000/mock/f90b66e99d580dc7b24e54a146b52317/test1'
var say = function(res) {
  console.log(res);
};
var script = document.createElement('script');
script.src = mockUrl + '/testSuccess?callback=say';
document.body.appendChild(script);

// jquery jsonp请求
$.ajax({
    type : "get",
    async: true,
    url : mockUrl + '/testSuccess', // 跨域的请求地址,jquery内部会处理jsonp请求逻辑，添加callback参数
    dataType : "jsonp",
    success : function(json){
      console.dir(json)
    },  
    beforeSend: function(){
    //jsonp方式时此方法不会触发，因为不是用 XHR发送的请求
    },
    error: function(xhr){
    //jsonp 方式时此方法不会触发
    }
});
~~~ 
### 2、自定义回调函数的参数
~~~javascript
// 自己通过script标签或者iframe请求
// 接口根地址-此处是我本地的调试地址
var mockUrl = 'http://localhost:3000/mock/f90b66e99d580dc7b24e54a146b52317/test1'
var say = function(res) {
  console.log(res);
};
var script = document.createElement('script');
script.src = mockUrl + '/testSuccess?jsonp_callback_name=hello&hello=say';
document.body.appendChild(script);
// jquery jsonp请求
$.ajax({
    type : "get",
    async: true,
    url : mockUrl + '/testSuccess?jsonp_callback_name=hello', // 
    dataType : "jsonp",
    sonp: "hello",//（不必要）定义请求参数名（相当于表单的name），供后台获取参数使用，默认为"&callback=?"中的callback
    jsonpCallback: "say"//（不必要）自定义jsonp回调函数名称，默认为jQuery自动生成的随机函数名替代上述?号；在本例中后台应该返回showLists(jsonData)
});
~~~ 