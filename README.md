# my-elema

> my-elema webapp单页应用

由于一直忙于大论文、小论文、专利的写作，好久没看前端知识了。目前终于把这个仿饿了么WebApp实现了。收获还是蛮大的。
所有咬牙坚持过的岁月都值得回味，它教会了我不再惶恐未来。只有自己强大，才不会恐惧现实的可怕！


##项目实现的功能：
商品预览：可以预览商家提供的所有商品
商品详情：用于展示商品的具体信息
添加购物车：用户可以将喜欢的商品添加到购物车，也可在购物车中删除商品，在把商品加入到购物车的时候利用贝塞尔曲线实现了抛物线动画
查看评论：可以查看用户对商品的评论，既可以查看所有的评论也可以查看只有内容的评论。

项目开发所需要的环境：（我用的是vuejs1.0 因为vuejs刚入门，所以从1开始的，如果安装的是vue2.0的，
可以根据我的这篇博客修改成vue1.0  http://blog.csdn.net/diligentkong/article/details/75040960）
##安装：
1、安装node：http://nodejs.cn/download/
git：https://git-scm.com/downloads

3、安装vue脚手架工具vue-cli:

$npm install vue-cli -g
4、进入代码根目录安装依赖：
$ npm install --save-dev
5、运行命令：
$ npm run dev
6、发布代码：
$ npm run build
发布完代码后会生成dist目录，dist目录包括 static (js和css)，index.html保存着项目的所有可运行的代码。

注意不能直接打开index.html运行，需要开启http server运行代码。
在根目录下新建一个js文件，名字product.server.js，里面写配置文件
/**
 * Created by Administrator on 2017/11/26.
 */
var express = require('express');
var config = require('./config/index');

var port = process.env.PORT || config.build.port;

var app = express();

var router = express.Router();

router.get('/', function (req, res, next) {
  req.url = '/index.html';
  next();
});

app.use(router);

var appData = require('./data.json');
var seller = appData.seller;
var goods = appData.goods;
var ratings = appData.ratings;

var apiRoutes = express.Router();

apiRoutes.get('/seller', function (req, res) {
  res.json({
    errno: 0,
    data: seller
  });
});

apiRoutes.get('/goods', function (req, res) {
  res.json({
    errno: 0,
    data: goods
  });
});

apiRoutes.get('/ratings', function (req, res) {
  res.json({
    errno: 0,
    data: ratings
  });
});

app.use('/api', apiRoutes);

app.use(express.static('./dist'));

module.exports = app.listen(port, function (err) {
  if (err) {
    console.log(err);
    return
  }
  console.log('Listening at http://localhost:' + port + '\n')
});

这里需要注意的要在 cofig中 index.js中的build中定义一个port：9001

$ node prod.server.js
打开浏览器输入localhost:9001看效果。



