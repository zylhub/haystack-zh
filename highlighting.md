# Highlighting

Haystack 提供两种高亮方式。你可以使用`SearchQuerySet.highlight`和内建`{% highlight %}`标签。每一种方法都有自己的优缺点，具体使用哪一种取决于你的需求。

如果你希望便捷、灵活、快速实现代码` {% highlight %}` 是你的选择。如果你更加关注加载速度并且只使用一个后端的话，`SearchQuerySet.highlight是你很好的一个选择`



