# 项目展示
主页界面：
![image](https://user-images.githubusercontent.com/57986069/155437644-be93de91-1f18-4b85-93c8-a289381df679.png)

搜索结果概要展示界面：
![image](https://user-images.githubusercontent.com/57986069/155437670-0117f4ab-723e-4be3-ac6e-584362dbf404.png)
![image](https://user-images.githubusercontent.com/57986069/155437680-aaa9632e-8ef3-4b85-b542-9a8aabb0e80f.png)

新闻详情界面：
![image](https://user-images.githubusercontent.com/57986069/155437700-c2ed96b9-b52c-4c96-bc63-20607e100334.png)

无匹配结果时的界面：
![image](https://user-images.githubusercontent.com/57986069/155437719-6a9cb947-c6dd-41cf-9a60-c5232787c03f.png)

# 使用说明
- 1.Web.py是该系统的入口，运行该文件即可打开此新闻检索系统。
- 2.在Web.py的最后一行代码中，若是app.run(host="localhost", port=1234)，则表示部署在本地；若是app.run(host="0.0.0.0", port=1234)，则表示部署到服务器上，局域网内可通过服务器IP和端口访问。
- 3.datasets文件夹存放新闻文档数据，templates文件夹存放系统的Web页面。

# 实现方法
### 1.使用jieba对新闻文档进行分词
- 该系统使用的数据集是从[中国新闻网](http://www.chinanews.com)中爬取到的1000篇新闻，主题包括了时政、国际社会、财经、金融、汽车、娱乐、体育、文化、理论、生活等各个方面。
- 在这里，我先使用Python的xml库对这1000篇新闻进行读取，再利用jieba分词库对每篇新闻的内容（包括新闻标题与新闻主体内容）进行分词操作。并且，为了能使分词更加精准，我在这里载入了jieba GitHub主页上给出的一个词典“dict.txt”。
- 然后，在分词前，我先将文本中所有的大写字母转成小写，并利用正则表达式剔除了所有标点符号，只保留了“中文”、“英文字母”和“数字”。最后在调用jieba库进行分词操作时，选择了全切割模式（cut_all=True），以保障尽可能全面地切割出文本中所有的词，以避免因关键词缺漏导致该新闻在后面无法检索到。

### 2.使用倒排索引（inverted index）存储数据
- 该系统使用倒排索引（inverted index）存储数据，没有调用MySQL等数据库。倒排索引的原理及构建的核心代码如下：
<div align=center>
<img src="https://user-images.githubusercontent.com/57986069/155450308-4cdc5ea5-8ef2-4741-ac99-ca487d56c230.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/57986069/155446062-621748fd-1b7b-4191-8bcd-533e36427f0e.png"/>
</div>

### 3.使用余弦相似度作为<query,document>的相关性分值的计算方式
- 在这里，我编写了如下所示的余弦相似度计算函数cos_sim。其输入为两个长度相同的列表（向量），第一个向量s1_cut_code为query的TF-IDF向量，第二个向量s2_cut_code为某一篇document的TF-IDF向量。其输出为query与该document的余弦相似度，并保留4位小数。
<div align=center>
<img src="https://user-images.githubusercontent.com/57986069/155446199-c04d7fc7-9469-485a-b0de-cdf489f58923.png"/>
</div>

- 在代码中，向量s1_cut_code对应cos-sim计算公式中的向量q，向量s2_cut_code对应cos-sim计算公式中的向量d。
<div align=center>
<img src="https://user-images.githubusercontent.com/57986069/155446991-bfd44ed9-1f48-4a8b-a22c-4bb99c299980.png"/>
</div>

### 4.增加索引保存机制，提高搜索系统的启动时间
- 在整个搜索系统中，最耗时是索引的构建，大约需要10分钟，而搜索仅仅只需要一两秒钟。
- 因此，我在这里加入了索引保存机制，在索引第一次构建好时，就将其另存为一个json文件（./dataset/built_index.json）。
- 如果在下一次启动时检测到有已构建好的索引时，就直接对其内容进行加载即可，无需再次计算构建（当然如果文档集有更新，也要重新构建索引）。
- ps.在加入了索引保存机制后，系统的启动时间由原来的10分钟变为了几秒钟。

### 5.使用HTML与JavaScript实现用户交互界面
- 具体为templates文件夹中的4个html文件——content.html、high_search.html、next.html、search.html。

### 6.使用Python的flask库实现前端与后台的数据传输
- 利用[flask](https://github.com/pallets/flask)实现前端（templates文件夹中的4个html文件）与后台（InvertIndex.py、SearchEngine.py）的数据传输，具体为Web.py文件。
