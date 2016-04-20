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
## 添加Haystack到`INSTALLED_APPS`

正如大多数Django的应用一样，你应该在你的设置文件(通常是`settings.py`)添加Haystack到`INSTALLED_APPS`.
示例：
```py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.sites',

    # Added.
    'haystack',

    # Then your usual apps...
    'blog',
]
```
## 修改你的`settings.py`
在你的`settings.py`中，你需要添加一个设置来指示站点配置文件正在使用的后端，以及其它的后端设置。
`HAYSTACK——CONNECTIONS`是必需的设置，并且应该至少是以下的一种：
### Solr
示例：
```py
HAYSTACK_CONNECTIONS = {
    'default': {
        'ENGINE': 'haystack.backends.solr_backend.SolrEngine',
        'URL': 'http://127.0.0.1:8983/solr'
        # ...or for multicore...
        # 'URL': 'http://127.0.0.1:8983/solr/mysite',
    },
}
```
### Elasticsearch
示例：
```py
HAYSTACK_CONNECTIONS = {
    'default': {
        'ENGINE': 'haystack.backends.elasticsearch_backend.ElasticsearchSearchEngine',
        'URL': 'http://127.0.0.1:9200/',
        'INDEX_NAME': 'haystack',
    },
}
```
### Whoosh
需要设置`PATH`到你的Whoosh索引的文件系统位置。
在你的网络服务器上保持关于权限的警告作为文档检索出来的应用。
示例：
```py
import os
HAYSTACK_CONNECTIONS = {
    'default': {
        'ENGINE': 'haystack.backends.whoosh_backend.WhooshEngine',
        'PATH': os.path.join(os.path.dirname(__file__), 'whoosh_index'),
    },
}
```
### Xapian
首先安装Xapian后端（http://github.com/notanumber/xapian-haystack/tree/master）
需要设置`PATH`到你的Xapian索引的文件系统位置。
在你的网络服务器上保持关于权限的警告作为文档检索出来的应用。
示例：
```py
import os
HAYSTACK_CONNECTIONS = {
    'default': {
        'ENGINE': 'xapian_backend.XapianEngine',
        'PATH': os.path.join(os.path.dirname(__file__), 'xapian_index'),
    },
}
```
### Simple
`simple`后台使用数据库本身基本的匹配。不建议在生成中使用，但他会返回结果。
```py
HAYSTACK_CONNECTIONS = {
    'default': {
        'ENGINE': 'haystack.backends.simple_backend.SimpleEngine',
    },
}
```

# 处理数据
## 创建`SearchIndexes`
`SearchIndexes`对象是Haystack决定那些数据应该放入索引和处理流数据的方式。你可以把它们看作是Django的`Models`或`Forms`，它们是基于字段和数据操作/存储的。

你通常为你期望索引的每一个`Model`都创建一个唯一的`SearchIndex`。虽然你可以在不同的model中重复使用相同的`SearchIndex`，只要你小心的做并且字段名很规范。

为了建立`SearchIndex`，所有的都是`indexes.SearchIndex`和`indexe.Indexable`的子类。定义要存储数据的字段，定义`get_model`方法。

我们会在下面创建和`Note`模型对应的`NoteIndex`。这个代码通常在`search_indexes.py`中。尽管这不是必须的。这使得Haystack能自动的检测到它。`NoteIndex`应该看起来像：

```py
import datetime
from haystack import indexes
from myapp.models import Note


class NoteIndex(indexes.SearchIndex, indexes.Indexable):
    text = indexes.CharField(document=True, use_template=True)
    author = indexes.CharField(model_attr='user')
    pub_date = indexes.DateTimeField(model_attr='pub_date')

    def get_model(self):
        return Note

    def index_queryset(self, using=None):
        """Used when the entire index for model is updated."""
        return self.get_model().objects.filter(pub_date__lte=datetime.datetime.now())
```
