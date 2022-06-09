# ElasticSearch学习笔记

## 1. 下载

下载地址：

https://www.elastic.co/cn/downloads/past-releases#elasticsearch

选择elasticsearch，选择版本号，点击下载即可

这里下载的是 Elasticsearch 7.17.4

下载地址是： https://www.elastic.co/cn/downloads/past-releases/elasticsearch-7-17-4

选择操作系统点击下载。我下载linux版本。

## 2. 在Linux服务器上安装

在windows上的安装比较简单，都是图形化操作，这里不再赘述。主要看下linux的安装过程

1. 添加用户
   
   添加一个用户es，密码设置成es，权限可以在后面再设置。解压完成之后。
   
   ```bash
   useradd es
   passwd es es
   chown es:es .
   ```

2.  解压
   
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
   
   * discovery.seed_hosts: ["192.168.3.35"]
     
     * 192.168.3.35 是本机的IP地址，可以是公网IP，也可以是域名
   
   * cluster.initial_master_nodes: ["node-1"] 
     
     * 主节点的名字，和上面的名字一样，默认是这个名字。有一行注释的：
     
     * #node.name: node-1，# Use a descriptive name for the node:      没有设置默认的就是这个名字当然可以自己修改为自己想改的名字。
   
   $修改完成之后保存退出$

5. 配置jvm
   
   ```bash
    vim config/jvm.options
   ```

        主要修改了堆内存，我这里都改成了1g，默认是4g

            -Xms1g
            -Xmx1g

## 3. 运行

- 切换到es用户
  
  su es

- 启动
  
  bin/elasticsearch

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

## 4.  常见问题

- 使用root用户启动报错的问题

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



- 线程数太小问题

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



> 解决方式:
> 
> vim /etc/security/limits.conf
> 
> - 在末尾添加
>   
>   ``` tex
>   * soft nofile 65536
>   * hard nofile 65535
>   * soft nproc 4096
>   * hard nproc 4096
>   ```
> 
> vim /etc/security/limits.d/20-nproc.conf
> 
> ```tex
> *          soft    nproc     4096
> ```
> 
> vim /etc/sysctl.conf
> 
> ```tex
> vm.max_map_count=262144
> ```
