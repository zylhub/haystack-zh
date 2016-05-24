# Views & Forms(视图和表单)
> 注释： 为了支持新的通用视图`haystack.generic_views.SearchView`，2.4版本移除了`haystack.views.SearchView`的支持。

Haystack自带一些默认django-style的、简单视图和表单帮助你开始以及覆盖共同的样例。包括提供如下：

+ 基本的只查询搜索
+ 模型搜索
+ 高亮搜索结果
+ 面搜索
+ 基本的高亮模型搜索结果

大多数处理过程通过`search`方法由haystack提供表单来完成。结果是，除了面类型的是使用标准的`SearchView`.

在forms和views之间只有非常少的耦合(其它一些依赖与存在的`search`方法)，所以你能使用forms或者views在你代码的任何地方来整合。

## Forms(表单)
`SearchForm`

这是最基本的表单类型，这个表单由一个字段组成，`q`(表示查询)。一旦搜索，这个表单将得到干净`q`字段中的内容，并且表现的像`auto_query`在你自定义的`SearchQuerySet '或者默认`SearchQuerySet`一样。

为了自定义`SearchQuerySet`，在你使用的`SearchQuerySet`中传入一个你要使用的`searchqueryset`参数。如果使用这个表单来关联一个`SearchView`，这个表单会在不需要任何额外工作的情况下，接受你提供给`SearchQuerySet`任何东西到视图。

这个`SearchForm`同时接受一个`load_all`参数(`True` or `False`),


### Creating Your Own Form(创建你自己的表单)

## Views(视图)

