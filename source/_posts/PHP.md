---
title: PHP 面向对象
date: 2017-11-17 13:20:46
tags:
		- PHP
categories:
		- 编程学习

---

世间万物皆对象，类在编程中起到举足轻重的作用。

<!-- more -->



20171117

##### 面向对象的基本概念

	面向对象(Object Oriented)
	对象 Object
	面向 Oriented

世间万物皆为对象

对象基本组成

**对象的组成元素**

	是对象的数据模型，用于描述对象的数据
	又被称为对象的属性，或者对象的成员变量

**对象的行为**

	是对象的行为模型，用于对象能够做什么事情
	又被称为对象的方法

<!-- more -->

对象独一无二、可以重复使用。

**面向对象**

>面向就是在编程的时候一直把对象放在心上

* 面向对象编程就是在编程的时候数据结构都通过对象的结构进行存储

* 属性 、方法

对象的结构贴合真实的世界，有益于业务


* 面向对象就是把生活中要解决的事情都用对象的方式进行存储(属性、方法)
* 对象与对象之间通过方法的调用完成互动(方法)




![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-20/45872772.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-20/29057997.jpg)

	一人球员

	元素：头、身体、名字、球队
	行为：跑步、运球、吃喝拉撒


>识别对象、识别对象的属性、识别对象的行为


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-20/55639016.jpg)

现在只是了解学习了对象，对象有自己的属性和行为。属性就是本身自带的特征，行为这个人能做什么。


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-20/9681191.jpg)

* 类。物以类聚，把具有相似特性的对象归类到一个类中，
* 类定义了这些相似对象拥有的相同的属性和方法。
* 类是相似对象的描述，称为类的定义，是该类对象的蓝图或者原型
* 类的对象称为类的一个实例

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-20/21978531.jpg)

很多相似的对象，被一个类给封装了。

类的实例化

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-20/82993159.jpg)


**案例一**

	<?php
	
	//类的定义以关键字class开始，后面跟着这个类的名称
	class NBaPlayer{
	    //定义属性
	    public $name="Jordan";
	    public $height = "198cm";
	    public $weight = "98kg";
	    public $team = "Bull";
	    public $Number = "23";
	    
	    //定义方法
	    public function run(){
	        echo "Running\n";
	    }
	     public function shoot(){
	        echo "Shootng\n";
	    }
	     public function dunk(){
	        echo "Dunking\n";
	    }
	}
	// 类到对象的实例化
	//类的实例化为对象时使用关键字new，new之后根类的名称和一对括号
	$jordan = new NBaPlayer();
	//对象中的成员属性可以通过->符号访问
	echo $jordan->name."\n";
	//对象中的成员方法可以通过->符号访问
	$jordan->run();
	$jordan->dunk();
	?>
	
**构造函数**

类似与python的self，对类的第一个实例化。在对象实例化的时候自动调用。$this-是php里面的伪变量，表示对象自身。可以通过$this->的方式访问对象的属性和方法。

	//每一次new实例化对象的时候，都会使用类后面的参数列表调用构造函数
	$james = new NBaPlayer("James","200cm","100kg","Hot","6");
	echo $james->name."\n";


构造函数

	<?php
	
	//类的定义以关键字class开始，后面跟着这个类的名称
	class NBaPlayer{
	    //定义属性
	    public $name="Jordan";
	    public $height = "198cm";
	    public $weight = "98kg";
	    public $team = "Bull";
	    public $Number = "23";
	    
	    //构造函数，在对象被实例化的时候自动调用
	    function __construct($name,$height,$weight,$team,$Number){
	        echo "In NBaPlayer constructor\n";
	        $this->name = $name;
	        //$this-是php里面的伪变量，表示对象自身。可以通过$this->的方式访问对象的属性和方法
	        $this->$height = $height;
	        $this->$weight = $weight;
	        $this->$team    = $team;
	        $this->$Number  = $Number;
	    }
	    
	    
	    //定义方法
	    public function run(){
	        echo "Running\n";
	    }
	     public function shoot(){
	        echo "Shootng\n";
	    }
	     public function dunk(){
	        echo "Dunking\n";
	    }
	}
	// 类到对象的实例化
	//类的实例化为对象时使用关键字new，new之后根类的名称和一对括号
	$jordan = new NBaPlayer("Lib","198cm","98kg","Bull","23");
	//对象中的成员属性可以通过->符号访问
	echo $jordan->name."\n";
	//对象中的成员方法可以通过->符号访问
	$jordan->run();
	$jordan->dunk();
	//每一次new实例化对象的时候，都会使用类后面的参数列表调用构造函数
	$james = new NBaPlayer("James","200cm","100kg","Hot","6");
	echo $james->name."\n";
	?>


