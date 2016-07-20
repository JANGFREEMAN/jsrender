#JsRender基本简介
就我目前所了解的，JsRender是基于Jquery的一个前端页面渲染插件。可以做到前后端口分离，前端通过ajax请求后台数据，后台拿到响应请求，返回Json格式的数据，然后前端通过
JsRender插件对json数据进行渲染达到前后端分离（后台只管传Json数据，前端Jsrender渲染数据。），而不再是通过JSP渲染页面。

#JsRender模板
JsRender把需要渲染的页面放在一个区域块状中，称之为模板，类似这种：
``` javascript
    <script id="personTmpl" type="text/x-jsrender">
     First name: {{:person.firstName()}} <br/>
     Last name: {{:person.lastName()}} <br/>
     Full name: {{:person.fullName()}}
    </script>
```
#JsRender标签语法
JsRender标签必须是这种格式：{{...}}，如果你想改成这种格式<%...%>，你可以这么修改```$.views.settings.delimiters("<%", "%>")```。

#JsRender常用标签
JsRender标签其实和JSP标签有很多类型的地方，下面介绍几种标签的使用用于解决项目的问题。

####{{: ...}}标签使用:
{{: pathOrExpr}}用于输出值，[例子](./demo/demo1.html)

```html
    var data = {address: {street: "Main Street"} };
    var tmpl = $.templates("<b>Street:</b> {{:address.street}}");
    var html = tmpl.render(data);
    // result: "<b>Street:</b> Main Street"
```

{{> pathOrExpr}}用于输出编码之后html，[例子](./demo/demo2.html)。

```html
    var data = {html:"<div> this is a div </div>" };
    var tmpl = $.templates("<b>html:</b> {{:html}}");
    var html = tmpl.render(data);
    // result: "<b>html:</b>&lt;div&gt;this is a div&lt;/div&gt;"
```

遍历数组，当被渲染的数据是一个数组类型的时候，调用render(data)函数的时候会遍历数组中的每一个对象，[例子](./demo/demo3.html)

```html
    var data = {{username:"zhangsan",age:"22"},{username:"wangwu",age:"23"}};
    var tmpl = $.templates("<b>user:</b> {{:username}} : {{:age}}");
    var html = tmpl.render(data);
    // result: "<b>user:</b>zhagnsan:22 <b>user:</b>wangwu:23"
```

{{for ...}}标签使用，用于遍历数组，[例子](./demo/demo4.html)

{{props ...}}标签使用 用于遍历一个json的key和value，[例子](./demo/demo5.html)

{{if ...}},{{else ...}} 标签使用，用于条件判断，(没有{{else if ...}} 标签)，[例子](./demo/demo6.html)

#JsRender自定义标签
除了使用JsRender定义好的内置标签我们还可以自定义标签如
```html
    $.views.tags(tagname,function(val){
        ....
    });
```
使用自定义标签可能会有两种情况一种是标签内不含有任何内容（{{mytag val/}}）,一种是标签是内含有内容（{{mytag val}} html....... {{/mytab}}）。[标签里不含内容的例子](./demo/demo7.html)，[标签里含有内容例子](./demo/demo8.html)或[标签里含有内容例子](./demo/demo9.html)

#JsRender之helper使用
$.views.helpers()，可以定义一些方法，在渲染的时候使用，定义的方法一般是与{{: ...}}和{{> ...}}标签一起使用，一般是对数据处理并输出。 [例子](./demo/demo10.html)

#JsRender之converters使用
在JsRender中，还可以定义转换器，其作用是对数据进行格式化和数据处理（个人感觉转换器的功能和helper的功能差不多），JsRender已内置了几种转换器，例如：{{html:xxx}}进行html进行编码、{{url:xxx}}对url编码、 {{daymonth:xxx}}对日期进行格式化。
也可以自定义转换器```$.views.converters(name, converterFn)```。[例子](./demo/demo11.html),[例子](./demo/demo12.html)
 






