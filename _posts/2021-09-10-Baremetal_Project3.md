---
title:  "베어메탈 프로그래밍 3"
excerpt: "MBED Library"

categories: BMP
tags:
  - [OS, C]
 
toc: true 
toc_sticky: true

date: 2021-09-10
last_modified_at: 2021-09-19
---
### MBED Library

**펌웨어의 역할** : HW를 제어하는 펌웨어는 기본적으로 하드웨어 레지스터의 전기적 신호를 확인하고 변화시켜서 제어함

- MCU 레지스터는 하드웨어(또는 주변 기기)와 통신하는 단일 바이트 크기의 공간
- 레지스터 주소값을 사용하여 직접 접근할 수 있음 ( 포인터가 중요한 이유 ! )

![image](https://user-images.githubusercontent.com/65602371/132632905-417d2d5b-f447-4a18-82ea-e19a6f073393.png)

  - 레지스터 접근을 위해 사용하는 Datasheet의 일부

&nbsp;

![image](https://user-images.githubusercontent.com/65602371/132633333-72811129-7795-44a9-bde8-1cad01271a64.png)

- 하드웨어가 변경되면, 레지스터의 구성이나 주소값 등이 모두 달라짐
- 따라서 펌웨어가 레지스터에 직접 접근하여 하드웨어를 제어하도록 개발하는 것은 비효율적
- MBED Library는 MCU HW에 종속적인 부분과 사용자 Application을 분리하여 계층화하였다
  - 더 효율적인 작업이 가능해 짐

**MBED Library** : HW를 추상화한 Data Structure를 사용하는 과정을 다시 추상화하여 객체지향적인 API로 제공한 것



### 보드 활용하기

