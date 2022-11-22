# OPP的理解

### 1.为什么javaScript设计成基于原型的模式

当我们创建一个构造函数，当前对象无法与其他对象产生联系，例如有的时候我们期望共有属性:

```
function People(age) {
    this.age = age
    this.nation = 'China'
}

// 父子大小明
let juniorMing = new People(12)
let seniorMing = new People(38)

// 有天他们一起移民了，此时我想改变他们的国籍为 America
juniorMing.nation = 'America'
// 但是改变小明一人的国籍并不能影响大明的国籍
console.log(seniorMing.nation) // China

```

(nation)国籍在这个例子中成为两个对象中共享的属性，但是由于没有类的概念，必须有一个新的机制来处理这部分，被**共享的属性**，这个就是prototype的由来

所以上面的例子可以变成

```
function People(age) {
    this.age = age
}

People.prototype.nation = 'China'

// 父子大小明
let juniorMing = new People(12)
let seniorMing = new People(38)

// 有天他们一起移民了，此时我想改变他们的国籍为 America
People.prototype.nation = 'America'

console.log(seniorMing.nation) // America
console.log(juniorMing.nation) // America

```

###  2.最简单的原型

结合上面部分

```
function Foo(name){
    this.name = name
}

let foo = new Foo('demoFoo')

console.log(foo.name) // demoFoo
```

构造函数可以通过Foo可以通过`Foo.prototype`来访问原型对象，同时生成的实例foo可以通过`__proto__`来访问原型对象

```
Foo.prototype === foo.__proto__ // true
foo.__proto__.constructor === Foo // true
```

### 3.原型链

什么是原型链？相信很多人在面试中都会遇到的问题

原型与原型层层链接的过程即为原型链

对象可以使用构造函数`prototype`原型对象的属性和方法，就是因为对象有`__proto__`原型存在每个对象都有`__proto__`原型的存在

```
function Star(name,age) {
    this.name = name;
    this.age = age;
}
Star.prototype.dance = function(){
    console.log('我在跳舞',this.name);
};
let obj = new Star('张萌',18);
console.log(obj.__proto__ === Star.prototype);//true

```

图解

​	![image-20221118215808796](C:\Users\liu\AppData\Roaming\Typora\typora-user-images\image-20221118215808796.png)

**原型的构造器**

​	原型的构造器指向构造函数

```
function Star(name) {
        this.name = name;
    }
    let obj = new Star('小红');
    console.log(Star.prototype.constructor === Star);//true
    console.log(obj.__proto__.constructor === Star); //true

```

当我们给构造函数的`prototype`重新赋值的时候我们需要手动去修正constructor属性不然会导致constructor丢失

```
  function Star(name) {
        this.name = name;
    }
    Star.prototype = {
        dance: function () {
            console.log(this.name);
        }
    };
    let obj = new Star('小红');
    console.log(obj.__proto__); //{dance: ƒ}
    console.log(obj.__proto__.constructor); //  ƒ Object() { [native code] }
    Star.prototype.constructor = Star;

```

**原型继承**

​	让一个普通的对象快速获得原本不属于它的属性和方法

​	当一个对象的prototype属性被更改，那么这对象就能获得这个新的属性和方法(小明这个对象把他的父对象更换成了亿万富豪小王，这个时候小明就会有小王金钱和豪车)

```
function Parent() {
    this.title = "parent"
}
function Child() {
    this.age = 13
}
const parent = new Parent()
Child.prototype = parent
const child = new Child()
console.log(child);
console.log('child.__proto__.constructor: ', child.__proto__.constructor);
console.log('parent.constructor: ', parent.constructor);//每一个实例也有一个constructor属性，默认调用prototype对象的constructor属性

```

结论:

- 原本的child.prototype被替换为parent对象后构造器的constructor没有被修改丢失之前的链接就成了单项

```
console.log(parent.constructor.prototype === parent) //false
```

这样才能使原型链从新链接

```
const parent = new Parent()
Child.prototype = parent
Child.prototype.constructor = Child; //添加了这行
const child = new Child()
```

