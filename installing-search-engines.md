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
> ```py
> /myproj/myapp/templates/search_configuration/solr.xml
> # ...or...
> /myproj/templates/search_configuration/solr.xml
> ```

你需要pysolr。通过pypi可以下载，最佳版本是使用（2.1.0+）。将pysolr.py放到你的PYTHONPATH

> note: pysolr有自己的依赖包，这并不在Haystack讨论的范围。请查阅：[https://pypi.python.org/pypi/pysolr](https://pypi.python.org/pypi/pysolr)

### More Like This

为了在Haystack中启用“More Like This”功能，你需要在启用MoreLikeThisHandler。将下面的一行配置加入到你的solrconfig.xml的`config标签中。`

```
<requestHandler name="/mlt" class="solr.MoreLikeThisHandler" />
```

### Spelling Suggestions（拼写建议）

要在Haystack中启用拼写建议功能，您需要启用SpellCheckComponent

首先需要使用SearchIndex类创建一个特殊的text字段，这里使用FacetCharField。这会使Solr的后处理失效，这可能会弄乱您的建议。示例如下：

```py
class MySearchIndex(indexes.SearchIndex, indexes.Indexable):
    text = indexes.CharField(document=True, use_template=True)
    # ... normal fields then...
    suggestions = indexes.FacetCharField()

    def prepare(self, obj):
        prepared_data = super(MySearchIndex, self).prepare(obj)
        prepared_data['suggestions'] = prepared_data['text']
        return prepared_data
```

然后，您可以在Solr中添加以下行到`solrconfig.xml`中的`config`标签中

```
<searchComponent name="spellcheck" class="solr.SpellCheckComponent">

    <str name="queryAnalyzerFieldType">textSpell</str>

    <lst name="spellchecker">
      <str name="name">default</str>
      <str name="field">suggestions</str>
      <str name="spellcheckIndexDir">./spellchecker1</str>
      <str name="buildOnCommit">true</str>
    </lst>
</searchComponent>
```

然后更改您的默认处理程序

```
<requestHandler name="standard" class="solr.StandardRequestHandler" default="true" />
```

```
<requestHandler name="standard" class="solr.StandardRequestHandler" default="true">
    <arr name="last-components">
        <str>spellcheck</str>
    </arr>
</requestHandler>
```

需要注意的是&lt;strname="field"&gt;suggestions&lt;/str&gt;对应于SearchIndex中的类（本示例中，我们假设主字段是text）

## Elasticsearch

官方下载地址：[http://www.elasticsearch.org/download/](http://www.elasticsearch.org/download/)

Elasticsearch基于Java，除了JRE之外还需要少数几个包。它也非常优秀，容易扩展，并具有先进的功能集。Haystack当前只支持Elasticsearch1.x和2.x。5.x暂不支持。

最佳的安装方式是采用包管理工具：

```
# On Mac OS X...
brew install elasticsearch

# On Ubuntu...
apt-get install elasticsearch

# Then start via:
elasticsearch -f -D es.config=<path to YAML config>

# Example:
elasticsearch -f -D es.config=/usr/local/Cellar/elasticsearch/0.90.0/config/elasticsearch.yml

```

You may have to alter the configuration to run on`localhost`when developing locally. Modifications should be done in a YAML file, the stock one being`config/elasticsearch.yml`

```

# Unicast Discovery (disable multicast)
discovery.zen.ping.multicast.enabled: false
discovery.zen.ping.unicast.hosts: ["127.0.0.1"]

# Name your cluster here to whatever.
# My machine is called "Venus", so...
cluster:
  name: venus

network:
  host: 127.0.0.1

path:
  logs: /usr/local/var/log
  data: /usr/local/var/data
```

> [http://django-haystack.readthedocs.io/en/v2.6.0/installing\_search\_engines.html](http://django-haystack.readthedocs.io/en/v2.6.0/installing_search_engines.html)
>
> 未完待续....



