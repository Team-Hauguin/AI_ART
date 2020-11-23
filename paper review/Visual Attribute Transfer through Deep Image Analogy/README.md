# Visual Attribute Transfer Through Deep Image Analogy
해당 논문은 Deep CNN을 이용한 Image analogy를 통해 이미지를 다른 style로 바꿔주게 된다.
다른 transfer와 다른점은 두 개의 input중 한 image에서는 구조적인 유사함을, 다른 image에서는 유사한 style을 갖게 된다. 

# Introduction 
### 1) Visual Attribute Transfer란?
하나의 이미지를 다른 color, tone, texture의 style로 바꿔주는걸 말한다. 
예를들어, 같은 scene을 묘사한경우 그림이나 스케치를 사진으로 변경하거나 반대 경우 모두를 포함한다

# Motivation
### 1) Analogy with Bidirectional Constraints
![KakaoTalk_20201123_234308040](https://user-images.githubusercontent.com/74186580/99975857-10a66380-2de6-11eb-9103-c270be37ae99.jpg)
유사한 appearance를 갖고 있거나, 구조적으로 동일한 위치에 있는 양방향의 제약조건을 가진 analogy를 확인할 수 있다. 

### 2) Reconstruction using CNN
![KakaoTalk_20201123_234412842](https://user-images.githubusercontent.com/74186580/99976039-4b100080-2de6-11eb-9120-eea55d8b93ca.jpg)
Coarsest layer에서는(image 상에서 relu5_1)에서 A와 B'은 구조적으로 매우 유사하게 된다. 
이에 따라 해당 Layer에서는 A'=A라고 가정을 하게 된다.
그 이후 Layer에서는 A에서 conyeny structure를 B'에서 detail information을 이전 layer에서 갖고 와서 다음 layer에서 이용 하게 된다. 

### 3) Deep Patchmatch


# Deep Image Analogy

### 1) System Pipeline

![KakaoTalk_20201123_042142406](https://user-images.githubusercontent.com/74186580/99914842-7988cf80-2d43-11eb-96f8-482658a4a314.jpg)

Deep Image Analogy는 image analogy와 deep CNN feature를 이용한 것으로 전체 system pipeline은 그림과 같다. input image A와 B'을 Preprocessing 과정을 통해 layer를 나누게 되고 각 layer에서 3~5 순서대로 진행되게 된다. 

### 2) Preprocessing
Input 이미지는 A와 B'으로 imagenet으로 pretrained 된 VGG19를 통과하여 Feature map을 얻게된다. 
첫번째로 layer 5에서 A'(5), A(5) 그리고 B(5), B'(5)는 구조적으로 유사함을 이용할 예정이므로 동일하다고 보고 Layer 4의 A'(4), B(4)를 생성하게 된다. 
> A'(5) = A(5) , B(5) = B'(5)

### 3) Nearest-Neighbor Field Search
![KakaoTalk_20201124_000020570](https://user-images.githubusercontent.com/74186580/99977794-7398fa00-2de8-11eb-9452-63863487d9ef.jpg)


### 4) Latent Image Recontruction 
![KakaoTalk_20201124_000122613](https://user-images.githubusercontent.com/74186580/99977635-3f253e00-2de8-11eb-9ed3-5b95312b9936.jpg)

