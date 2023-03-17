# Typescript学习笔记

##  一、数据类型
> 原始数据类型： undefined、symbol、string、null、number、boolean
>
> 
>
> 记忆口诀：U SO N B（你如此牛逼） 对应关系 undefined string object null number boolean


### 1. 简单的类型
~~~ typescript
//0、常用类型 【string, number, boolean】---------------
let str: string = 'hello';
let num: number = 123;
let bln: boolean = true;



//1、【字面量】字面量类型申明（限制变量的值就是字面量的值，不推荐）---------------
let a: 10;
a = 10;



//2、【联合类型】 可以使用 | 来连接多个类型---------------
let b: 'male' | 'femal';
b = 'male';
b = 'femal';
let c: number | string
c = 1;
c = 'hello';

//&连接： 表示同时满足
let k: {name: string} & {age: number};
k = {name:'adc', age:18}



//3、【any类型】 表示的是任意类型，一个变量设置类型为any后相当于对变量关闭了TS的类型检测---------------
//tips: 开发时不建议使用 any 类型
let d: any; //显示声明
//let d; 隐式声明，声明变量若不指定类型，则ts解析器会自动判断变量的类型为any
d = 10;
d = 'hello';
d = true;
let d1: string;
d1 = d; // 注意点： any类型可以赋值给任意变量



//4、【unknown】 表示未知类型,unknown 实际上就是一个类型安全的any---------------
let e: unknown;
e = 10;
e = true;
e = 'hello';
let s: string;
s = e; //error； 注意：unknown 类型的变量们不能赋值给其他变量



//5、【类型断言】可以用来告诉解析器变量的实际类型---------------
/*
* 类型断言语法：
*  变量 as 类型
*  <类型>变量
*/
let s: string;
s = e as string;
s = <string>e;



//6、【void】表示空，以函数为例，就是没有返回值---------------
function fn(): void {
    console.log(1); //此处 return undefined 均表示没有返回值
}



//7、【never】表示永远不会有返回结果（用的少）---------------
function fn2(): never {
    throw new Error('报错了！');  //代码报错就不会再有返回值了
}


//8、【对象】---------------
/*
* {} 用来指定对象中含有哪些属性
* 语法： {属性名: 属性值, 属性名: 属性值}
* 在属性后面加上?，表示该属性时可选的
* [propName: string]: any 表示任意类型的属性
*/
let obj: { name: string, age?: number, [propName: string]: any }
obj = { name: ' UZI', age: 20, pos: 'ADC' }

/**
 * 设置函数结构的类型声明
 * 语法：（形参1:类型, 形参2: 类型, ...）=> 返回值类型
 */
let fun1: (a: number, b: number) => number;
fun1 = function (a, b) { return a + b }



//9、【数组】---------------
/**
 * 表示数组的语法：
 * 1、类型[]; 
 * 2、Array<类型>
*/
let arr1: string[];
arr1 = ['hello', 'world']

let arr2: Array<string>;
arr2 = ['hello', 'world']



//10、【元祖,tuple】表示固定长度的数组---------------
/* 
* 语法: [类型1, 类型2，...]
*/
let tuple1: [string, number];
tuple1 = ['h', 1]



//10、【enum 枚举】常用来表示把所有的值列举出来 ---------------
enum Gender {
    Male,
    Female
}
let user:{name: string, gender: Gender};
user = {
    name: 'Admin',
    gender: Gender.Male
};
console.log(user.gender === Gender.Male) // true



//11、类型的别名： 简化类型的使用 ---------------
/* 
* 语法： type 别名 = 类型
*/
type myStringType = string;
let str1: myStringType;
str1 = 'hello';

type myGroup = 1 | 2| 3 | 4 | 5;
let st2: myGroup;
st2 = 2;

~~~




##  二、编译选项

### 1、从简单到全面的编译演化

> 想要编译单个文件 

```js
tsc app.ts
//=> 出现问题： 我改一次app.ts里的东西，我就得执行一次上面的命令，一个字“烦”， 怎么解决？？

tsc app.ts -w 
//=> 进入编译监视模式，此时解决了改一次编一次的问题。 
//那么第二个问题产生了，我有多个文件，难道开多个终端开编译监视模式监听文件修改吗? 一个字"烦"， 怎么解决？？

//于是救世主的 'tsconfig.json' 出现
```


### 2、自动编译整个项目的ts

> 1、若直接使用 tsc指令，则可以自动将当前目录下的所有文件编译为js文件。
>
> 2、能直接使用tsc指令编译整个项目的前提是： 在整个项目的根目录下创建一个ts的配置文件`tsconfig.json`。

### 3、tsconfig.json 详解

> 1、首先这是个JSON的配置，但是里面居然可以是写注释的。
>
> 2、tsconfig.json 是ts编译器的配置文件，ts编译器可以根据它的配置来对代码进行编译。

```json
{
    /* 
        "include": 用来指定哪些ts文件需要被编译，值是个数组，可指定多个路径
        路径: ** 表示任意目录
               * 表示任意文件
    */
    "include": [
        "./src/**/*"
    ],
    
    /*【最重要】"compilerOptions"：编译器的选项*/
    "compilerOptions": {
        //指定ts被编译为ES的版本；值（'es3', 'es5', 'es6', 'es2015', 'es2016', 'es2017', 'es2018', 'es2019'...）
        "target": "es5",

        //指定ts编译使用的模块化规范；值('common.js', 'amd', 'system', 'umd', 'es6', 'es2015', 'es2020', 'esnext')
        "module": "es2015",

        //指定项目中要使用的库 （浏览器中运行的代码一般不需要设置）
        //"lib": []

        //用来指定编译后的文件放置的目录
        "outDir": "./dist",

        //用来将代码合并为1个文件
        //"outFile": "./dist/bundle.js"

        //是否对js文件进行编译，默认false
        "allowJs": false,

        //是否坚持js代码是否符合ts语法规范，默认false
        "checkJs": false,

        //是否移除注释，默认false
        "removeComments": false,

        //不生成编译后的文件，默认false
        "noEmit": false,

        //当ts文件有错误时不生成编译后的文件，默认false
        "noEmitOnError": true,

        /* 下面的配置是跟ts的语法检查相关的配置
        -------------------------------------------------------------- */
        //所有严格检查的总开关，这里时true,下面全是true； false则下面检查全是false
        "strict": true,

        //用来设置编译后的文件是否使用严格模式“use strict”; 默认false
        "alwaysStrict": false,

        //设置是否允许隐式的any类型; 默认false
        "noImplicitAny": false,

        //不允许不明确类型的this; 默认false
        "noImplicitThis": false,

        //是否严格检查空值; 默认false
        "strictNullChecks": false
    }
}
```

