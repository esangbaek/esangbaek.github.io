---
title:  "베어메탈 프로그래밍 2 + ARM core2"
excerpt: "Chip에 대해 더 많이 알아보기"

categories: BMP
tags:
  - [OS, C]
 
toc: true 
toc_sticky: true

date: 2021-09-09
last_modified_at: 2021-09-24
---
### 베어메탈 프로그래밍과 관련된 몇가지 정보들

> Flash memory : 비 휘발성 ( Non-volatile )
>
> SRAM : 휘발성( Volatile ) , 빠른속도

&nbsp;

Actuator -> Interface -> Serial Communication -> CPU

**연결 protocol** : HW적으로 동작함

ex) LED제어	HW : GPIO pin에 연결, SW를 제외한 모든 과정

​						SW : 특정 port의 bit을 제어, register값 수정

