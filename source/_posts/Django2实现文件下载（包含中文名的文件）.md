---
title: Django2实现文件下载（包含中文名的文件）
date: 2019-08-16 00:56:45
tags: []
categories: Django
---

**前端html：**

    <a href="/download/">下载</a>

**url.py:**
    
    urlpatterns = [
        path('download/', download)
    ]

**views.py:**

    from django.http import FileResponse

    def download(request):
        file = open('./static/download/简历.docx', 'rb')
        response = FileResponse(file)
        response['Content-Type'] = 'application/vnd.openxmlformats-officedocument.wordprocessingml.document'
        response['Content-Disposition'] = 'attachment;filename= '+'简历'.encode('utf-8').decode('ISO-8859-1')+'.docx'
        return response

来解释一下download函数：

1、首先是打开存放文件的目录，‘.’表示根目录，也就是你整个django项目的根目录。我的“简历.docx”文件就放在djangoProjectName/static/download/目录下。

2、docx使用的MIME类型是 **application/vnd.openxmlformats-officedocument.wordprocessingml.document**。

3、因为文件名中有中文，如果直接在Content-Disposition中包含中文会导致文件名无法正常显示，因此要以 **'简历'.encode('utf-8').decode('ISO-8859-1')** 的形式进行转码。

 

网上搜到常用的有三种方法实现文件下载，我这边就试了这一种，其他你可以自己去搜一下然后试试。
