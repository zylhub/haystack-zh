# Management Commands\(管理命令\)

Haystack提供了几个易用的管理命令

## `clear_index`

clear\_index命令移除整个索引。请谨慎使用。除了标准的管理命令，还提供一些可选参数

    ``--noinput``:
        If provided, the interactive prompts are skipped and the index is
        uncerimoniously wiped out.
    ``--verbosity``:
        Accepted but ignored.
    ``--using``:
        If provided, determines which connection should be used. Default is
        ``default``.
    ``--nocommit``:
        If provided, it will pass commit=False to the backend.  This means that the
        update will not become immediately visible and will depend on another explicit commit
        or the backend's commit strategy to complete the update.

默认情况下，这是一个交互命令，并假定您不希望删除整个索引。

> Note: --nocommit 参数只在solr中支持

`update_index`

update\_index命令会根据内容更新你的索引。它会遍历所有索引的模型并更新索引中的记录。除了标准的管理命令选项，它还提供下面的参数选项：

    ``--age``:
        Number of hours back to consider objects new. Useful for nightly
        reindexes (``--age=24``). Requires ``SearchIndexes`` to implement
        the ``get_updated_field`` method. Default is ``None``.
    ``--start``:
        The start date for indexing within. Can be any dateutil-parsable string,
        recommended to be YYYY-MM-DDTHH:MM:SS. Requires ``SearchIndexes`` to
        implement the ``get_updated_field`` method. Default is ``None``.
    ``--end``:
        The end date for indexing within. Can be any dateutil-parsable string,
        recommended to be YYYY-MM-DDTHH:MM:SS. Requires ``SearchIndexes`` to
        implement the ``get_updated_field`` method. Default is ``None``.
    ``--batch-size``:
        Number of items to index at once. Default is 1000.
    ``--remove``:
        Remove objects from the index that are no longer present in the
        database.
    ``--workers``:
        Allows for the use multiple workers to parallelize indexing. Requires
        ``multiprocessing``.
    ``--verbosity``:
        If provided, dumps out more information about what's being done.

          * ``0`` = No output
          * ``1`` = Minimal output describing what models were indexed
            and how many records.
          * ``2`` = Full output, including everything from ``1`` plus output
            on each batch that is indexed, which is useful when debugging.
    ``--using``:
        If provided, determines which connection should be used. Default is
        ``default``.
    ``--nocommit``:
        If provided, it will pass commit=False to the backend.  This means that the
        updates will not become immediately visible and will depend on another explicit commit
        or the backend's commit strategy to complete the update.

> Note： --nocommit 参数只在Solr和Elasticsearch后端支持

示例：

```
# Update everything.
./manage.py update_index --settings=settings.prod

# Update everything with lots of information about what's going on.
./manage.py update_index --settings=settings.prod --verbosity=2

# Update everything, cleaning up after deleted models.
./manage.py update_index --remove --settings=settings.prod

# Update everything changed in the last 2 hours.
./manage.py update_index --age=2 --settings=settings.prod

# Update everything between Dec. 1, 2011 & Dec 31, 2011
./manage.py update_index --start='2011-12-01T00:00:00' --end='2011-12-31T23:59:59' --settings=settings.prod

# Update just a couple apps.
./manage.py update_index blog auth comments --settings=settings.prod

# Update just a single model (in a complex app).
./manage.py update_index auth.User --settings=settings.prod

# Crazy Go-Nuts University
./manage.py update_index events.Event media news.Story --start='2011-01-01T00:00:00 --remove --using=hotbackup --workers=12 --verbosity=2 --settings=settings.prod
```

## `rebuild_index`

重建索引操作：首先clearn_index 然后  update_\_index。 可以接受下面的可选参数：

    ``--age``:
        Number of hours back to consider objects new. Useful for nightly
        reindexes (``--age=24``). Requires ``SearchIndexes`` to implement
        the ``get_updated_field`` method.
    ``--batch-size``:
        Number of items to index at once. Default is 1000.
    ``--site``:
        The site object to use when reindexing (like `search_sites.mysite`).
    ``--noinput``:
        If provided, the interactive prompts are skipped and the index is
        uncerimoniously wiped out.
    ``--remove``:
        Remove objects from the index that are no longer present in the
        database.
    ``--verbosity``:
        If provided, dumps out more information about what's being done.

          * ``0`` = No output
          * ``1`` = Minimal output describing what models were indexed
            and how many records.
          * ``2`` = Full output, including everything from ``1`` plus output
            on each batch that is indexed, which is useful when debugging.
    ``--using``:
        If provided, determines which connection should be used. Default is
        ``default``.
    ``--nocommit``:
        If provided, it will pass commit=False to the backend.  This means that the
        update will not become immediately visible and will depend on another explicit commit
        or the backend's commit strategy to complete the update.

当你真的，真的想要一个完全重建的索引，这是你的选择。

## `build_solr_schema`

一旦你定义了SearchIndex类，这个命令会生成Solr需要的XML模式数据。接受如下参数：

    ``--filename``:
        If provided, directs output to a file instead of stdout.
    ``--using``:
        If provided, determines which connection should be used. Default is
        ``default``.

## `haystack_info`

提高Haystack的设置和模型的基本信息。



