---
title:  "C++ STL 사용방법 모음집"
excerpt: "C++ STL 액기스"

categories: C/C++
tags:
  - [C,C++]
 
toc: true 
toc_sticky: true

date: 2021-08-21
last_modified_at: 2021-08-22
sitemap:


---


> Language : C++ ![C++](https://img.shields.io/badge/c++-%2300599C.svg?style=for-the-badge&logo=c%2B%2B&logoColor=white)
>
> IDEs/Editors : ![Visual Studio](https://img.shields.io/badge/VisualStudio-5C2D91.svg?style=for-the-badge&logo=visual-studio&logoColor=white)



## C++ STL(Standard Template Library) 자료형 모음집

### Array

고정길이의 배열을 표현하기 위해 사용하는 array container이다. (std::array)



**선언하기**

```c++
#include <array>
std::array<int, 길이> 변수명 = {초기값};
```

>  ex) std::array<int,5> arr = {1,2,3,4,5};
>
>  위의 선언에서 arr={1,2,3}; 으로 한다면 나머지 값 들은 0으로 채워짐.



**멤버함수**

- **arr.begin()**
  - 첫번째 원소를 '가리킴' (iterator와 함께 사용)

- **arr.end()**
  - 맨 마지막 '다음' 원소를 가리킴(iterator와 함께 사용)

- **arr.rbegin()**
  - 배열을 거꾸로 했을 때 첫번째 원소를 가리킴

- **arr.rend()**
  - 배열을 거꾸로 했을 때 맨 마지막의 '다음' 원소를 가리킴

- **arr.cbegin(), arr.cend()**
  - 원소를 가리키는것은 begin, end와 동일하나, 원소 값 수정은 불가능하다.
  - ex) * arr.cbegin() = 10; ( 불가능, 하지만 .begin()으로 하면 가능하다! )

- **arr.front() = *arr.begin()**
  - 첫번째 원소의 '값'

- **arr.back() =/= *arr.end()**
  - 마지막 원소의 값 ( .end()와는 차이가 있음! )

- **arr.fill( value )**
  - arr내부 모든 원소의 값을 value로 바꿈 ( arr[]로 선언한 배열에는 적용불가 )

- **arr.swap( arr2 )**
  - ( 길이와 type이 같다면 ) 서로의 배열 원소들을 바꿈

- **arr.at( N )**
  - N번째 원소 값 (arr[N]에 비해 조금 더 안전하다. N이 배열 범위밖을 벗어나도 throw exception)

- **arr.empty()**
  - 비어있다면 true, 아니라면 false 반환

- **arr.size() = arr.max_size()**
  - 배열의 크기 반환



### Vector

동적으로 원소를 추가할 수 있어서 크기가 가변적이다.



**선언하기**

```c++
#include <vector>
std::vector<int> vec;
```

> std::vector< int > vec = {1,2,3,4,5};	크기가 5인 벡터
>
> std::vector< int > vec(10);	크기가 10인 벡터, 원소는 모두 0으로 초기화
>
> std::vector< int > vec(10,5);	크기가 10인 벡터, 원소는 모두 5로 초기화



**멤버함수**

- **vec.push_back( value );**
  - 맨 뒤에 원소 value 추가

- **vec.insert( vec.begin(), value );**
  - 원하는 위치에 원하는 값 삽입

> vec.insert( vec.begin()+4, value );
>
> 5번째 자리에 (vec[4]) 값이 value인 원소 삽입

- **vec.pop_back();**
  - 맨 마지막 원소 제거

- **vec.erase( vec.begin() );**
  - 원하는 위치, 범위의 원소 제거

> vec.erase( vec.begin() + 4 );
>
> - 5번째 원소 제거
>
> vec.erase( vec.begin() + 1, vec.begin() + 4 );
>
> - vec[1]~vec[3] 원소 제거

- **vec.clear();**
  - 벡터 초기화 (길이 = 0)

- **vec.resize( n );**
  - 크기를 n으로 바꿈 ( 늘어난 위치의 원소들은 0으로 초기화 )

- **vec.size(),  vec.empty(),  vec.front(),  vec.back(),  vec.begin(),  vec.end()**
  - array와 똑같은 사용법 (array 참조할 것 !)



### Stack

일반적인 스택이다. Last In First Out



**선언하기**

```c++
#include <stack>
std::stack<int> st;
```



**멤버함수**

- **st.push( value );**
  - push value! (잘 모르겠다면 stack부터 이해하고 오기!)

- **st.pop();**
  - Pop value!

- **st.top();**
  - 상단 원소 값 반환

- **st.swap( st2 )**
  - 스택 st, st2의 내용을 바꿈 ( 길이는 상관없음. type만 같으면 됨 )

- **st.size(),  st.empty()**
  - 설명이 필요한가?



### Queue

일반적인 큐 이다. First In First Out



**선언하기**

```c++
#include <queue>
std::queue<int> Q;
```



**멤버함수**

- **Q.push( value );**
  - 맨 뒤에 value 추가 ( 잘 모르겠다면 Queue부터 이해하고 오기 )

- **Q.pop();**
  - 맨 앞 데이터 삭제

- **Q.front();**
  - 맨 앞 데이터 값 반환

