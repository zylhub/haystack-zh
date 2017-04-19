# 架构预览

## `SearchQuerySet(搜索查询集)`

各个后端通用实现的功能：

* 标准的查询集（QuerySet）API
* 处理大多数查询
* 允许自定义“解析”API 或构建API

* 处理实际查到SearchQuery的调度

* 句柄自动创建查询

* 允许将原始查询直接传递到后端



## `SearchQuery`

需要每个后端单独实现

* 从结构化数据构建查询的方法
* 清理后端使用的保留字符串的方法

通用实现的功能：

* 为搜索添加过滤器\(filters\)/模型\(models\)/排序\(order-by\)/权重提升\(boost\)/限制数量\(limits\)的方法
* 执行原始搜索的方法

* 获取命中次数的方法

* 返回由后端提供的结果的方法（可能不是完整列表）



## `SearchBackend`

需要每个后端单独实现

* 连接搜索引擎
* 文档索引保存
* 文档索引移除
* 处理实际查询

## `SearchSite`

通用实现

* 从django.contrb.admin.sites.AdminSite 实现的标准API
* 处理每个站点的搜索模型的注册\(registering\)和注销\(unregistering\)
* 提供了一种向模型添加自定义索引的方法,比如ModelAdmins



## `SearchIndex`

为每个模型实现你所期望的模型

* 处理要生成索引的文档
* 填充文档附带的其他字段

* 提供一种限制哪些类型的对象被索引的方式

* 提供索引文档的方式

* 提供删除文档的方法



