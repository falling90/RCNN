# R-CNN  
 ## 0. Key Point
    1) Region proposals 로 Object 위치 탐색  &  CNN을 이용하여 Class 분류
    2) Larger data set으로 학습된 pre-trained CNN을 Fine-tunning
    
 ## 1. R-CNN 이란?  
    -. Regions with Convolutional Neuron Networks 의 약자로
       Region proposals 과 CNN (Convolutional Neural Networks) 을 결합한 모델
       
    ※ 참조
      -. R-CNN 이전에 Sliding window detection에 CNN을 적용한 OverFeat Model 존재
        → 문제점 : Bounding Box 부정확, 연산량 多 (모든 Sliding windows에 CNN을 적용)
   
 ## 2. 구조 및 절차
  2-1) 구조  
<img src="https://github.com/falling90/RCNN/blob/main/Reference/Image/1.png" width="1000px" height="300px"></img><br/>  


  2-2) Process  
<img src="https://github.com/falling90/RCNN/blob/main/Reference/Image/2.png" width="1000px" height="400px"></img><br/>  

    (1) Input Image에 Selective Search 알고리즘을 적용하여 Bounding box (region proposal) 2,000개 추출
    (2) 추출된 Bounding box를 Resize (Crop or Warp) 후 CNN에 입력
    (3) Bounding box의 4,096차원의 Feature 벡터 추출 (Fine tunning 되어 있는 Pre-trained CNN을 사용)
    (4) 추출된 Feature 벡터를 SVM을 이용하여 Class 분류
    (5) Bounding box regression을 적용하여 Bounding box의 위치 조정
    
    

 ## 3. 세부 설명  
  3-0) Input Image  

    -. Data : Image  
    -. Label : Bounding Box  

  3-1) Region Proposal (Selective Search)
    > Object Detection을 위해 가능한 후보 영역을 알아낼 수 있는 방법
<img src="https://github.com/falling90/RCNN/blob/main/Reference/Image/3.png" width="1000px" height="200px"></img><br/>  
<img src="https://github.com/falling90/RCNN/blob/main/Reference/Image/4.png" width="1000px" height="300px"></img><br/>  

    (1) 영역 생성 : Image의 초기 세그먼트를 정하여, 수많은 Region 영역 생성
    (2) 주변과 결합 : Greedy 알고리즘을 이용하여 각 Region을 기준으로 주변의 유사한 영역을 결함
    (3) 영역 제안 : 결합되어 커진 Region을 최족 Region Proposal로 제안
    
    → 논문에서의 이미지에 selective search를 적용하면 2,000개의 Region Proposal 생성
    
  3-2) CNN (Convolutional Neural Networks)

    (0) Input
      -. 추출된 Region proposal 이 Input으로 들어가게 되는데,
         CNN의 Input Size (227x227)로 Warp (resize) 하여 CNN에 입력
      ※ 논문에서는 Warp 과정에서 Object 주변 16 픽셀도 포함하여 성능 향상
      
    (1) AlexNet 형태 사용
      -. Convulutional Layer (5개) + FC Layer (2개)
        → Soft-max Layer 대신에 SVM을 사용하기 때문에 2개의 FC layer 존재
        
    (2) Pre-trained CNN
      -. ILSVRC 2012 Data-set 으로 학습된 Pre-trained CNN 모델 사용
      -. Object detection을 적용할 Data-set으로 Fine-tunning
    
  3-3) SVM Classifier
    > CNN으로 추출한 4,096 차원의 Feature 벡터를 SVM으로 분류
    
  3-4) Bounding Box Regression
    > SVM을 통해 분류된 Bounding box를 Ground-truth box와 비슷하게 조정해주는 역할
    
    ※ 적용 대상
      -. Selective Search로 검출된 2,000개의 Bounding box에 모두 적용하는 것이 아니라,
         Ground-truth box와 IoU(Intersection over Union)가 가장 높은 Bounding box를 선택하여
         Bounding box regression을 적용

<img src="https://github.com/falling90/RCNN/blob/main/Reference/Image/5.png" width="1000px" height="400px"></img><br/>  

 ## 4. 문제점  
    -. Selective Search 속도 자체가 느림.
    -. Selective Search 를 통한 검출된 Region Proposals 마다 CNN을 적용하기때문에 시간 多 소요.
    -. 합성곱 신경망(CNN) 입력을 위해 고정된 크기로 변환(Warp/Crop) 하는 과정에서 Image 정보 손실 발생
    -. 학습이 여러 단계로 이루어져 긴 학습시간과 대용량 저장공간 필요함.
