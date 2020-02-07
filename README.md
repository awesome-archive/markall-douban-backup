# 豆瓣数据备份

我的目标是备份豆瓣用户在豆瓣上的标记数据，而不是爬取豆瓣数据。所以获取信息的页面，我选取的是 **豆瓣标记信息列表** 的页面 **而不是条目详情页面**。

如该页面：[下一餐有折耳根想看的电影](https://movie.douban.com/people/hqweay/wish?start=15&sort=time&rating=all&filter=all&mode=grid)

## 结构说明

把获取豆瓣数据抽象为三步：获取页面、解析页面、数据存储。

核心是 `markall-douban-data-getter-func-creater.js`，用户可以通过该文件提供的方法创建一个 **获取某项信息的函数。**

如：

```javascript
let getDoubanWatchedMovies = createDoubanDataGetter("watchedMovies");
let getDoubanWishMovies = createDoubanDataGetter("wishMovies");
```

`markall-douban-data-getter-func-creater.js` 依赖 `get-douban-data-url.js` 、 `resolve-douban-data.js` 与 `save-douban-data-to-local.js`。

`get-douban-data-url.js`：通过参数获取豆瓣信息页面的 `url`。

`resolve-douban-data.js`：解析对应豆瓣信息页面的标签获取数据。

`save-douban-data-to.js`：备份数据的存储

**注意**：现在文件夹下的 resolve 文件是 `resolve-douban-data-to-json.js`，表示获取信息后以 JSON 格式存储。
同理，save 文件是 `save-douban-data-to-local.js`，表示存储至本地。

## 豆瓣信息页面的 URL 

可参考目录下的 `douban-data-urls.md`。

# 注意

豆瓣读过的书籍页面直接访问 403，但是先访问用户主页，再访问读过的书籍页面就没问题。对比两个页面的请求头发现相差一个 Cookie 字段，但是——我又没登录。看样子只要有该字段就没问题，服务器没做其它啥验证。

# 进度

现在框架已经搭起来了，接下来就是在 `resolve-douban-data.js` 中解析各个页面了...

不过影视对应的想看、在看、看过这三个页面的结构是一样的，其它的书籍、游戏也是如此。而且影视、书籍...之间的结构差异也不大，后面应该能很快搞定吧...（~~别别别立 flag~~）

## 已完成

### 影视

- [x] 电影—看过
- [x] 电影—想看
- [x] 影视—在看

### 书籍

- [x] 书籍—读过
- [x] 书籍—想读
- [x] 书籍—在读

### 音乐

- [x] 音乐—听过
- [x] 音乐—想听
- [x] 音乐—在听

### 游戏

- [x] 游戏—玩过
- [x] 游戏—想玩
- [x] 游戏—在玩

# 使用

执行 `yarn` 或 `npm` 导入依赖，在 `NodeJS` 环境执行。

## 样例

```javascript
// 参考 example.js
let createDoubanDataGetter = require('./markall-douban-data-getter-func-creater');

// 存储路径
const STORE_PATH = 'douban-data-backup/';

let getDoubanWatchedMovies = createDoubanDataGetter("watchedMovies");

getDoubanWatchedMovies("hqweay", STORE_PATH);
```

## 数据

参考 `/douban-data-backup` 下的文件。

# 其它

本来目的是想把 [MarkAll](https://github.com/hqweay/MarkAll) 里的豆瓣爬虫抽取为插件...

另外，话说保存数据格式为 `JSON`，其它格式可以使用在线转换工具嘛...

比如 JSON 转 CSV ：[https://www.bejson.com/json/json2excel/](https://www.bejson.com/json/json2excel/)

