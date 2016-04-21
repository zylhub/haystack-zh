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

这个`SearchForm`同时接受一个`load_all`参数(`True` or `False`),它决定了结果迭代中数据库怎样查询。这也自动从`SearchView`中接收。

Haystack中所有其它表单都继承于这里。

 `HighlightedSearchForm`

同`SearchView`一样除了它的`SearchQuerySet`结果中启用高亮结果`highlight`标签方法

`ModelSearchForm`

这个form添加新字段到form中。它迭代处理所有的注册models到当前的`SearchSite`并且为每一个创建了一个复选框。如果没有models被选中，将显示所有的结果。

`FacetedSearchForm`

 同`SearchForm`一样除了它添加了一个隐藏字段`selected_facets`，允许form基于用户选择面缩小检索结果。
 


### Creating Your Own Form(创建你自己的表单)

最简单的方法是从`SearchForm`中继承下来，并扩展`search`方法。如果这样做的话，你将节约大量的数据处理时间同时保持API对`SeachView`的兼容性。

例如，例如你为用户提供一个用户可选时间范围的搜索。你可能会创建一个如下的form

```py
from django import forms
from haystack.forms import SearchForm


class DateRangeSearchForm(SearchForm):
    start_date = forms.DateField(required=False)
    end_date = forms.DateField(required=False)

    def search(self):
        # First, store the SearchQuerySet received from other processing.
        sqs = super(DateRangeSearchForm, self).search()

        if not self.is_valid():
            return self.no_query_found()

        # Check to see if a start_date was chosen.
        if self.cleaned_data['start_date']:
            sqs = sqs.filter(pub_date__gte=self.cleaned_data['start_date'])

        # Check to see if an end_date was chosen.
        if self.cleaned_data['end_date']:
            sqs = sqs.filter(pub_date__lte=self.cleaned_data['end_date'])

        return sqs
```

这个表单添加了2个可选开始时间和结束时间的字段。在`search`方法中，我们
抓取父表单处理的结果。如果用户选择了`start/end`时间，我们会应用过滤器。最后，我们简单的返回了查询集`SearchrQuerySet`

## Views(视图)

`haystack.generic_views.SearchView`视图继承于Django标准的[FormView](https://docs.djangoproject.com/en/1.7/ref/class-based-views/generic-editing/#formview).示例视图能够使用任何基于django类视图修饰自定义，对于这个例子 在`get_queryset`中过滤检索结果。

```py
# views.py
from datetime import date

from haystack.generic_views import SearchView

class MySearchView(SearchView):
    """My custom search view."""

    def get_queryset(self):
        queryset = super(MySearchView, self).get_queryset()
        # further filter queryset based on some set of criteria
        return queryset.filter(pub_date__gte=date(2015, 1, 1))

    def get_context_data(self, *args, **kwargs):
        context = super(MySearchView, self).get_context_data(*args, **kwargs)
        # do something
        return context

# urls.py

urlpatterns = patterns('',
    url(r'^/search/?$', MySearchView.as_view(), name='search_view'),
)
```

## Upgrading
