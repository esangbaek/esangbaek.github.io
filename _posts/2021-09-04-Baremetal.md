---
title:  "베어메탈 프로그래밍 이란"
excerpt: "임베디드 시스템 - 베어 메탈"

categories: BMP
tags:
  - [OS, C]
 
toc: true 
toc_sticky: true

date: 2021-09-04
last_modified_at: 2021-09-08
---
### 베어 메탈(Bare metal) 프로그래밍이란?

_**Bare-metal programming is a term for programming that operates without various layers of abstraction**_

(운영체제와 같은 기본 추상화가 없는 하드웨어 환경, 보통 하나의 무한루프로 실행되는 펌웨어를 만듦)

**베어 메탈의 특징**

- OS (Operating System) 사용하지 않음
- 라이브러리 사용하지 않음
- 프로그램은 하드웨어 바로 위에 얹어져 있다.

![image](https://user-images.githubusercontent.com/65602371/132133341-f6da6fe1-3f40-4a17-9c38-249562469fd0.png)

- 주로 C언어 혹은 어셈블리어를 사용해 **라이브러리없이** cross compiler만 가지고 프로그래밍한다.
- 주요 목적 : I/O device (sensors, actuator) control

&nbsp;

### I/O interfacing method

- Memory mapped I/O
  - 디바이스와 메모리가 address space를 공유한다. 
  - Input/Output이 memory read/write와 똑같이 동작
  - no special commands for I/O
  - 대부분의 컴퓨터 시스템이 사용하는 방식
- Isolated I/O
  - 입출력을 위한 별도의 주소공간이 할당되어 있다.
  - special commands for I/O

BUS : Address lines, Data lines, Control lines

### I/O technique

- Programmed (Polling)
  - 상태 레지스터를 계속 감시하다가 상태가 변경되었을 때 값을 읽어들임
  - **CPU가 계속해서 검사함**
    - 장점 : 코드를 작성하기 편하고 이해하기 쉽다 (소규모에 적합)
    - 단점 : CPU를 점유하는 시간이 증가해 성능이 감소함, 실행시간이 길고 복잡하면 비효율적임

- Interrupt driven
  - interrupt가 발생하면 값을 읽어와서 처리
  - **변화를 CPU에 통보함**
- Direct Memory Access (DMA)
  - 주변장치들이 메모리에 직접 접근하여 읽거나 쓸 수 있도록 하는 기능
  - **CPU의 개입없이 I/O 장치와 기억장치 사이의 데이터를 전송하는 접근 방식**
    - CPU의 효율을 높일 수 있다
    - 프로그램 실행 중 인터럽트의 발생 횟수를 최소화할 수 있다
    - **Cache Coherence 문제를 야기할 수 있다**

&nbsp;

Bare Metal Programming은 주소값에 대한 이해가 중요하다!

