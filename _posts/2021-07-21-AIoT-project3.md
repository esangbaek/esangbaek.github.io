---
title:  "Maixduino를 이용해 아날로그 입출력 및 카메라 사용하기"
excerpt: "Maixduino 아날로그 입출력의 특징과 카메라,LCD"

categories: AIoT
tags:
  - [AIoT, tensorflow, python]
 
toc: true 
toc_sticky: true

date: 2021-07-21
last_modified_at: 2021-07-21
sitemap:


---


> Language : Python ![GitHub Pipenv locked Python version badge](https://img.shields.io/badge/python-v3.9-blue)

## Today I Learned

### 아날로그 입력

아날로그 신호는  연속된 값을 가지므로 0 또는 1만을 값으로 가지는 디지털 신호와 차이가 있다.

따라서 이를 디지털 신호로 처리하기 위해서는 아날로그-디지털 변환기(ADC)가 필요함. 이때 ADC는 전류의 값을 읽는것이 아닌 전압을 읽는다. 따라서 전압이 변해야 센서가 인식을 할 수가 있다.

Maixduino에는 6 channel ,12bit 해상도 ADC가 있음. A0~A5까지 6개의 핀이 할당되어있음. (단. 하나의 ADC가 있으므로 동시에 여러 채널 사용은 불가)

> 12bit 해상도의 의미 
>
> 2^12 = 4096,	3.3V의 전압을 사용할 경우 3.3/4096 = 약 0.8mV의 전압차이를 인식할 수 있다

하지만 Maixduino는 ADC가 별도의 칩에 들어있으므로 통신하는 과정이 별도로 필요하다.

```python
# 가변저항 출력 테스트
import time, network
from Maix import GPIO
from fpioa_manager import fm

class wifi():
    # IO map for ESP32 on Maixduino
    fm.register(25, fm.fpioa.GPIOHS10)	#cs
    fm.register(8, fm.fpioa.GPIOHS11)	#rst
    fm.register(9, fm.fpioa.GPIOHS12)	#rdy
    # USE Hardware SPI for other maixduino
    fm.register(28, fm.fpioa.SPI1_D0, force=True)	#mosi
    fm.register(26, fm.fpioa.SPI1_D1, force=True)	#miso
    fm.register(27, fm.fpioa.SPI1_SCLK, force=True)	#sclk
    nic = network.ESP32_SPI(cd=fm.fpioa.GPIOHS10, rst=fm.fpioa.GPIOHS11, rdy=fm.fpioa.GPIOHS12, spi=1)
    
while True:
    try:
        adc=wifi.nic.adc((0,))
    except Exception as e:
        print(e)
        continue
    for v in adc:
        print("ADC0 Value : %04d" %(v))
```



### 아날로그 출력

디지털 장치에서 아날로그 형태의 데이터 출력은 불가능하다!

하지만 이와 유사한 효과를 얻기 위해 펄스 폭 변조(**P**ulse **W**idth **M**odulation) 를 이용한다. 

- PWM신호는 디지털 신호의 일종이다.
- PWM신호 출력 기능이 아날로그 신호와 비슷하여 아날로그 출력으로 사용함.

PWM신호출력 예시

![image](https://user-images.githubusercontent.com/65602371/126500212-65bf9cbf-2aa4-41d1-8663-5b13706ce40d.png)

Duty Cycle이 커질수록 LED가 밝아진다고 볼 수 있다.



> PWM을 활용해 LED의 밝기를 바꿔보자

```python
# Breathing_LED.py
from machine import Timer, PWM
import time
import random

tim = Timer(Timer.TIMER0, Timer.CHANNEL0, mode=Timer.MODE_PWM)
ch1 = PWM(tim, freq=500000, duty=50, pin=21)
duty=0
dir=True
while True:
    if dir:
        duty += 2	#python에는 ++연산을 할 수 없다!!!
    else:
        duty -= 2
    if duty>100:
        duty = 100
        dir = False
    elif duty<0:
        duty = 0
        dir = True
    time.sleep_ms(50)
    ch1.duty(duty)
```



> 이번에는 PWM을 이용해 모터를 제어해 보자

```python
# control_moter.py

from machine import Timer, PWM
import utime

def servo_angle(angle): #angle : -90 ~ 90
    duty = ((angle+90)/180*10+2.5)	#2.5 ~ 12.5  *duty는 사용하는 서보모터마다 다 다름
    return duty

tim = Timer(Timer.TIMER0, Timer.CHANNEL0, mode=Timer.MODE_PWM)
ch = PWM(tim, freq=50, duty=0, pin=21)  #20ms 주기를 사용하기 위해 freq=50

while True:
    ch.duty(servo_angle(90))	#한쪽방향 제일 끝까지 회전
    utime.sleep_ms(1000)
    ch.duty(servo_angle(-90))	#반대쪽방향 제일 끝까지 회전
    utime.sleep_ms(1000)

```



> 똑같은 방법으로 이번에는 부저를 이용해 멜로디를 출력해보자

```python
# buzer_control.py
from fpioa_manager import fm
from machine import Timer, PWM
import utime
from Maix import GPIO

io_btn = 22
tim = Timer(Timer.TIMER0, Timer.CHANNEL0, mode=Timer.MODE_PWM)
ch = PWM(tim, freq=261, duty=50, pin=23)
fm.register(io_btn, fm.fpioa.GPIO0)
btn = GPIO(GPIO.GPIO0, GPIO.IN, GPIO.PULL_UP)

melody = [261,293,329,349,391,440,493,523]   # 도레미파솔라시도
while True:
    if btn.value() == 0:
        ch.duty(50)
        for i in melody:
            ch.freq(i)
            utime.sleep_ms(130)
        utime.sleep_ms(150)
        ch.duty(0)
        utime.sleep_ms(150)
        ch.duty(50)
        ch.freq(783)
        utime.sleep_ms(200)

    else:
        ch.duty(0)

```



### LCD 와 카메라 사용해보기

- LCD : QVGA (320 x 240), RGB565

> LCD에 원, 사각형, 선, 문자 출력해보기

```python
# use_lcd.py
import lcd,image,utime

lcd.init()
img = image.Image()
img.draw_circle((100,100,100),color=(0,255,0),fill=True)
lcd.display(img)
# (100,100)을 중심으로 반지름이 100인 원(채워진 초록색)
utime.sleep(1)
img.draw_rectangle((100,100,200,70),color=(255,0,0),fill=True)
lcd.display(img)
# (100,100)을 좌상단의 꼭짓점으로 하는 가로 200, 세로 70의 사각형(채워진 빨강색)
utime.sleep(1)
img.draw_line((0,0,lcd.width(),lcd.height()),thickness=10,color=(0,0,255))
lcd.display(img)
# (0,0)부터 (lcd폭,lcd높이)까지 잇는 선 굵기는 10 (파란색)
utime.sleep(1)
img.draw_string(100,100,"hello maixpy!",scale=2,color=(0,255,0))
lcd.display(img)
# (100,100)을 문자열의 제일 좌상단으로 하고 scale=2 크기의 문자 출력 (초록색)
utime.sleep(1)
lcd.clear()
```



- LCD와 카메라 응용

Blob (**B**inary **L**arge **Ob**ject)이란? 

이진 이미지의 연결된 픽셀 그룹을 말한다. 다음의 사진은 Blob만을 추출해낸 모습이다.

> MaixPy IDE -> Tools -> Machine Vision -> Threshold Editor

![image](https://user-images.githubusercontent.com/65602371/126512383-3f7277c0-d57e-41b0-8680-c1753616b274.png)

빨간 LED의 빨간색 부분을 Blob으로 추출해낸 모습이다.

아래 사진은 카메라에서 Blob부분을 인식하는 모습

![image](https://user-images.githubusercontent.com/65602371/126512903-1c35106c-24cd-402e-a70b-977a2b3efe03.png)



```python
import sensor, image, lcd, time

lcd.init()
sensor.reset()
sensor.set_pixformat(sensor.RGB565)
sensor.set_framesize(sensor.QVGA)
sensor.run(1)

blob_threshold = (63, 0, 127, -128, -25, 127)
while True:
    img = sensor.snapshot()
    blobs = img.find_blobs([blob_threshold])
    if blobs:									# blobs이 인식되었을때만
        for b in blobs:
            tmp=img.draw_rectangle(b[0:4])
            tmp=img.draw_cross(b[5],b[6])
            lcd.display(img)
```



여기서 blob_threshold = (63, 0, 127, -128, -25, 127) 값은 무엇일까?

* LAB Color Space 

![image](https://user-images.githubusercontent.com/65602371/126515346-c44337b0-e1f1-480f-b34d-1fe9f96b9622.png)

국제 조명 위원회 (CIE)에서 규정한 색상값으로 사람눈이 감지할 수 있는 색차와 색공간에서 수치로 표현한 색차를 거의 일치시킬 수 있는 색공간이다. 

이전에는 RGB라는 색공간으로 표현했지만 이는 인간이 느끼는 두색간의 색차와 계산된 수치로 나타내는 색차가 색상에 따라서 많은 차이를 보이는 반면 LAB은 균일한 색공간 좌표로서 눈과 매우 근소한 차이를 보여준다.

그림에서 볼 수 있다시피, L은 검은색과 하얀색의 정도(명도), A는 Red와 Green의 정도, B는 Yellow와 Blue의 정도를 나타낸다.

다시 blob_threshold = (63, 0, 127, -128, -25, 127) 를 살펴보면, (L min, L max, A min, A max, B min, B max) 값을 나타낸 것이다. (Blob을 추출할 때 하단에 좌표처럼 보이는것이 LAB 값이다.)

