# Hash Table (static)

## 해시 테이블이란?
> 해시 테이블(hash table), 해시 맵(hash map), 해시 표는 컴퓨팅에서 키를 값에 매핑할 수 있는 구조인, 연관 배열 추가에 사용되는 자료 구조이다. 해시 테이블은 해시 함수를 사용하여 색인(index)을 버킷(bucket)이나 슬롯(slot)의 배열로 계산한다. -위키백과-

## 특징
- 키와 값의 쌍으로 이루어진 값들의 리스트이다.
- 키는 해당하는 값들을 지칭하기 위한 일종의 표시이다.
- 해시 테이블의 값 룩업(값 찾기) 는 딱 한 단계만 걸리므로 평균적으로 O(1)이다. 하지만 해시 충돌이 일어날 수 있으므로 최악의 경우 O(N)이다.
- 해시 함수를 이용한다.

## 해시 함수란?

- 임의의 크기를 가지는 type의 데이터를 고정된 크기를 가지는 type의 데이터로 변환하는 함수
- (hash table에서) 임의의 데이터를 정수로 변환하는 함수

```text
누구나 자료구조와 알고리즘(도서) 161p 발췌

예를 들어 다음처럼 단순하게 글자와 숫자를 짝지을 수 있다.
A = 1
B = 2
C = 3
D = 4
E = 5

위 코드에 따르면 
ACE는 135로,
CAB는 312로,
DAB는 412로,
BAD는 214로 변환된다.

문자를 가져와 숫자로 변환되는 이러한 과정을 해싱이라 부른다. 글자를 특정 숫자로 변환하는 데 사용한 코드를 해시 함수라 부른다.
... 중략

해시 함수가 유효하려면 딱 한 가지 기준을 충족해야 한다. 해시 함수는 동일한 문자열을 해시 함수에 적용할 때마다
항상 동일한 숫자로 변환해야 한다.
```

## 해시 충돌 (hash_collision)
- 해시 충돌은 피할 수 없다.
- key는 다른데 hash가 같을 때
- key도 hash도 다른데 hash % map_capa 결과가 같을 때

## 해시 충돌 해결 방법
1. open addressing (CPython의 dictionary)
2. separate chaining (Java의 HashMap)

## Java의 HashMap
| 구현            | 데이터 접근 시간              | 삽입/삭제 시간           | 해시 충돌 해결 방법 | dafault initial capacity      | resize 타이밍 | resize 규모 | hash table capacity |
|---------------|------------------------|--------------------|-------------|-------------------------------|------------|-----|---------------|
| hash table 사용 | (보통) 모든 데이터를 상수 시간에 접근 |(보통) 모든 데이터를 상수 시간에 접근|  Seperate Chaining | 16          | map cpapcity의 3/4 이상 데이터 존재 시 | 2x  | power of 2          ||

### Reference
- https://baeharam.netlify.app/posts/data%20structure/hash-table
- https://www.youtube.com/watch?v=ZBu_slSH5Sk&t=3s
- 누구나 자료구조와 알고리즘(도서) 