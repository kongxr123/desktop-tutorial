# 微博舆论监控系统
 
## 项目介绍
使用爬虫周期性地爬取微博热搜榜，获取热搜列表，每条热搜内的热门微博和每条微博的评论。使用文本分类算法对每条评论进行情感分析，将所有微博数据和情感分析的结果存储在Redis里。运行Web服务直观地展示微博数据和情感分析结果。


## 项目结构介绍

### util包
`HttpUtil`类负责发送http请求。`RedisUtil`类负责使用`Jedis`连接Redis服务器并返回Jedis实例读写数据到Redis。`SegmentUtil`类重写了分词方法。

### entity包
`Hot`类、`Weibo`类和`Comment`类分别表示热搜、微博和评论的实体类，它们存储了基本的元数据和进行情感分析后的加工数据。

### crawler包
`CrawlTask`类继承了`TimerTask`能够周期性的运行，对微博热搜榜进爬取，进行情感分析，然后将数据存储到Redis。具体的爬取任务由`WeiboParser`类负责，由于微博页面基本都是动态的，请求微博相应的接口返回`Json`数据，然后使用`jackson`框架解析微博元数据。

### classifier包
`Train`类读取语料文件，存储到内存里，调用`HanLP`对文本进行分词处理，并计算每个词的`卡方值`，得到显著的词作为特征。最后生成`Model`类的模型。`Model`使用朴素贝叶斯算法进行文本分类。

## 运行
* 下载了`pox.xml`中依赖的包后，在IDE中直接运行`Application`类中的main函数。
* 或者使用`mvn clean package`命令将项目打包成jar，使用`java -jar *.jar`运行。

看控制台的输出，爬完一次微博后在浏览器打开`http://localhost:8080`查看Web页面。
