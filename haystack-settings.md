# Haystack Settings\(设置\)

如果你要扩展或者改变haystack的默认行为，你可以通过改变settings.py中的一些设置达到目的。这是Haystack设置的完整列表

## `HAYSTACK_DEFAULT_OPERATOR`

**Optional**

该设置控制SearchQuerySet链式查询的默认行为

合法的选项有：

```
HAYSTACK_DEFAULT_OPERATOR = 'AND'
HAYSTACK_DEFAULT_OPERATOR = 'OR'
```

默认是`AND`

## `HAYSTACK_CONNECTIONS`

要求：

此设置控制后端所提供的可用选项。它应该是类似于以下（完整）示例的嵌套字典

```
HAYSTACK_CONNECTIONS = {
    'default': {
        'ENGINE': 'haystack.backends.solr_backend.SolrEngine',
        'URL': 'http://localhost:9001/solr/default',
        'TIMEOUT': 60 * 5,
        'INCLUDE_SPELLING': True,
        'BATCH_SIZE': 100,
        'EXCLUDED_INDEXES': ['thirdpartyapp.search_indexes.BarIndex'],
    },
    'autocomplete': {
        'ENGINE': 'haystack.backends.whoosh_backend.WhooshEngine',
        'PATH': '/home/search/whoosh_index',
        'STORAGE': 'file',
        'POST_LIMIT': 128 * 1024 * 1024,
        'INCLUDE_SPELLING': True,
        'BATCH_SIZE': 100,
        'EXCLUDED_INDEXES': ['thirdpartyapp.search_indexes.BarIndex'],
    },
    'slave': {
        'ENGINE': 'xapian_backend.XapianEngine',
        'PATH': '/home/search/xapian_index',
        'INCLUDE_SPELLING': True,
        'BATCH_SIZE': 100,
        'EXCLUDED_INDEXES': ['thirdpartyapp.search_indexes.BarIndex'],
    },
    'db': {
        'ENGINE': 'haystack.backends.simple_backend.SimpleEngine',
        'EXCLUDED_INDEXES': ['thirdpartyapp.search_indexes.BarIndex'],
    }
}
```

此设置不提供默认设置

主键（default&friends）是您的应用程序的标识符。您可以使用它们作为方法或kwarg使用API​​的任何地方

这里至少有一个default键

ENGINE选项是所有后端都要指定的。



此外，每个后端都可能需要其他选项

* Solr
  * URL：The URL to the Solr core.

* Whoosh
  * `PATH`- The filesystem path to where the index data is located.

* Xapian
  * `PATH`- The filesystem path to where the index data is located.

以下选项是可选的：

`INCLUDE_SPELLING`

* - Include spelling suggestions. Default is`False`
* `BATCH_SIZE`- How many records should be updated at once via the management commands. Default is`1000`
* `TIMEOUT`- \(Solr and ElasticSearch\) How long to wait \(in seconds\) before the connection times out. Default is`10`
* `STORAGE`- \(Whoosh-only\) Which storage engine to use. Accepts`file`or`ram`. Default is`file`
* `POST_LIMIT`- \(Whoosh-only\) How large the file sizes can be. Default is`128*1024*1024`.
* `FLAGS`- \(Xapian-only\) A list of flags to use when querying the index.
* `EXCLUDED_INDEXES`- A list of strings \(as Python import paths\) to indexes you do**NOT**want included. Useful for omitting third-party things you don’t want indexed or for when you want to replace an index.
* `KWARGS`- \(Solr and ElasticSearch\) Any additional keyword arguments that should be passed on to the underlying client library.

## `HAYSTACK_ROUTERS`

**Optional**

此设置控制如何执行路由以允许不同的后端处理更新/删除/读取

示例：

```
HAYSTACK_ROUTERS = ['search_routers.MasterSlaveRouter', 'haystack.routers.DefaultRouter']
```

默认`['haystack.routers.DefaultRouter']`

## `HAYSTACK_SIGNAL_PROCESSOR`

**Optional**

此设置控制采用什么SignalProcessor类用于处理Django的信号，并保持搜索索引最新。

示例：

```
HAYSTACK_SIGNAL_PROCESSOR = 'haystack.signals.RealtimeSignalProcessor'
```

默认`'haystack.signals.BaseSignalProcessor'`

## `HAYSTACK_DOCUMENT_FIELD`

**Optional**

此设置控制Haystack的默认搜索字段

示例：

```
HAYSTACK_DOCUMENT_FIELD = 'wall_o_text'
```

默认是 `text`

## `HAYSTACK_SEARCH_RESULTS_PER_PAGE`

**Optional**

当使用`SearchView`以及它的子类的时候，此设置控制每页显示的检索结果数量

示例：

```
HAYSTACK_SEARCH_RESULTS_PER_PAGE = 50
```

默认是20

## `HAYSTACK_CUSTOM_HIGHLIGHTER`

**Optional**

此选项允许你在模板标签`{%hightlight%}`中使用自定义的

示例：

```
HAYSTACK_CUSTOM_HIGHLIGHTER = 'myapp.utils.BorkHighlighter'
```

没有默认值，Haystack直接使用默认实现

## `HAYSTACK_ITERATOR_LOAD_PER_QUERY`

**Optional**

此设置控制通过使用SearchQuerySet迭代一次获取到的结果数量。如果您一次通常消耗大量部件，您可以打破其设置，以获得更好的性能

示例：

```
HAYSTACK_ITERATOR_LOAD_PER_QUERY = 100
```

默认值是一次10个结果

## `HAYSTACK_LIMIT_TO_REGISTERED_MODELS`

**Optional**

此设置允许您控制Haystack是否会限制注册模型的搜索结果。这是布尔值。

如果你的索引不会在注册模型之外使用，那么你可以关闭并获得小到中等的性能提升

示例：

```
HAYSTACK_LIMIT_TO_REGISTERED_MODELS = False
```

默认值是`True`

## `HAYSTACK_ID_FIELD`

**Optional**

此设置允许您控制`Haystack`内部使用的唯一字段名称。很少需要，除非你的字段名与Haystack的默认值相冲突

示例：

```
HAYSTACK_ID_FIELD = 'my_id'
```

默认是`id`

## `HAYSTACK_DJANGO_CT_FIELD`

**Optional**

此设置允许您控制Haystack内部使用的内容类型字段名称。很少需要，除非你的字段名与Haystack的默认值相冲突。

