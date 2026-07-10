1.  [Programming](README.md)
2.  [Programming](Programming_98307.md)
3.  [Web](Web_352452663.md)
4. [Javascript](JavaScript_282329107.md)
5. [기본 문법](366379068.md)

# 클래스, 객체

## 객체
- 서로 연관된 함수와 변수를 그룹화해서 정리하는 도구
- 메소드 포함

- 예제
```javascript
	const dog = {
	    name: '삽살개',
	    age: 2
	};

	console.log(dog.name);
	console.log(dog.age);
```
- 예제2
```javascript
var cat = {
	name: '러시안 블루',
	age: 10,
	call:function() {
		return name;
	},
	miyao:function(val) {
		return age - val;
	}
}
```
### 객체 선언
- {key: value}
- key가 공백이 있을 때 '' 사용
- 예제
```javascript
	const ironMan = {
	    name: '토니 스타크',
	    actor: '로버트 다우니 주니어',
	    alias: '아이언맨'
	}
```
- 함수에서 객체를 파라미터로 받기
```javascript
	function print(hero) {
		const text = '${hero.alias}(${hero.name}) 역할은 ${hero.actor} 입니다.';
	    console.log(text);
	}

	print(ironMan);
```
### 객체 생성
```javascript
	var avaengers = {'captain': 10, 'hulk':20, 'ironman':30};
```
- captain 이 key, 10이 value
- 객체를 만드는 다른 방법
  - 첫 번째
```javascript
	var avengers = {};
    avengers['captain'] = 10;
    avengers['hulk'] = 20;
    avengers['ironman'] = 30;
```
- 두 번째
```javascript
	var avengers = new object();
    avengers['captain'] = 10;
```
- 값 가져오기  (for in)
```javascript
	alert(avengers['hulk']);
	alert(avengers.hulk);
	for(key in avengers) {
	    document.write("key : " + key + " value: " + avengers[key] + "</br>");
	}
```
- 객체에 함수 정의
```javascript

	var avengers = {
		'list' : {'captain':10, 'hulk': 20, 'ironman': 30},
	    'show':function() {
		    for(var name in this.list) {
			    document.write(...);
		    }
		}
	};
	avengers.show();
```
- 객체 활용
```javascript

	var Body = {
		setColor:function(color) {
			document.querySelector('body').style.color = color;
		},
		setBackgroundColor:function(color){
			document.querySelector('body').style.backgroundColor = color;
		}
	}
```
### 객체 배열
- 정의
```javascript
  Object.entries(objects) : [[키,값], [키,값]] 형태의 배열로 변환
  Object.keys(objects): [키, 키, 키] 형태의 배열로 변환
  Object.values(objects) : [값, 값, 값] 형태의 배열로 변환
```

#### 객체 배열 함수
##### forEach
- 예제
```javascript

	const array = [1,2,3,4,5];
	  array.forEach(num => {
	  console.log(num);
	})
```

##### for. in
- 객체의 각 속성 출력
- 예제
```javascript
var memberObject = {
	manager:'gorilla',
	host:'tiger',
	account:'mouse'
}

for ( var name in memberObject) {
	console.log(name, memberObject[name]);
}
```
##### map
- 배열 안의 원소 변환 시 사용
- 파라미터로 변환할 형식 입력으로 받음
```javascript

	const array = [1,2,3,4,5];
	const square = n => n * n;
	const squared = array.map(square);
```
- 또는 변환 함수명 없이
```javascript

	const squared = array.map(n => n * n);
```
##### indexOf
- 찾는 항목이 몇 번째 원소인지 검색

##### findIndex
- 검색하고자 하는 항목이 배열이나 객체일 때 몇 번째 원소인지 찾음
- findIndex 함수에 검사하고자 하는 조건을 넣어 처리
```javascript

	const index = totos.findIndex(toto => toto.id ===3); //totos 배열에서 원소별 id 값이 3인 배열의 위치
```
##### find
- findIndex와 비슷하지만 찾아낸 값을 반환

##### filter
- 배열에서 특정 조건을 만족하는 값들만 따로 추출하여 새로운 배열 생성
```javascript

	const newArray = totos.filter(toto => toto.done === false);
```
##### splice
- 배열에서 특정 항목을 제거할 때 사용
- splice(i, n) : i번째 인덱스부터 n 개 삭제

##### slice
- 배열을 잘라낼 때 사용, 기존 배열은 건들지 않고 새로운 배열 생성
- const sliced = numbers.slice(0,2) : 0번째 인덱스부터 2번째 인덱스 전까지

##### shift, pop
- shift: 첫번째 원소를 배열에서 추출(원소 사라짐)
- pop: 마지막 원소를 배열에서 추출
- numbers.pop();

