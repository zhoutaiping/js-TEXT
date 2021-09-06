## 基础类型

```
string number boolean null undefined Array emum  any void never object

any : 任何类型
void: 没有任何类型
----------------------------------------------
function warnUser(): void {
  console.log("This is my warning message");
}
let unusable: void = undefined;
----------------------------------------------
never: 永不存在的值的类型
-----------------------------------------------
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
  throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
  return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
  while (true) {
  }
}
------------------------------------------------

```

## 接口 interface 定义属性类型

```
1: 元素必须
interface LabelledValue {
  label: string;
}

2：不全都是必需的
interface SquareConfig {
  color?: string;
  width?: number;
}


function printLabel(labelledObj: LabelledValue) {
  console.log(labelledObj.label);
}

let myObj = {size: 10, label: "Size 10 Object"};
printLabel(myObj);

```

## 类 class

```
1：继承
class Animal {
    move(distanceInMeters: number = 0) {
        console.log(`Animal moved ${distanceInMeters}m.`);
    }
}

class Dog extends Animal {
    bark() {
        console.log('Woof! Woof!');
    }
}

const dog = new Dog();
dog.bark();
dog.move(10);
dog.bark();

2：修饰符  公共public，私有private 、受保护protected 、只读readonly  的修饰符 当创建类的实例时，默认会调用类对应的constructor构造函数。

class Animal {
    public name: string; //外部可以重写

    private private_name: string;  // 外部不能重写 私有成员不能被外部访问

    protected protected_name: string;  // 外部不能重写, 继承的派生可以引用  被保护的成员不能被外部访问

    readonly readonly_name: string;

    protected constructor(theName: string) { this.protected_name = theName; }
}



// Employee 能够继承 Animal
class Employee extends Animal {
    private department: string;

    constructor(protected_name: string, department: string) {
        super(protected_name);
        this.department = department;
    }
}


let howard = new Employee("Howard", "Sales");  // √

let john = new Animal("John"); // 错误: 'Person' 的构造函数是被保护的.

let read = new Animal()
read.readonly_name = '1' // 错误! readonly_name 是只读的.


```

## 函数

```
let sampleVariable: { bar: number };
function foo(sampleParameter: { bar: number }) {
  //
}


interface Foo {
  foo: string;
}

// Return type annotated as `: Foo`
function foo(sample: Foo): Foo {
  return sample;
}

// 可选参数
function foo(bar: number, bas?: string): void {
  // ..
}
// 设置默认值
function foo(bar: number, bas: string = 'hello') {
  console.log(bar, bas);
}


foo(123);
foo(123, 'hello');
```
