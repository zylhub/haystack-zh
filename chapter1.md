# haystack 入门教程

搜索是一个日益重要的话题。用户越来越依赖于搜索从噪声信息中分离和快速找到有用信息。此外，搜索搜索可以洞察那些东西是受欢迎的，改善网站上难以查找的东西。

为此，Haystack试图整合自定义搜索，使其尽可能简单的灵活和强大到足以处理更高级的用例。
Haystack是一个可重复的应用(也就是说，它值依赖与自身的代码，并只专注于搜索)，它很好地扮演着您控制的应用程序和第三方应用的角色（比如，`django.contrib.*`我们不用修改它的源代码） 。

Haystack也是一个可插拔的后端（很像Django的数据库层），所以几乎你所有写的代码都可以在不同搜索引擎之间便捷切换。

这个教程假设你了解Django各主要部分（`models/forms/views/settings/URLconfs`）和典型案例使用。还有的更高级设置可用的快捷方式，以及钩，但是这些不会在这里覆盖。
作为示例的目的，我们会在简单的note应用上添加搜索功能。在`myapp/models.py`中：
```py
from django.db import models
from django.contrib.auth.models import User


class Note(models.Model):
    user = models.ForeignKey(User)
    pub_date = models.DateTimeField()
    title = models.CharField(max_length=200)
    body = models.TextField()

    def __unicode__(self):
        return self.title
```
最后，在使用Haystack之前，你需要从选择一个搜索后端开始。这里有一个[安装搜索引擎](http://django-haystack.readthedocs.org/en/v2.4.1/installing_search_engines.html)的快速指南，稍后你会看到每一个引擎的官方说明。

# 安装
使用你最喜欢的Python包管理来从`PyPI`安装应用，例如。

    pip install django-haystack
    
# 配置
添加Haystack到`INSTALLED_APPS`
