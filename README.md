# this问题

**1. `this`在函数体内时,`this`指向调用当前函数体的对象**

**示例:**

```javascript
function Person(name,age){
		this.name=name;
		this.age=age;
		console.log(this);//Person {name: "mm", age: "21"}
	}
	var person=new Person('mm','21');//Person()构造函数中的this是指向person的，因此person有了name和age两个属性
	console.log(person.name);
	console.log(person.age);
```

**2. `this`在函数体外,那么`this`指向`window`对象**

**示例:**

```javascript
	var a=1;
	console.log(this);//'window'
	console.log(this.a);//1
```

**3. 在事件处理程序中，`this`指向触发该事件的对象(IE除外)**

**示例2: DOM0级**

```javascript
window.onload=function(){
	 var oBtn=document.getElementsByClassName('button');
	 for(var i=0;i<oBtn.length;i++){
	 	oBtn[i].onclick=function(){
	 		console.log(this);//按钮
	 		console.log(this.innerHTML);
	 		}
	 	}
	 }
```

**示例2: DOM2级**

```javascript
window.onload=function(){
	var oBtn=document.getElementsByClassName('button');
	for(var i=0;i<oBtn.length;i++){
		//除了IE
		oBtn[i].addEventListener('click',function(){console.log(this);},false);//当前按钮标签
		//IE下
		// oBtn[i].attachEvent('onclick',function(){console.log(this);});//window对象
	}
	}
```

**4. 在有定时器时,`this`指向window**

**示例1:**

```javascript
function Aaa(){
	 	this.b=12;//this是Aaa()的对象
		setInterval(this.show,1000);//this是window 
	}
	
	 Aaa.prototype.show=function(){//是定义在 window上的 
		console.log(this.b,'Aaa的this');//undefine  this是window.因为是window调用了这个函数体 而window上并没有定义属性b
	 }
	var obj=new Aaa();
```

**示例2:解决定时器问题**

```javascript
function Bbb(){
		var _this=this;//把当前this->Bbb对象存下来了
		this.c=33;
		timer=setInterval(function(){
			_this.show();
			console.log(_this,'定时器内部');//定时器内写一个匿名函数，将保存下来的_this值传进去
		},1000);//调用show方法的是Bbb对象
	}
	Bbb.prototype.show=function(){
		console.log(this.c,'Bbb的this');//33
	}
	var obj2=new Bbb();
```

**5. 在用了`apply`和`call`函数后，被调用函数内部的`this`为`apply`和`call`第一个参数指定的**

**示例:**

```javascript
function n(p,y){//
		console.log(arguments,'arguments1');
		y=function(){
			console.log(p,'传进来的参数');
			p=2;//改变传进来的参数p
		}
		console.log(y,'y');
		return function(){//闭包 没有外部变量的引用是不是不叫闭包？
			console.log(arguments,'arguments');
			console.log(arguments[1]===y);
			var p=3;//局部变量
			y();
			console.log(this.p,'this.p');//window.p为undefined因为没有全局变量
			console.log(p,'p');//要显示的t为局部变量 p=3;
		}.apply(this,arguments);//this在函数m()内，this指向调用函数m的对象window,arguments[p,y],在执行完y函数后，p=8,y是一个函数,所以arguments[9,function]所以在匿名函数内部this指向window 
	}
	n(8,7);//返回匿名函数,由于匿名函数通过调用apply方法调用了自己，所以匿名函数才能执行，否则返回的就是一个匿名函数并不会执行。
```

