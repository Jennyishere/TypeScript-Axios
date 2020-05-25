# TypeScript-Axios
## 安装ts  
#### 全局安装  
npm install -g typescript  
#### 检查安装版本  
tsc -V  
## 编译ts  
tsc xxx.ts 编译 
node xxx.ts 运行
## 类型注解
：类型  如，:string
## 接口
概念：使用接口来描述一个对象的属性
## 属性接口
### 接口中可定义 确定属性、可选属性、任意属性、只读属性
1、确定属性
interface UserInfo {
  name: string;
  age: number;
}

const myInfo: UserInfo = {
  name: "haha",
  age: 20
};  
接口中约束好的确定属性，定义对象变量的时候 不能少

2、可选属性 
interface UserInfo {  
  name: string;  
  age: number;  
  sex?:string  
}  
const myInfo: UserInfo = {  
  name: "haha",  
  age: 20  
};  
接口中的可选属性，是表示在对象变量中可以不存在  

3、任意属性  
interface UserInfo {  
  name: string;  
  age: number;  
  sex?:string ;  
  [propName: string]:any;  
}  
const myInfo: UserInfo = {  
  name: "haha",  
  age: 20,  
  test1: 'lala',  
  test2: 'ff',  
  test3:123  
};  
注：一旦定义了任意属性，那么确定属性和可选属性的类型都必须是任意属性类型的子类； 

定义了任意属性后，对象变量中的属性个数才可以出现比接口的属性数量多的情况  

4、只读属性  
interface UserInfo {  
  readonly id: number;  
  name: string;  
  age: number;  
  sex?: string;  
  [propName: string]: any;  
}

const myInfo: UserInfo = {  
  id: 1,  
  name: "haha",  
  age: 20,  
  test1: "lala",  
  test2: "ff",  
  test3: 123  
};
只读属性也是确定属性，在对象变量定义的时候必须有值，此后不能修改  
## 函数接口
对方法传入的参数以及返回值进行约束  
interface Func {  
  (param1: string, param2: number): boolean;  
}  
let myFunc: Func = function(param1, param2){  
  return typeof param1 === "string" && typeof param2 === "number";  
};  
myFunc("22222", 1111);  
## 索引接口（不常用） 
可对数组或对象进行约束  
interface ArrIndex {  
    [index:number]: string  
}  
interface Obj {  
　　[index:string]:string  
}  
let myArr: ArrIndex = ['first','second']  
let myObj:Obj = {  
　　name: 'kkkk'  
}  
## 类接口  
对类进行约束，和抽象类有点相似  
 类实现接口implements  
接口继承接口  
接口继承类  
 
# 基础类型
## 布尔值  
let isDone: boolean = false
## 数字  
let decLiteral: number = 20  
## 字符串  
let name: string = 'bob'  
## 数组  
let list: number[] = [1, 2, 3] number还能规定数组里的内容类型  
## 元组 Tuple  
概念： 元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。  
let x: [string, number]  
## 枚举  
enum 类型是对 JavaScript 标准数据类型的一个补充  
## any  
在编程阶段还不清楚类型的变量指定一个类型  
## void  
void 类型像是与 any 类型相反，它表示没有任何类型。  
## null 和 undefined
## never  
never 类型表示的是那些永不存在的值的类型， never 类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型  
## object  
## 类型断言 
一是“尖括号”语法：  
let someValue: any = 'this is a string'  
let strLength: number = (<string>someValue).length  
另一个为 as 语法： 
let someValue: any = 'this is a string'  
let strLength: number = (someValue as string).length  
  
 
## 初始化项目  
### 使用TypeScript library starter框架   
git clone https://github.com/alexjoverm/typescript-library-starter.git ts-axios  
cd ts-axios  
npm install  

### 后台
使用express框架写一个简单的service，定义后端的路由
```js
const express = require('express')
const bodyParser = require('body-parser')
const webpack = require('webpack')
const webpackDevMiddleware = require('webpack-dev-middleware')
const webpackHotMiddleware = require('webpack-hot-middleware')
const WebpackConfig = require('./webpack.config')

const app = express()
const compiler = webpack(WebpackConfig)

app.use(webpackDevMiddleware(compiler, {
  publicPath: '/__build__/',
  stats: {
    colors: true,
    chunks: false
  }
}))

app.use(webpackHotMiddleware(compiler))

app.use(express.static(__dirname))

app.use(bodyParser.json())
app.use(bodyParser.urlencoded({ extended: true }))

const port = process.env.PORT || 8080
module.exports = app.listen(port, () => {
  console.log(`Server listening on http://localhost:${port}, Ctrl+C to stop`)
})
```

### 目标 可以使用axios发起请求
axios({
  method: 'get',
  url: '/simple/get',
  params: {
    a: 1,
    b: 2
  }
})
## 思路：function axios(config)==>xhr(config)==>confi:AxiosRequestConfig==>processConfig(config):url

### 创建入口文件
src 目录下，先创建一个 index.ts 文件，作为整个库的入口文件，然后我们先定义一个 axios 方法，并把它导出，如下：

```js

function axios(config) {
}
export default axios  
```

### 给 config 参数定义一种接口类型，定义 AxiosRequestConfig 接口类型
创建一个 types 目录，在下面创建一个 index.ts 文件，作为我们项目中公用的类型定义文件。

接下来我们来定义 AxiosRequestConfig 接口类型：

 ```js
 export interface AxiosRequestConfig {
  url: string
  method?: string
  data?: any
  params?: any
}  
//为了让 method 只能传入合法的字符串，我们定义一种字符串字面量类型 Method：
export type Method = 'get' | 'GET'
  | 'delete' | 'Delete'
  | 'head' | 'HEAD'
  | 'options' | 'OPTIONS'
  | 'post' | 'POST'
  | 'put' | 'PUT'
  | 'patch' | 'PATCH'
 ``` 
####  然后回到 index.ts，我们引入 AxiosRequestConfig 类型，作为 config 的参数类型，如下：

```js

