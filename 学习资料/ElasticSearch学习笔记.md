# ElasticSearch学习笔记

## Linux系统下安装ElasticSearch

### 1. 下载

下载地址：

https://www.elastic.co/cn/downloads/past-releases#elasticsearch

选择elasticsearch，选择版本号，点击下载即可

这里下载的是 Elasticsearch 7.17.4

下载地址是： https://www.elastic.co/cn/downloads/past-releases/elasticsearch-7-17-4

选择操作系统点击下载。我下载linux版本。

### 2. 在Linux服务器上安装

在windows上的安装比较简单，都是图形化操作，这里不再赘述。主要看下linux的安装过程

1. 添加用户
   
   添加一个用户es，密码设置成es，权限可以在后面再设置。解压完成之后。
   
   ```bash
   useradd es
   passwd es es
   chown es:es .
   ```

2. 解压
   
   ```bash
   tar -zxvf  elasticsearch-7.17.4-linux-x86_64.tar.gz
   ```

3. 配置jdk
   
   elasticsearch已经内置了jdk，所以我们不需要自己安装jdk了，只需要我们配置环境变量。
   
   ```bash
   vim /etc/profile
   ```
   
   最下方添加：
   
   export ES_JAVA_HOME='/home/elasticsearch/jdk'
   
   > '/home/elasticsearch/jdk'  这个是自己的elasticsearch的解压目录的jdk的路径。
   
   配置完成之后保存退出。
   
   ```bash
   source /etc/profile
   ```
   
   立即生效。当前目录是在elasticsearch的解压的根目录下。

4. 配置elasticsearch.yml
   
   我们用vim打开配置文件
   
   ```bash
   vim config/elasticsearch.yml
   ```
   
   打开配置：
   
   * discovery.seed_hosts: ["192.168.3.35"] 集群主机列表
     
     * 192.168.3.35 是本机的IP地址，可以是公网IP，也可以是域名
   
   * discover.seed_providers: 基于配置文件配置集群主机列表
   
   * cluster.initial_master_nodes: ["node-1"]  启动的时候初始化的参与选主的node，生产环境必填
     
     * 主节点的名字，和上面的名字一样，默认是这个名字。有一行注释的：
     
     * #node.name: node-1，# Use a descriptive name for the node:      没有设置默认的就是这个名字当然可以自己修改为自己想改的名字。
   
   $修改完成之后保存退出$

5. 配置jvm
   
   ```bash
    vim config/jvm.options
   ```

        主要修改了堆内存，默认是4g

            -Xms4g
            -Xmx4g

### 3. 启动运行

- 切换到es用户
  
  su es

- 启动
  
  bin/elasticsearch

- 后台启动
  
  bin/elasticsearch -d

- 启动完成之后访问地址:http://192.168.3.35:9200/
  
  看到:
  
  ```json
  {
    "name" : "localhost.localdomain",
    "cluster_name" : "elasticsearch",
    "cluster_uuid" : "_na_",
    "version" : {
      "number" : "7.17.4",
      "build_flavor" : "default",
      "build_type" : "tar",
      "build_hash" : "79878662c54c886ae89206c685d9f1051a9d6411",
      "build_date" : "2022-05-18T18:04:20.964345128Z",
      "build_snapshot" : false,
      "lucene_version" : "8.11.1",
      "minimum_wire_compatibility_version" : "6.8.0",
      "minimum_index_compatibility_version" : "6.0.0-beta1"
    },
    "tagline" : "You Know, for Search"
  }
  ```
  
  说明启动成功

### 4.  启动常见问题

- **使用root用户启动报错的问题**

```tex
[ERROR][o.e.b.ElasticsearchUncaughtExceptionHandler] [localhost.localdomain] uncaught exception in thread [main]
org.elasticsearch.bootstrap.StartupException: java.lang.RuntimeException: can not run elasticsearch as root
    at org.elasticsearch.bootstrap.Elasticsearch.init(Elasticsearch.java:170) ~[elasticsearch-7.17.4.jar:7.17.4]
    at org.elasticsearch.bootstrap.Elasticsearch.execute(Elasticsearch.java:157) ~[elasticsearch-7.17.4.jar:7.17.4]
    at org.elasticsearch.cli.EnvironmentAwareCommand.execute(EnvironmentAwareCommand.java:77) ~[elasticsearch-7.17.4.jar:7.17.4]
    at org.elasticsearch.cli.Command.mainWithoutErrorHandling(Command.java:112) ~[elasticsearch-cli-7.17.4.jar:7.17.4]
    at org.elasticsearch.cli.Command.main(Command.java:77) ~[elasticsearch-cli-7.17.4.jar:7.17.4]
    at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:122) ~[elasticsearch-7.17.4.jar:7.17.4]
    at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:80) ~[elasticsearch-7.17.4.jar:7.17.4]
Caused by: java.lang.RuntimeException: can not run elasticsearch as root
    at org.elasticsearch.bootstrap.Bootstrap.initializeNatives(Bootstrap.java:107) ~[elasticsearch-7.17.4.jar:7.17.4]
    at org.elasticsearch.bootstrap.Bootstrap.setup(Bootstrap.java:183) ~[elasticsearch-7.17.4.jar:7.17.4]
    at org.elasticsearch.bootstrap.Bootstrap.init(Bootstrap.java:434) ~[elasticsearch-7.17.4.jar:7.17.4]
    at org.elasticsearch.bootstrap.Elasticsearch.init(Elasticsearch.java:166) ~[elasticsearch-7.17.4.jar:7.17.4]
    ... 6 more
uncaught exception in thread [main]
java.lang.RuntimeException: can not run elasticsearch as root
    at org.elasticsearch.bootstrap.Bootstrap.initializeNatives(Bootstrap.java:107)
    at org.elasticsearch.bootstrap.Bootstrap.setup(Bootstrap.java:183)
    at org.elasticsearch.bootstrap.Bootstrap.init(Bootstrap.java:434)
    at org.elasticsearch.bootstrap.Elasticsearch.init(Elasticsearch.java:166)
    at org.elasticsearch.bootstrap.Elasticsearch.execute(Elasticsearch.java:157)
    at org.elasticsearch.cli.EnvironmentAwareCommand.execute(EnvironmentAwareCommand.java:77)
    at org.elasticsearch.cli.Command.mainWithoutErrorHandling(Command.java:112)
    at org.elasticsearch.cli.Command.main(Command.java:77)
    at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:122)
    at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:80)
For complete error details, refer to the log at /home/elasticsearch/logs/elasticsearch.log
```

> 这个错误提示比较明确，不能使用root启动。
> 
> 解决方式:  
> 
> 1. 使用非root用户启动即可
> 
> 2. 启动时加临时参数
>    
>    ./elasticsearch -Des.insecure.allow.root=true
> 
> 3. 或者 用vi打开elasicsearch执行文件，在变量ES_JAVA_OPTS使用前添加以下命令
>    
>    ES_JAVA_OPTS="-Des.insecure.allow.root=true"

- **ES因为需要大量的创建索引文件，需要大量的打开系统的文件，所以我们需要解除linux系统当中打开文件最大数目的限制，不然ES启动抛错**

```tex
ERROR: [3] bootstrap checks failed. You must address the points described in the following [3] lines before starting Elasticsearch.
bootstrap check failure [1] of [3]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65535]
bootstrap check failure [2] of [3]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
bootstrap check failure [3] of [3]: the default discovery settings are unsuitable for production use; at least one of [discovery.seed_hosts, discovery.seed_providers, cluster.initial_master_nodes] must be configured
ERROR: Elasticsearch did not exit normally - check the logs at /home/elasticsearch/logs/elasticsearch.log
[2022-06-07T17:38:01,231][INFO ][o.e.n.Node               ] [localhost.localdomain] stopping ...
[2022-06-07T17:38:01,253][INFO ][o.e.n.Node               ] [localhost.localdomain] stopped
[2022-06-07T17:38:01,254][INFO ][o.e.n.Node               ] [localhost.localdomain] closing ...
[2022-06-07T17:38:01,268][INFO ][o.e.n.Node               ] [localhost.localdomain] closed
[es@localhost elasticsearch]$ cd logs/
```

  *解决方式:*