3.5 1120

__析构函数__

	//析构函数，在程序执行结束的时候会自动调用
    //析构函数通常被用于清理程序使用的资源。比如程序使用了打印机，那么可以在析构函数里面释放打印机资源
    function __destruct(){
        echo "Destroying" . $this->name . "\n";
    }

当这个对象再不使用的时候，或者变量设置为空时，会触发析构函数。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-21/49413323.jpg)

__对象继承__

继承

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-21/56404250.jpg)


父类和子类

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-21/90270523.jpg)

继承好处

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-21/89246432.jpg)

	<?php
	
	class Human{
	    public $name;
	    public  $height;
	    public  $weight;
	    
	    public function eat($food){
	        echo $this->name." is eating ".$food."<br>";
	    }
	}
	
	
	//类的定义以关键字class开始，后面跟着这个类的名称
	//在PHP中可以用extends关键字来表示类的继承，后面跟父类的类名
	// PHP中extends后面只能跟一个类的类名，这就是php的单继承原则
	class NBaPlayer extends Human{
	    //定义属性
	    public $team = "Bull";
	    public $Number = "23";
	    
	    //构造函数，在对象被实例化的时候自动调用
	    function __construct($name,$height,$weight,$team,$Number){
	        echo "In NBaPlayer constructor<br/>";
	        $this->name = $name;
	        //$this-是php里面的伪变量，表示对象自身。可以通过$this->的方式访问对象的属性和方法
	        //父类中的属性，可以通过$this来访问
	        $this->$height = $height;
	        $this->$weight = $weight;
	        $this->$team    = $team;
	        $this->$Number  = $Number;
	    }
	    //析构函数，在程序执行结束的时候会自动调用
	    //析构函数通常被用于清理程序使用的资源。比如程序使用了打印机，那么可以在析构函数里面释放打印机资源
	    function __destruct(){
	        echo "Destroying" . $this->name . "<br/>";
	    }
	    
	    //定义方法
	    public function run(){
	        echo "Running<br/>";
	    }
	     public function shoot(){
	        echo "Shootng<br/>";
	    }
	     public function dunk(){
	        echo "Dunking<br/>";
	    }
	}
	// 类到对象的实例化
	//类的实例化为对象时使用关键字new，new之后根类的名称和一对括号
	$jordan = new NBaPlayer("Lib","198cm","98kg","Bull","23");
	//对象中的成员属性可以通过->符号访问
	echo $jordan->name."</br>";
	//继承父类的方法
	//在子类的对象上可以直接访问父类中定义的方法和属性
	$jordan->eat("appple");
	
	?>

结果

	In NBaPlayer constructor
	Lib
	Lib is eating appple
	DestroyingLib

__访问控制__

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-21/96551441.jpg)

私有属性 private。实例化的时候，不能直接访问

