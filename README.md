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
  - Darknet, PyTorch, Tensor Flow 프레임워크 별 Object Detection 모델 활용
    - 객체 탐지의 정확도에 초점을 두어 학습
    - 탐지할 객체의 데이터 부족이나 시간과 같은 특정 상황을 고려하여 학습 시간과 데이터 수를 제한하여 학습
  - 모델별 특징과 장단점 비교

  ##### 2) 목표
  - 특정 학습 조건 별 성능이 뛰어난 모델을 선정
    - 학습시간 최소화, 학습 데이터 최소화 
  - 차후 실종자 탐지, 미아 찾기 같은 분야에 기여 하고자 함 

<br/>


#### 4. 제공 데이터

  ##### 1) Data 구조
![image](https://user-images.githubusercontent.com/78459545/121801181-fe502200-cc70-11eb-843d-653eca501ba5.png)
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
  
#### 5. 사용환경, Tool 및 알고리즘 설명
![image](https://user-images.githubusercontent.com/78459545/121863359-bc89af00-cd36-11eb-8740-3205f0ae7b89.png)
  - Google Colab pro 환경 내에 NVIDIA cudnn 설치하여 GPU 활성화
  - Darknet, PyTorch, Tensor Flow 프레임워크 별 YOLOv4, YOLOv5, TensorFlow Object Detection API 를 이용하여 proto data 학습 진행

  ##### 1) 사용환경, Tool
  - Tool : LabelImg, Yololabel, IINA player
  - 라이브러리 : TensorFlow GPU(install NVIDIA cudnn)
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
  
  #### 6. 전체 과정(요약)

  ##### 1) Labeling
  특정인 라벨링
![image](https://user-images.githubusercontent.com/78459545/121805419-2eee8680-cc86-11eb-980f-d1c9270b778b.png)
  
  A 영상과 B 영상에서 특정인 학습 후, C 영상에서 탐지
  ![image](https://user-images.githubusercontent.com/78459545/121805359-f3ec5300-cc85-11eb-84d0-ae27eba418d8.png)

  ##### 2) 데이터 학습 예시
  ![image](https://user-images.githubusercontent.com/78459545/121805939-7c6bf300-cc88-11eb-94ae-7839234fad98.png)
  - YOLOv4 학습시 구성요소
    - config(.cfg) : 해당 모델의 학습 layer가 포함됨
    - .data : weight 저장경로, classes.txt(label) 경로, 학습 데이터 경로가 포함됨
    - classes.txt : label
    - yolov4.conv.137 : pre-trained model
 
  ##### 3) 학습된 모델 구현 테스트
  과적합/미인식 영상 삽입
  - 테스트 모델(TFOD, V4, V5) 전체적으로 과적합/미인식 결과가 확인되어 개선 시도
  
  ##### 4) 정확도 개선 시도
  - TFOD
    - 학습 Epochs 추가
    - 성능이 좋으나 무거운 모델 적용 후 테스트 영상 재생시간 급격히 상승(2시간)
    - MOG 적용하여 BackGound 삭제 후 테스트 시 오히려 성능 하락 발생
    - Affine(Shear, Scale) 데이터 증강 적용 후 성능 변동 불확실
  
  - YOLOv4
    - 학습 Epochs 추가
    - 이미지 사이즈(width, height) 상승으로 더 자세한 학습 시도 후 테스트시 제공 받은 데이터 화질이 떨어져 결과 차이 없음
    - 라벨링된 데이터와 테스트 영상의 환경이 실내/실외인 점을 고려하여 노출도, 색조 증강 후 성능 상승
  
  - YOLOv4
    - 학습 Epochs 추가
    - 데이터 증강 전처리
 
  #### 7. 데이터 증강(Data Augmentation) 및 모델 비교
  ##### 1) TFOD
  - MOG 적용하여 BackGound 삭제 후 Detection → 오히려 성능 하락
    - background gif 삽입 (ppt 17 page)
  - Affine(Shear, Scale) augment 적용 → 오히려 성능 변동 불확실
    - affine gif 삽입 (ppt 19,20 page)
  - 
  ##### 2) YOLOv4
  
  ##### 3) YOLOv5
  
  ##### 4) 모델 비교(학습 최소화)
