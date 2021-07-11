# CCTV를 이용한 대상자 추적 Project

<br/>

<image src="https://user-images.githubusercontent.com/78460413/121121882-6eb70780-c85b-11eb-8d83-a39c5641a164.gif" width="700">

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
  - Darknet, PyTorch, Tensor Flow 프레임워크 별 Object Detection 모델 활용
    - 객체 탐지의 정확도에 초점을 두어 학습
    - 탐지할 객체의 데이터 부족이나 시간과 같은 특정 상황을 고려하여 학습 시간과 데이터 수를 제한하여 학습
  - 모델별 특징과 장단점 비교

  ##### 2) 목표
  - 특정 학습 조건 별 성능이 뛰어난 모델을 선정
    - 학습시간 최소화, 학습 데이터 최소화 kv
  - 차후 실종자 탐지, 미아 찾기 같은 분야에 기여 하고자 함 

<br/>

#### 4. 제공 데이터

  ##### 1) Data 구조
<image src="https://user-images.githubusercontent.com/78459545/121801181-fe502200-cc70-11eb-843d-653eca501ba5.png" width="500">

  ##### 1-1) 구조 상세(파일명 기준)
  - Multi view tracking Indoor / Multi view tracking Outdoor
    - 실내/실외 cctv 영상
  - 01-50_market / 01-50_street
    - 1~50번 마켓/길거리 영상
  - Frames
    - 영상 별 정지 이미지 목록
    - 라벨링 데이터로 활용
  - sample_frames
    - 탐지할 타겟 이미지
  - videos
    - 카메라 각도 별 영상 목록
  
<br/>

  #### 5. 사용환경, Tool 및 알고리즘 설명

  ##### 1) 사용환경, Tool
<image src="https://user-images.githubusercontent.com/78459545/121863359-bc89af00-cd36-11eb-8740-3205f0ae7b89.png" width="500">

  - Tool : LabelImg, Yololabel, IINA player
  - 라이브러리 : TensorFlow GPU
    - Google Colab pro 환경 내에 NVIDIA cudnn 설치하여 GPU 가속 활성화
  - 프레임워크 : TensorFlow, Darknet, PyTorch
  - 학습 알고리즘 : TensorFlow Object Detection, YOLOv4, YOLOv5

  ##### 2) 알고리즘 설명
  - TFOD
    - Tensorflow 기반 Object Detection API
    - 다양한 Pre-Trained Model(Model Zoo) 존재
    - 사용자가 직접 Layer를 수정하지 않음(Pre-Trained Model 적용)

  - YOLO v4
    - Darknet 기반 Object Detection API
    - TFOD와 V5 대비 많은 참고 자료 많음
    - 기존 YOLOv3 대비 더 많은 레이어 존재(config file 기준 v3 788 line, v4 1154 line)
    - config 파일을 이용한 데이터 증강과 레이어 수정의 자유도가 높음

  - YOLO v5
    - Darknet 기반이 아닌 pytorch 기반 Object Detection API
    - 레이어 수에 따라 s , m , l , x 로 구분되며 크기에 따라 속도와 정확도의 차이를 보임

<br/>

  #### 6. 전체 과정(요약)

  ##### 1) Labeling
  특정인 라벨링
<image src="https://user-images.githubusercontent.com/78459545/121805419-2eee8680-cc86-11eb-980f-d1c9270b778b.png">
  
  A 영상과 B 영상에서 특정인 학습 후, C 영상에서 탐지
<image src="https://user-images.githubusercontent.com/78459545/121805359-f3ec5300-cc85-11eb-84d0-ae27eba418d8.png">

  ##### 2) 데이터 학습 예시
<image src="https://user-images.githubusercontent.com/78459545/121805939-7c6bf300-cc88-11eb-94ae-7839234fad98.png"> 
  
  - YOLOv4 학습시 구성요소
    - config(.cfg) : 해당 모델의 학습 layer가 포함됨
    - .data : weight 저장경로, classes.txt(label) 경로, 학습 데이터 경로가 포함됨
    - classes.txt : label
    - yolov4.conv.137 : pre-trained model
 
  ##### 3) 학습된 모델 구현 테스트
