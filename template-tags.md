# Template Tags\(模板标签\)

Haystack 自带了一些通用的模板标签，使用这些标签可以在模板中实现一些特殊功能。

## `highlight`（高亮显示）

使用文本块高亮显示文本块中的查询词。提供可选择的HTML标签来封装需要高亮显示的单词，可以使用css标签和最大长度标签。

HTML标签默认使用span，highlighted 高亮显示的CSS类，默认截取200个字符。

语法规则：

```
{% highlight <text_block> with <query> [css_class "class_name"] [html_tag "span"] [max_length 200] %}
```

示例：

```
# Highlight summary with default behavior.
{% highlight result.summary with query %}

# Highlight summary but wrap highlighted words with a div and the
# following CSS class.
{% highlight result.summary with query html_tag "div" css_class "highlight_me_please" %}

# Highlight summary but only show 40 words.
{% highlight result.summary with query max_length 40 %}
```

高亮标签可以根据需要重写。详见[http://django-haystack.readthedocs.io/en/v2.6.0/highlighting.html](http://django-haystack.readthedocs.io/en/v2.6.0/highlighting.html)

## `more_like_this`

从索引中获取类似的数据项，以查找与提供的检索内容相似的数据内容