import { AxiosRequestConfig } from './types'

function axios(config: AxiosRequestConfig) {
}

export default axios
```
#### 在 src 目录下创建一个 xhr.ts 文件，我们导出一个 xhr 方法，它接受一个 config 参数，类型也是 AxiosRequestConfig 类型  
import { AxiosRequestConfig } from './types'

export default function xhr(config: AxiosRequestConfig) {
}
接下来，我们来实现这个函数体逻辑，如下：

```js

export default function xhr(config: AxiosRequestConfig): void {
  const { data = null, url, method = 'get' } = config

  const request = new XMLHttpRequest()

  request.open(method.toUpperCase(), url, true)

  request.send(data)
} 
```
### 引入 xhr 模块
编写好了 xhr 模块，我们就需要在 index.ts 中去引入这个模块，如下：

``` js
import { AxiosRequestConfig } from './types'
import xhr from './xhr'

function axios(config: AxiosRequestConfig): void {
processConfig(config)
  xhr(config)
}
function processConfig (config: AxiosRequestConfig): void {
  config.url = transformUrl(config)
}
function transformUrl (config: AxiosRequestConfig): string {
  const { url, params } = config
  return bulidURL(url, params)
}
export default axios 
```

配置package.json
```js
"dev": "node examples/server.js"
```

## 处理请求 url 参数
### 需求分析
最终请求的 url 是 /base/get?a=1&b=2,这样服务端就可以通过请求的 url 解析到我们传来的参数数据了。实际上就是把 params 对象的 key 和 value 拼接到 url 上

```js
//参数值为数组
axios({
  method: 'get',
  url: '/base/get',
  params: {
    foo: ['bar', 'baz']
  }
})
//最终请求的 url 是 /base/get?foo[]=bar&foo[]=baz'。

//参数值为对象
axios({
  method: 'get',
  url: '/base/get',
  params: {
    foo: {
      bar: 'baz'
    }
  }
})
//最终请求的 url 是 /base/get?foo=%7B%22bar%22:%22baz%22%7D，foo 后面拼接的是 {"bar":"baz"} encode 后的结果。

//参数值为 Date 类型
const date = new Date()

axios({
  method: 'get',
  url: '/base/get',
  params: {
    date
  }
})
//最终请求的 url 是 /base/get?date=2019-04-01T05:55:39.030Z，date 后面拼接的是 date.toISOString() 的结果。

//特殊字符支持
//对于字符 @、:、$、,、、[、]，我们是允许出现在 url 中的，不希望被 encode。

axios({
  method: 'get',
  url: '/base/get',
  params: {
    foo: '@:$, '
  }
})
//最终请求的 url 是 /base/get?foo=@:$+，注意，我们会把空格 转换成 +。

//空值忽略
//对于值为 null 或者 undefined 的属性，我们是不会添加到 url 参数中的。

axios({
  method: 'get',
  url: '/base/get',
  params: {
    foo: 'bar',
    baz: null
  }
})
//最终请求的 url 是 /base/get?foo=bar。

//丢弃 url 中的哈希标记
axios({
  method: 'get',
  url: '/base/get#hash',
  params: {
    foo: 'bar'
  }
})
//最终请求的 url 是 /base/get?foo=bar

//保留 url 中已存在的参数
axios({
  method: 'get',
  url: '/base/get?foo=bar',
  params: {
    bar: 'baz'
  }
})
//最终请求的 url 是 /base/get?foo=bar&bar=baz
```

### helpers/util.ts： 用于判断传入的参数类型
```js
const toString = Object.prototype.toString

export function isDate (val: any): val is Date {
  return toString.call(val) === '[object Date]'
}

export function isObject (val: any): val is Object {
  return val !== null && typeof val === 'object'
}
```
### helpers/url.ts： 处理url
```js
import { isDate, isObject } from './util'

function encode (val: string): string {
  return encodeURIComponent(val)
    .replace(/%40/g, '@')
    .replace(/%3A/gi, ':')
    .replace(/%24/g, '$')
    .replace(/%2C/gi, ',')
    .replace(/%20/g, '+')
    .replace(/%5B/gi, '[')
    .replace(/%5D/gi, ']')
}

export function bulidURL (url: string, params?: any) {
  if (!params) {  //如果传入参数为空 不用处理
    return url
  }
  const parts: string[] = []   //定义一个字符串数组

  Object.keys(params).forEach((key) => {  //遍历参数对象
    let val = params[key]
    if (val === null || typeof val === 'undefined') {    //过滤掉空的
      return
    }
    let values: string[]
    if (Array.isArray(val)) {
      values = val
      key += '[]'
    } else {
      values = [val]
    }
    values.forEach((val) => {
      if (isDate(val)) {
        val = val.toISOString()
      } else if (isObject(val)) {
        val = JSON.stringify(val)
      }
      parts.push(`${encode(key)}=${encode(val)}`)
    })
  })

  let serializedParams = parts.join('&')

  if (serializedParams) {
    const markIndex = url.indexOf('#')
    if (markIndex !== -1) {
      url = url.slice(0, markIndex)
    }

    url += (url.indexOf('?') === -1 ? '?' : '&') + serializedParams   //拼接起来
  }

  return url
}
```