Target
<image src="https://user-images.githubusercontent.com/78459545/122564111-f5e85480-d07f-11eb-84f3-2c74478edaea.gif">
  - 테스트 모델(TFOD, V4, V5) 전체적으로 과적합/미인식 결과가 확인됨
  
  ##### 4) 정확도 개선 시도
  - TFOD
    - 학습 Epochs 추가
    - 배경 차분(Gaussian Mixture Model) 적용하여 BackGound 삭제 후 테스트
    - Affine(Shear, Scale) 데이터 증강 적용
  
  - YOLOv4
    - 학습 Epochs 추가
    - pre-trained 모델 미적용
    - 이미지 사이즈(width, height) 증강 적용
    - 노출도(exposure), 색조(hue) 증강 적용
  
  - YOLOv5
    - 학습 Epochs 추가
    - 데이터 증강 전처리(saturation, flip, blur, dropout 등)
    - 라벨링 방식 변화(타겟 3등분 라벨링)
    - 데이터 증강 후 학습시간 최소화
 
  #### 7. 프로젝트 최종 결과
  ##### 1) TFOD
  
  - MOG 적용하여 BackGound 삭제 후 Detection → 성능 하락
    - 요인 : 실내 등의 빛 파장 또한 움직임으로 인식됨
  ***************background gif 삽입**********************
  - Affine(Shear, Scale) augment 적용 → 성능 변동 불확실
  ***************affine gif 삽입***********************

  ##### 2) YOLOv4
  
  - 학습 Epochs 추가 → 정확도 증가(과적합 방지, 인식률) / 개선 전후 학습 epochs: 300(10분 학습) / 2000(65분 학습)
<image src="https://user-images.githubusercontent.com/78459545/122723100-822b8f00-d2ad-11eb-8cbd-bc96afcf5f1f.gif" width="400"/><image src="https://user-images.githubusercontent.com/78459545/122723291-c28b0d00-d2ad-11eb-8f83-dad02d508a38.gif" width="400"/>
  
  - 이미지 사이즈(width, height) 증강 적용 → 성능 변동 없음
    - 요인 : 저화질 영상인 관계로 자세한 학습 불가(학습 시간만 증가)
  
  - 노출도(exposure), 색조(hue) 증강 적용 → 정확도 증가(요인: 실외 이미지도 라벨링 처리한 점을 고려하여 노출도 조절)
<image src="https://user-images.githubusercontent.com/78459545/122715476-e184a180-d2a3-11eb-8479-cb54c0576370.gif" width="400"/><image src="https://user-images.githubusercontent.com/78459545/122715637-1ee92f00-d2a4-11eb-8aff-6486d4790cfe.gif" width="400"/>
  
  - pre-trained 모델 미적용 → 정확도 감소 / 적용 모델 학습 epochs: 2000(67분) 미적용 모델 학습 epochs: 5000(140분) 
<image src="https://user-images.githubusercontent.com/78459545/122727593-28799380-d2b2-11eb-9de5-9c5e17c0b6f7.gif" width="400"/><image src="https://user-images.githubusercontent.com/78459545/122727441-fd8f3f80-d2b1-11eb-9b70-f8cf1694bcdd.gif" width="400"/>
  
  ##### 3) YOLOv5
  
  - 데이터 증강 후 학습시간 최소화 : 원본 데이터, epochs 1000/76분학습  //  증강 데이터 활용, epochs 40/19분학습
<image src="https://user-images.githubusercontent.com/78460413/125187468-c7b6f880-e26a-11eb-8aab-da51b6103bb1.gif" width="400"/><image src="https://user-images.githubusercontent.com/78460413/125187437-99d1b400-e26a-11eb-8f6e-8cd6cd13907e.gif" width="430"/>


  
  - 라벨링 방식 변화(타겟 3등분 라벨링) → 인식 빈도 향상
<image src="https://user-images.githubusercontent.com/78460413/125188320-76106d00-e26e-11eb-86cc-382764955005.gif" width="400"/><image src="https://user-images.githubusercontent.com/78460413/125188457-d0113280-e26e-11eb-9a56-7e36722c4b88.gif" width="400"/>


  
  ##### 4) 모델 비교(학습 최소화)

  - 원본 데이터 1/2, 학습시간 10분 적용(좌 : YOLOv4 / 우 : YOLOv5)

<image src="https://user-images.githubusercontent.com/78459545/122730706-7cd24280-d2b5-11eb-9ada-971c875f6ed2.gif" width="400"/><image src="https://user-images.githubusercontent.com/78459545/122730573-57ddcf80-d2b5-11eb-98c8-bac2699607d0.gif" width="400"/>
  - 차이점
    - YOLOv5의 경우 YOLOv4 대비 단시간 학습(10분 이하)는 타겟 탐지가 어려움
    - YOLOv4는 색 증강에 효과를 봤지만, YOLOv5는 색 관련 증강으로 성능 증가가 어려움
    - 충분한 학습 시간이 주어지는 경우 IoU 수치와 육안상으로 봤을 때 YOLOv5의 정확도가 가장 우수함
