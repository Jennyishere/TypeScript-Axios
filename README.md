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

### 目标 可以使用axios发起请求
axios({
  method: 'get',
  url: '/simple/get',
  params: {
    a: 1,
    b: 2
  }
})

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
  xhr(config)
}

export default axios 
```
