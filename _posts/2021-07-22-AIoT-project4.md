---
title:  "딥러닝, 기계학습, tensorflow"
excerpt: "개념 이해와 tensorflow로 실습해보기"

categories: AIoT
tags:
  - [AIoT, tensorflow, python]
 
toc: true 
toc_sticky: true

date: 2021-07-22
last_modified_at: 2021-07-22
sitemap:


---


> Language : Python ![GitHub Pipenv locked Python version badge](https://img.shields.io/badge/python-v3.9-blue)

## Today I Learned

### 딥러닝, 기계학습, 인공지능에 대해서

![image](https://user-images.githubusercontent.com/65602371/126641451-f62f5f88-059e-4349-9554-7930de1efa48.png)

딥러닝으로 할 수 있는 것들

- 분류 (Classification)
- 회귀 (Regression)
- 물체 인식 (Object Detection)

- 영상 분할 (Image Segmentation)
- 강화 학습 (Reinforcement Learning)



딥러닝의 기본 구조 (퍼셉트론: Perceptron)는 인간의 신경세포 (뉴런)를 모방하여 만들었다.

![image](https://user-images.githubusercontent.com/65602371/126650766-2e56f77e-c378-4ac1-99eb-b9d01dc1f94b.png)

​			b: bias, w1,w2 : weight

입력값 x1, x2와 가중치 (w1, w2)를 곱한 값을 더하고 활성함수를 거쳐 출력값 y를 만들어 낸다. (여러신호를 하나의 신호로 만들어준다)



### 모델의 학습과 최적화

- 지도학습 : 입력과 함께 정답을 알려주고 그 정답을 맞추도록 하는 학습 방법
- 비지도학습 : 정답 제공없이 데이터로부터 정보를 추출하는 학습 방법 (ex 분류)
- 손실 함수 : 알고리즘이 얼마나 학습을 잘했는지 표현하는 지표, 값이 낮을수록 학습이 잘 된 것이라고 볼 수 있다. 

> 손실 계산 함수

![image](https://user-images.githubusercontent.com/65602371/126665093-a3b1e065-88f5-4f6a-87c7-8355a7571f4a.png)

> 경사 하강법	(미분을 통해 기울기가 0에 가깝도록 하는 값을 찾는다)

![image](https://user-images.githubusercontent.com/65602371/126665311-4d4bf7e5-d527-42ec-9a9b-45e65ceacebb.png)

경사하강법을 사용할 때 x값을 얼마나 이동시키면서 최적화할지 결정하는 값을 학습률(Learning Rate)라고 한다.



### MNIST데이터를 이용해 기계학습하기 with CNN 

> CNN (Convolutional Neural Network)

```python
CNN_with_MNIST.py

import tensorflow as tf
import numpy as np
import matplotlib.pyplot as plt

# data preparation
(x_train, y_train), (x_test, y_test) = tf.keras.datasets.mnist.load_data()	

# 데이터의 각 값들은 0~255로 이루어져 있다. 하지만 이 수는 계산하다보면 너무 커지므로 작게 해줄필요가 있음
x_train, x_test = x_train/255.0, x_test/255.0
y_train, y_test = tf.keras.utils.to_categorical(y_train), tf.keras.utils.to_categorical(y_test)	# x_train,x_test에 한 것과 동일한 작업

# build model
model = tf.keras.models.Sequential([
    tf.keras.layers.Reshape((28,28,1), input_shape=(28,28)),
    tf.keras.layers.Conv2D(16, kernel_size=(3,3), padding='VALID', activation='relu'),
    tf.keras.layers.Conv2D(32, kernel_size=(3,3), padding='VALID', activation='relu'),
    tf.keras.layers.MaxPool2D((2,2)),  # 12*12*32
    tf.keras.layers.Conv2D(64, kernel_size=(3,3), padding='VALID', activation='relu'),
    tf.keras.layers.Flatten(),	#2D이미지를 1차원 벡터로 변환하는 과정
    tf.keras.layers.Dense(1024, activation='relu'),
    tf.keras.layers.Dense(10, activation='softmax')	#10개의 feature로 표현
])

model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])

model.fit(x_train, y_train, batch=64, epochs=10, validation_data=(x_test, y_test))

np.argmax(x_test[10])	#훈련된 모델로 x_test[10]값을 예측해보기
np.argmax(y_test[10])	#예측한 결과와 비교해보기

```



### 이미지 파일 분석해보기

이미지를 이용해 CNN모델로 학습하던 중 한가지 의문이 들었다.

![image](https://user-images.githubusercontent.com/65602371/126635849-90a54f5a-6b6e-4b00-b192-48be2e7b7e93.png)

blue_guy.shape = (144, 144, 4)

내가 보는 사진은 2차원 사진인데 shape를 출력해보면 3차원으로 이루어져 있는 것을 알 수 있다.

사실 색상 정보를 담고있는 여러개의 층이 하나로 합쳐지면서 사진이 만들어지는 것이다. (셀로판지를 겹치는 것 처럼!)

그렇다면 이 사진을 R,G,B로 분해하여 각 층이 어떻게 생겼는지 알아보자.

```python
import matplotlib.pyplot as plt
import numpy as np
import cv2

blue_guy = cv2.imread("blueguy.png")
(B,G,R) = cv2.split(blue_guy)	# cv2는 B,G,R의 순서이다.
cv2.imshow("RED",R)		#RED층만 분리	사진1
cv2.imshow("GREEN",G)	#GREEN층만 분리	사진2
cv2.imshow("BLUE",B)	#BLUE층만 분리	사진3
cv2.waitKey(0)

merged = cv2.merge([B,G,R])	#나누어진 3개의 사진을 다시 하나로 합침 (처음 원본과 동일해짐)

cv2.imshow("Merged", merged)	#사진4
cv2.waitKey(0)

zeros = np.zeros(blue_guy.shape[:2}, dtype="uint8"])
cv2.imshow("Only Red", cv2.merge([zeros,zeros,R]))	#빨간색만 분리해냄	사진5
cv2.imshow("Only Green", cv2.merge([zeros,G,zeros]))	# 초록색만 분리해냄	사진6
cv2.imshow("Only Blue", cv2.merge([B,zeros,zeros]))	#파란색만 분리해냄	사진7
cv2.waitKey(0)

```

사진1

![image](https://user-images.githubusercontent.com/65602371/126635998-f810bbda-fe53-4054-951b-7419d09e013d.png)

사진2

![image](https://user-images.githubusercontent.com/65602371/126635969-e0fa0d67-b0b8-43df-892a-3e6ffe6ad9f2.png)

사진3

![image](https://user-images.githubusercontent.com/65602371/126635915-a875413c-eaf1-4221-b484-b8c859b6ea0e.png)

?? RGB로 분리했는데 왜 회색이지??

흑백이 single-channel이기 때문이다. 이미지의 3개 채널을 single-channel 3개로 바꾸어서 이러한 현상이 발생하는 것이다.

우선 이렇게 나누어진 사진을 하나로 합쳐보자.

사진4

![image](https://user-images.githubusercontent.com/65602371/126636043-b9d2c84d-b43c-437b-840e-edff99b8290d.png)

다시 원본으로 돌아왔다!

이번에는 아까 분리했었던 이미지들을 컬러이미지로 변환해보자.

사진5

![image](https://user-images.githubusercontent.com/65602371/126636143-47fc085d-1450-4699-a97c-9fa094674267.png)

사진6

![image](https://user-images.githubusercontent.com/65602371/126636118-4fc376c1-2f23-4ea2-a912-1429a335edb4.png)

사진7

![image](https://user-images.githubusercontent.com/65602371/126636093-bb1aa733-24be-487a-a2a4-1e57c827ab76.png)



**성공!!** (모자가 사라진건 아쉽다 :(  )



#### 참고자료

http://blog.comart.io/posts/opencv-rgb-1

https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=rhrkdfus&logNo=221378369642