##### unshift
- shift의 반대
- 배열의 맨 앞에 새 원소 추가
- numbers.shift(5);

##### concat
- 여러 개의 배열을 하나의 배열로 합침
```javascript

	arr1.concat(arr2);
```
##### join
- 배열 안의 값들을 문자열 형태로 합쳐줌
```javascript

	array.join();  // 1,2,3,4,5
	array.join(' '); // 1 2 3 4 5
	array.join(', '); //1, 2, 3, 4, 5
```
##### reduce
- 그래서 reduce()가 뭔데? 설명이 없음
- 예제
```javascript

	const numbers = [1,2,3,4,5];
	let sum = 0;
	numbers.forEach(n => {
	    sum += n;
	});
	console.log(sum);

	// reduce 사용 예
	let sum = array.reduce((accumulator, current) => accumulator + current, 0);
	// accumulator + current( 콜밸함수), 0 reduce 함수 초기값

	// reduce 수정
	let sum = numbers.reduce((accumulator, current) => {
		return accumulator + current;
	}, 0)
```

## 프로토 타입과 클래스

### 객체 생성자
- 함수를 통해서 새로운 객체를 만들고 그 안에 넣고 싶은 값 혹은 함수들을 구현할 수 있게 해줌
- 예제: 객체 생성자 정의
```javascript

	function Animal(type, name, sound) {
		this.type = type;
	    this.name = name;
	    this.sound = sound;
	    this.say = function() {
		    console.log(this.sound);
		};
	}

	// 객체 생성(constructor)
	const dog = new Animal('개', '멍멍이', '멍멍');
```
- 함수의 이름은 대문자
- 새로운 객체를 만들 때 new 사용

### 객체 생성자 상속 받기
- 새로운 Dog 객체 생성자 구현시 Animal 기능 재사용
- 예제
```javascript

	function Animal(type, name, sound) {
	    this.type = type;
	    this.name = name;
	    this.sound = sound;
	}

	Animal.prototype.say = function() {
	    console.log(this.sound);
	};

	Animal.prototype.sharedValue = 1;

	function Dog(name, sound) {
	    Animal.call(this, '개', name, sound);
	}
	Dog.prototype = Animal.prototype;
```
- Animal을 상속 받아 Dog를 생성하는 데 첫 번째 인자 this 추가, 나머지 Animal 파라미터 추가

### 프로토 타입
- 객체 생성자 함수를 사용하여 특정 함수 또는 값을 재사용하는 것
- 객체를 만들 때마다 속성이나 함수를 객체수 만큼 만들게 됨 -> 공통적인 함수를 매번 안 만들 수 없는가? 라는 의문에서 탄생
- 예제
```javascript

	function Animal(type, name, sound) {
	    this.type = type;
	    this.name = name;
	    this.sound = sound;
	    Animal.prototype.say = function() {
		    console.log(this.sound);
		}
		Animal.prototype.sharedValue = 1;
	};
```
- 프로토 타입 정확한 이해가 필요
- 다른 교재에서는 prototype 함수를 객체 바깥에서 선언
```javascript
function Animal(type, name, sound) {
	this.type = type;
	this.name = name;
	his.sound. =sound;
}

Animal.prototype.say = function() {
	console.log(this.sound);

}
```
- 메소드는 prototype으로 선언하라는게 추천 사항

## 클래스
- ES5 에서는 객체 생성자 함수를 사용하여 비슷한 작업 구현
- ES6부터는 class 라는 문법 추가됨 상속도 쉽게 가능
- 객체를 생성할 때 자동으로 constructor함수 호출

- 예제
```java

	class Animal {
		constructor(type, name, sound) {
			this.type = type;
		    this.name = name;
		    this.sound = sound;
	    }

	    say() {
	      console.log(this.sound);
	    }
	}

	const dog = new Animal(...);
```
- 클래스 상속
```java

	class Dog extends Animal {
		constructor(name, sound) {
			super('개', name, sound);
	    }
	}
```
### 객체 비구조화 할당 문법
- 사용시 \${hero.name}을 const {alias, name, actor} = hero;
- const text = '\${alias}(\${name}) 역할은 \${actor} 입니다로 사용 가능
- 파라미터 단계에서 객체 비구조화 할당 처리
```javascript

  function print({alias,name, actor}) {
    const text = '${hero.alias}(${hero.name}) 역할은 ${hero.actor} 입니다.';
    console.log(text)
  }
```


### 객체 안에 함수 넣기
- 예제
```javascript

  const dog = {
    name: '삽살개',
    sound: '멍멍',
    age: 2,
    say: function say() {
      console.log(this.sound);
    }
  };

  dog.say();
```

