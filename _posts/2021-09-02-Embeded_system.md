---
title:  "임베디드 시스템이란"
excerpt: "임베디드 시스템 개요"

categories: ES
tags:
  - [OS, C]
 
toc: true 
toc_sticky: true

date: 2021-09-02
last_modified_at: 2021-09-02
---
### Embedded System이란?

_A computer system dedicated to a particular function within a larger mechanical or electrical system_

_Information processing systems embedded into a larger product_

- 특정 목적을 수행하기 위해 기계 혹은 전자 시스템 내에 들어가는 컴퓨터 시스템          

&nbsp;

&nbsp;

**간단한 임베디드 시스템 구조**

(센서) -> (인터페이스) -> (CPU) -> (인터페이스) -> (액추에이터)        

&nbsp;

**또 다른 의미의 임베디드 시스템**

- Reactive systems
- Real-Time systems
- Cyber-physical	  (규모가 크고, 복잡, 네트워크 구조)
- Control engineering systems

&nbsp;

### 임베디드 시스템의 특징

- Dependable (Dependability) : 중요한 역할을 수행하기 때문
  - Reliability    ex) 단위 시간동안 오류가 발생할 확률
  - Availability    ex) 특정 시간에 정상적으로 동작할 확률
    - Reliability 와 Availability는 밀접한 관련이 있지만 Reliability가 높다고 Availability가 높은것은 아니다.
  - Safety    ex) 고장이 났을 때 미치는 damage의 정도
  - Security    
  - Maintainability    ex) 얼마나 쉽게 적은 비용으로 적은 시간안에 개선할 수 있는가

일반 프로그램(예: 어플, 웹)은 사용중에 문제가 생겨도 큰 문제가 되지 않는 경우가 많다.

반면에 임베디드 시스템은 비행기, 자동차, 우주선 등에 사용되므로 오류가 치명적인 결과를 초래할 가능성이 높다.

따라서 개발을 하되, 위의 성질을 만족하게 하는것이 더 중요하다!

&nbsp;

- Efficient
  - Energy efficient
  - Memory efficient  ( especially for systems on a chip )
  - Processor efficient
  - Weight, space efficient
  - Cost efficient

&nbsp;

- Real-time constraints

  - hard real-time constraints
    - 제한 시간안에 처리를 못 할 경우 시스템에 치명적인 결과를 초래한다.

  - soft real-time constraints
    - 제한 시간안에 처리를 못 하여도 전체 시스템에 큰 영향이 없다.

&nbsp;

- Safety problems  (Mission Critical)
  - 제품의 안전을 위해 국제 규격을 만족해야 한다. ex) ISO26262

&nbsp;

- Centralized or Distributed System
  - centralized systems은 distributed systems에 비해 간단한 구조이다.
  - Distributed systems는 network에 의해 서로 동작하므로 프로그래밍이 더 까다롭다.

&nbsp;

### 임베디드 시스템 디자인의 어려운 점

- Real-time parallel and distributed programming
- Relation with control engineering
- Intricate dependency between HW, application SW, OS, middleware
- Certification authorities
- ...

현재 산업 현장은 Model-based design이다. 

사용 예 : AUTOSAR

