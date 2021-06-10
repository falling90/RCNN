# R-CNN  
 ## 1. R-CNN 정의  
   -. 약자 : Regions with Convolutional Neuron Networks  
   -. 정의 : 영역을 설정하고 CNNS을 활용하여 물체 인식(Object Detection)을 수행하는 신경망  
   
   
>**R-CNN Algorithms**

    1. Segmentation을 통해 초기 Region 생성(유사한 색상으로 매칭)
    2. Selective Search 을 통해 약 2,000 여개의 Region Proposal 진행
    3. 각 Region 別 Crop & Warp 진행(※ CNN 입력을 위해서 고정된 크기로 변환 필요)
    4. 각각 CNN 적용을 통해 Feature Map 생성
    5. 추출된 Feature Map 은 Classifiers(SVM)을 통해 분류한다.
    
    * 학습
        - Input으로 Image와 
        - 세 단계로 진행 (Conv Fine Tune → SVM Classification → BB Regression)

<img src="https://github.com/falling90/Object_Detection/blob/main/Reference/Image/1.R-CNN/1.PNG" width="800px" height="300px"></img><br/>  
----------------------------------------------------------------------------------------------------------------------------------------  
<img src="https://github.com/falling90/Object_Detection/blob/main/Reference/Image/1.R-CNN/2.PNG" width="800px" height="400px"></img><br/>  
----------------------------------------------------------------------------------------------------------------------------------------  
<img src="https://github.com/falling90/Object_Detection/blob/main/Reference/Image/1.R-CNN/3.PNG" width="800px" height="300px"></img><br/>  
----------------------------------------------------------------------------------------------------------------------------------------  
<img src="https://github.com/falling90/Object_Detection/blob/main/Reference/Image/1.R-CNN/4.PNG" width="800px" height="500px"></img><br/>  

>**R-CNN 단점**

    -. Object Detection 속도 자체가 느림.
    -. Selective Search 를 통한 검출된 Region Proposals 마다 CNN을 적용하기때문에 시간 多 소요.
    -. 합성곱 신경망(CNN) 입력을 위해 고정된 크기로 변환(Warp/Crop) 하는 과정에서 Image 정보 손실 발생
    -. 학습이 여러 단계로 이루어져 긴 학습시간과 대용량 저장공간 필요함.
