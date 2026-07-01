
1.  [Programming](README.md)
2.  [Programming](Programming_98307.md)
3.  [Web](Web_352452663.md)
4. [Javascript](JavaScript_282329107.md)
5. [기본 문법](366379068.md)

# 클래스, 객체

## 객체
- 서로 연관된 함수와 변수를 그룹화해서 정리하는 도구

- 예제
	``` javascript
	const dog = {
	    name: '삽살개',
	    age: 2
	};

	console.log(dog.name);
	console.log(dog.age);	
	```

### 객체 선언
- {key: value}
- key가 공백이 있을 때 '' 사용
- 예제
	``` javascript
	const ironMan = {
	    name: '토니 스타크',
	    actor: '로버트 다우니 주니어',
	    alias: '아이언맨'
	}
	```

- 함수에서 객체를 파라미터로 받기
	``` javascript
	function print(hero) {
		const text = '${hero.alias}(${hero.name}) 역할은 ${hero.actor} 입니다.';
	    console.log(text);
	}

	print(ironMan);	
	```

### 객체 생성
	``` javascript
	var avaengers = {'captain': 10, 'hulk':20, 'ironman':30};	
	```

- captain 이 key, 10이 value
- 객체를 만드는 다른 방법
  - 첫 번째
	``` javascript
	var avengers = {};
    avengers['captain'] = 10;
    avengers['hulk'] = 20;
    avengers['ironman'] = 30;
	```

  - 두 번째
	``` javascript
	var avengers = new object();
    avengers['captain'] = 10;
	```

- 값 가져오기  (for in)
	``` javascript
	alert(avengers['hulk']);
	alert(avengers.hulk);
	for(key in avengers) {
	    document.write("key : " + key + " value: " + avengers[key] + "</br>");
	}	
	```

- 객체에 함수 정의
	``` javascript
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
	``` javascript
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
  ``` javascript
  Object.entries(objects) : [[키,값], [키,값]] 형태의 배열로 변환
  Object.keys(objects): [키, 키, 키] 형태의 배열로 변환
  Object.values(objects) : [값, 값, 값] 형태의 배열로 변환
  ```

#### 객체 배열 함수
##### forEach
- 예제
	``` javascript
	const array = [1,2,3,4,5];
	  array.forEach(num => {
	  console.log(num);
	})
	```

##### map
- 배열 안의 원소 변환 시 사용
- 파라미터로 변환할 형식 입력으로 받음
	``` javascript
	const array = [1,2,3,4,5];
	const square = n => n * n;
	const squared = array.map(square);
	```

- 또는 변환 함수명 없이
	``` javascript
	const squared = array.map(n => n * n);
	```

##### indexOf
- 찾는 항목이 몇 번째 원소인지 검색

##### findIndex
- 검색하고자 하는 항목이 배열이나 객체일 때 몇 번째 원소인지 찾음
- findIndex 함수에 검사하고자 하는 조건을 넣어 처리
	``` javascript
	const index = totos.findIndex(toto => toto.id ===3); //totos 배열에서 원소별 id 값이 3인 배열의 위치
	```

##### find
- findIndex와 비슷하지만 찾아낸 값을 반환

##### filter
- 배열에서 특정 조건을 만족하는 값들만 따로 추출하여 새로운 배열 생성
	``` javascript
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
	``` javascript
	arr1.concat(arr2);
	```
##### join
- 배열 안의 값들을 문자열 형태로 합쳐줌
	``` javascript
	array.join();  // 1,2,3,4,5
	array.join(' '); // 1 2 3 4 5
	array.join(', '); //1, 2, 3, 4, 5
	```

##### reduce
- 그래서 reduce()가 뭔데? 설명이 없음
- 예제
	``` javascript
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
	``` javascript
	function Animal(type, name, sound) {
		this.type = type;
	    this.name = name;
	    this.sound = sound;
	    this.say = function() {
		    console.log(this.sound);
		};
	}

	// 객체 생성
	const dog = new Animal('개', '멍멍이', '멍멍');
	```
  
- 함수의 이름은 대문자
- 새로운 객체를 만들 때 new 사용

### 객체 생성자 상속 받기
- 새로운 Dog 객체 생성자 구현시 Animal 기능 재사용
- 예제
	``` javascript
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
- 예제
	``` javascript
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

## 클래스
- ES5 에서는 객체 생성자 함수를 사용하여 비슷한 작업 구현
- ES6부터는 class 라는 문법 추가됨 상속도 쉽게 가능

- 예제
	``` javascript
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
	``` javascript
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

  <div class="code panel pdl" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  function print({alias,name, actor}) {
    const text = '${hero.alias}(${hero.name}) 역할은 ${hero.actor} 입니다.';
    console.log(text)
  }
  ```

  </div>

  </div>

### 객체 안에 함수 넣기

- 예제

  <div class="code panel pdl" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
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

  </div>

  </div>

### Getter, Setter

- get, set 붙여서 사용

- 예제

  <div class="code panel pdl" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
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

  </div>

  </div>

- sum() 함수를 호출하지 않아도 sum만 언급해도 값이 출력됨

- set 을 함수 앞에 붙여 파라미터 설정

  <div class="code panel pdl" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  set b(value) {
    this.b = value;
  }
  ```

  </div>

  </div>

- function을 쓸 필요 없이 set, get 으로 Setter, Getter언

# 모듈

- 코드의 재활용성을 높이고 유지보수를 쉽게 할 수 있는 방향, 방법 추구

- 재활용성이 높은 코드를 분리, 별도로 만들어 사용

<!-- -->

- 웹 브라우저에서 로직 모듈화

- Javascript의 변화 : 이전과 같이 **<u>웹 브라우저에서만 수행</u>**되는 것이 아닌 nodejs와 같이 <u>서버에서 수행</u>되거나 Google Apps Script같이 <u>구글 제품 위에서 수행</u>됨 =\> 자바스크립트의 유일한 호스트(웹브라우저)라는 말은 더 이상 사용되지 않음

<!-- -->

- 모듈이 없는 sciprt 예제\

  <div class="code panel pdl" style="border-width: 1px;">

  <div class="codeContent panelContent pdl">

  ``` syntaxhighlighter-pre
  <html>
  <script>
  function welcome() {
    return 'Hello World';
  }
  alert(welcome());
  </script>
  </html>
  ```

  </div>

  </div>

  - welcome() 함수가 자주 사용될수록(여러 페이지에서) welcome() 을 html 마다 정의하여 사용 → 별도의 파일에 welcome()을 정의하여 그 파일을 각 html 파일에서 include 하여 사용 =\> 모듈화

  - 예로 greeting.js 파일에 welcome() 함수를 정의

- 라이브러리

  - 모듈과 같은 개념

  - 자주 사용되는 로직은 재사용하기 편리하도록 잘 정리한 일련의 코드들의 집합 ex) jQuery