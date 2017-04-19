# Installing Search Engines

## Solr

官方下载地址：[http://www.apache.org/dyn/closer.cgi/lucene/solr/](http://www.apache.org/dyn/closer.cgi/lucene/solr/)

Solr基于Java，只需要少数几个JRE和Jetty包。它的性能非常好，具有高级的功能集。虽然可以通过一点努力让它在Solr 1.4上工作，但是Haystack建议使用Solr3.5+。 安装比较简单：

```
curl -LO https://archive.apache.org/dist/lucene/solr/4.10.2/solr-4.10.2.tgz
tar xvzf solr-4.10.2.tgz
cd solr-4.10.2
cd example
java -jar start.jar
```

你需要修正你的schema。你可以从你的应用中运行./manage.py build\_solr\_schema. 把这个输出放到 solr-4.10.2/example/solr/collection1/conf/schema.xml 中，然后重启Solr

> note： `build_solr_schema使用 template来生成schema.xml.`Haystack使用一些明智的默认设置提供了默认模板。如果你想提供你自己的模板，你需要把它放在search\_configuration/solr.xml。在您的应用程序指定的目录中TEMPLATE\_DIRS。例如：
>
> ```
> /myproj/myapp/templates/search_configuration/solr.xml
> # ...or...
> /myproj/templates/search_configuration/solr.xml
> ```

你需要pysolr。通过pypi可以下载，最佳版本是使用（2.1.0+）。将pysolr.py放到你的PYTHONPATH

> note: pysolr有自己的依赖包，这并不在Haystack讨论的范围。请查阅：https://pypi.python.org/pypi/pysolr

### More Like This

为了在Haystack中启用“More Like This”功能，你需要在启用MoreLikeThisHandler。将下面的一行配置加入到你的solrconfig.xml的`config标签中。`

```
<requestHandler name="/mlt" class="solr.MoreLikeThisHandler" />
```

### Spelling Suggestions（拼写建议）

要在Haystack中启用拼写建议功能，您需要启用SpellCheckComponent



