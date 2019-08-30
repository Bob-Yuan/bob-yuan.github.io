---
title: Django中JSON的使用
date: 2018-05-24 10:14:13
tags: []
categories: Django
---

## 一、后端向前端传JSON数据

使用python的json.dumps方法+js的JSON.parse方法进行前后端json数据的传递

**1、后端views.py中：**

    def login(request):
        Users = models.User.objects.all()
        username = []
        for i in Users:
            username.append(i.username)
        return render(request, "login.html",{'username': json.dumps(username)})

&emsp;&emsp;其中 username是我User表中的一个字段，json.dumps()是python中json库的一个函数，将python对象编码成json字符串。python的json库还有一个函数是json.loads()，将已编码的json字符串解码为python对象，在下面也会介绍。

&emsp;&emsp;然后，通过render将json格式的username传到前端。



**2、前端html中：**

    var usernames = "{{ username }}";
    var usernames_r = usernames.replace(/\&quot;/g, '\"');
    var usernames_r_p = JSON.parse(usernames_r);

&emsp;&emsp;js中对json的处理也有两个函数。JSON.parse() 方法用于将一个 JSON 字符串转换为对象。另一个方法JSON.stringify() 用于将 JavaScript 值转换为 JSON 字符串。

&emsp;&emsp;最终前端得到的usernames_r_p是个列表(原谅我取的变量名)，然后前端js就能使用列表中的数据啦！我们可以alert一下，看看usernames_r_p输出的是什么。


json可以转化的类型很多，这边只举了个list的例子，其他类型的还有(自己尝试)：

![](https://s2.ax1x.com/2019/08/30/mXr25D.png)
    

 

## 二、前端向后端传JSON数据

js的JSON.stringify()方法+python的json.loads()方法。实例：

**1、前端html中：**

    <script>
        function sub()
        {
            var a = document.getElementById("aa").value;
            var b = document.getElementById("bb").value;
            var str = {"a": a, "b": b};
            var str_json = JSON.stringify(str);
            document.getElementById("cc").value = str_json;
            return true;
        }
    </script>
 
    <input type="text" id="aa"/>
    <input type="text" id="bb"/>
    <form method="POST" οnsubmit="return sub()">
        {% csrf_token %}
        <input type="hidden" name="data" id="cc"/>
        <input type="submit" value="提交"/>
    </form>

**2、后端views.py中：**

    def login(request):
        if request.method == 'POST':
            print(request.POST['data'])
            result = json.loads(request.POST['data'])
            print(result['a'])
 