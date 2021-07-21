---
title:  "Maixduino를 이용해 LED,스위치 제어하기"
excerpt: "Maixduino개발환경 설정 및 개발도구 사용하기"

categories: AIoT
tags:
  - [AIoT, tensorflow, python]
 
toc: true 
toc_sticky: true

date: 2021-07-20
last_modified_at: 2021-07-22
sitemap:


---


> Language : Python ![GitHub Pipenv locked Python version badge](https://img.shields.io/badge/python-v3.9-blue)

## Today I Learned

### Micro Processor vs Micro Controller



| 마이크로 프로세서                                            | 마이크로 컨트롤러                                            |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 컴퓨터의 중앙처리장치를 하나의 칩으로<br />구현한 반도체 소자 | 메인보드 + Hard disk (Single Chip Computer)                  |
|                                                              | 작고 가벼운 제어장치 구성을 위해 사용되는<br />특화된 마이크로 프로세서의 일종 |

- Micro Controller 사용 예

| 분야   | 사용 예                       |
| ------ | ----------------------------- |
| 의료   | 의료기 제어, 자동 심박계      |
| 교통   | 신호등 제어, 주차장 관리      |
| 감시   | 출입자/침입자 감시, 산불 감시 |
| 가전   | 에어컨, 세탁기, 전자레인지    |
| 음향   | CD플레이어, 전자 타이머       |
| 사무   | 복사기, 무선 전화기           |
| 자동차 | 엔진 제어, 충돌 회피          |
| 기타   | 게임기, 등등..                |



### Maixduino



![image](https://user-images.githubusercontent.com/65602371/126338995-3d1a9106-a593-443f-ac85-992adc7def87.png)

![image](https://user-images.githubusercontent.com/65602371/126339067-dd9f0008-0591-413f-8739-258a81ebd41a.png)

> Maixduino 상세정보 (아두이노와 똑같은 핀 번호를 사용. 하지만 고유한 번호 또한 존재.)



### How to Setup

- MaixPy IDE
  - http://dl.sipeed.com/shareURL/MAIX/MaixPy/ide/v0.2.5
- ftdi_vcp_driver 
  - https://dl.sipeed.com/MAIX/tools/ftdi_vcp_driver
- K-Flash
  - https://github.com/sipeed/kflash_gui/releases/tag/v1.6.7



### LED (출력)

- 간단한 LED 제어 코드

```python
# led_control.py

from fpioa_manager import fm
from Maix import GPIO
import utime

io_led_red = 15
fm.register(io_led_red, fm.fpioa.GPIO0)

led_red = GPIO(GPIO.GPIO0, GPIO.OUT)
# LED 출력 : GPIO.OUT, 입력 : GPIO.IN
while(True):
    led_red.value(1)	# LED ON
    utime.sleep_ms(1000)
    led_red.value(0)	# LED OFF
    utime.sleep_ms(1000)
```

사용하고자 하는 핀번호와 GPIO번호를 fpioa_manager에 등록(register)한다. 이때 핀 번호는 K210의 번호를 사용해야함.

입력한 GPIO 번호와 입출력 타입을 정해서 변수에 할당.

.value(1)	작동!	

.value(0)	작동 중지



### Switch (입력)

- Switch를 통해 LED를 제어하는 코드

```python
# switch.py

from fpioa_manager import fm
from Maix import GPIO
import utime

io_btn = 22
fm.register(io_btn, fm.fpioa.GPIO1)

btn = GPIO(GPIO.GPIO1, GPIO.IN)

while(True):
    print(btn.value())
```

버튼의 상태를 출력할 때 눌러지지 않았는데도 1,0이 반복되어서 나타나거나 1이 나오는 경우가 있다. 이는 플로팅 상태로 인해 발생하는 문제이다. 

> 플로팅(Floating)
>
> - 스위치를 누르지 않았지만 누르는 것과 같은 효과가 발생함. 즉 전류를 흘려보내지 않았는데 전류가 흐름.
> - 이는 장치의 오작동을 발생시키는 원인 중 하나이다. 따라서 이를 해결하기 위해서는 입력핀의 전압을 고정해야 한다. 이 때 사용하는 것이 풀업(Pull Up)저항과 풀다운(Pull Down)저항이다.

따라서 눌러지지 않았을 때는 0으로 해줘야 하기 때문에 풀다운저항을 사용해야한다.

```python
# switch_pulldown.py

from fpioa_manager import fm
from Maix import GPIO
import utime

io_btn = 22								# 사용할 버튼의 번호
fm.register(io_btn, fm.fpioa.GPIO1)		# 버튼 핀 번호 등록

btn = GPIO(GPIO.GPIO1, GPIO.IN, GPIO.PULL_DOWN)		# GPIO.PULL_DOWN 옵션 추가

while(True):
    print(btn.value())					# push : 1 출력	not push : 0 출력
```

- 발생할 수 있는 문제
  - 분명 버튼을 누르고 있는 상태인데도 아주 짧은 시간동안 버튼을 눌렀다가 떼는 현상이 발생할 수 있다.

하드웨어적인 문제로 스위치의 <u>상태가 변하는 순간 열림과 닫힘이 많게는 수십번 반복되는 현상</u>을 채터링(Chattering) 이라고 한다.

이를 방지하기 위해 sleep 함수를 사용할 수도 있다.



### 기타

한번 누를때마다 변수값을 바꾸고 싶을 때 (누르는 시간에 관계없이 한번 눌렀다 뗐을때)

```python
current_state = btn.value()
if current_state == 1:
    if prev_state == 0:
        # 변화시키고자 하는 변수 연산식
        prev_state = 1
else:
    prev_state = 0
```

눌렀을 때 current_state값이 1이 되면서 prev_state값도 같이 바뀌고 변수값을 한번 바꾸고 나면 prev_state가 포함된 if문에 해당하지 않기 때문에 더이상 변수값을 변화시키지 않는다.
