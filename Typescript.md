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



##  三、Webpack配置

### webpack.config.js

```javascript
const path = require('path');
//引入html插件
const HTMLWebpackPlugin = require('html-weboack-plugin');
//引入clean插件
const {CleanWebpackPlugin} = require('clean-webpack-plugin');



//webpack中所有的配置信息都应该写在module.exports中
module.exports = {
    //指定入口文件
    entry:'./src/index.ts',

    //指定输出目录
    output: {
        path: path.resolve(__dirname, 'dist'),
        //打包后的文件名
        filename:'bundle.js',

        //告诉webpack不使用箭头函数
        environment:{
            arrowFunction: false
        }
    },

    //指定webpack打包时要使用模块
    module:{
        //指定要加载的规则
        rules: [
            {
                //指定的时规则生效的文件
                test:/\.ts$/,
                use:[
                    //配置babel
                    {
                        loader: 'babel-loader',
                        options: {
                            //设置预定义的环境
                            presets: [
                                [
                                    //指定环境插件
                                    '@babel/preset-env',
                                    //配置信息
                                    {
                                        targets: {
                                            'chrome': '60'
                                        },

                                        //指定corejs版本
                                        'corejs': '3',

                                        //使用corejs的方式： 按需加载
                                        'useBuildIns': 'useage'

                                    }
                                ]
                            ]
                        }
                    }
                ]
            },
            'ts-loader'
        ],
        
        //要排除的文件
        exclude: /node_modules/
    },

    //配置webpack的插件
    plugins: [
        new CleanWebpackPlugin(),
        new HTMLWebpackPlugin({
            template:'./src/index.html'
        })
    ],

    //用来设置引用模块: 解决模块化的问题
    resolve:{
        extensions: ['.ts', '.js']
    }
    
}
```







## 四、类

####  1、基础用法

```typescript
class Person {
    //实例属性，只能被实例访问
    name: '孙悟空'

    //静态方法，只能被类调用
    static age: 18

    //只读属性，只能读不能改
    readonly sex = '男'


    //方法
    sayHello() {
        console.log('Hello,大家好');

    }
}
const per = new Person();
```



#### 2、构造函数 和 this

```typescript
class Dog {
    //先申明实例属性
    name: string;
    age: number;

    //constructor被称为构造函数，构造函数会在 new Dog()时被执行
    constructor(name: string, age: number) {
        //this表示当前new出来的实例
        this.name = name;
        this.age = age;
    }

    bark() {
        console.log(`${this.name} 汪汪汪!`)
    }
}

const dog1 = new Dog('小黑', 2);
const dog2 = new Dog('旺财', 8);
```





#### 3、类的继承和super

```typescript
//1、定义Animal类
class Animal {
    name: string;
    age: number;

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }

    sayHello() {
        console.log(`动物再叫！`)
    }
}

/**继承的语法： 
 * --- class 子类 extends 父类 （eg: class Dog extends Animal）
 * 
 * 继承的作用：
 * --- 通过继承可以将多个类中共有的代码写在一个父类中，这样只需要写一次即可，且让所有子类同时
 * 拥有父类的属性和方法
 * 
 */


//2、定义Dog类，使Dog类继承Animal类
class Dog extends Animal {
    color: string;

    //子类重写constructor
    //tips：如果在子类中写了构造函数，在子类的构造函数中必须对父类的构造函数进行调用
    constructor(name: string, age: number, color: string) {
        super(name, age); //调用父类的构造函数： Animal.constructor.call(this,name, age)
        this.color = color;
    }

    //- 添加方法
    run() {
        console.log(`${this.name}会跑`);
    }

    //- 方法重写
    sayHello(): void {
        console.log(`汪汪汪!`)
    }
}


//3、定义Cat类，使Cat类继承Animal类
class Cat extends Animal {
    //- 添加方法
    jump() {
        console.log(`${this.name}会跳`);
    }

    //- 方法重写
    sayHello(): void {
        console.log(`喵喵喵！`)
    }
}
```

