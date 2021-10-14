---
title:  "베어메탈 프로그래밍 1 + ARM core"
excerpt: "Bare metal programming을 시작하기에 앞서"

categories: BMP
tags:
  - [OS, C]
 
toc: true 
toc_sticky: true

date: 2021-09-08
last_modified_at: 2021-09-09
---
### 임베디드 개발 패러다임

![image](https://user-images.githubusercontent.com/65602371/132520150-6d83a1a0-61d7-4f7b-b1c7-d5dd82eb35e8.png)

**임베디드 소프트웨어의 발전 과정**

- 기존의 자동차 임베디드 소프트웨어는 제일 왼쪽과 같이 HW위에 바로 SW가 얹어져 있는 구조였다. 따라서 하드웨어에 완전히 종속적인 low-level programming을 하였다. 이 과정의 단점은 하드웨어가 바뀌면 그 위의 소프트웨어도 많이 바꾸어야했다.
- 위의 단점을 보완하기 위해 가운데 그림과 같이 Basic Software 개념을 도입해 HW와 독립적으로 SW개발이 가능하게 되었다. 
- 하지만 회사마다 Application Software와 Basic Software에서 사용하는 모듈과 HW가 다르기 때문에 서로 호환하는데 문제가 있었고, 이를 해결하고자 각 회사들의 주요 기술들을 모아서 ASW와 BSW를 표준화 시킨것이 AUTOSAR이다.

&nbsp;

MCU, I/O device등에 완전히 종속적인 low-level programming -> Bare-Metal Programming

&nbsp;

### 보드 및 사용 프로그램 소개

사용 보드

- Bare-Metal Programming을 하기 위한 보드
  - STM NUCLEO F401RE CPU Board (ARM cortex-M4 기반)
  - Core : ARM 32-bit Cortex-M4 CPU, 84MHz
  - 512Kbytes Flash memory (Non-Volatile)
  - 96Kbytes SRAM (Volatile)

![image](https://user-images.githubusercontent.com/65602371/132523603-e4d71e42-bd6b-484c-a90d-c22bc7e53b13.png)

**Circuit Diagram**

![image](https://user-images.githubusercontent.com/65602371/132525588-b52c1ab0-a13e-4d0d-a9b7-3278e7a7c139.png)

&nbsp;

펌웨어 개발 환경

- Keil uVision5 IDE

&nbsp;

### ARM Processors

영국의 ARM사에서 개발한 RISC 구조를 가지는 일련의 32비트 프로세서군을 가르키는 용어

- 1980년 초반 : 영국 Acorn사에서 개인용 컴퓨터에 내장할 수 있는 새로운 프로세서의 개발을 시작
- 1985.4 : 최초의 32비트 RISC 프로세서 개발 (ARM1)

- 1980후반 : Advanced RISC Machine 설립

&nbsp;

**특징**

- 내부 구성이 간단하여 동작속도가 빠르면서도 전력 소비가 적음
- ARM1 부터 시작하여 현재는 Cortex까지 발전하였음
  - Cortex란?  ARMv7 Architecture의 코어를 가지는 프로세서
- 휴대폰, 게임기 등의 모바일 기기 부문에서 많이 사용하고 있음
- <u>ARM사는 Microprocessor를 만드는 회사가 아닌</u> 코어를 설계해서 Microprocessor를 제조하는 회사에 설계내용을 지적재산권 형태로 판매함
- 이를 구입한 회사는 여기에 각종 주변 장치(Peripherals, Memory, I/O..)를 추가하여 완성된 형태의 Microprocessor를 만든다

> **RISC vs CISC**


| RISC(Reduced Instruction Set Computer)  | CISC(Complex Instruction Set Computer) |
| --------------------------------------- | -------------------------------------- |
| 명령어의 길이가 고정되어 있다           | 명령어의 길이가 가변적이다             |
| CPU의 명령어를 최소화하여 단순함        | 복잡하고 기능이 많은 명령어            |
| 전력소모가 적음, 빠른 속도, 저렴한 가격 | 전력소모가 큼, 비싼 가격, 느린 속도    |
| 간단한 HW구조, 임베디드에 주로 사용     | 전력원이 연결된 PC에 주로 사용         |

&nbsp;

> **MPU vs MCU**

MPU ( Micro Processor Unit )

- ALU ( Arithmetic Logic Unit) + CU ( Control Unit ) => One Chip

MCU ( Micro Controller Unit )

- Microprocessor core + System BUS + RAM,ROM + I/O => One Chip
- 외부장치를 바로 연결할 수 있기 때문에 효율적으로 사용가능하다.

&nbsp;

### Cortex Processors

ARMv7 Architecture의 코어를 가지는 프로세서

- A(Application), R(Real Time), M(Microcontroller) series가 있다
- 각 시리즈별로 큰 숫자일 수록 고성능이다

&nbsp;

**Cortex-A series**   (MPU)

- 가장 고성능의 프로세서
- 복잡한 운영환경, 고성능이 요구되는 어플리케이션의 구현에 적합
- 스마트폰, 넷북 같은 기기에 사용
- Architecture : ARMv7-A

&nbsp;

**Cortex-R series**

- Real time 동작이 요구되는 high-end embedded system

- 임베디드 제품에서 필요한 복잡한 알고리즘 제어와 실시간 작업처리가 가능
- Architecture : ARMv7-R

&nbsp;

**Cortex-M series**

- 8bit, 16bit MCU시장을 타겟으로 한 프로세서
- Microcontroller 및 FPGA에 최적화된 프로세서
- 자동차나 전자분야 등 가격에 민감한 임베디드 어플리케이션 분야에 사용
- Architecture : ARMv7-M

&nbsp;

> FPGA (Field Programmable Gate Array) 란?
>
> 개발자가 논리회로를 원하는 의도에 맞춰 동작하게 할 수 있다. 
>
> 개발자가 직접 로직을 설계하고 이 과정에서 사용되는 것이 HDL(Hardware Description Language)이다. 
>
> 대표적으로 Verilog와 VHDL이 있음.

&nbsp;

**Cortex Processor <-> Cortex based MCU**

- Cortex Processor : ARM사에서 설계한 프로세스 코어

- Cortex based Processor : 프로세서 코어를 바탕으로 여러 가지 주변 장치를 추가하여 완성된 MCU

&nbsp;

### ARM Processor에 내장된 기능

Memory Protection Unit ( MPU )

- 동작모드 : Thread mode, Handler mode
- 메모리 접근 레벨 : Privileged level, User level
- 인터럽트 서비스 루틴(ISR) 진입과 탈출의 시간이 빠르다
- 메모리 엑세스에 대한 제한 ( 메모리 접근 레벨 )
- 일반적으로는 운영체제가 MPU를 설정하여 Previleged level에서 사용하는 데이터를 User level의 프로그램에서 접근하지 못하도록 막는 형태로 사용한다

&nbsp;

Nested Vectored Interrupt Controller

- 프로세서에 내장된 인터럽트 제어기
- 모든 Interrupt(Exception)에 대한 우선 순위를 결정하고 이를 처리함
- 인터럽트가 발생하여 처리하고 있는 중에 더 높은 우선 순위의 인터럽트가 발생하면 현재 처리하고 있는 동작을 멈추고 높은 우선 순위의 인터럽트를 처리한다. 우선 순위가 높은 인터럽트가 끝나면 다시 실행을 하게 된다. 
- NVIC는 미리 정해진 우선 순위에 따라 자동적으로 중첩 인터럽트 동작을 수행함
- Vector table : 메모리 내에 ISR의 시작 주소를 저장하고 있음
- Vectored Interrupt : Vector table을 이용해 인터럽트 발생 시 별도의 소프트웨어 필요없이 바로 ISR의 시작 주소를 알 수 있다. 이는 인터럽트 처리 속도가 빨라지게 해준다. 과거에는 SW적으로 처리를 하였으나, 현재는 HW Circuit으로 만들어 더 효율적이 되었음.

&nbsp;

> **Exceptions**

System Exception과 External Interrupt로 나뉜다

- System Exception : 프로세서 내에서 발생 (최대 15개까지 가능)
- External Interrupt : 주변 장치(GPIO, Timer 등)에서 발생 (최대 240개까지 가능, 제조사별로 구성 상이)

