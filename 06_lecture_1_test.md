# Контрольный тест для Лекции 1

## 1. Наследование двух классов

Даны два класса: `User` и `Admin`.
 
~~~js
function User()
{
	this.name = "I'm a user.";
}

User.prototype.showName = function()
{
	console.log(this.name);
}

function Admin()
{
	User.apply(this, arguments);
}
~~~

Необходимо наследовать класс `Admin` от `User`. Было предложено несколько вариантов:

~~~js
//1.
BX.extend(Admin, User);

//2.
Admin.prototype = new User();

//3.
Admin.prototype = User.prototype;

//4.
Admin.prototype = Object.create(User.prototype);

//5.
Admin.prototype.__proto__ = User.prototype;

//6.
Object.setPrototypeOf(Admin.prototype, User.prototype);
~~~

Все варианты дали одинаковый результат.

~~~js
var admin = new Admin();
admin.showName(); //выведет в консоль I'm a user.
~~~

**Задача:** все ли приведенные примеры правильные? Объясните плюсы и минусы каждого варианта наследования. 


## 2. Вызов функции-конструктора без new

Объект класса `Menu` создается обычный образом `var menu = new Menu();`
~~~js
function Menu(options)
{
	options = options || {};
	this.delay = options.delay || 100;
}

var menu = new Menu({ delay: 300 });
~~~

**Задача:** модифицировать код функции `Menu` так, чтобы создать объект меню можно было без оператора `new`.
~~~js
var menu = new Menu({ delay: 300 });
var menu2 = Menu({ delay: 300 });
~~~


## 3. Обнуление прототипа

~~~js
function User()
{

}

User.prototype = {
  flag: true
};

var user = new User();
User.prototype = {};
console.log(user.flag);
~~~

**Задача:** что будет выведено в консоле? Объясните свой ответ.

## 4. Внешняя библиотека и Object.prototype

На сайте подключена внешняя JS-библиотека, которая записала несколько свойств в Object.prototype:
~~~js
Object.prototype.browser = function() {};
Object.prototype.clone = function() {};
Object.prototype.jslibVersion = "3.5";
~~~

У вас стоит задача вывести свойства объекта на экран:
~~~js
var properties = {
	name: "Ivan",
	lastName: "Ivanov",
	nickname: "Vano"
};

for (var property in properties)
{
	console.log(property, properties[property]);
}
~~~

Но из-за js-библиотеки результат не тот, что нам нужен:
~~~
name Ivan
lastName Ivanov
nickname Vano
browser function () {}
clone function () {}
jslibVersion 3.5
~~~

**Задача:** приведите не менее 3-х способов (а лучше не менее 5), как обойти эту проблему. 
Цикл по объекту должен отобразить только собственные значения. Модифицировать код внешней библиотеки нельзя.

## 5. Разные конструкторы, но одинаковый результат

Даны две функции:
~~~js
function MyClassA()
{
}

function MyClassB()
{
}
~~~

Можно ли написать код этих функций так, чтобы `new MyClassA()` и `new MyClassB()` дали абсолютно одинаковый результат?
~~~js
new MyClassA() === new MyClassB(); // true
~~~

Если да, приведите пример кода.


## 6. Структура класса
 
Стоит задача создать класс для управления меню на сайте. Были предложены три варианта реализации.
 
~~~js
var Menu = {
	init: function(options)
	{
		//code
	},
	
	show: function()
	{
		//code
	},
	
	close: function()
	{
		//code
	}
}
~~~
 
~~~js
function Menu(options)
{
	//code
}

Menu.prototype.show = function()
{
	//code
}

Menu.prototype.close = function()
{
	//code
}
~~~
 
~~~js
function Menu(options)
{
	this.show = function()
	{
		//code
	}
    
	this.close = function()
	{
		//code
	} 
}
~~~ 
 
**Задача:** объясните плюсы и минусы каждого подхода. Какой вариант предложили бы вы?


## 7. Структура класса 2

Даны два класса `Slider` и `BackSlider`.

~~~js
function Slider(options)
{
	options = options || {};
	this.autoHide = options.autoHide === true; 
}

Slider.prototype = {
	open: function()
	{
		//code
	},

	close: function()
	{
		//code
	}
};


function BackSlider(options)
{
	Slider.apply(this, arguments); 
}