仔细看`age`的调用。

	<?php
	
	class Human{
	    public $name;
	    public  $height;
	    public  $weight;
	    
	    public function eat($food){
	        echo $this->name." is eating ".$food."<br>";
	    }
	}
	
	
	//类的定义以关键字class开始，后面跟着这个类的名称
	//在PHP中可以用extends关键字来表示类的继承，后面跟父类的类名
	// PHP中extends后面只能跟一个类的类名，这就是php的单继承原则
	class NBaPlayer extends Human{
	    //定义属性
	    public $team = "Bull";
	    public $Number = "23";
	    private $age = "40"; //private的类成员只能在类的内部被访问
	    
	    
	    //构造函数，在对象被实例化的时候自动调用
	    function __construct($name,$height,$weight,$team,$Number){
	        echo "In NBaPlayer constructor<br/>";
	        $this->name = $name;
	        //$this-是php里面的伪变量，表示对象自身。可以通过$this->的方式访问对象的属性和方法
	        //父类中的属性，可以通过$this来访问
	        $this->$height = $height;
	        $this->$weight = $weight;
	        $this->$team    = $team;
	        $this->$Number  = $Number;
	    }
	    //析构函数，在程序执行结束的时候会自动调用
	    //析构函数通常被用于清理程序使用的资源。比如程序使用了打印机，那么可以在析构函数里面释放打印机资源
	    function __destruct(){
	        echo "Destroying" . $this->name . "<br/>";
	    }
	    
	    //定义方法
	    public function run(){
	        echo "Running<br/>";
	    }
	     public function shoot(){
	        echo "Shootng<br/>";
	    }
	     public function dunk(){
	        echo "Dunking<br/>";
	    }
	    public function getAge(){
	        echo $this->name. ".'s age is ". $this->age ."<br>";
	    }
	}
	// 类到对象的实例化
	//类的实例化为对象时使用关键字new，new之后根类的名称和一对括号
	$jordan = new NBaPlayer("Lib","198cm","98kg","Bull","23");
	//echo $jordan->age."<br>";
	echo $jordan->getAge();
	?>


在类的方法里面，把private属性，封装里面，可以实例化的时候，进行访问。

__Static(静态)关键字__


静态成员


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-22/63158017.jpg)



1. 静态属性用于保存类的公有数据
2. 静态方法里面只能访问静态属性
3. 静态成员不需要实例化对象就可以访问
4. 类的内部可以通过self或static关键字访问自身
5. 可以通过parent关键字访问父类的静态
6. 可以通过类的名称在类定义外部访问静态成员

	  public static $president = "David";
	    
	    //静态方法定义时在访问控制关键字后面添加static关键字
	    public static function changePre($newPrs){
	        //在类定义中使用静态成员的时候，用self跟着::操作符即可
	        //访问属性的时候::要有$，方法的时候没有$
	        //parent::$ 访问父类静态成员
	        self::$president = $newPrs;
	    }


__面向对象 Final成员__

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-22/91247587.jpg)


	<?php
	//子类中编写和父类方法名完全一致的方法，可以完成对父类方法的重写
	// 对于不想被任何类继续的类可以在class之前添加final关键字
	//对于不想被子类重写的方法，可以方法定义的前面添加final关键字
	final class BaseClass{
	    public function test(){
	        echo "baseClass::test<br/>";
	    }
	    //添加final关键字能够让这个方法不能在子类中重写
	    final public function test1(){
	        echo "baseClass::test<br/>";
	    }
	}
	
	class ChildClass extends BaseClass{
	    public function test(){
	        echo "ChildClass::test called<br/>";
	    }
	    public function test1(){
	        echo "ChildClass::test<br/>";
	    }
	}
	$obj = new ChildClass();
	$obj->test();
	?>


static、self

	<?php
	//parent 关键字可以用于调用被子类重写的方法
	//self关键字可以访问类自身的成员方法，也可以用于访问自身的静态成员和类常量。不能用于访问类自身的属性；；使用常量的时候不需要在常量名称前面添加$符号
	// static关键字用于访问类自身定义的静态成员，访问静态属性时候需要在属性前面添加$符号
	
	class BaseClass{
	    public function test(){
	        echo "baseClass::test<br/>";
	    }
	    
	    public function test1(){
	        echo "baseClass::test<br/>";
	    }
	}
	
	class ChildClass extends BaseClass{
	    const CONST_VALUE = 'A constant value';
	    private static $sValue = 'static value';
	    public function test(){
	        echo "ChildClass::test called<br/>";
	        parent::test();
	        //用parent关键字可以访问父类被重写的方法
	        self::called();
	        echo self::CONST_VALUE."<br/>";
	        echo static::$sValue."<br/>";
	    }
	   public function called(){
	       echo "ChildClass::called() called<br/>";
	   }
	}
	$obj = new ChildClass();
	$obj->test();
	?>

