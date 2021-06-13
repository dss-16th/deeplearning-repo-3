# CCTV를 이용한 대상자 추적 Project

<br/>

![2021-06-08 12 57 27](https://user-images.githubusercontent.com/78460413/121121882-6eb70780-c85b-11eb-8d83-a39c5641a164.gif)

### 1. 진행 기간 : 2021. 5. 4. ~ 2021. 6. 3.

### *2. 참여자*
- [고원진](https://github.com/wonjin77)
- [이지홍](https://github.com/jihong7107)
- [이경무](https://github.com/rudan916)


<br/>


### 3. 진행 방향 및 목표

  ##### 1) 진행 방향
  - 제공 받은 CCTV 영상 데이터를 활용
  - 모델 학습에 사용될 데이터만 라벨링
  - Darknet, Pytorch, Tensor Flow 프레임워크 별 Object Detection 모델 활용
    - 객체 탐지의 정확도에 초점을 두어 학습
    - 탐지할 객체의 데이터 부족이나 시간과 같은 특정 상황을 고려하여 학습 시간과 데이터 수를 제한하여 학습
  - 모델별 특징과 장단점 비교

  ##### 2) 목표
  - 특정 학습 조건 별 성능이 뛰어난 모델을 선정
    - 학습시간 최소화, 학습 데이터 최소화 
  - 차후 실종자 탐지, 미아 찾기 같은 분야에 기여 하고자 함 


<br/>


#### 3-1. Proto Labeling & Learning

  ##### 1) Labeling
  - labelimg를 이용한 labeling
  
  ##### 2) Proto Data 학습 진행
  - YOLOv4, YOLOv5, TensorFlow Object Detection API 를 이용하여 proto data 학습 진행

  ##### 3) Proto Data 재학습
  - labeled image data 감소와 모델 성능 비교하여 최적 & 최소한의 data수 확인


#### 3-2. 전체 data 적용

  ##### 1) indoor / outdoor cctv data 최소 Labeling 진행

  ##### 2) Data 학습 진행

  ##### 3) 성능 확인


#### 3-3. webcam을 이용한 실시간 추적 모델 만들기

  ##### 1) 특정 대상 학습

  ##### 2) 모델 적용 및 평가
  
