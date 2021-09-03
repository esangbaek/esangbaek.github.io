---
title:  "차량용 전장 장치에 대해서"
excerpt: "임베디드 시스템 - 전장 장치"

categories: ES
tags:
  - [OS, C]
 
toc: true 
toc_sticky: true

date: 2021-09-03
last_modified_at: 2021-09-03
---
### 자동차 전장 장치

- 전장 장치 추세

자동차에서 전자 제어 유닛(ECU : Electric Control Unit)의 비율이 계속해서 높아지고 있는 추세이다.

또한 Software의 비중도 같이 증가하고 있다.

![image](https://user-images.githubusercontent.com/65602371/132014052-7981a1d5-7a6a-40d0-9aaa-00802ca05bf8.png)

출처 : https://jmagazine.joins.com/forbes/view/319023

과거에는 엔진 점화, 라디오 등에 쓰이던 전장부품이 현대로 오면서 분야가 점점 확대 되고 있고, 따라서 SW와 ECU의 중요성이 높아져가고있다.

&nbsp;

- 전장 장치의 종류
  - **Engine electronics**
  - **Chassis electronics (샤시)**
    - Transmission (Powertrain) electronics
  - **Body electronics**
  - Passive safety  샤시,바디에 중요
  - Active safety
  - Passenger comfort  샤시, 바디에 중요
  - Infortainment systems  예) 음악, 영화, 업무..

> **Active safety 와 Passive safety의 차이점**
>
> **Active safety** : 사고를 방지하기 위한 역할 (능동적임, work to prevent accidents)
>
> ​	사용 예 : 충돌방지 장치
>
> **Passive safety** : 사고가 일어나면 반응해서 사람을 보호하기 위한 역할 (수동적임, activate during a collision to protect the driver and passengers)
>
> ​	사용 예 : 에어백
>
> 최근에는 두 가지가 혼합된 형태인 **Integrated Safety System**이 등장하였다.

&nbsp;

기계적 장치보다 ECU를 사용하면 더욱 정밀하게 제어가 가능해 전체적인 효율을 높일 수 있다.

-> 성능과 안정성이 증가함.

&nbsp;

### Electric Control Unit (ECU)

- ECU사용의 이점
  - Reduced exhaust emission
  - Increased fuel efficiency
  - Improved drive-ability
  - Smoother and/or quieter engine operation
  - Safety functions
  - Comfort functions
  - Interactions with other systems
  - Diagnostics

&nbsp;

- MPU (Micro Processor Unit) vs MCU (Micro Controller Unit)
  - MPU 구성 : 제어장치, 연산장치, BUS
    - 사용 예 : 핸드폰내의 연산장치
  - MCU 구성 : MPU + Memory + I/O  => One chip에 만들어 낸 것

MPU를 써서 제어장치를 만들지 않는다. 

​	이유 : 비용문제, Board의 크기가 너무 커짐 (메모리, I/O, ...를 추가하게 되면)

&nbsp;

- ECU 구성요소
  - Microprocessor/ Microcontroller
  - Input Output interface
    - Sensors
    - Actuators
  - Memory Unit
    - Boot Memory
    - Program memory
    - Calibration memory
    - Data memory

&nbsp;

**그렇다면 ECU는 어떻게 발전하고 있는 것일까?**

*E/E : Electrical / Electronic  (전기/전자)

Distributed E/E Architecture (Module 구조, 각각의 기능에 해당하는 ECU가 존재) ->

Cross Domain Centralized E/E Architecture (Domain에 중점을 두고 기능이 중앙 집중식) ->

Vehicle Centralized E/E Architecture (클라우드 컴퓨팅이 최종적인 목표)