结果

	ChildClass::test called
	baseClass::test
	ChildClass::called() called
	A constant value
	static value

__面向对象--接口__

接口就是把不同类的共同行为进行了定义，然后在不同的类中实现不同的功能。

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-23/41352028.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-23/56695859.jpg)

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-23/21028410.jpg)

	<?php
	
	//interface关键字用于定义接口
	interface ICanEat{
	    //接口里面的方法不需要有方法的实现
	    public function eat($food);
	}
	// implements关键字用于表示类实现某个接口
	class Human implements ICanEat{
	    //实现了某个接口之后，必须提供接口中定义方法的具体实现
	    public function eat($food){
	        echo "Human eating ".$food."<br/>";
	    }
	}
	class Animal implements ICanEat{
	    //实现了某个接口之后，必须提供接口中定义方法的具体实现
	    public function eat($food){
	        echo "Animal eating ".$food."<br/>";
	    }
	}
	
	$obj = new Human();
	$obj->eat('Apple');
	$monkey = new Animal();
	$monkey->eat('Banana');
	//不能实例化接口
	//$eatObj = new ICanEat();
	//可以用instanceof关键字来判断某个对象是否实现了某个接口
	var_dump($obj instanceof ICanEat);
	echo "<br/>";
	function checkEat($obj){
	    if($obj instanceof ICanEat){
	        $obj->eat('food');
	    }else{
	        echo "The Obj can't eat<br/>";
	    }
	}
	checkEat($obj);
	checkEat($monkey);
	//可以用extends让接口继承接口
	interface ICanPee extends ICanEat{
	    public function pee();
	}
	//当类实现子接口时，父接口定义的方法也需要在这里类里面具体实现
	class Human1 implements ICanPee{
	    public function pee(){    
	    }
	    public function eat($food){}
	}
	
	?>


__多态__

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-23/30364784.jpg)

	function checkEat($obj){
	    if($obj instanceof ICanEat){
	        $obj->eat('food');
	    }else{
	        echo "The Obj can't eat<br/>";
	    }
	}
	//相同一行代码，对于传入不同的接口实现的对象的时候，表现是不同的，这就是多态
	checkEat($obj);
	checkEat($monkey);


__抽象类__

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-23/65541363.jpg)

介于类和接口之间。

既有接口又有共有的方法。

	<?php
	// abstract 关键字用于定义抽象类
	abstract class ACanEat{
	    //在抽象方法前面添加 abstract关键字可以标明这个方法是抽象方法，不需要具体实现
	    abstract public function eat($food);
	    
	    //抽象类中可以包含普通的方法，有方法的具体实现
	    public function breath(){
	        echo "Breath use the air.<br/>";
	    }
	}
	
	//继承抽象类的关键字是extends
	class Human extends ACanEat{
	    //继承抽象类的子类需要实现抽象类中定义的抽象方法
	    public function eat($food){
	        echo "Human eating ". $food . "<br/>";
	    }
	}
	
	class Animal extends ACanEat{
	    public function eat($food){
	        echo "Animal eating ". $food . "<br/>";
	    }
	}
	
	$man = new Human();
	$man->eat("Apple");
	$man->breath(); //和Aminal共用了抽象类的breath方法
	$monkey = new Animal();
	$monkey->eat("Banana");
	$monkey->breath();
	?>

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-23/60730869.jpg)

__魔术方法__

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-24/65985995.jpg)

__tostring()__

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-24/57904783.jpg)

	<?php
	class MagicTest{
	    public function __tostring(){
	        return "This is the class MagicTest ";
	    }
	}
	//可以直接被调用，没有使用$obj->__tostring()
	$obj = new MagicTest();
	echo $obj;
	?>

	This is the class MagicTest 


