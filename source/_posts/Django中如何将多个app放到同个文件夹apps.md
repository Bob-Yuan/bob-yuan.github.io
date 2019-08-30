---
title: Django中如何将多个app放到同个文件夹apps
date: 2019-06-10 00:30:00
tags: []
categories: Django
---

使用pytcharm新建apps文件夹，将之前的app目录全部拖入apps文件夹中，这时，如果我们import其他文件，pycharm就会用红色下划
波浪线提示报错

    from message import views

我们可以通过右键文件夹——>Mark Directory as——>Sources Root，这样pycharm就知道可以去标记的文件夹找我们要import的模块
，就不会报错了。

但我们在终端运行python manage.py runserver 8080时,仍然会报模块不存在的错误

    ImportError: No module named message

这是因为在pycharm标记Sources Root，但python解释器还是不知道源码路径，此时要在settings中设置app的路径：

    BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__))) #在后面添加：
    sys.path.insert(0,os.path.join(BASE_DIR,'apps'))

注意：此时的from message import views必须写在设置路径语句之后。因此在慕课网教程《Python2.7到3.6完美升级强力django+杀手级xadmin》中，在settings.py中设置了app的路径之后，将from message import views写在了manage.py中时用cmd运行仍然会报错。因为manage.py这个文件是在settings.py之前执行的，也就是settings中设置的app路径还没生效。


还有记得要注册app哦:)