#### 4、抽象类

```typescript
/** 抽象类语法： 
 *      abstract class 父类 （eg: abstract class Animal）
 * 
 * 特性：
 *  --- 抽象类和其他类区别不大，只是不能用来创建对象（不能 new Animal()）
 *  --- 抽象类可以定义抽象方法
 */
abstract class Animal {
    name: string;
    age: number;

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }

    /* 
    * 1、抽象方法使用abstract 开头， 没有方法体
    * 2、抽象方法只能定义在抽象类中，子类必须对抽象类进行重写
    */
    abstract sayHello(): void;
}
```





## 五、接口

#### 1、接口的简单介绍

> 接口其实就是定义一个规范

```typescript
/* 用法1：
* 描述一个对象的类型
* type myType {} 不能重复定义
* interface MyInter {} 可以重复定义
*/
type MyType = {
    name: string,
    age: number,
    [propName: string]: any
}

//接口就是用来定义一个类（对象）的结构
interface MyInter {
    name: string;
    age: number;
}


//声明
const obj: MyType = {
    name: '孙悟空',
    age: 18
}

const obj2: MyInter = {
    name: '孙悟空',
    age: 18
}



/**
 *  用法2：
 *  接口可以在定义类的时候限制类的结构
 * tips： 接口中所有属性都不能有实际的值；接口只定义对象的结构，而不考虑实际值
 * 
 * */
interface MyInter2{
    name: string;

    sayHello(): void;
}

//定义类时，可以使类去实现一个接口
//tips: 什么是实现接口？ 实现接口就是使类满足接口的要求。
class MyClass implements MyInter2 {
    name: string;

    constructor(name: string){
        this.name = name;
    }

    sayHello(): void{
        console.log('哈哈哈！')
    }
}
```





## 六、属性的封装

```typescript
/** 
 * 定义一个人的类
 *  TS可以在属性前加修饰符 (static、 readonly、public、private)
 * public: 修饰的属性可以在任意位置被访问（修改）； 默认值
 * private: 私有属性只能在内部访问（修改）
 *  -- 通过在类中设置方法使得属性可以被外部访问
 *
 * protected: 受保护的属性；只能在当前类和当前类的子类中使用（访问、修改）
 * 
 **/
class Person {
    private _name: string;
    private _age: number;

    constructor(name: string, age: number) {
        this._name = name;
        this._age = age;
    }

    //读取器
    get name() {
        return this._name;
    }

    //存储器
    set name(value: string) {
        this._name = value
    }

    get age() {
        return this._age;
    }
    set age(value: number) {
        if (value >= 0) {
            this._age = value;
        }
    }
}

//使用
const per = new Person('猪八戒', 20);
per.name = '孙悟空';
per.age = 118;
```



## 七、泛型

> 若是在定义函数或者类时，遇到类型不明确就可以使用泛型

```typescript
/**
 * 用法1：指定单个泛型
 */
function fn1<T>(m: T): T {
    return m;
}

//可以直接调用具有泛型的函数
fn1(10);//-> 不指定泛型，TS可以自动对类型进行推断
fn1<string>('hello'); //指定泛型



/**
 * 用法2：指定多个泛型
 */
function fn2<T, K>(a: T, b: K): K {
    console.log(a);
    return b;
}

fn2<number, string>(12, 'hello')



/**
 * 用法3： 限制泛型的范围
 * <T extends Inter>: 表示泛型T必须是Inter的实现类（子类）
 */
interface Inter {
    length: number
}
function fn3<T extends Inter>(a: T): number {
    return a.length
}
fn3('hello');
fn3({ name: '孙悟空', length: 2 });




/**
 * 用法4： 类中使用泛型
 */
class MyClass<T>{
    name: T;
    constructor(a: T) {
        this.name = a;
    }
}

const mc = new MyClass<string>('孙悟空')
```

