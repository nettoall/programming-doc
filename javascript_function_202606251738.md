
1.  [Programming](README.md)
2.  [Programming](Programming_98307.md)
3.  [Web](Web_352452663.md)
4. [Javascript](JavaScript_282329107.md)
5. [기본 문법](366379068.md)

# 함수
- 반복되는 로직을 사용 가능한 모듈로 정의
- 형식 : function 함수명(인자…) { 코드; return 반환값; }

- 익명함수로 정의도 가능
	``` javascript
	var numbering = funcation() {
    i = 0;
    while(i < 10) {
      document.write(i);
      i += 1;
    }
  }
  numbering();
	```

## 화살표 함수

- function 대시 => 사용
	``` javascript
	const add = (a,b) => {
	  return a + b;
	}
	```

## 함수 줄이기
- 예제
	``` javascript
	const add = (a, b) => a+b;
	```




