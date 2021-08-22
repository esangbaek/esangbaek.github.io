---
title:  "C++ STL 사용방법 모음집2 (algorithm,string,..)"
excerpt: "C++ STL 액기스2"

categories: C/C++
tags:
  - [C,C++]
 
toc: true 
toc_sticky: true

date: 2021-08-22
last_modified_at: 2021-08-22
sitemap:


---


> Language : C++ ![C++](https://img.shields.io/badge/c++-%2300599C.svg?style=for-the-badge&logo=c%2B%2B&logoColor=white)
>
> IDEs/Editors : ![Visual Studio](https://img.shields.io/badge/VisualStudio-5C2D91.svg?style=for-the-badge&logo=visual-studio&logoColor=white)



## C++ STL(Standard Template Library) 자료형 모음집

### Algorithm, String, ...

Algorithm : 원소들에 대해 작업할 수 있는 여러가지 함수가 들어있다.

String : 문자열을 다룰 때 사용한다

#### Algorithm

**라이브러리 가져오기**

```c++
#include <algorithm>
```



**유용한 함수**

- **sort( a.begin(), a.end() )**
  - 배열, 벡터 a를 정렬함 (오름차순)
  - 시간복잡도 : O(nlog n)  ,  퀵정렬기반
  - 내림차순으로 하고 싶다면 sort( a.begin(), a.end(), greater<>() )
  - 다른 방식의 사용 예시
    - sort( &arr[2], &arr[5] )	: 	3~5번째 원소 정렬
- **reverse( a.begin(), a.end() )**
  - 배열, 벡터, 문자열의 순서를 뒤집는다
  - sort와 같이 사용하면 내림차순으로 만들 수 있다.
- **copy( temp.begin(), temp.end(), origin.begin() );**
  - 벡터에서 주로 사용한다. (strncpy 대용으로 사용 가능. 단, '\0'를 추가해야한다.)
  - 복사할 자리에 공간이 있어야 한다.
- **max_element( arr.begin(), arr.end() )**
  - max_element( &arr[0], &arr[last index]+1 )	:	똑같은 방식
  - 가장 큰 값의 참조값을 반환
  - *max_~로 값을 반환하게 할 수 있다.
    - min_element()	:	가장 작은 값의 참조값을 반환
- lower_bound & upper_bound
  - lower_bound( vect.begin(), vect.end(), value )
    - value값 보다 같거나 큰 첫번째 원소의 위치 참조값
    - 없다면 end를 return 한다
  - upper_bound( vect.begin(), vect.end(), value )
    - 처음으로 value값을 초과하는 원소의 주소
    - 없다면 end를 return
  - 만약 index를 반환받고 싶다면?
    - lower_bound( vect.begin(), vect.end(), value ) - vect.begin()
    - upper도 동일하게 사용
    - 배열일 경우는
      - upper_bound( arr, arr+10, val ) - arr

#### String

라이브러리 가져오기

```c++
#include <string>
```

유용한 사용법

- int -> string 변환
  - to_string( 정수 )
    - ex) string str;	str=to_string(1234);
- string -> int 변환
  - atoi(), std::string c_str()
    - ex) string str="123";    int num=atoi(str.c_str());
    - c_str()은 해당하는 string의 첫번째 문자 주소값을 반환한다
- substr()
  - 문자열의 일부를 추출할 때, 일부를 복사할 때 사용

```c++
string numbers = "0123456789";
numbers.substr()	-> "0123456789"	문자열 복사 가능
numbers.substr(3)	-> "3456789"	인덱스 이후 모든 문자열
numbers.substr(3,2)	-> "34"		인덱스 이후 부분 문자열
```



#### Cmath

라이브러리 가져오기

```c++
#include <cmath>
```



- 절댓값
  - abs()

#### 기타

- cout, cin 최적화하기

```c++
ios::sync_with_stdio(false);
cin.tie(NULL);
cout.tie(NULL);
```

c, c++ 스타일의 입출력을 혼용하지 않게 해서 독립된 자신만의 버퍼를 사용한다.

- 대량의 데이터를 출력할 때
  - endl 대신 "\n"을 사용해보자 !
  - endl은 버퍼를 flush하기 때문에 느리다