__invoke 当作对象被调用了

	<?php
	class MagicTest{
	   
	    public function __invoke($x){
	        echo "__invoke called with parameter ".$x."<br/>";
	    }
	}
	$obj = new MagicTest();
	$obj(5);
	?>

	__invoke called with parameter 5


![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-24/7112287.jpg)

__call__

	<?php
	class MagicTest{
	   
	   public function __call($name,$arguments){
	       echo "Calling ". $name . " with parameter " . implode(", ",$arguments)."<br/>";
	   }
	   
	}
	$obj = new MagicTest();
	$obj->runTest("part1","part2");
	?>

没有runTest()，但是可以调用`__call()`。

	Calling runTest with parameter part1, part2


____callStatic__

	<?php
	class MagicTest{
	   
	   public static function __callStatic($name,$arguments){
	       echo "Static Calling ". $name . " with parameter " . implode(", ",$arguments)."<br/>";
	   }
	   
	}
	$obj = new MagicTest();
	MagicTest::runTest("part1","part2");
	?>

	结果 Static Calling runTest with parameter part1, part2

方法的重载

`__get/__set`

![](https://image-1258195556.cos.ap-shanghai.myqcloud.com/qiniu/17-11-24/34451143.jpg)

`__get()`

	<?php
	class MagicTest{
	   
	  public function __get($name){
	      return "Getting the property " .$name."<br/>";
	  }
	   
	}
	$obj = new MagicTest();
	echo $obj->className;
	?>
	
	Getting the property className

`__set()`

	<?php
	class MagicTest{
	   
	  public function __set($name,$value){
	      echo "Setting the property " .$name. " to value " .$value . "<br/>";
	  }
	   
	}
	$obj = new MagicTest();
	$obj->className= "MagicTest";
	?>

	Setting the property className to value MagicTest

__isset
	
	<?php
	class MagicTest{
	   
	  public function __isset($name){
	      echo "__isset invoked <br/>";
	      return true;
	  }
	   
	}
	$obj = new MagicTest();
	echo '$obj->className is set?'.isset($obj->className)."<br/>";
	echo '$obj->className is empty?'.empty($obj->className);
	?>

	//结果

	__isset invoked
	$obj->className is set?1
	__isset invoked
	$obj->className is empty?1


__isset

	<?php
	class MagicTest{
	   
	  public function __isset($name){
	      echo "__isset invoked <br/>";
	      return false;
	  }
	   
	}
	$obj = new MagicTest();
	echo '$obj->className is set?'.isset($obj->className)."<br/>";
	echo '$obj->className is empty?'.empty($obj->className)."<br/>";
	?>

	结果

	__isset invoked
	$obj->className is set?
	__isset invoked
	$obj->className is empty?1


unset

	<?php
	class MagicTest{
	   
	  public function __isset($name){
	      echo "__isset invoked <br/>";
	      return false;
	  }
	  public function __unset($name){
	      echo "unsetting property ".$name."<br>";
	  }
	   
	   
	}
	$obj = new MagicTest();
	echo '$obj->className is set?'.isset($obj->className)."<br/>";
	echo '$obj->className is empty?'.empty($obj->className)."<br/>";
	unset($obj->className);
	?>

	//结果

	__isset invoked
	$obj->className is set?
	__isset invoked
	$obj->className is empty?1
	unsetting property className


__clone

复制

	<?php
	
	class NBaPalyer{
	    public $name;
	    function __clone(){
	        $this->name = 'TBD';
	    }
	}
	$james = new NBaPalyer();
	$james->name = 'James';
	echo $james->name."<br/>";
	$james2 = clone $james;
	echo "Before set up: James2's: ".$james2->name."<br/>";
	$james2->name = 'James2';
	echo "James's:".$james->name."<br/>";
	echo "James2's:".$james2->name."<br/>";
	?>

	James
	Before set up: James2's: TBD
	James's:James
	James2's:James2


__总结__

	概念：对象，对象的组成，特点
	面向对象：
	原则：


##### 参考

[https://www.imooc.com/learn/184](https://www.imooc.com/learn/184)
