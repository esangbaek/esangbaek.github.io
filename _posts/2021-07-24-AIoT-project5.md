---
title:  "프로젝트 해보기"
excerpt: "Maixduino활용 프로젝트!"

categories: AIoT
tags:
  - [AIoT, python]
 
toc: true 
toc_sticky: true

date: 2021-07-24
last_modified_at: 2021-08-06
sitemap:


---


> Language : Python ![GitHub Pipenv locked Python version badge](https://img.shields.io/badge/python-v3.9-blue)

## Today I Learned

### Maixduino 로 활용할 수 있는 프로젝트

1. QR Code Scan
2. MNIST Text Classification
3. Face Detection
4. Object Detection



필요한 것 : MaixPy IDE, kflash_gui, 각각에 맞는 model파일, 올바른 버전의 펌웨어파일

* 펌웨어 파일은 주소값 <u>0x000000</u>에 설치할 것	(kflash_gui 이용)
* model파일은 주소값 <u>0x300000</u>에 설치할 것     (kflash_gui 이용)
* model에 따라 호환되는 펌웨어버전이 다르므로 주의할 것

https://maixhub.com

 -> 다양한 프로젝트가 있음 :)





### 사용할 모델 선정하기

앞서 나온 4가지 모델 중 **3번 Face Detection**을 응용 해보려고 한다.



이를 이용해 안면인식 CCTV를 만들어 보았다.

- 현재의 CCTV는 고정된 형태가 대부분이며 특히 사람을 감지할 때 시야각에 들어오는 부분만 촬영이 가능하다. 때문에 이 부분을 개선하기 위해 시야각에 안면이 감지되었을 경우 카메라가 추적하는 기능을 넣어보았다. 카메라에 안면이 인식되지 않는다면 다시 제자리로 돌아오게 하였다. 사람이 한적한 곳에서 사용하기 적합할 것이라 생각한다. 서보모터의 경우 형태는 다르지만 기존의 서보모터와 동일하게 작동한다.
- AI적 요소 : Face Detection
- IoT적 요소 : PWM방식을 이용한 서보모터 제어, 카메라



안면이 감지되었을 때 감지된 안면의 중심 좌표가 가운데로 위치하도록 서보모터의 각도를 조절하게 하였다.  안면이 감지되지 않았을 때는 초기위치로 서서히 돌아오게 하였다.



동작하는 영상 ( 얼굴이 이동하는것에 따라서 카메라도 같이 이동한다! )

![Hnet-image](https://user-images.githubusercontent.com/65602371/128385379-338f34d6-2bce-43a9-9fc1-0da7ecab7d30.gif)



소스코드

```python
# Face_detection_proj.py

import sensor, image, lcd, time
import KPU as kpu
import gc, sys
from machine import Timer, PWM
import utime

def servo_angle(angle): #angle : -90 ~ 90
    duty = ((angle+90)/180*1+2.5)      #duty는 사용하는 서보모터마다 다 다름
    return duty

tim = Timer(Timer.TIMER0, Timer.CHANNEL0, mode=Timer.MODE_PWM)
ch = PWM(tim, freq=50, duty=0, pin=21)  #20ms 주기를 사용하기 위해 freq=50


def lcd_show_except(e):
    import uio
    err_str = uio.StringIO()
    sys.print_exception(e, err_str)
    err_str = err_str.getvalue()
    img = image.Image(size=(224,224))
    img.draw_string(0, 10, err_str, scale=1, color=(0xff,0x00,0x00))
    lcd.display(img)

def main(model_addr=0x300000, lcd_rotation=0, sensor_hmirror=False, sensor_vflip=False):
    try:
        sensor.reset()
    except Exception as e:
        raise Exception("please check hardware connection, or hardware damaged! err: {}".format(e))
    sensor.set_pixformat(sensor.RGB565)
    sensor.set_framesize(sensor.QVGA)
    sensor.set_hmirror(sensor_hmirror)
    sensor.set_vflip(sensor_vflip)
    sensor.run(1)

    lcd.init(type=1)
    #lcd.mirror(True)
    lcd.rotation(lcd_rotation)
    lcd.clear(lcd.WHITE)
    m_angle = 850
    ch.duty(servo_angle(m_angle))
    x=0
    y=0

    anchors = (1.889, 2.5245, 2.9465, 3.94056, 3.99987, 5.3658, 5.155437, 6.92275, 6.718375, 9.01025)
    try:
        task = None
        task = kpu.load(model_addr)
        kpu.init_yolo2(task, 0.5, 0.3, 5, anchors) # threshold:[0,1], nms_value: [0, 1]
        while(True):
            img = sensor.snapshot()
            t = time.ticks_ms()
            objects = kpu.run_yolo2(task, img)
            t = time.ticks_ms() - t
            if objects:
                for obj in objects:
                    img.draw_rectangle(obj.rect())
                    #print(obj.rect()[1]) #obj dictionary is not class  rect()[0] : x, [1] : y
                    x=obj.rect()[0]+obj.rect()[2]//2
                    y=obj.rect()[1]+obj.rect()[3]//2
                    #print("centor: x-"+str(x)+" y-"+str(y)) #obj is not dictionary. It is class  rect()[0] : x, [1] : y
                    if(x<155):
                        m_angle=m_angle-10
                        if(m_angle<20):
                            m_angle = 20
                        ch.duty(servo_angle(m_angle))
                    elif(x>165):
                        m_angle=m_angle+10
                        if(m_angle>1750):
                            m_angle=1750
                        ch.duty(servo_angle(m_angle))
            else:
                if(m_angle>850):
                    m_angle=m_angle-10
                    ch.duty(servo_angle(m_angle))
                else:
                    m_angle=m_angle+10
                    ch.duty(servo_angle(m_angle))
            img.draw_string(0, 200, "centor:%d, %d" %(x,y), scale=2)
            lcd.display(img)
    except Exception as e:
        raise e
    finally:
        if not task is None:
            kpu.deinit(task)


if __name__ == "__main__":
    try:
        main( model_addr=0x300000, lcd_rotation=0, sensor_hmirror=True, sensor_vflip=False)
        # main(model_addr="/sd/m.kmodel")
    except Exception as e:
        sys.print_exception(e)
        lcd_show_except(e)
    finally:
        gc.collect()

```



