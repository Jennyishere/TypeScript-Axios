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
 
