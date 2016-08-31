## baidu-ocr-idcard
基于[百度OCR](http://apistore.baidu.com/apiworks/servicedetail/146.html)的二代身份证识别,for node

## 前言
1. 本库是当时的兴趣使然，后期可能不会维护
2. 建议不要将本库使用到生产环境，如果百度停止提供OCR服务，可能会导致本服务失效
3. 如果想提高识别出身份证正面的几率，可能需要对头像做模糊处理

## 功能
扫描第二代身份证，可在回调函数中进一步处理返回数据

## 限制
由于基于`百度ocr`，所以它有什么限制，这就有什么限制。目前比较受影响的限制有：
- 只支持jpg，且只能上传300KB以内的图片
- 上传身份证之前请先对身份证上的头像进行模糊处理，否则`百度ocr`可能无法识别。

## 安装
在命令行中输入：
```javascript
$ npm install baidu-ocr-idcard
```

## 使用
```javascript
var idcardOCR = require('../lib/baidu-ocr-idcard.js').create('your baidu api key');
    idcardOCR.scan(idcard, side, function(err, data) {
        if(err){
            res.json(err);
            return;
        }
        res.json(data);
    })
```

## 接口
### create
创建一个`IDCardOCR`对象，需传入`百度api key`。
```javascript
var idcardOCR = require('../lib/baidu-ocr-idcard.js').create('your baidu api key');
```

### scan
扫描第二代身份证，可在回调函数中进一步处理返回数据。
```javascript
IDCardOCR.prototype.scan(idcard, side, cb)
```
`idcard`为身份证图片的路径，请务必填写正确的图片路径，否则可能造成意想不到的后果。

`side`为身份证图片的正反面，正面为`'obverse'`，反面为`'reverse'`，其余所有值都为自动识别正反面，不过出于语义化的考虑，建议您填写`'auto'`。

`cb`为回调函数，它接受两个参数:
```javascript
idcardOCR.scan(idcard, side, function(err, data) {})
```
第一个参数是`err`,第二个参数是`data`，它们都是`PlainObject`，格式为`{errNum:...,errMsg:...,retData:...}`。

`err`：若`err`存在，则代表出错；不存在，则代表识别成功。`errNum`一定为`-1`（Number类型），`errMsg`有可能为对象或者字符串，`retData`一定为空字符串''。

`data`：`errNum`一定为`0`（Number类型），`errMsg`为字符串，`retData`的格式分两种情况，正面如下：
```javascript
{
	name:'string',			//姓名
	sex:'string',			//性别
	nation:'string',		//民族
	birthday:'string',		//生日
	residence:'string',		//住所
	idNum:'string',			//身份证号码
	side:'string'			//值为'obverse'
}
```
反面如下：
```javascript
{
	authority:'string',		//签发机关
	validPeriod:'string',	//有效期
	side:'string'			//值为'reverse'
}
```

## [Demo](https://github.com/DophinL/baidu-ocr-idcard/tree/master/examples)
运行前请先在根目录使用`$ npm install`安装依赖，然后`$ cd examples`，`$ node index.js`，然后在浏览器中输入`localhost:8888`即可启动。**请注意在`examples/index.js`中相应代码处填写自己的`baidu api key`，或者添加一个`BAIDU_APIKEY`环境变量**。

## 测试
安装依赖后，在项目根目录输入`$ mocha`即可。**请注意在`test/test.js`中相应代码处填写自己的`baidu api key`，或者添加一个`BAIDU_APIKEY`环境变量**。

## 感谢
[百度OCR](http://apistore.baidu.com/apiworks/servicedetail/146.html)

[百度OCR for node](https://github.com/JeremyWei/baidu-ocr)

## License
The MIT License (MIT)
