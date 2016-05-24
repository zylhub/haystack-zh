# SearchField API

## `class SearchField`

`SearchField`和它的子类提供一种方式来声明那些数据是你感兴趣的索引.它们通过`SearchIndexes`来使用,很像表单中的`forms.*Field`或者模型中的  `models.*Field`

它们都提供了在索引中存储数据的方式,以及数据放入索引前的预处理.Haystack使用`SearchIndex`类的所有字段来确定搜索引擎架构的样子.

在实践中,你可能不会真正使用基类`SearchField`,作为子类来处理真实数据要好得多.


### Subclasses

Haystack包括下面的字段类型:

+ `BooleanField`
+ `CharField`
+ `DateField`
+ `DateTimeField`
+ `DecimalField`
+ `EdgeNgramField`
+ `FloatField`
+ `IntegerField`
+ `LocationField`
+ `MultiValueField`
+ `NgramField`

等价的faceted版本是:

+ `FacetBooleanField`
+ `FacetCharField`
+ `FacetDateField`
+ `FacetDateTimeField`
+ `FacetDecimalField`
+ `FacetFloatField`
+ `FacetIntegerField`
+ `FacetMultiValueField`

### Usage(用法)

 虽然`SearchField`可以自行使用,但是它们一般在`SearchIndex`中使用.你在声明中使用它们,就像`django.forms.Form`或`django.db.models.Model`对象一样.例如:
 
 ```py
from haystack import indexes
from myapp.models import Note


class NoteIndex(indexes.SearchIndex, indexes.Indexable):
    text = indexes.CharField(document=True, use_template=True)
    author = indexes.CharField(model_attr='user')
    pub_date = indexes.DateTimeField(model_attr='pub_date')

    def get_model(self):
        return Note
 ```
这将字段和索引挂钩(hook up),当更新`Model`对象时,拉取相关数据并索引存储做准备做预处理.

### Field Options