> \# 切换到root用户
> 
> ```tex
> vim /etc/security/limits.conf
> 在末尾添加
> * soft nofile 65536
> * hard nofile 65535
> * soft nproc 4096
> * hard nproc 4096
> ```

- **max number of threads[1024]  for user[es] is too low, increase to at least[4096] 无法创建本地线程问题，用户最大可创建线程数太小**

 *解决方式：*

> ```tex
> vim /etc/security/limits.d/20-nproc.conf
> 修改如下配置
> *          soft    nproc     4096
> ```

- **max virtual memory areas vm.max_map_count[65530] is too low,increase to at least[262144] 最大虚拟内存太小，调大系统的虚拟内存**
  
  *解决方式：*

> ```tex
> vim /etc/sysctl.conf
> 追加如下内容
> vm.max_map_count=262144
> 保存退出之后执行如下命令
> sysctl -p
> ```

**启动报错# error updating geoip database**

*解决方案*

在elasticsearch.yml中添加如下配置：

```bash
ingest.geoip.downloader.enabled: false
```

重新启动就没有问题了

## 安装客户端Kibana

### 介绍

> Kibana是一个开源分析和可视化平台，旨在与Elasticsearch协同工作

### 下载并解压Kibana

#### 下载

下载地址： https://www.elastic.co/cn/downloads/past-releases#kibana 

![](C:\Users\20220509\AppData\Roaming\marktext\images\2022-06-09-10-39-48-image.png)

版本和Elasticsearch一致

#### 解压

```bash
tar -zxvf kibana-7.17.4-linux-x86_64.tar.g
```

### 配置

-  修改kibana.yml配置文件
  
  ```bash
  vim kibana-7.17.4-linux-x86_64/config/kibana.yml
  #kibana 服务器端口号
  server.port: 5601
  #服务器ip
  server.host: "0.0.0.0"
  #elasticsearch的访问地址
  elasticsearch.hosts:["http://192.168.3.35:9200"]
  #kibana汉化
  i18n.locale:"zh-CN"
  ```

### 运行kibana

- 前台启动（可以看到启动日志，关闭命令行窗口服务停止）
  
  ```bash
  bin/kibana
  ```

- 后台运行
  
  ```bash
  nohup bin/kibana &
  ```

- 访问kibana http://192.168.3.35:5601/

![](C:\Users\20220509\AppData\Roaming\marktext\images\2022-06-09-16-46-45-image.png)

## 安装Head插件

#### 安装依赖

1. 安装node
   
   #### 下载
   
   wget https://nodejs.org/dist/v14.15.4/node-v14.15.4-linux-x64.tar.xz
   
   或者
   
   使用浏览器下载，再传到服务器
   
   #### 解压
   
   tar -xf node-v14.15.4-linux-x64.tar.xz
   
   #### 创建软连接
   
   cd /usr/bin 
   
   ln -s /usr/local/node/bin/node node
   
   ln -s /usr/local/node/bin/npm npm
   
   看node版本号：node -v

2. 安装grunt:
   
   npm install -g grunt-cli
   
   创建软链：ln -s /usr/local/node/bin/grunt grunt
   
   输入：grunt -version命令检查是否安装成功

#### 安装elasticsearch-head

