# BookSourceRepository

The repository is based on GitHub to provide an open book source repository for the Deepink, and welcomes the introduction of new book sources.



##### 目前已收录

| 书源 | API | 类型 | 排行榜 | 账号 | 版本 |
| :----: | ------------- | :--: | :----: | :----: | :---: |
| 废文网 | https://www.sosad.fun/ | 网文 | 0 | 是 | 1.0.6 |
| 晋江 | http://www.jjwxc.net | 网文 | 0 | 是 | 1.0.1 |
| 纵横 | http://www.zongheng.com | 网文 | 0 | 是 | 1.0.1 |
| 飞卢| https://b.faloo.com| 网文 | 0 | 是 | 1.0.1 |
| 铁血读书| https://book.tiexue.net| 网文 | 5 | 是 | 1.0.5 |
| i猎奇| https://ilieqi.net/| 网文 | 0 | 是 | 1.1.0 |
| 连城读书| http://www.lcread.com/| 网文 | 0 | 是 | 1.1.0 |
| 书耽 | https://www.shubl.com| 网文 | 0 | 是 | 1.0.1 |
| 17k | https://www.17k.com| 网文 | 0 | 是 | 1.1.4 |
| 汤圆创作 | http://www.itangyuan.com| 网文 | 0 | 是 | 1.0.1 |
| 刺猬猫 | https://www.ciweimao.com| 网文 | 0 | 是 | 1.0.3 |
| 第八区 | http://www.dibaqu123.com| 网文 | 0 | 否 | 1.0.1 |
| 书山 | http://shushan.zhangyue.net| 网文 | 0 | 是 | 1.0.1 |
| 耽美小说网 | http://www.zongheng.com | 网文 | 0 | 否 | 1.0.1 |
| 梧桐中文 | http://www.wtzw.com | 网文 | 0 | 否 | 1.0.2 |
| 长夜读 | https://www.changyedu.com| 网文 | 0 | 否 | 1.0.2 |
---


### 勉强能搜索看书的 后两个贼慢 登陆验证还没怎么加
* 晋江 (普通章节和V章节分开了)
* 纵横
* 废文
* 连城读书
* 第八区
* 17k(卷名位置获取有问题)
* 爱猎奇
* 指尖阅读
* 铁血读书
* 飞卢
* 汤圆创作
* 梧桐中文
* 新书在线
* 耽美小说网
* 长夜读


### 只能搜索的
* 书山（获取目录问题）
* 刺猬猫
* 书耽







### 书源制作
准备工作，需了解基础的Html/CSS/JavaScript内容，以及Jsoup选择器功能。

参考内容：[Jsoup选择器文档](https://jsoup.org/apidocs/org/jsoup/select/Selector.html) 和 [Jsoup调试神器](https://try.jsoup.org)


#### 1. 源网站
教程以 [作品集](http://zuopinj.com) 为例讲解。

#### 2. 基础信息
根据源网站信息创建一个书源文件：作品集.json

```
{
  "name": "作品集",
  "version": 100,
  "category": 0,
  "url": "http://zuopinj.com",
  "charset": "utf-8"
}
```
属性讲解

| 属性 | 含义 | 讲解 |
| :-: | :-: | --- |
| name | 书源名 | 推荐使用源网站名字清晰明了|
| version | 版本号 | 新建写100（1.0.0）当内容变化时递增，如101（1.0.1）|
| category | 类别 | 0（出版）、1（网文）、2（轻小说）、3（综合）|
| url | 源网站 | 务必真实有效，否则会影响到后续属性调用|
| charset | 字符集 | utf-8 gbk gbk2312 影响search等请求时的编码，非源网站的编码 |

#### 3. 搜索
书源必须具有搜索功能，填充入上一步的json中如下：

```
{
  ... 第2步内容
  "search": {
    "link": "http://so.zuopinj.com/search/index.php@post->tbname=bookname&show=title&tempid=3&keyboard=${key}",
    "list": ".search-bookele"
  }
}
```
属性讲解

| 属性 | 含义 | 讲解 |
| :-: | :-: | --- |
| link | 地址 | ${key}代表搜索关键词，搜索时自动替换为用户输入的词； @post-> 表明后续使用POST方式传递参数，如果是GET方式不需要特殊声明，例如 http://search.com?key=${key} 意味key使用GET方式请求内容 |
| list | 结果 | 通过Jsoup选择结果元素 |

举例说明：

请求地址：http://so.zuopinj.com/search/index.php@post->tbname=bookname&show=title&tempid=3&keyboard=剑

请求结果：

![](images/3.png)

##### 使用.search-bookele选择出```<div class="search-bookele">```元素，作为搜索结果进行解析。

#### 4. 元数据
通过第3步获得到搜索列表时，需要解析出书籍元数据用于页面显示。此时在json中再次补充：

```
{
  ... 第2步内容
  "metadata": {
    
  }
  "search": { ... 第3步内容 }
}
```

##### 书源属性讲解

- name ：书源名字
- version ：版本号，100即1.0.0
- category : 类别，0（出版）、1（网文）、2（轻小说）、3（综合）
- url ：来源链接（真实有效）
- charset ：字符集 utf-8 gbk gbk2312
- metadata： 元数据（每个子属性支持多个匹配，适应不同场景，比如排行榜或搜索）
    - name： 书名
    - author： 作者
    - cover：封面
    - summary： 摘要
    - category： 分类，例如：玄幻，武侠，连载等
    - status：状态（选填），完结 或 连载
    - update：更新时间（选填）（不填将无法检查更新）
    - lastChapter：更新章节（选填）（不填将无法检查更新）
    - link： 书籍详情地址
    - catalog：目录地址（不填视为目录与详情同一个地址）
- catalog： 目录
    - list：章节列表
    - orderBy：排序方式 0（分卷正序章节正序）1（分卷倒序章节倒序）2（分卷正序章节倒序）3（分卷倒序章节正序）
    - booklet：分卷（选填）
        - name：分卷名
        - list：章节列表
    - chapter： 章节
        - name：章节名
        - link：章节链接
- content：内容
    - text： 文本
    - filter：内容过滤（数组）
- search：搜索
    - link：链接
    - list：列表
- rank：排行榜（可选）
    - link：链接（数组）
        - name：名字
        - link：链接
    - list：列表
    - page：分页
        - index：起始索引
        - limit：单页数量
        - begin：起始页面拼接字符
        - next：下一页拼接字符
