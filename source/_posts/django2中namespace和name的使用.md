---
title: django2中namespace和name的使用
date: 2019-08-27 14:44:24
tags: [a,b,c]
categories: python
---
#### 一、在Django <= 1.11 我们通过关键词namespace参数定义名称空间

**1、projects/urls.py**

    from django.conf.urls import url,include
 
    urlpatterns = [
        url(r'blog/', include('blog.urls', namespace='blog')) 
    ]

**2、apps/urls.py**

    from django.conf.urls import url
    from . import views
 
    urlpatterns = [
        url(r'^login/$', views.login, name="login"),
    ]
 

#### 二、在Django 2.0+ 我们可以省略namespace，把namespace定义到被include的urls.py中去，使用app_name定义名称空间。

**1、projects/urls.py**

    urlpatterns = [
        path(r'^blog/', include('blog.urls')) 
    ]

**2、apps/urls.py**

    from django.urls import path
    from . import views
 
    app_name = 'blog'
 
    urlpatterns = [
        path('login/', views.login, name="login"),
    ]
 

#### 三、在templates中我们还是像原来一样的使用方法

    {% url 'blog:login' %}
