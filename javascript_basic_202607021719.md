
# 기본 문법

## 변수 선언
- let 사용
  - var로 정의 안한 변수는 전역 변수가 되기도 함(ES 6 이전)
  - ES6 부터는 var로 변수 선언 안하는 추세임
  - var는 함수 영역 scope 변수 선언시
  - let, const는 block 영역 scope
  - 변수의 이름은 특수 문자를 제외한 모든 문자 사용
  - ; 명령의 끝, 줄바꿈으로 생략 가능
  - let {} 블럭 내에서만 유효
```javascript
javascript
    function calcSum(n) {
      let sum = 0; // let 선언 변수, 재선언 안 됨
      for(let i = 1; i < n; i++) {
        sum += i;
      }
      console.log(sum);
    }
```

- 전역변수로 사용시 변수 이름과 초기값만 할당
```javascript
javascript
    function calcSum(n) {
      sum = 0 ; //전역변수 선언
      ...
    }
```

- 변수 호이스팅
    - 변수의 선언과 할당을 분리해서 선언부분을 scope의 가장 위쪽으로 끌어 올림
```javascript
      var x = 10;

      function dispalyNumber() {
          console.log("x is " + x);
          console.log("y is " + y);
          let y = 20; // var y = 20 선언시 y는 undefned로 출력됨
      }
```

- let y = 20. 선언 대신 var y = 20; 으로 선언시 변수 호이스팅이 발생
      - 6라인에 var y = 20 선언하면 var y; 선언이 y 변수 사용하기 전으로 정의가 됨(코드 상이 아닌 실행시에)
      - y = 20 값은 할당은 6라인에서 이루어지기 때문에 5라인ㅇ의 console.log 에서 undefinded가 찍힘
      - let 선언시 오류 발생. var의 잘못된 사용을 let으로 정상처리한 느낌

### let vs var
- var 똑같은 이름으로 여러 번 선언 가능
- var 최근 사용 안 함


 
 const
  - 상수 변수
```javascript
javascript
    const currentYear = 2020;
    console.log(currentYear);
    const currentYear; //변수 재선언 오류 발생
```

- for 내 변수는 let으로 처리
  - ES6에서 var 보다 let으로 변수 선언 권장

### 숫자

- 숫자형 선언 안함
  - let value = 1;

- 문자열 정의
  - '', ““ 로 감쌈
  - let text = ‘hello’;

- boolean
  - let good = true;
  - let loadin = false

  - null vs undefined
    - null: 값이 없음
    - undefined: 값이 아직 설정되지 않음

# 주석
- // 한줄 주석
- /\* \*/ 여러 줄 주석