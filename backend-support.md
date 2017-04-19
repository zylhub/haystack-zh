# Backend Support\(后端支持\)

## 支持的后端：

* [Solr](http://lucene.apache.org/solr/ "Solr")
* [Elashticsearch\_](http://django-haystack.readthedocs.io/en/v2.6.0/backend_support.html#id7)
* [Whoosh](https://bitbucket.org/mchaput/whoosh/)
* [Xapian](http://xapian.org/)

## 后端功能

### Solr

已经完成并包含在Haystack的功能：

* 完整的SearchQuerySet支持

* 自动查询建立

* “More Like This”功能

* 数据项权重提升

* 面搜索

* 存储（非索引）字段

* 高亮显示

* 位置搜索

* 要求：pysolr\(2.0.13+\) & Solr3.5+

### Elasticsearch

已经完成并包含在Haystack的功能：

* 完整的SearchQuerySet支持

* 自动查询建立

* “More Like This”功能

* 数据项权重提升

* 面搜索

* 存储（非索引）字段

* 高亮显示

* 位置搜索

* 要求：[elasticsarch1](https://pypi.python.org/pypi/elasticsearch).x 或2.x。目前不支持Elasticsearch5.X：请参阅[\#1383](https://github.com/django-haystack/django-haystack/issues/1383)

### Whoosh

已经完成并包含在Haystack的功能：

* 完整的SearchQuerySet支持

* 自动查询建立

* “More Like This”功能

* 数据项权重提升

* 存储（非索引）字段

* 高亮显示

* 要求：whoosh\(2.0.0+\)

### Xapian

已经完成并包含在Haystack的功能：

* 完整的SearchQuerySet支持

* 自动查询建立

* “More Like This”功能

* 数据项权重提升

* 面搜索

* 存储（非索引）字段

* 高亮显示

* 要求：Xapian1.0。5+ &python-xapian1.0.5+

* 后端下载地址：[xapian-haystack](http://github.com/notanumber/xapian-haystack/)



## Backend Support MatrixBackend

| SearchQuerySet Support | Auto Query Building | More Like This | Term Boost | Faceting | Stored Fields | Highlighting | Spatial |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |


| Solr | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |


| Elasticsearch | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |


| Whoosh | Yes | Yes | Yes | Yes | No | Yes | Yes | No |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |


| Xapian | Yes | Yes | Yes | Yes | Yes | Yes | Yes \(plugin\) | No |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |


## Unsupported Backends & Alternatives\(不支持的后端和替代品\)

如果你有一个搜索引擎，你希望得到Haystack的支持，当前推荐的方法是开发[xapian-haystack](https://pypi.python.org/pypi/xapian-haystack)插件，这样你的项目可以独立于Haystack核心进行开发和测试。

## Sphinx

这个后台多年来一直被要求多次，但还没有志愿者维护。如果您想要处理，请联系Haystack维护者，以便您的项目可以链接到这里，如果需要，可以添加到GitHub上的django-haystack组织。



