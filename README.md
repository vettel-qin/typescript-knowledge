# 1、TypeScript 是什么

TypeScript 是一种由微软开发的开源、跨平台的编程语言。它是 JavaScript 的超集，最终会被编译为 JavaScript 代码。

## 1.1 TypeScript 与 JavaScript 的区别

| TypeScript                                     | JavaScript             |
| ---------------------------------------------- | ---------------------- |
| 可以在编译期间发现并纠正错误                   | 只能在运行时发现错误   |
| 强类型，支持静态和动态类型                     | 弱类型，没有静态型选项 |
| 最终被编译成 JavaScript 代码，使浏览器可以理解 | 可以直接在浏览器中使用 |
| 支持泛型、接口等                               | 不支持泛型、接口等     |

# 2、TypeScript 基础类型

## 2.1 Boolean 类型

```
  let isBoolean: boolean = false;
  // ES5: var isBoolean = false;
```

## 2.2 Number 类型

```
  let count: number = 10;
  // ES5: var count = 10;
```

## 2.3 String 类型

```
  let name: string = "vettel";
  // ES5: var name = "vettel";
```

## 2.4 Symbol 类型

```
const sym = Symbol();

let obj = {
  [sym]: "vettel",
};

console.log(obj[sym]); // vettel
```

## 2.5 Array 类型

```
  let arr: number[] = [1,2,3];
  // ES5: var arr = [1,2,3];

  let arr: Array<number> = [1,2,3]; // Array<number>泛型语法
  // ES5: var arr = [1,2,3];
```

## 2.6 Enum 类型

使用枚举我们可以定义一些带名字的常量，可以清晰地表达意图，TypeScript 支持数字和字符串的枚举。

### 2.6.1 数字枚举

```
  enum Response {
    Yes,
    No,
  }

  let dir: Response = Response.Yes;
  console.log(dir); // 0
```

默认情况下，Yes 的初始值为 0，其余的成员会从 1 开始自动增加。就是 Response.Yes 的值为 0，Response.No 的值为 1。

当然我们也可以设置 Yes 的初始值

```
  enum Response {
    Yes = 1,
    No,
  }

  let dir: Response = Response.No;
  console.log(dir); // 2
```

### 2.6.2 字符串枚举

在 TypeScript 2.4 版本，允许我们使用字符串枚举。在一个字符串枚举里，每个成员都必须用字符串字面量，或另外一个字符串枚举成员进行初始化。

```
  enum Response {
    Yes = "YES",
    No = "NO,
  }

  let dir: Response = Response.Yes
  console.log(dir); // YES
```

### 2.6.3 异构枚举

异构枚举的成员值是数字和字符串的组合

```
  enum Response {
    Yes = "YES",
    No = 1,
  }

  console.log(Response.Yes); // YES
  console.log(Response[1]); // No

```

## 2.7 Any 类型

在 TypeScript 中，任何类型都可以被归为 any 类型。这让 any 类型成为了类型系统的顶级类型（也被称作全局超级类型）。

```
  let value: any;
  value = true; // OK
  value = 10;   // OK
  value = [];   // OK
```

## 2.8 Unknown 类型

就像所有类型都可以赋值给 any,所有类型也都可以赋值给 unknown

```
  let value: unkonwn;
  value = true; // OK
  value = 10;   // OK
  value = [];   // OK
```

## 2.9 Tuple 类型

在 JavaScript 中是没有元组的，元组是 TypeScript 中特有的类型，其工人方式类似于数组。

```
  let tuple: [string, boolean] = ["vettel", true];
```

## 2.10 Void 类型

void 类型像是跟 any 类型相反，它表示没有任何类型。当一个函数没有返回值时，通常会见到其返回值类型是 void

```
  function fn():void {
    ...
  }
```

## 2.11 Null 和 Undefined 类型

TypeScript 里，undefined 和 null 两者有各自的类型分别为 undefined 和 null。
默认情况下 null 和 undefined 是所有类型的子类型，就是说你可以把 null 和 undefined 赋值给 number 类型的变量。

```
  const u: undefined = undefined;
  const n: null = null;
```

## 2.12 Never 类型

never 类型表示的是那些永不存在的值的类型。在函数中它通常表现为抛出异常或无法执行到终止点（例如无限循环）

```
  // 抛出异常的函数的返回值为never
  function error(msg: string): never {
    throw new Error(msg);
  }

  // 无法执行到终止点
  function loop(): never {
    while(true) {}
  }
```

# 3、TypeScript 断言

## 3.1 类型断言

有时候你会遇到这样的情况，你会比 TypeScript 更了解某个值的详细信息。这时候可能通过类型断言告诉编译器，“相信我，我知道自己在干什么”
类型断言有两种形式：

### 3.1.1 "尖括号"语法

```
  let value: any = "this is a string";
  const strLength: number = (<string>value).length;
```

### 3.1.2 as 语法

```
  let value: any = "this is a string";
  const strLength: number = (value as number).length;
```

## 3.2 非空断言

在上下文中当类型检查器无法断定类型时，操作符！可以用于断言操作对象是非 null 和非 undefined 类型。具体而言，x!将从 x 值域中排除 null 和 undefined.

### 3.2.1 忽略 undefined 和 null 类型

```
  function fn(val: string | undefined | null) {
    const str: string = val; // Error
    const ignoreUndefinedAndNull: string = val!; // OK
  }
```

### 3.2.2 调用函数时忽略 undefined 类型

```
  type NumGenerator = () => number;

  function myFunc(numGenerator: NumGenerator | undefined) {
    const num1 = numGenerator(); // Error
    const num2 = numGenerator!(); // OK
  }
```

### 3.2.3 确定赋值断言

在 TypeScript2.7 版本中引入了确定赋值断言，即允许在实例属性和变量后面放置一个！号，从而告诉 TypeScript 该属性会被明确地赋值

```
let x: number;
const y: number = 10;
fn();
console.log(x + y); // Error
function fn() {
  x = 10;
}
```

上述代码的异常是说变量 x 在赋值前被使用了。要解决这个问题可以使用确定赋值断言。

```
let x!: number;
const y: number = 10;
fn();
console.log(x + y); // OK
function fn() {
  x = 10;
}
```

# 4、联合类型和类型别名

## 4.1 联合类型可以通过管道（|）将变量设置多种类型。

```
  let val: string | number;
  val = 12;
  val = "vettel";
```

## 4.2 类型别名

类型别名用来给一个类型起个新名字

```
  type Msg = string | string[];
  let val: Msg;
```

# 5、交叉类型

在 TypeScript 中交叉类型是将多个类型合并为一个类型，通过&运算符可以将现有的多种类型叠加到一起成为一种类型

```
  interface A {
    name: string;
    age: number;
  }

  interface B {
    name: string;
    gender: string;
  }

  let a: A & B;

```

# 6、TypeScript 接口

在 TypeScript 里，接口的作用就是类型命名和为你的代码或第三方代码定义契约。
