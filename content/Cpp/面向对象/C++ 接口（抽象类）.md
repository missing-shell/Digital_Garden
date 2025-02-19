### 定义

接口描述了类的行为和功能，而不需要完成类的特定实现。

C++ 接口是使用**抽象类**来实现的，抽象类与数据抽象互不混淆，数据抽象是一个把实现细节与相关的数据分离开的概念。

如果类中至少有一个函数被声明为==纯虚函数==，则这个类就是抽象类。纯虚函数是通过在声明中使用 "= 0" 来指定

设计**抽象类**（通常称为 ABC）的目的，是为了给其他类提供一个*可以继承*的适当的基类。抽象类不能被用于*实例化*对象，它只能作为**接口**使用。如果试图实例化一个抽象类的对象，会导致编译错误。

因此，如果一个 ABC 的子类需要被实例化，则必须实现每个纯虚函数，这也意味着 C++ 支持使用 ABC 声明接口。如果没有在派生类中*重写纯虚函数*，就尝试实例化该类的对象，会导致编译错误。

可用于实例化对象的类被称为**具体类**。

### 设计策略

面向对象的系统可能会使用一个==抽象基类==为所有的外部应用程序提供一个适当的、通用的、标准化的接口。然后，派生类通过继承抽象基类，就把所有类似的操作都继承下来。

外部应用程序提供的功能（即公有函数）在抽象基类中是以纯虚函数的形式存在的。这些纯虚函数在相应的派生类中被实现。

这个架构也使得新的应用程序可以很容易地被添加到系统中，即使是在系统被定义之后依然可以如此。

> 定义一个函数为==虚函数==，不代表函数为不被实现的函数。
> 定义他为虚函数是为了允许用**基类**的指针来调用**子类**的这个函数。
> 定义一个函数为==纯虚函数==，才代表函数没有被实现。
> 定义纯虚函数是为了实现一个**接口**，起到一个规范的作用，规范继承这个类的程序员必须实现这个函数。