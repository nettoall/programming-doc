1.  [Programming](README.md)
2.  [Programming](Programming_98307.md)
3.  [Java](Java_25001989.md)
4.  [Java Basic](Java-Basic_399278081.md)
5.  [Collections](Collections_25002006.md)

# List Collections

Created by Dongwook Han, last modified on 3월 15, 2024

## List
- 객체를 인덱스로 관리
- 객체의 번지 참조
- 중복 허용, 순서 보장
- 길이 동적 변화
- ArrayList, Vector, LinkedList
- 빈번한 객체 삭제나 삽입이 일어날 경우 LinkedList 사용 권장
  

![](attachments/25002021/25002040.jpg)

- List 공통 Method

| 기능                       | 메소드                        | 설명              |
| ------------------------ | -------------------------- | --------------- |
| 객체 추가                    | boolean add(E e)           | 주어진 객체를 맨 끝에 추가 |
| void add(int index, E e) | 주어진 인덱스에 객체 추가             |                 |
| set(int index, E e)      | 주어진 인덱스에 객체 변경             |                 |
| 객체 검색                    | boolean contains(Object o) | 객체 포함여부         |
| E get(int index)         | 주어진 인덱스의 객체 리턴             |                 |
| isEmpty()                | 컬렉션이 비었는지 여부               |                 |
| int size()               | 저장된 전체 객체수                 |                 |
| 객체 삭제                    | void clear()               | 모든 객체 삭제        |
| E remove(int index)      | 주어진 인덱스 객체 삭제              |                 |
| boolean remove(Object o) | 주어진 객체 삭제                  |                 |

## ArrayList

### 특징
- 배열은 생성시 크기 고정,
- ArrayList는 저장 용량 초과시 자동으로 저장 용량 증가
- 기본 생성(10개의 객체를 저장할 수 잇는 초기 용량 할당)
  List\<String\> list = new ArrayList\<String\>();
- ArrayList에 객체 추가시 add(int index, E e) 사용시 해당 index 부터 마지막 인덱스까지 1씩 밀려남
- remove(int index, E e) 사용시 해당 인덱스부터 끝까지 1씩 당겨짐
- 동적인 객체 추가가 아닌 고정된 수의 객체 추가시 다음과 같이 정의
  List\<T\> list = ArrayList.asList(T… a);
```java
java
  List<String> list = ArrayList.asList("테스트1","테스트2");
```

## Vector
- 생성
  List\<E\> list = new Vector\<E\>();
- synchronized 메소드로 구성되어 Thread-safe 함 → Thread 사용시 공유 Collection 선언시 사용

## LinkedList
- ArrayList(내부 배열에 객체를 저장해서 인덱스로 관리) vs LinkedList(인접 참조를 링크해서 체인처럼 관리)

- 구조

![](attachments/25002021/25067558.jpg)

- 특정 인덱스의 객체 삭제시 앞뒤 링크만 변경됨
- 특정 인덱스의 객체 추가시 앞뒤 링크만 변경됨
- 잦은 객체 삽입/삭제 시 LinkedList가 ArrayList보다 나으나,ArrayList 끝에서만 객체 추가/삭제시에는 LinkedList보다 빠름
