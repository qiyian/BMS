# 零食妞后台管理系统。页面使用Bootstrap+AngularJs构建。服务器使用Express+Mysql基于Node.JS实现。 #
## 项目描述 ##

```

	git clone git@github.com:qiyian/BMS.git
```

 mysql数据库建立数据库，数据库名test，表名：good 结构如下

![](http://ogbpwv1g9.bkt.clouddn.com/zzz.jpg)

![](http://ogbpwv1g9.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20161108210657.jpg)

 newexpress文件夹下 npm i 安装依赖包 npm run build 启动服务

```

	npm start

```


```

	//npm版本3.3.12
	npm -v
	//node版本v5.2.0
	node -v
```
### 使用git，通过npm包管理器，安装express 和express种子生成器（express-generator 生成目录文件）； ###

```

	//express
	cnpm i	express -g

	//express-generator
	npm install express-generator -g

	

```

/bin: 用于应用启动

/public: 静态资源目录

/routes：可以认为是controller（控制器）目录

/views: jade模板目录，可以认为是view(视图)目录

app.js 程序main文件

目录结构

![](http://ogbpwv1g9.bkt.clouddn.com/1.jpg)
![](http://ogbpwv1g9.bkt.clouddn.com/2.jpg)
![](http://ogbpwv1g9.bkt.clouddn.com/3.jpg)

### 3、启动express项目 ###

```
	npm start

```

通过localhost就可以访问了

### 新建mysql建表 ###
路由信息新建一个conf文件夹，再新建db.js
表名：test


### 接口说明 ###

通过id删除商品：http://localhost:3100/goodDel?id=115

添加商品：http://localhost:3100/goodAdd?
name=123&desc=123&price=123&sum=234

通过id修改商品：http://localhost:3100/goodUpdate?name=123&desc=2313&price=12&sum=12&id=1

查询所有商品：http://localhost:3000/goodAll

查询单个商品：http://localhost:3100/goodById?id=112

### 程序运行思路 ###

1、浏览器访问地址(localhsot)访问路由文件/routes下的index.js

```
	var express = require('express');
	var router = express.Router();
	//关联主程序
	var goodlist = require('../good/goodlist.js');
	
	/* GET home page. */
	//进入主页面信息
	router.get('/', function(req, res, next) {
	  res.render('index', { title: 'Express' });
	});
	
	//增
	router.get('/goodAdd',function(req,res,next){
		goodlist.goodadd(req,res,next);
	});
	
	//删
	router.get('/goodDel',function(req,res,next){
		goodlist.gooddelete(req,res,next);
	});
	//改
	router.get('/goodUpdate',function(req,res,next){
		goodlist.goodupdate(req,res,next);
	});
	//查
	router.get('/goodAll',function(req,res,next){
		goodlist.goodAll(req,res,next);
	});
	router.get('/goodById',function(req,res,next){
		goodlist.goodById(req,res,next);
	});
	
	module.exports = router;
```
2、访问localhost/goodAll进入查询全部商品路由，路由调用goodlist里的方法goodAll （注意：把goodlist导入进来goodlist.js）

```

	//得到所有商品 在路由routes调用本方法，
		goodAll: function (req, res, next) {
	        pool.getConnection(function(err, connection) {
	            connection.query($sql.goodAll, function(err, result) {
	                jsonWrite(res, result);
	                connection.release();
	            });
	        });
	    }

```
3、这个方法调用goodsql.js里面的sql语句 ，并返回相应结果jsonwrite
```

	var good={
		//增
		goodinsert:'INSERT INTO `good` (`id`,`name`,`desc`,`price`,`sum`) VALUES(0,?,?,?,?)',
		//删
		gooddelete: 'delete from good where id=?',
		//改
		goodupdate:'UPDATE `good` SET `name`=?,`desc`=?,`price`=?,`sum`=? WHERE `id`=?',
	    //查
	    goodAll: 'select * from good',
	    goodById: 'select * from good where id=?'
	}
	
	module.exports=good;
```
###解决问题###
1、解决跨域问题（在app.js里面添加）
```
	//设置跨域访问
	app.all('*', function(req, res, next) {
	    res.header("Access-Control-Allow-Origin", "*");//*表示允许的域名地址，本地则为'http://localhost' 
	    res.header("Access-Control-Allow-Headers", "X-Requested-With");
	    res.header("Access-Control-Allow-Methods","PUT,POST,GET,DELETE,OPTIONS");
	    res.header("X-Powered-By",' 3.2.1')
	    res.header("Content-Type", "application/json;charset=utf-8");
	    next();
	});
```	

2、拼凑sql语句

	var id = +req.query.id; // 为了拼凑正确的sql语句，这里要转下整数
  
### 界面 ###
![](http://ogbpwv1g9.bkt.clouddn.com/4.jpg)
### 增加 ###
![](http://ogbpwv1g9.bkt.clouddn.com/5.jpg)
### 修改 ###
![](http://ogbpwv1g9.bkt.clouddn.com/6.jpg)
### 排序 ###
![](http://ogbpwv1g9.bkt.clouddn.com/7.jpg)
