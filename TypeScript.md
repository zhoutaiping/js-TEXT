## 接口   interface   定义属性类型

```
interface LabelledValue {
  label: string;
}

```

## 类   class 

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

2：修饰符  公共public，私有private 、受保护protected 、只读readonly  的修饰符
   当创建类的实例时，默认会调用类对应的constructor构造函数。

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