- **Q.back();**
  - 맨 뒤 데이터 값 반환

- **Q.size(), Q.empty(), Q.swap( q2 )**
  - 설명 생략



### Priority Queue

일반적인 큐 와 사용방식은 같다. 하지만 큐 가 정렬이 된다는 특징이 있다.



**선언하기**

```c++
#include <queue>
std::priority_queue<int> pq;
```

> <==>  std::priority_queue < int, vector< int >, greater< int > > pqq;
>
> - pqq.top() 이 제일 작은 수가 된다.



**멤버함수**

- **pq.push( value );**
  - value 추가

- **pq.pop();**
  - 맨 앞 원소 제거	( 원소중에 제일 큰 값이 제일 앞으로 나온다 )

- **pq.top();**
  - 맨 앞 값 반환

- **pq.size(),  pq.empty()**
  - 설명 생략



.back(),  .front()는 사용 불가능하다 !



### Map

중복을 허용하지 않는 Key 와 Value 쌍으로 이루어진 Tree.	<Key, Value>

Map들은 정렬이 불가능하다!



**선언하기**

```c++
#include <map>
std::map<int, string> mp;
//key를 기준으로 오름차순 정렬이 된다.

//내림차순으로 하고 싶다면
std::map<int, string, greater<int>> mp;
```



**멤버함수**

- **mp.insert( std::make_pair( key, value ) );**
  - 원소 추가.	make_pair(key, value) 대신에 { key, value }도 가능
  - mp[key]=value의 방법으로도 insert와 수정 가능
- **mp.erase( key );**
  - 원소 삭제,  범위로 삭제 할 수 있다.
- **mp.clear();**
  - 내용을 다 지움 ( 원소 개수 = 0 )
- **mp.begin(),  mp.end()**
  - 설명 생략	(Vector와 동일)
- **mp.find( key );**
  - key값에 해당하는 iterator를 반환
- **mp.count( key );**
  - key값에 해당하는 원소의 개수 반환 ( 1 or 0 )
- **mp.empty(),  mp.size()**
  - 설명 생략



**사용 예제**

```c++
#include <map>
#include <iostream>
using namespace std;

int main()
{
	std::map<int, int> mp;
	mp.insert(make_pair(10, 3));
	mp.insert({ 200,30 });
	mp.insert({ 3,300 });
	mp.insert({ 3,10000 });
	mp.insert({ 1,333 });

	cout << "Initial state" << endl;
	for (auto a : mp) {
		cout << a.first <<"--"<<a.second << endl;
	}
	//정렬되어서 들어가는지 확인
	cout << "Size : " << mp.size() << "\n\n";

	cout << mp[100] << endl;
	cout << mp[101] << endl;
	cout << "After get value from not inserted pair" << endl;
	//insert 하지 않은 key값을 확인했을 때 어떻게 되는지 확인
    //insert하지 않고 value를 확인하면 0이다.
	cout << "Size : " << mp.size() << "\n\n";

	mp.erase(mp.begin(), mp.find(100));
	cout << "After erase some pairs" << endl;
	for (auto a : mp) {
		cout << a.first << endl;
	}
	//find를 이용해 erase해보기
	//만약 find로 key값을 못찾았다면 clear()와 동일한 결과를 낸다.
	cout << "Size : " << mp.size() << "\n\n";

	mp.clear();
	cout << "After Clear" << endl;
	cout << "Size : " << mp.size() << endl;
}
```

```bash
Initial state
1--333
3--300
10--3
200--30
Size : 4			넣은 순서대로 결과가 나옴

0
0
After get value from not inserted pair
Size : 6

After erase some pairs
100
101
200
Size : 3

After Clear
Size : 0
```



### Unordered Map

map보다 더 빠른 탐색을 하기 위해 사용

Time Complexity of **map** : O(log n) 

Time Complexity of **unordered_map** : O(1)



**선언하기**

```c++
#include <unordered_map>
std::unordered_map<int, int> um;
```



**멤버함수**

- **um.insert( { key, value } );**
- **um.insert( std::make_pair( key, value ) );**
- **um[key] = value;**
  - 모두 동일하게 pair를 insert한다
  - um[key]=value의 경우, value값을 수정할 수 있다.
- **um.erase();**
  - 원소 삭제.   범위로 삭제할 수 있다.
  - ex) um.erase( um.find(3), um.end() );
    - key값 3을 insert한 것 부터 모두 삭제 (3 포함)
- **um.empty(),   um.size(),   um.find( key ),   um.count( key ),  um.clear(), um.begin(), um.end()**
  - 설명 생략



### Multi Map

중복되는 key가 가능한 map



**선언하기**

```c++
#include <map>
std::multimap<int, int> mmp;
```



**멤버함수**

- **mmp.insert(std::pair<int, int>(key, value));**
- **mmp.insert( { key, value } );**
  - key-value 쌍 insert
- **mmp.erase();**
  - 원소 삭제,  범위로 삭제 할 수 있다.
  - Unordered_map 참조하기
- **mmp.empty(), mmp.size(), mmp.find( key ), mmp.count( key ), mmp.clear(), mmp,begin(), mmp.end()**
  - 설명 생략
