
1.  [Programming](README.md)
2.  [Programming](Programming_98307.md)
3.  [Web](Web_352452663.md)
4. [Javascript](JavaScript_282329107.md)
5. [기본 문법](366379068.md)

# 제어문

## 비교 연산자

- == : 동등 연산자 return true/false, 타입 검사 안 함
- === : 일치 연산자, 좌항과 우항이 “정확”하게 같은 때 true/false. 타입까지 검사

ex)
``` javascript
1 == '1' // true

1 === '1' // false

null == undefined // true

null === undefined // false

true == 1 // true

true === 1 // false

true == '1' // true

true === '1' // false
```

- == 보다 === 사용을 추천
- != : 부정
- !== : 정확하게 같지 않다. === 참고
- \> : 좌항이 우항보다 크다
- \>= : 크거나 같다.

### 연산자 사용 예
``` javascript
const a = 1;
const b = '1';
const equals = a === b; // false
const equals = a == b; //true
0 == false; // true
null == undefined; // true
```

### 두 값 일치
- !== : 타입 검사까지 수행
- != : 타입 검사 수행 안 함

## 조건문

### Boolean (true/false)

- if, else, else if
- 예제
	``` html
	<input id="night_day" type="button" value="night">
	```
- 버튼을 눌렀을 때 값이 변하도록 하는 코드 작성(value가 night 면 day 버전으로 바뀌고, Value가 day 이면 night 버전으로 바뀌도록)
- 해법
	``` html
	<input id="night_day" type="button" value="night" onclick="
	if(document.querySelector('#night_day').value ==='night') {
		document.querySelector('body').style.backgroundColor = 'black';
		document.querySelector('body').style.color = 'white';
		document.querySelector('#night_day').value = 'day';
	}
	else {
		document.querySelector('body').style.backgroundColor = 'white';
		document.querySelector('body').style.color = 'black';
		document.querySelector('#night_day').value = 'night';
	}
	">
	```

#### 코드 리팩토링
- 해당 html 태그 내의 onclick에서 사용할 시 document.querySelector 부분을 this로 중복 처리 가능
- 동일한 로직으로 다른 버튼을 구성할 시 this로 간결하게 처리할 수 있음
	``` html
	<input id="night_day" type="button" value="night" onclick="
	if(this.value ==='night') {
		document.querySelector('body').style.backgroundColor = 'black';
		document.querySelector('body').style.color = 'white';
		this.value = 'day';
	}
	else {
		document.querySelector('body').style.backgroundColor = 'white';
		document.querySelector('body').style.color = 'black';
		this.value = 'night';
	}
	">
	```

- 반복되는 구문을 변수로 선언해서 중복 제거 가능
- document.querySelector('body') 를 변수 처리
	``` javascript
	var target = document.querySelector('body');
	
	if(this.value ==='night') {
		target.style.backgroundColor = 'black';
		target.style.color = 'white';
		this.value = 'day';
	}
	else {
		target.style.backgroundColor = 'white';
		target.style.color = 'black';
		this.value = 'night';
	}
	```
### 논리연산자

- && : and 연산자
- || : or
- ! : not

### Boolean 대체

- 0 / 1로 boolean 대체
- if (0) // false
- if(1) // true (0 이 아닌 값을 true)
- 기타 false 로 간주되는 데이터형
  - if(!'')
  - if(!undefined)

    ``` javascript
    var a;
    if(!a) // false
    if(!null) // false
    if(!NaN) // false
    ```

## 반복문

### 배열
#### 배열 정의
- 일반 배열
	``` javascript
	const array = [1,2,3,4,5];
	```

- 객체 배열
	``` javascript
	const objects = [{name:'기린', age:2}, {name:'사슴', age:4}];
	```

- 사용 예제 
	``` javascript
	var member = ['test', ‘junit’,'marvel'];
	```

#### 배열 함수 (추가)
  - push : 배열 추가
	``` javascript
	member.push('ironman'); // 추가
	```
  - concat : 배열에 여러개 추가
	``` javascript
	member = member.concat(['hulk','spiderman']); // 복수 추가
	```
  - unshift : 배열 시작점에 추가
	``` javascript
	member.unshift('captain'); // 배열의 시작점에 추가
	```
  - splice : 지정한 위치에 배열 추가가
	``` javascript
	member.(2,0, ‘hawkeye’); // 두 번째 인덱스 뒤에 추가
	```

- 예제
	``` javascript
	objects.push({
    name: '사자',
    age: 10
  });
	```

#### 배열 함수(삭제)
  - shift : 배열 첫 번째 원소 제거
	``` javascript
	member.shift(); // 배열의 첫 번째 원소 제거
	```
  - pop : 배열 끝의 원소 제거
	``` javascript
	member.pop(); // 배열 끝의 원소 제거
	```

#### 배열 함수(정렬)
  - sort : 정렬
	``` javascript
	member.sort();
	```
  - reverse ; 역순 정렬
	``` javascript
	member.reverse(); // 역순 정렬
	```

#### 배열 크기
  - 배열의 길이 출력
	  - objects.length



### 반복문

#### 반복문의 종류
- while : 조건이 true 일 때 계속 실행
- do - while : 실행한 뒤 조건이 true 이면 계속 실행
- for: 

##### while
- 배열과 while 반복문
- 조건이 true 일 때 계속 실행
- 예제: 배열 내의 내용을 출력
	``` javascript
	fruits = ["mango", "watermelron", "banana","apple"];
	
	var i = 0;
	while(i < fruits.length) {
		document.write('<li>' + fruits[i] + '</li>');
		i = i + 1;
	}
	```

- 응용 예제 : html 내의 모든 link를 가져와서 색을 바꾼다. 
	``` javascript
	var alist = document.querySelectorAll('a');
	
	var i = 0;
	while(i < alist.length) {
		alist[i].style.color = 'blue';
		i = i + 1;
	}
	```


- 반복문 내 제어
  - break : 중간에서 중지(반복문 안에서 break 실행되면 반복문 즉시 종료)
  - continue : 해당 실행문은 중단되나 반복문은 계속됨
	``` javascript
	for(var i = 0; i < 10; i++ ) {
      if( 1 === 5) {
        continue; // 5에서 아래 write 실행 안 하고 반복문 수행 
      }
      document.write('coding everybody' + i);
    }
	```
    
### for…of
- 배열에 관한 반복문
	``` javascript
	let numbers = [1,2,3,4,5];
	for(let number of numbers) {
    console.log(number);
	}
	```

### for…in
- 객체를 위한 반복문
	``` javascript
	for ( let key in objects) {
		console.log('${key}: ${object[key]}');
	}
	```
  