### Getter, Setter
- get, set 붙여서 사용
- 예제
```javascript

  const numbers = {
    a: 1,
    b: 2.
    get sum() {
      console.log('sum 실행');
      return this.a + this.b;
    }
  };

  console.log(numbers.sum);
```
 - sum() 함수를 호출하지 않아도 sum만 언급해도 값이 출력됨

- set 을 함수 앞에 붙여 파라미터 설정
```javascript

  set b(value) {
    this.b = value;
  }
```
 - function을 쓸 필요 없이 set, get 으로 Setter, Getter 선언

## 상속
- 기존 클래스를 확장
- 부모 클래스 호출 시 super 사용
- 예제
	```javascript
	class Person {
		constructor(name, first, second) {
			this.name = name;
			this.first = first;
			this.second = second;
		}
		sum() {
			return this.first + this.second;
		}
	}
	
	class PersonPlus extends Person {
		constructor(name, first, second, third) {
			super(name, first, second);
			this.third = third;
		}
		
		sum() {
			return super.sum() + this.third;
		}
		
		avg() {
			reutrn (this.first + this.second + this.third)/3;
		}
	}
	
	var kim = new PersonPlus('kim', 10, 20, 30);
	console.log("kim.sum()", kim.sum());
	console.log("kim.avg()", kim.avg());
	```
- 자바 스크립트는 Object간에 상속도 가능함.
- **객체가 직접 다른 객체를 상속 받을 수 있고 얼마든지 상속 관계를 바꿀 수 있음**
- super object로 부터 상속을 받고 있는 sub object가 다른 객체로부터 상속을 받고 싶다면 링크만 바꾸면 됨 : prototype link
- prototype link가 가르키는 객체를 prototype object 라고 함.

### Proto
- 자바 스크립트 class를 사용하지 않고 \__proto\__를 사용해 상속
- \__proto\__ 라는 prototype link를 통해서 객체를 상속 받음
- 예제
	```javascript
	var superObj = {superVal:'super'}
	var subObj = {subVal:'sub'}
	subObj.__proto__ = superObj;
	consolelog('subObj.subVal => ', subObj.subVal);
	consolelog('subObj.superVal =>', subObj.superVal);
	```
- 추가 설명
	- 자바스크립트는 프로토타입 기반 언어라고 함
	- 모든 객체들이 메소드와 속성들을 상속받기 위한 명세로 Prototype 객체를 가짐
	- 쉽게 말하면 모든 함수, 변수 등은 프로토 타입 객체와 연동되어 관리하고 있다.
### Object.create()
- 객체를 상속하는 새로운 객체 생성
- \__proto\__의 대체제
- 예제
```javascript
var superObj = {superVal: 'super'}
var subObj = Object.create(superObj);
subObj.subVal = 'sub';

console.log('subObj.subVal =>', subObj.subVal);
console.log('subObj.superVal =>', subObj.superVal);

subObj.superVal = 'sub';
console.log('superObj.superVal =>', superObj.superVal);
```

- html 파일 내에 위의 js를 포함한 후, console 선언 전에 debugger 키워드 추가
- 웹 브라우저 개발자 도구에서 실행 중 해당 부분 멈춤
- subObj.\__proto\__ = superObj로 설정됨이 확인됨.

## 객체와 함수
- 설명하고자 하는 주요 내용은 자바 스크립트에서 함수도 객체이기 때문에 자체 메소드 call, bind를 가지고 있음
### Call
- 객체와 관련 없는 함수를 연결하는 방법 : call
- 자바 스크립트에서 함수도 객체이므로 call() 메소드를 가지고 있음. call을 사용하여 객체를 지정할 수 있음
	```javascript
	var kim = {name:'kim', first:10, second:20}
	var lee = {name: 'lee', first:10, second:10}
	
	// this 지정되어 있지 않음
	function sum() {
		return this.first + this.second;
	}
	
	// call 함수로 객체를 지정
	console.log("sum.call(kim)", sum.call(kim));
	console.log("sum.call(lee)", sum.call(lee));
	```

- call은 여러 인자를 가질 수 있음
- 예제
```javascript
	var kim = {name:'kim', first:10, second:20}
	var lee = {name: 'lee', firstL10, second:10}
	
	// 필요없는 선언
	//lee.__proto__ = kim
	
	// prefix는 call의 두번째 인자로 취급됨
	function sum(prefix) {
		return prefix + (this.first + this.second);
	}
	
	// call 함수로 객체를 지정
	console.log("sum.call(kim)", sum.call(kim,'=>'));
	console.log("sum.call(lee)", sum.call(lee,':'));
```