BackSlider.prototype = Object.create(Slider.prototype);
BackSlider.prototype.constructor = BackSlider;
BackSlider.prototype = {
	open: function()
	{
		Slider.prototype.open.apply(this, arguments);
		//code
	},

	bounce: function()
	{
		//code
	}
}
~~~

**Задача:** все ли правильно в этом коде? Если нет, предложите свой вариант.

## 8. Цепочки

Класс `Counter` выглядит следующим образом:

~~~js
function Counter(initialValue)
{
	this.counter = initialValue || 0;
}

Counter.prototype = {
	increment: function()
	{
		this.counter++;
	},
	
	decrement: function()
	{
		this.counter--;
	},
    
	show: function()
	{
		console.log(this.counter);
	}
}
~~~

**Задача:** модифицируйте код класса так, чтобы можно было выполнить такой код:
~~~js
var counter = new Counter(100);
counter.show().increment().increment().show().decrement().show(); //выведет: 100, 102, 101
~~~

## 9. Параметры по умолчанию

Конструктор класса `Slider` принимает набор опций в виде объекта `options`. Опции могут быть переданы частично или 
не переданы вообще. Если опция не установлена, то тогда он должна браться из опций по умолчанию.
~~~js
function Slider(options)
{
	var defaultOptions = {
		delay: 100,
		duration: 1000,
		direction: "left",
		className: "slider"
	};
}
~~~

**Задача:** С помощью метода `Object.create` реализовать механизм "опций по умолчанию": 
если опция не передана конструкторе, брать ее из `defaultOptions`. 

## 10. Свойство constructor

Даны два класса `Grid` и `CrmGrid`.
~~~js
function Grid()
{
	
}

function CrmGrid()
{
	
}

CrmGrid.prototype = Object.create(Grid.prototype);

var grid = new CrmGrid();

console.log(grid.constructor === CrmGrid);
console.log(grid.constructor === Grid);
~~~

**Задача:** что будет показано в консоле? Объясните результат.

## 11. Наследование и this

~~~js
var parent = {
	setFlag: function()
	{
		this.flag = true;
	}
}

var child = {
	__proto__: parent
}

child.setFlag();
~~~

**Задача:** в какой объект запишется свойство `flag`: в `parent` или `child`? Поясните свой ответ.


## 12. Вызовы метода

Дан следующий класс `User`. 

~~~js
function User(name) 
{
  this.name = name;
}

User.prototype.showName = function() 
{
  console.log(this.name);
}

var user = new User("Arnold");
~~~

**Задача:** одинаково ли сработают следующие вызовы? Поясните свой ответ.
~~~js
user.showName();
user.__proto__.showName();
User.prototype.showName();
Object.getPrototypeOf(user).showName();
~~~

## 13. Счетчик объектов

Класс `User` создает пользователей на странице.

~~~js
function User(name) 
{
  this.name = name;
}

var user = new User("Arnold");
~~~

**Задача:** написать функцию `User.count`, которая будет возвращать количество созданных пользователей.

~~~js
var arnold = new User("Arnold");
var kevin = new User("Kevin");

console.log(User.count()); //покажет 2

new User("Alexey");
new User("Ivan");
new User("Donatello");

console.log(User.count()); //покажет 5
~~~


## 14. Контекст

Дан следующий код:

~~~js
var lastName = "Ivanov";
var obj = {
   lastName: "Petrov",
   prop: {
      lastName: "Donatello Kovalski",
      getLastName: function() {
         return this.lastName;
      }
   }
};
  
console.log(obj.prop.getLastName());
var getLastName = obj.prop.getLastName;
  
console.log(getLastName());
~~~

**Задача:** какой результат будет выведен в консоль? Обоснуйте свой ответ. 
Как сделать, чтобы вызов `getLastName` вернул нужный результат? 


## 15. Контекст 2

Следующий код считает количество кликов на странице.

~~~js
var MouseClicker = {
	clicks: 0,
	init: function()
	{
		var body = document.body;
		body.addEventListener("click", this.handleClick);
	},

	handleClick: function(event)
	{
		this.clicks++;
	},

	showClicks: function()
	{
		console.log(this.clicks);
	}
}

MouseClicker.init();
~~~

**Задача:** Что происходит в этот коде? Почему он не работает? Предложите несколько вариантов решения проблемы.
