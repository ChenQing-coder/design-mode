### Head First 设计模式笔记
#### 设计模式入门
##### 设计原则
- 找出应用中可能需要改变之处，把它们独立出来，不要和那些不需要改变的代码混在一起
- 针对接口编程，而不是针对实现编程（针对接口编程就是针对超类型编程，超类型主要是指
抽象类，或者接口，从而实现多态，从而实现弹性）
- 多用组合，少用继承（使用组合建立系统具有很大的弹性，可以在运行时动态改变行为，
可以提高代码的扩展性和可维护性）
- 为了交互对象之间的松耦合设计而努力，松耦合的设计之所以能让我们建立有弹性的OO系统
，能够应对变化，是因为把对象之间的互相依赖性降到了最低。
- 类应该对外开放，对修改关闭（遵循开放-关闭原则，通常会引入新的抽象层次，增加代码
的复杂度。你需要把注意力集中到**最有可能发生变化**的地方，然后应用开放-关闭原则。如果每个
地方都应用的话，会导致代码抽象度高，难以理解，也是一种浪费）
- 依赖倒置原则：要依赖抽象，不要依赖具体类。（不管高层组件还是低层组件都应该依赖于抽象）
> how to do（针对的是可变化的部分）
1. 变量不可以持有具体类的引用
2. 不要让类派生自具体类
3. 不要覆盖基类中已实现的方法
> 策略模式：定义了算法族，分别封装起来，让它们之间可以互相替换，此模式让算法的变化
独立于算法的客户。    
- 好莱坞原则：别调用我，我们会调用你。将决策权放在高层模块中，以便决定如何以及何时调用底层模块。

就对象的方法，一般只应该调用属于一下范围的：
1. 就对象本身
2. 被当作方法参数而传递进来的对象
3. 此方法所创建或者实例化的任何对象
4. 对象的任何组件，把组件想象成被实例变量所引用的任何对象。
但是要⚠️，**如果是某对象的其他方法返回的结果，不要调用该对象的方法**。（System.on.println）,而是应该用一个对象
接住，然后用作为一个方法的参数传入。
应用这个原则，可以减少对象之间的依赖，减少维护成本。但是会增加复杂度和开发成本。
- 单一责任：一个类应该只有一个引起变化的原则
> 类的每个责任都有改变的潜在区域。超过一个责任，意味着超过一个改变的区域。