### bind
- call은 실행할 때마다 this 객체를 지정하여 사용
- 객체를 내부적으로 고정하고자 할 때에는 bind 사용
- 예제
	```javascript
	var kim = {name:'kim', first:10, second:20}
	var lee = {name: 'lee', first:10, second:10}
	
	// this 지정되어 있지 않음
	function sum(prefix) {
		return prefix + (this.first + this.second);
	}
	
	// call 함수로 객체를 지정
	console.log("sum.call(kim)", sum.call(kim,'=>'));
	console.log("sum.call(lee)", sum.call(lee,':'));
	
	// sum 함수 호출하기 전에 객체를 지정 
	var kimSum = sum.bind(kim, '->');
	console.log('kimSum()', kimSum());
	```

## prototype vs \__proto\__
- 자바 스크립트의 함수는 객체이며 프로퍼티를 가질 수 있음
- 함수를 선언하면 함수 객체와 함수 프로토타입 객체가 생성됨.
- 예제
	```javascript
	function Person (name, first, second) {
		this.name = name;
		this.first = first;
		this.second = second;
	}
	// Person Prototype 객체에 sum() 생성
	Person.prototype.sum = function(){}
	
	var kim = new Person('kim', 10, 20)
	var lee = new Person('leel', 10,10)
	
	// Person 객체에서 sum() 메소드를 찾고 없으면 Person.prototype 에서 sum() 찾음
	kim.sum();
	```
- Prototype은 객체가 생성될 때 항상 생성된다고 보면 되고
- \__proto\__는 객체가 prototype을 참조하는 링크

## 생성자를 통한 상속
- 상속에서 정의한 Person class를 prototype을 사용하여 구현
- class를 function 으로 변경
- 예제
	```javascript
	function Person (name, first, second) {
		this.name = name;
		this.first = first;
		this.second = second;
	}
	
	Person.prototype.sum = function() {
		return this.first + this.second;
	}
	
	function PersonPlus(name, first, second, third) {
		Person.call(this, name, first, second);
		this.third = third;
	}
	
	PersonPlus.prototype.avg = function() {
		reutrn (this.first + this.second + this.third)/3;
	}
			
	var kim = new PersonPlus('kim', 10, 20, 30);
	console.log("kim.sum()", kim.sum());
	console.log("kim.avg()", kim.avg());
	```

- PersonPlus 가 Person을 상속 받았으나 Person.prototype에 정의된 sum 메소드를 호출 못함
- Person.prototype에 대한 상속 내지 호출이 필요함.
- PersonPlus.prototype.\__proto\__ = Person.prototype 을 정의
	- \__proto\__는 deprecated되었기 때문에 Object.create() 을 사용하여 정의
- 예제
	```javascript
	function Person (name, first, second) {
		this.name = name;
		this.first = first;
		this.second = second;
	}
	
	Person.prototype.sum = function() {
		return this.first + this.second;
	}
	
	function PersonPlus(name, first, second, third) {
		Person.call(this, name, first, second);
		this.third = third;
	}
	
	PersonPlus.prototype = Object.create(Person.prototype);
	
	// prototype이 Person.prototype으로 변경되면 constructor 변경됨
	PersonPlus.prototype.constructor = PersonPlus;
	
	PersonPlus.prototype.avg = function() {
		reutrn (this.first + this.second + this.third)/3;
	}
			
	var kim = new PersonPlus('kim', 10, 20, 30);
	console.log("kim.sum()", kim.sum());
	console.log("kim.avg()", kim.avg());
	```

- PersonPlus prototype이 Person.prototype을 바라보도록 했기 때문에 constructor가 Person을 보도록 바뀜
- PersonPlus.prototype.constructor = PersonPlus 를 재정의해야 함.
# 모듈
- 코드의 재활용성을 높이고 유지보수를 쉽게 할 수 있는 방향, 방법 추구
- 재활용성이 높은 코드를 분리, 별도로 만들어 사용
- 웹 브라우저에서 로직 모듈화

- Javascript의 변화 : 이전과 같이 **웹 브라우저에서만 수행**되는 것이 아닌 nodejs와 같이 서버에서 수행되거나 Google Apps Script같이 구글 제품 위에서 수행됨 =\> 자바스크립트의 유일한 호스트(웹브라우저)라는 말은 더 이상 사용되지 않음

- 모듈이 없는 sciprt 예제
```html

  <html>
  <script>
  function welcome() {
    return 'Hello World';
  }
  alert(welcome());
  </script>
  </html>
```
 - welcome() 함수가 자주 사용될수록(여러 페이지에서) welcome() 을 html 마다 정의하여 사용 → 별도의 파일에 welcome()을 정의하여 그 파일을 각 html 파일에서 include 하여 사용 =\> 모듈화
  - 예로 greeting.js 파일에 welcome() 함수를 정의

- 라이브러리
  - 모듈과 같은 개념
  - 자주 사용되는 로직은 재사용하기 편리하도록 잘 정리한 일련의 코드들의 집합 ex) jQuery
