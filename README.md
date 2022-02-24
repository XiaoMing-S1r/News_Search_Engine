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
