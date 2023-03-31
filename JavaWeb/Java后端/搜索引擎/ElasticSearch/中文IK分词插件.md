# 一、分词概念

- 一个tokenizer(分词器）接收一个字符流，将之分割为独立的tokens（词元，通常是独立的单词)，然后输出tokens流。
- 例如，whitespace tokenizer遇到空白字符时分割文本。它会将文本“Quick brown fox!"分割为[Quick, brown, fox!]。
- 该tokenizer(分词器）还负责记录各个term（词条）的顺序或position位置(用于phrase短语和word proximity词近邻查询），以及term(词条）所代表的原始word(单词）的start(起始）和end(结束）的character offsets（字符偏移量)(用于高亮显示搜索的内容）。Elasticsearch提供了很多内置的分词器，可以用来构建custom analyzers(自定义分词器）。

# 二、安装IK分词器

- IK分词器是ES的一个插件

- GitHub：https://github.com/medcl/elasticsearch-analysis-ik

- 安装过程：

    - 在Github中下载elasticsearch-analysis-ik-version.zip压缩包（注意：需要和ES版本一致）
    - 将其导入ES配置文件的plugins目录中
    - 解压压缩包
    - 在ES的bin目录运行命令检查插件是否安装成功

    ```shell
    // 列出所有已安装的插件
    eleasticsearch-plugins list
    ```

    - 重启ES即可使用

-  请求使用：通过发送_analyze请求可以选择分词器和文本查看分词后的状况

```http
<post>
http://localhost:9200/_analyze
json-body：
{
	// 分词器：standant标准分词器，ik_max_word分词器
	"analyzer": "",
	// 需要分词的文本
	"text": "",
}
```

# 三、自定义扩展词库

- 可以自己创建一个项目，然后处理请求，代理自定义的分词字典，然后由IK分词器向该项目发送请求，以获取自定义的字典文本文件。
- （推荐使用）或者可以使用Nginx代理一个字典文本文件，然后IK分词器向Nginx发送请求，以获取自定义的字典文本文件。

## 1、映射Nginx字典文件

- 创建字典文件 fenci.txt（.txt文件）
- 配置Nginx映射到该文件，直接在nginx的html目录下创建文件/es/fenci.txt即可
- 通过ip:80/es/fenci.txt即可访问

## 2、配置IK-Config文件

- 在ES的plugins/ik/config目录的IKAnalyzer.cfg.xml文件进行配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
	<comment>IK Analyzer 扩展配置</comment>
	<!--用户可以在这里配置自己的扩展字典 -->
	<entry key="ext_dict"></entry>
	 <!--用户可以在这里配置自己的扩展停止词字典-->
	<entry key="ext_stopwords"></entry>
	<!--用户可以在这里配置远程扩展字典 -->
	<entry key="remote_ext_dict">
    	// 远程分词地址，即 nginx 中的txt访问路径
    </entry>
	<!--用户可以在这里配置远程扩展停止词字典-->
	<!-- <entry key="remote_ext_stopwords">words_location</entry> -->
</properties>
```

- 再次重启ES服务即可使用