1. [GitHub - mobz/elasticsearch-head: A web front end for an elastic search cluster](https://github.com/mobz/elasticsearch-head)

2. 解压

3. 修改配置文件Gruntfile.js文件，添加hostname:"*"

4. 安装： cd elasticsearch-head 
   
   npm install

5. npm run start启动服务

6. 访问地址：http://ip:9100

## Elasticsearch安装插件

两种方式，一种是在线安装，一种是下载安装。

**注意：安装和删除插件后重启Elasticsearch才生效**

> 查看当前ES安装的插件
> 
> ```bash
> 进入到es的bin目录，执行如下命令
> elasticsearch-plugin list
> ```

或者是打开目录plugins查看

#### 1. 在线安装方式

- 在bin目录下，执行如下命令
  
  ```bash
  ./elasticsearch-plugin install analysis-icu
  ```

- 

#### 2.离线安装

解压插件到plugins目录 

```bash
unzip elasticsearch-analysis-ik-8.2.0.zip -d ./ik-analysis
```

删除压缩包，重启es

## 倒排索引压缩算法

![](C:\Users\20220509\AppData\Roaming\marktext\images\2022-07-07-16-54-27-image.png)

![](C:\Users\20220509\AppData\Roaming\marktext\images\2022-07-07-17-26-55-image.png)

![](C:\Users\20220509\AppData\Roaming\marktext\images\2022-07-07-17-28-10-image.png)

## Elasticsearch索引

在线文档地址：[Elasticsearch Guide [7.17] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/index.html)

> 索引命名必须小写，不能以下划线开头

### 索引（Index）

>         一个索引就是一个拥有几分相似特征的文档的集合。比如说，可以有一个客户数据的索引，有一个产品目录索引，还有一个订单数据的索引。
> 
>         一个索引由一个名字来标识（必须全是小写字母的），并且当我们要对对应于这个索引中的文档进行索引、搜索、更新和删除的时候，都要使用到这个名字。

### 文档（Document）

- Elasticsearch是面向文档的，文档是所有可搜索数据的最小单位
  
  - 日志文件中的日志项
  
  - 一部电影的具体信息/一张唱片的详细信息
  
  - MP3播放器里的一首歌/一篇PDF文档中的具体内容

- 文档会被序列化成JSON格式，保存在Elasticsearch中
  
  - JSON对象由字段组成
  
  - 每个字段都有对应的字段类型（字符串/布尔/日期/二进制/范围类型）

- 每个文档都有一个Unique ID
  
  - 可以自己指定ID或者通过Elasticsearch自动生成

- 一篇文档包含了一系列字段，类似数据库表中的一条记录

- JSON文档，格式灵活，不需要预先定义格式
  
  - 字段的类型可以指定或者通过Elasticsearch自动推算
  
  - 支持数组/支持嵌套

#### 文档元数据

元数据，用于标注文档的相关信息：

- _index: 文档所属的索引名

- _type: 文档所属的类型名

- _id: 文档唯一Id

- _source: 文档的原始Json数据

- version: 文档的版本号，修改删除操作version都会自增1

- seq_no：和version一样，一旦数据发生更改，数据也一直是累计的。Shard级别严格递增，保证后写入的Doc的seq_no大于先写入的Doc的seq_no。

- primary_term: primary_term主要是用来回复数据时处理当多个文档的seq_no一样时的冲突，避免Primary Shared上的写入被覆盖。每当Primary Shard发生重新分配时，比如重启，Primary选举等，_primary_term会递增1。

#### 并发场景下修改文档

seq_no和primary_term是对version的优化，7.x版本的ES默认使用这种方式控制版本，所以当在高并发环境下使用乐观锁机制修改文档时，要带上当前文档的seq_no和primary_term进行更新：

```json
POST /es_db/_doc/2?if_seq_no=2&if_primary_term=6
{
    "name": "李四xxx"
}
```

如果版本号不一样就会修改失败

#### 核心配置

- network.host 即提供服务的ip地址，一般配置为本节点所在服务器的内网地址，此配置会导致节点由开发模式转为生产模式，从而触发引导检查。

- network.publish_host: 即提供服务器的IP地址，一般配置为本节点所在服务器的公网地址。

- http.port: 服务端口号，默认9200，通常范围9200--9299

- transport.port: 节点通信端口，默认9300，通常范围9300——9399

- discovery.seed_hosts: 此设置提供集群中其他候选节点的列表，并且可能处于活动状态且可以联系以播种发现过程。每个地址可以是IP地址，也可以是通过DNS解析为一个或多个IP地址的主机名。

- cluster.initial_master_nodes:指定集群初次选举中用到的候选节点，称为集群引导，只在第一次形成集群时需要，如果配置了network.host，则此配置必须配置。重新启动节点或将新节点添加到现有集群时不要使用此配置。

### 索引操作

#### 创建索引

**格式：** PUT /索引名称

```bash
#创建索引
PUT /es_db

#创建索引时可以设置分片数和副本数
PUT /es_db
{
    "settings": {
        "number_of_shards": 3,
        "number_of_replicas": 2    
    }
}


#修改索引配置
PUT /es_db/_settings
{
    "index": {
        "number_of_replicas": 1     
    }
}
```

#### 查询索引

格式：GET /索引名称

#### 索引是否存在

格式：HEAD /索引名称

#### 删除索引

格式：DELETE /索引名称

#### 打开索引

格式：POST /es_db/_open

#### 关闭索引

格式：POST /es_db/_close

### 文档操作

#### 添加文档

- 格式：[PUT | POST] /索引名称/[_doc | _create ]/id

```bash
PUT /es_db/_doc/1
{
  "name": "张三"
}

#查询指定id的文档
GET /es_db/_doc/1

POST /es_db/_doc
{
  "name": "李四",
  "age": 12
}


#查询所有文档
GET /es_db/_search
```

**注意：** POST和PUT都能起到创建/更新的作用，PUT需要对一个具体的资源进行操作也就是要确定id才能进行更新/创建，而POST是可以针对整个资源集合进行操作的，如果不写id就由ES生成一个唯一id进行创建新文档，如果填了那就针对这个id的文档进行创建/更新

_create 是如果已经存在了id就会报错，不会进行修改 

#### 修改文档

- 全量更新，整个json都会被替换，格式和新增一样
  
  如果文档存在，现有文档会被删除，新的文档会被索引

- 局部更新，格式：POST /索引名称/update/id
  
  update不会删除原来的文档，而是实现真正的数据更新。

格式：POST /es_eb/_update/id

```bash
POST /es_db/_udpate/1
{
    "doc": {
        "age": 28
    }
}

#查询文档
GET /es_db/_doc/id
```

- 使用_update_by_query更新文档

格式：POST /es_db/_update_by_query

```bash
POST /es_db/_update_by_query
{
    "query": {
        "match": {
            "_id": 1
        }
    },
    "script": {
        "source": "ctx.source.age = 30"
    }
}
```

#### 查询文档

- 根据id查询文档，格式：GET /索引名称/_doc/id
  
  ```bash
  GET /es_db/_doc/1
  ```

- 条件查询search，格式：GET /索引名称/doc/_search
  
  ```bash
  # 查询前10条文档
  GET /es_db/_doc/_search
  {
     "query":{
         "term":{
             "name": {
                 "value": "张三"
             }
         }
     }
  }
  ```

ES Search API 提供了两种条件查询搜索方式：

- REST分割的请求URI，直接将参数带过去

- 封装到request body中，这种方式可以定义更加易读的JSON格式
  
  ```bash
  #通过URI搜索，使用"q"指定查询字符串，“query string syntax” KV键值对
  
  #条件查询，如要查询age等于28岁的_search?q=*:***
  GET /es_db/_doc/_search?q=age:28
  ```
  
  #泛微查询，如要查询age在25到26岁之间的_search?q=***[** TO **] 注意： TO必须大写
  GET /es_eb/_doc/_search?q=age[25 TO 26

#### 删除文档

格式：DELETE /索引名称/_doc/id

```bash
DELETE /es_db/_doc/1
```

#### 批量操作

##### 批量写入

批量对文档进行写操作是通过_bulk的API来实现的

- 请求方式:  POST 

- 请求地址：_bulk

- 请求参数：通过_bulk操作文档，一般至少有两行参数（或偶数行参数）
  
  - 第一行参数为指定操作的类型及操作的对象（index，type和id）
  
  - 第二行参数才是操作的数据

参数类似于：

```bash
{"actionName":{"_index":"indexName", "_type":"typeName", "_id":"id"}
{"field1":"value1", "filed2":"value2"}
```

- actionName: 表示操作类型，主要有create,index,delete和update

批量创建文档create

```bash
POST _bulk
{"create": {"_index": "article", "_type":"_doc","_id":3}}
{"id": 3, "title":"test", "content":"fox老师"}
{"create": {"_index": "article", "_type":"_doc","_id":4}}
{"id": 4, "title":"test", "content":"fox老师"}
```

index

```bash
POST _bulk
{"index": {"_index": "article", "_type":"_doc","_id":3}}
{"id": 3, "title":"test", "content":"fox老师"}
{"index": {"_index": "article", "_type":"_doc","_id":4}}
{"id": 4, "title":"test", "content":"fox老师"}
```

- 如果原文档不存在，则创建

- 如果原文档存在，则是替换（全量修改原文档）

##### 批量删除delete

```bash
POST _bulk
{"delete": {"_index": "article", "_type":"_doc","_id":3}}
{"delete": {"_index": "article", "_type":"_doc","_id":4}}
```

##### 批量修改update

```bash
POST _bulk
{"update": {"_index": "article", "_type":"_doc","_id":3}}
{"doc": {"title":"test", "content":"fox老师"}}
{"update": {"_index": "article", "_type":"_doc","_id":4}}
{"doc": {"content":"fox老师"}}
```

##### 组合应用

```bash
POST _bulk
{"create": {"_index": "article", "_type":"_doc","_id":4}}
{"id": 4, "title":"test", "content":"fox老师"}
{"update": {"_index": "article", "_type":"_doc","_id":3}}
{"doc": {"title":"test", "content":"fox老师"}}
{"delete": {"_index": "article", "_type":"_doc","_id":4}}
```

##### 批量读取

es的批量查询可以使用mget和msearch两种。其中mget是需要我们知道他的id，可以指定不同的index，也可以指定返回值source。msearch可以通过字段查询来进行一个批量的查找。

_mget

```bash
#可以通过ID批量获取不同的index和type的数据
GET _mget
{
    "docs":[
        {"_index": "es_db", "_id":1},
        {"_index": "article","_id":4}
    ]
}



#mget 查询
GET _mget
{
  "docs": [
    {
      "_index": "movies",
      "_id": 12,
      "_source": [
        "name",
        "price"
      ]
    },
    {
      "_index": "movies",
      "_id": 10,
      "_source": {
        "exclude": [
          "content",
          "type"
        ]
      }
    },
    {
      "_index":"good",
      "_id":1
    }
  ]
}

# 简化查询

GET movies/_mget
{
  "ids":[1,2,3]
}
```

_msearch

在 _msearch中,请求格式和_bulk类似。查询一条数据需要两个对象，第一个设置index和type，第二个设置查询语句。查询语句和search相同。如果只是查询一个index，我们可以在url中带上index，这样，如果查该index可以直接用空对象表示。

```bash
GET /_msearch
{}0
{"query": {"match_all": {}}, "from": 0, "size":2}
{"index": ""}
{"query": {"match_all": {}}}
```

### Mapping 映射

#### 1.Mapping 简介

##### 1.1mapping是什么

ES中的mapping有点类似于关系数据库中表结构的概念，在MySQL中，表结构里包含了字段名称，字段的类型还有索引信息等。在Mapping里也包含了一些属性，比如字段名称、类型、字段使用的分词器，是否评分、是否创建索引等属性，并且在ES中一个字段可以有多个类型。

##### 1.2如何查看索引映射

查看完整的索引mapping

GET index_name/_mapping

查看索引中指定字段的mapping

GET index_name/_mapping/field/field_name

#### 2.动态mapping(dynamic mapping)

当你是一个新手的时候你可以使用动态映射，ES会在你添加一个文档的时候自动添加一个映射字段。生产环境不建议使用动态映射

#### 3.手动映射：explicit mapping

手动映射顾名思义就是允许你去选择映射的定义，例如：字段类型、日期格式、自定义控制新增字段的动态映射。

##### 3.1 手动创建映射

```bash
#手动创建映射
PUT good2
{
  "mappings": {
    "properties": {
      "title": {
        "type": "text",
        "fielddata": true
      }
    }
  }
}

#修改映射，字段类型，分词器等都不允许修改
PUT good2/_mapping
{
  "properties": {
    "title": {
      "type": "text",
      "fielddata": true
    }
  }
}
```

### 4.ES数据类型

#### 4.1概述

每个字段都有字段数据类型或字段类型。其大致分为两种：**会被分词的字段和不会被分词的字段类型。**

- 会被分词的类型：text、match、only_text等

- 不会被分词的类型：keyword、数值类型等

当然数据类型的划分可以分为很多种，比如按照**基本数据类型和复杂数据类型**来划分

#### 4.2ES支持的数据类型

![](C:\Users\20220509\AppData\Roaming\marktext\images\2023-01-09-17-27-59-image.png)

### 5.Mapping的参数

- coerce 是否允许强制转换，只对数字起作用
  
  ```console
  # 是否允许强制转换 coerce 默认true
  PUT good_properties_coerce
  {
    "mappings": {
      "properties": {
        "title":{
          "type": "integer",
          "coerce": true
        }
      }
    }
  }
  
  DELETE good_properties_coerce
  
  GET good_properties_coerce
  
  GET good_properties_coerce/_source/2
  
  PUT good_properties_coerce/_doc/2
  {
    "title":"1"
  }
  
  #另外一种写法，coerce只对数字强制转换
  
  PUT coerce_test
   {
   "settings": {
   "index.mapping.coerce": false
   },
   "mappings": {
   "properties": {
   "number_one":{
   "type": "integer"
   },
   "number_two":{
   "type": "long"
   }
   }
   }
   }
  
  GET coerce_test
  
  PUT coerce_test/_doc/1
   {  
  "number_one":1,
   "number_two":"2"
   }
  GET coerce_test/_doc/1
  ```

- copy_to 将某个属性值拷贝到某个属性里

```json
# copy_to 演示
PUT copy_to_test
{
  "mappings": {
    "properties": {
      "first_name":{
        "type": "text",
        "copy_to": "full_name"
      },
      "last_name":{
        "type": "text",
        "copy_to": "full_name"
      },
      "full_name":{
        "type": "text"
      }
    }
  }
}

PUT copy_to_test/_doc/2
{
  "first_name":"san",
  "last_name":"li"
}

GET copy_to_test

GET copy_to_test/_search
{
  "query": {
    "match": {
      "full_name":"san zhang"
    }
  }
}
```

- doc_values

- dynamic 是否可以动态添加新字段

- enabled
  
  ```bash
  # enabled 创建索引的时候使用的话不会对其创建倒排索引，不支持全局搜索
  
  PUT test_enable
  {
    "mappings": {
      "enabled":false
    }
  }
  
  PUT test_enable/_doc/1
  {
    "title":"标题1",
    "content":"内容1"
  }
  
  PUT test_enable/_doc/2
  {
    "title":"标题2",
    "content":"内容2"
  }
  
  GET test_enable/_search
  
  GET test_enable/_search
  {
    "query": {
      "match": {
        "title": "标题"
      }
    }
  }
  
  DELETE test_enable
  ```

- fields 为一个字段指定多个子字段
  
  ```bash
  PUT test_fields
  {
    "mappings": {
      "properties": {
        "address": {
          "type": "text",
          "fields": {
            "provice": {
              "type": "text",
              "analyzer": "english"
            },
            "city": {
              "type": "keyword",
              "ignore_above": 10  # 超过长度会被忽略
            }
          }
        }
      }
    }
  }
  
  POST test_fields/_doc
  {
    "address":"China Hebei"
  }
  
  GET test_fields
  
  GET test_fields/_search
  {
    "query": {
      "match": {
        "address.provice": "China"
      }
    }
  }
  
  DELETE test_fields
  ```

- index
  
  ```bash
  PUT test_index
  {
    "mappings": {
      "properties": {
        "name":{
          "type": "text",
          "index": false
        },
        "desc":{
          "type": "text"
        }
      }
    }
  }
  
  POST test_index/_doc
  {
    "name":"zhangsan",
    "desc":"这是张三"
  }
  
  GET test_index/_search
  
  GET test_index/_search
  {
    "query": {
      "match": {
        "desc": "张"
      }
    }
  }
  ```

- norms 是否禁用评分

### 6.date类型

```bash
PUT test_date
{
  "mappings": {
    "properties": {
      "name":{
        "type": "text"
      },
      "create_date":{
        "type": "date",
        "format": 
            "yyyy-MM-dd||yyyy-MM-dd HH:mm:ss||strict_date_optional_time||epoch_millis"

      }
    }
  }
}


GET test_date

POST test_date/_doc
{
  "name":"zhangsan",
  "create_date":"2022-01-11 11:11:11"
}

POST test_date/_doc
{
  "name":"lisi",
  "create_date":"2022-01-11"
}

POST test_date/_doc
{
  "name":"wangwu",
  "create_date":"2022-01-11T00:00:00.000Z"
}

GET test_date/_search
```

### 7.分词器Text Analysis

```bash
# 分词器
GET _analyze
{
  "text": ["What are you doing", "你在做什么？"],
  "analyzer": "chinese"
}


# 切词器
GET _analyze
{
  "tokenizer": "standard",
  "filter": ["uppercase"], 
  "text": ["What are you doing?","www.baidu.com"]
}


# 自定义词项过滤器
PUT test_analyzer_filter
{
  "settings": {
    "analysis": {
      "filter": {
        "my_filter": {
          "type": "stop",
          "stopwords": [
            "www"
          ],
          "ignore_case": true
        }
      }
    }
  }
}

DELETE test_analyzer_filter

GET test_analyzer_filter

GET test_analyzer_filter/_analyze
{
  "tokenizer": "standard",
  "filter": [
    "my_filter"
  ],
  "text": [
    "www WWW DDD mmm baidu com"
  ]
}

GET _analyze
{
  "tokenizer": "standard",
  "filter": ["stop"],
  "text": ["a the what is a english"]
}


PUT test_analyzer_synonym
{
  "settings": {
    "analysis": {
      "filter": {
        "my_synonym":{
          "type":"synonym",
          "synonyms":["a,b,c=>d"]
        }

      }
    }
  }
}
DELETE test_analyzer_synonym

PUT test_analyzer_synonym1
{
  "settings": {
    "analysis": {
      "filter": {
        "my_synonym":{
          "type":"synonym",
          "synonyms":["a,b,c,d"]
        }
      }
    }
  }
}

DELETE test_analyzer_synonym1

GET test_analyzer_synonym

GET test_analyzer_synonym1/_analyze
{
  "tokenizer": "standard",
  "filter": ["my_synonym"],
  "text": ["b"]
}
```

### IK分词器热词加载

- 通过配置文件扩展，但是需要配置完成之后重启ElasticSearch

- 通过远程加载更新Remote

- 通过Mysql进行热词更新

### ES检索原理分析

****

**索引的原理**

索引是加速数据查询的重要手段，其核心原理是通过不断的缩小想要获取数据的范围来筛选出最终想要的结果，同时把随机的事件变成顺序的事件。

**磁盘IO与预读**

磁盘IO是程序设计中非常高昂的操作，也是影响程序性能的重要因素，因此应当尽量避免过多的磁盘IO操作，有效的路由内存可以大大的提升程序的性能。在操作系统层面，发生一次IO时，不光把当前磁盘地址的数据读取到内存中，而是把相邻的数据也读取到内存缓冲区内，局部预读性原理告诉我们，当计算机访问一个地址数据的时候，与其相邻的数据也会很快被访问到。每一次IO读取的数据我们称之为一页（page）。具体一页有多大数据跟操作系统有关，一般分为4k或8k，也就是我们读取一页内的数据时候，实际上才发生了一次的IO，这个理论对于索引的数据结构设计非常有帮助。

**倒排索引**

当数据写入ES时，数回通过分词 被切分为不同的term，ES将term与其对应的文档列表建立一种映射关系，这种结构就是**倒排索引**。如下图所示：

![](C:\Users\20220509\AppData\Roaming\marktext\images\2022-06-10-16-19-04-image.png)

为了进一步提升索引的效率，ES在term的基础上利用term的前缀或者后缀构建了term index，用于对term本身进行索引，ES实际的索引结构如下图所示：

![](C:\Users\20220509\AppData\Roaming\marktext\images\2022-06-10-16-17-41-image.png)

这样当我们去搜索某个关键词时，ES首先根据他的前缀或者后缀迅速缩小关键词的在term dictionary中的范围，大大减少了磁盘IO的次数。

- 单词词典（Term Dictionary）：记录所有文档的单词，记录单词到倒排列表的关联关系

- 倒排列表（Posting List）记录了单词对应的文档结合，由倒排索引项组成

- 倒排索引项（Posting）
  
  - 文档ID
  
  - 次频TF-该单词在文档中出现的次数，用于相关性评分
  
  - 位置（Position）-单词在文档中分词的位置。用于短语搜索（match phrase query）
  
  - 偏移（Offset）-记录单词的开始结束位置，实现高亮显示

### ES高级查询Query DSL

---

ES中提供了一种强大的检索数据方式，这种检索方式称之为Query DSL（Domain Specified Language）,Query DSL时利用Rest API传递JSON格式的请求体（RequestBody）数据与ES进行交互，这种方式的丰富查询语法让ES检索变得更强大，更简洁。

[Script query | Elasticsearch Guide [7.17] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/7.17/query-dsl-script-query.html)

语法：

```bash
GET /es_db/_doc/_search {json请求体数据}
可简化为下面写法
GET /es_db/_search {json请求数据}
```

#### 查询所有match_all

使用match_all，默认只会返回10条数据

原因：_search查询默认采用的时分页查询，每页记录数size的默认值是10。如果项显示更多数据，指定size

```bash
GET /es_db/_search
等同于
GET /es_db/_earch
{
    "query":{
        "match_all":{}
    },
    "size": 5,
    "from": 0
}
```

返回指定条数size

size关键字：指定查询结果返回指定条数。默认返回值10条

#### 分页查询from

from关键字：用来指定起始返回位置，和size关键字连用可实现分页效果

思考：size可以无限增加吗？

答案：最大不能超过10000

#### 分页查询Scroll

改动index.max_result_window参数值的大小，只能解决一时的问题，当索引的数据量持续增长时，在查询全量数据时还是会出现问题。而且会增加ES服务器内存大结果集消耗完的风险。最佳实践还是根据异常提示中的采用scroll api更高效的请求大量数据集。

```bash
#查询命令中新增scroll=1m，说明采用游标查询，保持游标查询窗口一分钟
#这里由于测试数据两不够，所以size值设置为2.
#实际使用中为了减少游标查询的次数，可以将值适当方法，比如设置为1000。
GET /es_db/_search?scroll=1m
{
    "query": { "match_all": {} },
    "size": 1000
}
```

多次根据scroll_id游标查询，直到没有数据返回则结束查询。采用游标查询索引全量数据，更安全高效，限制了单次对内存的消耗。

#### 指定字段排序

注意：会让得分失效

```json
GET /es_db/_search
{
    "query":{
        "match_all":{}
    },
    "sort":[
        {
            "age":"desc"
        }
    ]
}
```

#### match

match在匹配时会对所查找的关键字进行分词，然后按照分词匹配查找

match支持以下参数：

- query：指定匹配的值

- operator：匹配条件类型
  
  - and：条件分词后都要匹配
  
  - or：条件分词后有一个匹配即可（默认）

- minmun_should_match: 最低匹配度，即条件在倒排索引中最低的匹配度

```json
GET /es_db/_search
{
  "query":{
    "match": {
      "name": {
        "query":"张三1111",
        "minimum_should_match":5

      }
    }
  }
}
```

#### 短语查询match_phrase

match_phrase查询分析文本并根据分析的文本创建一个短语查询。match_phrase会将检索关键词分词。match_phrase的分词结果必须在被检索字段的分词中都包含，而且顺序必须相同，默认必须是连续的。

如何解决词条间隔的问题？可以借助slop参数，slop参数告诉match_phrase查询词条能够相隔多远时仍然将文档是为匹配。

![](C:\Users\20220509\AppData\Roaming\marktext\images\2023-01-31-14-46-12-image.png)

```json
GET movies/_search
{
  "query": {
    "match_phrase": {
      "content": {
        "query": "的 故事"
      }
    }
  }
}

GET movies/_search
{
  "query": {
    "match_phrase_prefix": {
      "content": {
        "query": "的 故事",
        "slop": 2
      }
    }
  }
}
```

#### 多字段查询multi_match

可以根据字段类型，决定是否使用分词查询，得分最高排在最前面

```bash
GET good/_search
{
  "query": {
    "multi_match": {
      "query": "这是",
      "fields": ["name","desc"]
    }
  }
}
```

#### query_string

允许我们在单个查询字符串中指定AND | OR | NOT条件，同时也和multi_match query一样，支持多字段搜索。和match类似，但是match需要指定字段名，query_string实在所有字段中搜索，范围广泛

注意：查询字段分词就将查询条件分词查询，查询字段部分此将查询条件不分词查询

```json
#未指定字段查询
GET /es_db/_search
{
  "query": {
    "query_string": {
      "query": "三"
    }
  }
}

# 指定单个字段查询
GET /es_db/_search
{
  "query": {
    "query_string": {
      "default_field": "name", 
      "query": "三"
    }
  }
}
```

#### simiple_query_string

类似query_string，但是会忽略错误的语法，同时只支持部分查询语法，不支持AND、OR、NOT，会当作字符串处理。用符号代替+代替AND | 代替OR -代替NOT

#### 关键字（精确）查询term

term用来使用关键词查询（精确匹配），还可以用来查询没有被进行分词的数据类型。term时表单语义的最小单位，搜索和利用统计语言模型进行自然语言处理都需要处理term。match在匹配时会对所查找的关键词进行分词，然后按分词匹配查找，而term会直接对关键词进行查找。一般模糊查找的时候，多用match，而精确查找时可以使用term。

- ES中默认使用分词器为标准分词器，标准分词器对于英文单词分词，对于中文是单字分词

- 在ES的Mapping Type中keyword、date、integer、long、double or ip 这些类型不分词，只有text类型分词。
  
  ```json
  GET /es_db/_search
  {
    "query": {
      "term": {
        "name.keyword": {
          "value": "张三xx"
        }
      }
    }
  }
  ```

可以通过constant score将查询结果转换成一个Filtering，避免算分，并利用缓存，提高性能。

- 将query转换成filter，忽略TF-IDF计算，避免相关性算分的开销。

- Filter可以有效利用缓存

![](C:\Users\20220509\AppData\Roaming\marktext\images\2022-06-15-16-13-00-image.png)

#### prefix前缀搜索

它会对分词后的term进行前缀搜索

- 不会分析搜索字符串，传入的前缀就是想要查找的前缀

- 默认状态下，前缀查询不做相关度分数计算，他只是将所有匹配的文档返回，然后赋予相关分数值为1。它的行为更像是一个过滤器而不是查询。两者实际的区别就是过滤器是被缓存的，而前缀查询不行。

prefix的原理：要遍历所有倒排索引，并比较每个term是否是指定的前缀开头。

#### 通配符查询wildcard

通配符查询：工作原理和prefix相同，只不过他不是比较开头，它能支持更为复杂的匹配模式。

#### 范围查询range

- range: 范围关键字

- gte: 大于等于

- lte：小于等于

- gt：大于

- lt：小于

- now：当前时间

#### ids

ids关键字：值为数组类型，用来根据一组id获取多个对应的文档

![](C:\Users\20220509\AppData\Roaming\marktext\images\2022-06-15-16-30-58-image.png)

#### 模糊查询fuzzy

在实际的搜索中，我们有时候会打错字，从而导致搜索不到。elasticsearch中，我们可以使用fuzziness属性来进行模糊查询，从而达到搜索错别字的情形。

fuzzy查询会用到两个很重要的参数，fuzziness，prefix_length

- fuziness：表示输入的关键字通过几次操作可以转换成为ES库里面的对应field的字段
  
  - 操作是指：新增一个字符，删除一个字符，修改一个字符，每次操作可以机组编辑距离为1。
  
  - 如中文集团到中为集团编辑距离就是1，只需要修改一个字符
  
  - 该参数默认值为0，即不开启模糊查询
  
  - 如果fuzziness值在这里设置成2，会把编辑距离为2的东东集团也查询出来

- prefix_length:表示限制输入关键字和ES对应查询field的内容开头的第n个字符必须完全匹配，不允许错别字匹配
  
  - 如果这里等于1，则表示开头的字必须匹配，不匹配则不返回
  
  - 默认值也是0
  
  - 加大prefix_length的值可以提交效率和准确率

注意：fuzzy模糊查询 最大模糊错误必须在0-2之间

- 搜索关键字长度为2，不允许存在模糊查询

- 搜索关键字长度为3-5，允许1次模糊

- 搜索关键字长度大于5，允许最大2次模糊

#### 高亮查询highlight

highlight关键字：可以让符合条件的文档中的关键字高亮。

highlight相关属性：

- pre_tags前缀标签

- post_tags后缀标签

- tags_schema设置为styled可以使用内置高亮央视

- require_field_match多字段高亮需要设置为false

示例：

```json
GET /es_db/_search
{
  "query": {
    "term": {
      "name.keyword": {
        "value": "张三xx"
      }
    }
  },
  "highlight": {
    "fields": {
      "*": {}
    }
  }
}
```

```json
GET /es_db/_search
{
  "query":{
    "match": {
      "name": {
        "query":"张三",
        "minimum_should_match":1

      }
    }
  }, 
  "highlight": {
    "pre_tags": ["<span style='color:red'>"],
    "post_tags": ["</span>"], 
    "fields": {
      "*":{}
    }
  }
}


#多字段完成高亮
GET /es_db/_search
{
  "query":{
    "match": {
      "name": {
        "query":"张三",
        "minimum_should_match":1

      }
    }
  }, 
  "highlight": {
    "pre_tags": ["<span style='color:red'>"],
    "post_tags": ["</span>"], 
    "require_field_match": "false", 
    "fields": {
      "*":{}
    }
  }
}
```

#### 想关性和相关性算分

搜索时用户和搜索引擎的对话，用户关心的时搜索结果的相关性

- 是否可以找到所有相关的内容

- 有多少不相关的内容被返回了

- 文档的打分是否合理

- 结合业务需求，平衡搜索排名

如何衡量相关性

- precision（查准率）尽可能返回较少的无关文档

- Recall（查全率）尽量返回较多的相关文档

- Ranking是否能够按照相关度进行排序

##### 相关性(Relevance)

搜索的相关性算分，描述了一个文档和查询语句匹配的程度。ES会对每个匹配查询条件的结果进行算分_score。

打分的本质时排序，需要把最符合用户需求的文档排在前面。ES 5之前，默认的相关性算分采用TF-IDF，现在采用BM25.

##### 什么是TF-IDF

TF-IDF（term frequency-inverse document frequency）是一种用于信息检索与数据挖掘的常用加权技术。

- TF-IDF被公认为是信息检索领域最重要的发明，除了在信息检索，在文献分类和其他相关领域也有着非常广泛的应用。

- IDF的概念，最早是剑桥大学的“斯巴克.琼斯”提出
  
  - 1927年——“关键词特殊性的统计解释和它在文献检索中的应用”，但是没有从理论上解释IDF应该是用log（全部文档数/检索词出现过的文档总数），而不是其它函数，也没有做进一步的研究。
  
  - 1970年，1980年代撒尔顿和罗宾逊，进行了进一步的证明和研究，并用香农信息论做了证明

- 现代搜索引擎，对TF-IDF进行了大量细微的优化

#### 布尔查询bool Query

一个bool查询是一个或者多个查询子句的组合，总共包括4种子句，其中2中会影响算分，2种不影响算分。

- must: 相当于&&,必须匹配，贡献算分

- should：相当于||, 选择性匹配，贡献算分

- must_not： 相当于 ！，必须不能匹配，不贡献算分

- filter：必须匹配，不贡献算法

在Elasticsearch中，有Query和Filter两种不同的Context

- Query Context：相关性算分

- Filter Context：不需要算分（Yes or No），可以利用Cache，获得更好的性能

相关性并不只是全文检索的专利，也适用于yes|no的字句，匹配的子句越多，相关性评分越高。如果多条查询子句被合并为一条符合查询语句，比如bool查询，则每个查询子句计算得出的的评分会被合并到总的相关性评分中。

bool查询语法

- 子查询可以任意顺序出现

- 亏嵌套多个查询

- 如果你的bool查询中，没有must条件，should中必须至少满足一条查询

```json
GET /es_db/_search
{
  "query": {
    "bool": {
      "should": [
        {
          "term": {
            "name": {
              "value": "张三"
            }
          }
        }
      ]
    }
  }
}
```

#### 映射

```json
{
    "properties":{
        "name":{
            "type":"text", //可以分词的
            "index":"true" //可以索引
        },
        "sex":{
            "type":"keyword", //关键字，不可以分词
            "index":"true" //可以索引
        },
        "tel":{
            "type":"keyword", //可以分词的
            "index":"false" //可以索引
        }
    }
}
```

## 聚合查询

语法：

GET index_name/_search

{

    "aggs":{    

            "<aggs_name>": {

                    "<aggs_type>":{    

                            "field":"<field_name>"                    

                       }

                }

    }

}

### 分桶聚合 Bucket aggregations

例如：统计不同分类的电影名字

```json
GET movies/_search
{
  "size": 0, 
  "aggs": {
    "aggs_type": {
      "terms": {
        "field": "type.keyword",
        "size": 100
      }
    }
  }
}
```

### 指标聚合 Metrics aggregations

最大值、最小值、平均值、求和等

```json
# 指标聚合查询
GET good/_search
{
  "size": 0,
  "aggs": {
    "aggs_max": {
      "max": {
        "field": "price"
      }
    },
    "aggs_min":{
      "min": {
        "field": "price"
      }
    },
    "aggs_avg":{
      "avg": {
        "field": "price"
      }
    }
  }
}
另一种查看所有指标

GET good/_search
{
  "size": 0, 
  "aggs": {
    "price_avg_min": {
      "stats": {
        "field": "price"
      }
    }
  }
}


# 去重
GET movies/_search
{
  "size": 0, 
  "aggs": {
    "name_count": {
      "cardinality": {
        "field": "name.keyword"
      }
    }
  }
}
```

![](C:\Users\20220509\AppData\Roaming\marktext\images\2023-01-28-15-46-23-image.png)

### 管道聚合 Pipeline aggregations

对聚合结果的二次聚合

通过平均价格最低和最高的电影分类

```json
GET movies/_search
{
  "size": 0,
  "aggs": {
    "movie_type": {
      "terms": {
        "field": "type.keyword",
        "size": 100
      },
      "aggs": {
        "avg_price": {
          "avg": {
            "field": "price"
          }
        }
      }
    },
    "price_avg_min": {
      "min_bucket": {
        "buckets_path": "movie_type>avg_price"
      }
    },
    "price_avg_max": {
      "max_bucket": {
        "buckets_path": "movie_type>avg_price"
      }
    }
  }
}
```

![](C:\Users\20220509\AppData\Roaming\marktext\images\2023-01-28-16-11-54-image.png)

### 嵌套聚合

统计不同种类的电影的不同级别的价格情况

```json
GET movies/_search
{
  "size": 0,
  "aggs": {
    "type_lv_price": {
      "terms": {
        "field": "type.keyword"
      },
      "aggs": {
        "lv": {
          "terms": {
            "field": "level.keyword",
            "size": 10
          }
        },
        "price": {
          "stats": {
            "field": "price"
          }
        }
      }
    }
  }
}
```

### 基于查询结果的聚合

- 从警匪片中找到价格最贵的电影

```json
GET movies/_search
{
  "query": {
    "match": {
      "type": "警匪"
    }
  },
  "aggs": {
    "price_max_agg": {
      "max": {
        "field": "price"
      }
    }
  }
}
```

```json
# 查询警匪片价格在10和50之间的电影，并且查询最小值
GET movies/_search
{
  "query": {
    "match": {
      "type.keyword": "警匪"
    }
  },
  "aggs": {
    "prices": {
      "terms": {
        "field": "price",
        "size": 10
      }
    },
    "min_price":{
      "min": {
        "field": "price"
      }
    }
  },
  "post_filter": {
    "range": {
      "price": {
        "gte": 10,
        "lte": 50
      }
    }
  }
}

结果展示：

{
  "took": 14,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 3,
      "relation": "eq"
    },
    "max_score": 1.3291358,
    "hits": [
      {
        "_index": "movies",
        "_id": "11",
        "_score": 1.3291358,
        "_source": {
          "name": "神探大战",
          "content": "神探间的混战",
          "level": "4",
          "type": "警匪",
          "price": 50
        }
      },
      {
        "_index": "movies",
        "_id": "12",
        "_score": 1.3291358,
        "_source": {
          "name": "猎豹出击",
          "content": "历史剧",
          "level": "3",
          "type": "警匪",
          "price": 30
        }
      },
      {
        "_index": "movies",
        "_id": "14",
        "_score": 1.3291358,
        "_source": {
          "name": "飞虎队",
          "content": "飞虎队英雄事迹",
          "level": "4",
          "type": "警匪",
          "price": 10
        }
      }
    ]
  },
  "aggregations": {
    "min_price": {
      "value": 10
    },
    "prices": {
      "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0,
      "buckets": [
        {
          "key": 10,
          "doc_count": 1
        },
        {
          "key": 30,
          "doc_count": 1
        },
        {
          "key": 50,
          "doc_count": 1
        },
        {
          "key": 130,
          "doc_count": 1
        }
      ]
    }
  }
}
```

### 聚合排序

```json
# 影片分类价格最低价排序

GET movies/_search
{
  "aggs": {
    "price_aggs": {
      "terms": {
        "field": "type.keyword",
        "order": {
          "price_state_aggs>stats_aggs.min": "desc"
        }
      },
      "aggs": {
        "price_state_aggs": {
          "filter": {
            "range": {
              "price": {
                "gt": 20
              }
            }
          },
          "aggs": {
            "stats_aggs": {
              "stats": {
                "field": "price"
              }
            }
          }
        }
      }
    }
  }
}
```

### histogram 直方图 或 柱状图

```json
# missing是空值的会默认成90
GET movies/_search?size=0
{
  "aggs": {
    "price_aggs": {
      "histogram": {
        "field": "price",
        "interval": 20,
        "keyed": true,
        "missing": 90, 
        "order": {
          "_count": "asc"
        }
      }
    }
  }
}


结果展示：
{
  "took": 12,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 17,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "aggregations": {
    "price_aggs": {
      "buckets": {
        "80.0": {
          "key": 80,
          "doc_count": 1
        },
        "100.0": {
          "key": 100,
          "doc_count": 1
        },
        "120.0": {
          "key": 120,
          "doc_count": 1
        },
        "0.0": {
          "key": 0,
          "doc_count": 2
        },
        "60.0": {
          "key": 60,
          "doc_count": 2
        },
        "40.0": {
          "key": 40,
          "doc_count": 4
        },
        "20.0": {
          "key": 20,
          "doc_count": 6
        }
      }
    }
  }
}
```

- data_histogram

- auto_histogram
  
  同理

### Percentile 百分位统计 或 饼状图

```json
#价格所占百分比
GET movies/_search
{
  "aggs": {
    "percentile_price_aggs": {
      "percentiles": {
        "field": "price",
        "percents": [
          1,
          5,
          25,
          50,
          75,
          95,
          99,100
        ]
      }
    }
  }
}

执行结果：
"aggregations": {
    "percentile_price_aggs": {
      "values": {
        "1.0": 10,
        "5.0": 10,
        "25.0": 25,
        "50.0": 35,
        "75.0": 55,
        "95.0": 120.99999999999999,
        "99.0": 130,
        "100.0": 130
      }
    }

# 价格内的所占百分比
GET movies/_search
{
  "aggs": {
    "percentitles_price_aggs": {
      "percentile_ranks": {
        "field": "price",
        "values": [
          10,
          15,
          20,
          50,
          100,
          200
        ]
      }
    }
  }
}


执行结果：
  "aggregations": {
    "percentitles_price_aggs": {
      "values": {
        "10.0": 6.25,
        "15.0": 12.5,
        "20.0": 18.75,
        "50.0": 68.75,
        "100.0": 91.07142857142857,
        "200.0": 100
      }
    }
  }
```

## Scripting脚本查询

### ctx

- 语法
  
  ctx._source.属性=值

```json
POST movies/_update/2
{
  "script": {
    "source": "ctx._source.price-=1"
  }
}

# 简写
POST movies/_update/2
{
  "script": "ctx._source.price-=ctx._version"
}
```

### Scripting的增删改查

```json
# upsert 如果存在更新，不存在新增数据
POST movies/_update/2
{
  "script": {
    "source": "ctx._source.price+=2"
  },
  "upsert": {
     "name": "上海滩",
    "content": "上海滩腥风血雨",
    "level": "2",
    "type": "枪战片",
    "price": 24
  }
}
#参数化
POST movies/_update/2
{
  "script": {
    "source": "ctx._source.price+=(params.price_param)",
    "params": {
      "price_param": 2
    }
  }
}
两段代码效果相同
```

打折出售

```json
# 打折售卖
GET movies/_search
{
  "script_fields": {
    "price": {
      "script": {
        "source": "doc.price.value"
      }
    },
    "discount_price": {
      "script": {
        "source": "[doc['price'].value * params['discount_9'],doc['price'].value * params['discount_8'],doc['price'].value * params['discount_7']]",
        "params": {
          "discount_9": 0.9,
          "discount_8": 0.8,
          "discount_7": 0.7
        }
      }
    }
  }
}
```

```json
# 创建一个脚本
# 语法POST _scripts/id {"script":{"lang":"painless","source":""}}
POST _scripts/calculation_discount
{
  "script": {
    "lang": "painless",
    "source": "[doc['price'].value * params['discount_9'],doc['price'].value * params['discount_8'],doc['price'].value * params['discount_7']]"
  }
}

# 查看
GET _scripts/calculation_discount

# 调用脚本
GET movies/_search
{
  "script_fields": {
    "movie_price":{
      "script":{
        "source": "doc.price.value"
      }
    },
    "movie_price_discount": {
      "script": {
        "id": "calculation_discount",
        "params": {
          "discount_9":0.9,
          "discount_8":0.8,
          "discount_7":0.7
        }
      }
    }
  }
}
```

```json
# 多行脚本
POST movies/_update/1
{
  "script": {
    "source": """
      if(ctx._source.price==100){
        ctx._source.price-=10
      }else{
         ctx.op="noop"
      }
    """
  }
}

# 价格小于100 的，价格是90 的电影的票价和
GET movies/_search
{
  "query": {
    "range": {
      "price": {
        "lt": 100
      }
    }
  },
  "aggs": {
    "less_100_agg": {
      "sum": {
        "script": {
          "lang": "painless", 
          "source": """
            long p=doc['price'].value;
            if(doc['price'].value==50){
              return p;
            }
            return 0;
          """
        }
      }
    }
  }
}
```

## 模糊查询

### 1. 前缀搜索

概念：以XXX开头的搜索，不计算相关度评分

**注意**：

- 前缀搜索匹配的是term(词项)，而不是field

- 前缀搜索的性能很差

- 前缀搜索没有缓存

- 前缀搜索尽可能把前缀长度设置的更长

语法：

GET index_name/_search

{    

        "query":{

            "prefix":{

                   "field_name":{

                            "value":"word_prefix"

                     }            

            }

        }

 }

```json
GET movies/_search
{
  "query": {
    "prefix": {
      "name": {
        "value": "上海滩"
      }
    }
  }
}


#搜索上海滩开头的词项（分词后以上海滩开头的）
#所以创建索引要指定分词器，要不然默认分词器会把中文拆成一个一个的
```

### 2. 通配符 wildcard

**概念：通配符运算符是匹配一个或多个字符的占位符。例如：* 通配符匹配零个或多个字符。**

注意：通配符匹配也是term

语法：

GET <index>/_search

{

    "query":{

        "wildcard":{

            "<field>":{

                     "value":"<word_with_wildcard>"

            }

        }

    }

}

```json
GET movies/_search
{
  "query": {
    "wildcard": {
      "name": {
        "value": "*子弹*"
      }
    }
  }
}
```

### 3. 正则表达式

语法：

GET /_search

{

    "query":{

        "regexp":{

            "<field>":”正则表达式“

        }

    }

}

### 4. 模糊查询 fuzzy

例子：

```json
GET movies/_search
{
  "query": {
    "fuzzy": {
      "name": {
        "value": "弹子",
        "fuzziness": 1
      }
    }
  }
}


GET movies/_search
{
  "query": {
    "match": {
      "name": {
        "query":"子弹大",
        "fuzziness": 1
      }
    }
  }
}
```

### 5.ngram和edge ngram

```json
GET _analyze
{
  "tokenizer": "edge_ngram",
  "text": ["zhangsan is a good boy"]
}
#返回结果
{
  "tokens": [
    {
      "token": "z",
      "start_offset": 0,
      "end_offset": 1,
      "type": "word",
      "position": 0
    },
    {
      "token": "zh",
      "start_offset": 0,
      "end_offset": 2,
      "type": "word",
      "position": 1
    }
  ]
}
```

## Suggester

### 1.term Suggester

```json
POST <index>/_search
{
    "suggest":{
        "suggest_name":{
            "text":"value",
             "term":{
                "field":"field_name"
            }
        }
    }
}

例子：
POST _bulk
{"index":{"_index":"english_word","_id":1}}
{"title":"zhangsan lisi wangwu zhaoliu zhouqi zhengba"}
{"index":{"_index":"english_word","_id":2}}
{"title":"zhanger lisan wangsi zhaoqi zhouliu zhengba"}
{"index":{"_index":"english_word","_id":3}}
{"title":"zhangsi wangsi wangsan zhaoqi zhouwu zhengba"}

GET english_word/_search
{
  "suggest": {
    "english_suggester": {
      "text": "zhangliu",
      "term": {
        "field": "title"
      }
    }
  }
}
# 只有英文可以，中文不可以。返回结果

{
  "took": 7,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 0,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  },
  "suggest": {
    "english_suggester": [
      {
        "text": "zhangliu",
        "offset": 0,
        "length": 8,
        "options": [
          {
            "text": "zhangsi",
            "score": 0.71428573,
            "freq": 1
          },
          {
            "text": "zhaoliu",
            "score": 0.71428573,
            "freq": 1
          }
        ]
      }
    ]
  }
}
```

## Java整合ES

### 1、添加依赖

```xml
        <dependency>
            <groupId>co.elastic.clients</groupId>
            <artifactId>elasticsearch-java</artifactId>
            <version>8.4.3</version>
        </dependency>

        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.13.4</version>
        </dependency>

        <dependency>
            <groupId>jakarta.json</groupId>
            <artifactId>jakarta.json-api</artifactId>
            <version>2.1.1</version>
        </dependency>
```

### 2、创建测试类

// 创建ES客户端 连接

```java
   private static ElasticsearchClient connectES() {
        RestClient restClient = RestClient.builder(new HttpHost("192.168.3.35", 9200)).build();
        elasticsearchTransport = new RestClientTransport(restClient, new JacksonJsonpMapper());
        // 创建同步客户端
        ElasticsearchClient elasticsearchClient = new ElasticsearchClient(elasticsearchTransport);
        // 创建异步客户端，异步客户端不用关闭
        //ElasticsearchAsyncClient elasticsearchAsyncClient = new ElasticsearchAsyncClient(elasticsearchTransport);
        return elasticsearchClient;
    }
```

### 3、创建索引

```java
 private static void createIndex() {
        try {
            if (existsIndex()) {
                System.out.println("索引已经存在");
                return;
            }
            ElasticsearchClient elasticsearchClient = connectES();
            CreateIndexResponse createResponse = elasticsearchClient.indices().create(c -> c.index("person"));
            System.out.println("创建索引结果：" + createResponse.acknowledged());
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                System.out.println("这里会执行吗？");
                elasticsearchTransport.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }
```

### 4、索引是否存在

```java
  // 索引是否存在
    private static boolean existsIndex() throws IOException {
        ElasticsearchClient elasticsearchClient = connectES();
        ExistsRequest existsRequest = new ExistsRequest.Builder().index("person").build();
        return elasticsearchClient.indices().exists(existsRequest).value();
    }
```

### 5、使用客户端查询

```java
   private static void getIndex() throws IOException {
        ElasticsearchClient elasticsearchClient = connectES();
        GetIndexResponse response = elasticsearchClient.indices().get(g -> g.index("person"));
        System.out.println("查询结果" + response);
        elasticsearchTransport.close();
    }
```

### 6、删除索引

```java
    private static void deleteIndex() throws IOException {
        ElasticsearchClient elasticsearchClient = connectES();
        DeleteIndexResponse response = elasticsearchClient.indices().delete(d -> d.index("person"));
        System.out.println("删除结果" + response.acknowledged());
        elasticsearchTransport.close();
    }
```

### 7、创建文档

- 包含单个添加文档，批量添加文档，普通方式和lambda方式

```java
 // 添加文档
    // 单个添加文档
    private static void addDocument() throws IOException {
        Person person = new Person();
        person.setId(1001);
        person.setName("Tom");
        person.setAge(10);

        ElasticsearchClient elasticsearchClient = connectES();
//        CreateRequest createRequest = new CreateRequest.Builder<Person>().index("person").id("1001").document(person).build();
//        CreateResponse createResponse = elasticsearchClient.create(createRequest);
//        System.out.println("文档创建相应对象："+createResponse);
        Result result = elasticsearchClient.create(req -> req.index("person").id("1001").document(person)).result();
        System.out.println("文档创建相应对象：" + result);
        elasticsearchTransport.close();
    }

    // 批量添加
    private static void batchAddDocument() throws IOException {
        ElasticsearchClient elasticsearchClient = connectES();

//        List<BulkOperation> list = new LinkedList<>();
//        for (int i=0; i<10; i++){
//            CreateOperation<Person> createOperation = new CreateOperation.Builder<Person>().index("person")
//                    .id("200"+i)
//                    .document(new Person(2000+i, "Tom"+i, 20+i)).build();
//            BulkOperation bulkOperation = new BulkOperation.Builder().create(createOperation).build();
//            list.add(bulkOperation);
//        }
//        BulkRequest bulkRequest = new BulkRequest.Builder().operations(list).build();
//        BulkResponse response = elasticsearchClient.bulk(bulkRequest);
//        System.out.println("批量新增数据的相应："+response);

//        lambda方式
        List<Person> personList = new LinkedList<>();
        for (int i = 0; i < 10; i++) {
            personList.add(new Person(200 + i, "Tom" + i, 20 + i));
        }
        elasticsearchClient.bulk(req -> {
            personList.forEach(p ->
                    req.operations(b -> b.create(co -> co.index("person").id(p.getId().toString()).document(p)))
            );
            return req;
        });
        elasticsearchTransport.close();
    }
```

### 8、查询文档

```java
 // 查询文档
    private static void queryDocument(String field, String value) throws IOException {
//        MatchQuery matchQuery = new MatchQuery.Builder()
//                .field(field).query(value).build();
//
//        Query query = new Query.Builder().match(matchQuery).build();
//        ElasticsearchClient elasticsearchClient = connectES();
//        SearchRequest searchRequest = new SearchRequest.Builder().query(query).build();
//        SearchResponse<Person> searchResponse = elasticsearchClient.search(searchRequest, Person.class);
//        System.out.println(searchResponse);

        //lambda表达式方式
        ElasticsearchClient elasticsearchClient = connectES();
        SearchResponse<Person> searchResponse = elasticsearchClient.search(req -> {
            req.query(q -> q.match(m -> m.field(field).query(value)));
            return req;
        }, Person.class);
        System.out.println(searchResponse);
        elasticsearchTransport.close();
    }
```

### 9、异步操作

```java
elasticsearchAsyncClient = new ElasticsearchAsyncClient(elasticsearchTransport);


// 异步查询
    private static void asyncExistsIndex() throws IOException {
        connectES();
        elasticsearchAsyncClient.indices().exists(req->req.index("person")).thenApply(BooleanResponse::value).whenComplete((response, error)->{
            System.out.println("执行结果："+response);
        });
        System.out.println("主线程执行完毕...");
    }
```

## EQL操作

        EQL的全名是Event Query Language(EQL)。事件查询语言是一种基于时间的时间序列数据（例如日志，指标和跟踪）的查询语言。在Elastic Security 平台上，当输入有效的EQL时，查询会在数据节点上编译，执行查询并返回结果。这一切都快速、并行的发生，让用户立即看到结果。

- EQL的优点

- EQL使你可以表达事件之间的关系
  
  许多查询语言允许您匹配单个事件。EQL使用可以匹配不通事件类别和事件跨度的一系列事件。

- EQL的学习曲线很低
  
  EQL语法看起来像其它常见查询语言，例如SQL。EQL是你可以直观的编写和读取查询，从而可以进行快速迭代的搜索。

- EQL设计用于安全用例
  
  尽管你可以将其用于人任何基于事件的数据，但我们创建了EQL来进行来进行威胁搜寻。EQL不仅支持危害指标（IOC）搜索，而且可以描述IOC范围的活动。

### SQL查询

POST _sql?format=txt

{

"query":""""

select * from "my-sql-index"

""""

}