- SOLID 包括单一职责原则，开闭原则，里氏替代原则，接口隔离原则，依赖倒置原则
- LKP 最少知识原则（墨忒耳法则）：要减少对象之间的交互，只留几个密友
- DbC(design by contract契约式编程)：核心思想是对软件系统中的元素之间相互合作以及“责任”与“义务”的比喻
- KISS（keep it simple，stupid):指在设计当中应当注重简约的原则
- DRY(Once and only once Or Don't repeat yourself):代码和测试所构成的系统，必须能够表达所应表达的内容，但是不能含有任何重复代码
- YAGNI(You aren't gonna need it 过度工程化):产品设计的比使用期望有更大的鲁棒性（强健性）或性能，或者不必要的复杂度。

内聚：它用来度量一个类或者模块紧密地达到单一目的或者责任，当一个模块或者一个类被设计成只支持一组相关功能时，我们就说
它高类聚，反之则是低内聚。
#### 观察者模式
> 观察者模式定义了对象之间的一对多的依赖，这样一来，当一个对象改变状态时，它的所有依赖
都会收到通知并自动更新
- 应用：java 中具体的体现Observable(遗弃)/swings/javaBeans（PropertyChangeListener）就
前端来说应该是事件监听、订阅等，promise(push),function,function*(pull))
#### 装饰者模式
> 动态地将责任附加到对象上，若要扩展功能，装饰者提供了比继承更有弹性的替代方案

- 装饰者与被装饰者具有相同的超类型（这里是利用继承达到类型匹配，而不是利用继承获得行为）
- 你可以用一个或者多个装饰者包装对象
- 既然装饰者和被装饰者具有相同的超类型，所以在任何需要原始对象的场合，都是可以用装饰过的
对象替代它
- 装饰者可以在所委托被装饰者的行为之前或者之后，加上自己的行为，以达到特定的目的。
- 对象可以在任何时候被装饰，可以在运行时动态的添加。
- **装饰者会导致设计中出现许多小对象，如果过度使用，会让程序变得复杂**。
在java 中文件输入和输出 采用的是装饰者模式，一般如下面代码这样：
```
        beverage = new Soy(new Mocha(new HouseBlend()));
```
#### 工厂方法模式
**所有的工厂都是用来封装对象的创建**
##### 工厂方法
>定义了一个创建对象的接口，但是由于子类决定要实例化的类是哪一个。工厂方法让类把实例化推迟到子类。从而实现
使用和创建的解耦
- 简单工厂，就是把需要实例化的对象，放在一起集中处理，当需要修改的时候，直接修改就该类就可以了。也可以采用静态
方法来实现，但是有一个缺点就是无法通过extends来修改代码。
```
public static Pizza createPizza(String type){
        Pizza pizza;
        if(type.equals("cheese")){
            pizza=new ChicagoCheesePizza();
        }else if(type.equals("greek")){
            pizza=new NYGreekPizza();
        }else{
            pizza=new NYPepperonPizza();
        }
        return pizza;
    }
```
- 简单工厂 vs 工厂模式，简单工厂则是把所有实例化的逻辑，在一个地方处理完了；但是工厂方法是创建一个框架，
让子类决定要如何实现。
```
public abstract class PizzaStore {

    Pizza orderPizza(String type){
        Pizza pizza =createPizza(type);
        return pizza;
    }
    abstract Pizza createPizza(String type);// factory method
}
public class NYStylePizzaStore extends PizzaStore {

    @Override
    Pizza createPizza(String type) {
        Pizza pizza;
        if(type.equals("cheese")){
            pizza=new NYCheesePizza();
        }else if(type.equals("greek")){
            pizza=new NYGreekPizza();
        }else{
            pizza=new NYPepperonPizza();
        }
        return pizza;
    }
}
```
##### 抽象工厂模式
> 提供一个接口，用于创建相关或者依赖对象的**家族**，而不需要明确指定的具体
类型。抽象工厂主要是负责创建一组产品的接口
```
public interface PizzaIngredientFactory {

    public Dough createDough();
    public Sauce createSauce();
    public Cheese createCheese();
    public Veggies[] createVeggies();
    public Pepperoni createPepperoni();
    public Clams createClams();

}
```
##### 工厂方法 vs 抽象工厂
###### 相同点：
都是负责创建对象，减少应用程序和具体类之间的依赖，实现松耦合。同时都是
针对抽象编程，而不是具体的类编程。

###### 不相同点：

- 工厂方法是使用继承，把对象的创建委托给子类，子类实现工厂方法创建对象。
适用于一个产品线。
- 抽象方法是使用对象组合，对象的创建被实现工厂接口所暴露出来的方法中。
适用于一个产品家族的抽象。
#### 单件模式（Singleton Pattern）
> 用来创建独一无二的，只能有一个实例的对象的入场券。 
单件模式确保程序中一个类最多只有一个实例，同时通过静态方法提供了全局访问点，
##### 在多线程中的单件模式
在多线程中， 单件模式可能会造成一个问题，会造成多个线程共用一个实例的情况，为了避免，可以采用如下方法(感觉这块更多线程交互较多，
第一和第三不是太理解)：
- 在静态方法上添加synchronized字段，可以迫使每个线程在进入这个方法之前，要等候别的线程离开这个方法，也是说两个线程
不会同时进入这个方法。但是这个方法会导致性能下降。(ChocolateBoilerFirst）
- 初始化时直接创建单件实例,可以保证线程安全，而没有延迟初始化（ChocolateBoilerTwo）
- 使用双重检查锁，减少使用同步。(ChocolateBoilerThird)
#### 命令模式
> 将请求封装层对象，这可以让你使用不同的请求、队列，或者日志请求来参数化其他对象，命令模式也可以支持撤销操作。
- 命令模式将发出请求的对象和执行请求的对象解耦。
- 在**被解耦的两者之间是通过命令对象进行沟通**的，命令对象封装了接收者和一个或者一组动作。
- 宏命令是命令的一种简单的延伸，允许调用多个命令。
- 适用于队列请求、日志请求。
#### 适配器
> 将一个类的接口，转换成客户期望的另一个接口。适配器让原本不兼容的类可以合作无间。
适配器有两种：
- 对象适配器：指的是使用对象组合，以修改接口包装被适配器者；同时被适配者的任何子类都可以搭配着适配器。
- 类适配器：主要是通过多重继承来实现的，但是Java 不支持多重继承。
##### 装饰器 vs  适配器
- 相同点：都是用来包装对象的
- 不同点： 装饰器是用来有一些新的行为或者责任要加入你的设计中，而适配器是进行一些转换，传送，将一个接口转换成
另一个接口。
##### 外观模式
> 提供了一个统一的接口，用于访问子系统中的一群接口。外观定义了一个高层接口，让子系统更容易。
外观只是提供了简化的接口，但是依然将系统的完整功能暴露出来了。此外，外观模式允许客户实现与任何子系统解耦。
#### 模版方法模式
> 在一个方法中定义一个算法的骨架，而将一些步骤延迟到子类中。模版方法使得子类可以在不改变算法结构的情况下，
重新定义算法中的某些步骤。
- 钩子是一种被声明在抽象类中的方法，但是只有空的或者默认的实现。钩子的存在，可以让子类有能力对算法的不同点进行挂钩，
要不要钩子有子类自行决定。
当子类必须提供算法中的某个方法或者步骤的实现时，就使用抽象方法。如果算法的这个部分是可选的，就用钩子。
- 策略模式 vs 模版方法模式 vs 工厂方法
1. 策略模式和模版方法都是封装算法，但是一个是组合，一个是继承。
2. 工厂方法是模版方法的一种特殊版本 
#### 迭代器模式
> 提供一种方法顺序访问一个聚合对象中的各个元素，而又不暴露器内部的表示。迭代器模式把在元素之间游走的责任
交给了迭代器，而不是聚合对象。这不仅让聚合对象的接口变得简洁，而且可以让聚合对象专注于其他的事情。
- 迭代器允许访问聚合的元素，而不需要暴露它的内部结构
- 迭代器提供了一个通用的接口，让我们遍历聚合项，当我们编码使用聚合项时，可以使用多态机制。
#### 组合模式
> 允许你将对象组合成树形结构来表现"整体/部分"层次结构，组合能让客户以一致的方式处理个别对象以及对象组合
- 组合模式提供了一个结构，可同时包容个别对象和对象组合
- 组合模式允许客户对个别对象以及组合对象一视同仁
- 组合结构内的任意对象称为组件，组合可以是组件，也可以是叶节点
- 在实现组合模式时，有许多设计上的折中，你根据需要平衡透明性和安全性。
#### 状态模式
>允许对象在内部状态改变时改变它的行为，对象看起来好像修改了它的类。这种模式，是将状态封装成独立的类，将动作
委托到代表当前状态的对象。

状态模式 vs 策略模式
1. 我们将一群行为封装在状态对象中，context 的行为随时可委托到那些状态对象中的一个。随着时间的流逝，当前状态
在状态对象集合中游走改变，以反应内部的状态。**状态模式是不用在context中放置许多条件判断的替代方法**
2. 而策略模式，是客户主动指定context所要组合的策略对象是哪一个。对于策略模式来说，通常只有一个最适合的策略模式
我们把策略模式想成是除了继承之外的弹性替代方案。

#### 代理模式
> 为另一个对象提供一个替身或占位符以控制对这个对象的访问。

使用代理模式创建代表对象，让代表对象**控制某对象的访问**，被代理对象可以是远程的对象、创建开销大的对象或需要安全
控制的对象。
##### 分类
- 远程代理可以作为另一个jvm上对象的本地代表。调用代理的方法，会被代理利用网络转发到远程执行，并且结果会通过
网络返回代理，再由代理将结果转给客户
- 虚拟代理**作为创建开销大的对象的代表**。虚拟代理经常直到我们真正需要一个对象的时候才创建它。当对象在创建前或者创建
时，由虚拟代理来扮演对象的替身。对象创建后，代理就会将请求直接委托给对象。
- 保护代理基于调用者控制对象方法的访问

同时代理模式还有许多变体例如：缓存代理，同步代理，防火墙代理，写入时复制代理。java 内置的代理支持，可以
根据需要建立动态代理，并将所有调用分配到所选的处理器。同时 代理也会导致你的设计类数目增加。

代理模式 vs 装饰器模式 vs 适配器
- 代理模式：控制对象的访问，代理是实现相同的接口
- 装饰器模式：为对象增加新的行为
- 适配器会改变对象适配的接口

#### 复合模式
> 结合两个或者以上的模式，组成一个解决方案，解决一再发生的一般性问题。

- MVC 是复合模式，结合了观察者模式、策略模式和组合模式
- 模型使用了观察者模式，以便观察者更新，同时保持两者之间解耦；控制器是视图的策略，视图可以使用不同的控制器
实现不同的行为；视图使用了组合模式实现用户界面。
- Model2是MVC在WEB应用，控制器变成了servlet,而HTML 变成了视图。

#### 总结
- 让设计模式自然而然地出现在你的设计中，而不是为了使用而使用。
- 设计模式并非僵化的教条，你可以依据自己的需求采用或者调整
