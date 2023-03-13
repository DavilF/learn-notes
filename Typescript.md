# Typescript学习笔记

##  一、数据类型
> 原始数据类型： undefined、symbol、string、null、number、boolean
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

~~~

