### Nueral Head Reenactment With Latent Pose Descriptor

본 논문은 Few-shot Learning 기법을 사용하여 GAN 기반의 Head Reenactment를 구현하는 방법을 제안한다.

논문을 이해하기 위해 필요한 지식은 Meta-Learning, Content Loss, Adversarial Loss, Dice Coefficient Loss, Landmark 정도가 있다.

# 참고 내용
## 1) Meta Learning 
<img src="https://github.com/Team-Hauguin/AI_ART/blob/main/paper%20review/Nueral%20Head%20Reenactment%20With%20Latent%20Pose%20Descriptor/img/meta_learning.PNG" width="30%"></img>

위 그림은 Meta-Learning을 개념적으로 설명한 것이다.
Meta-Learning이란, 학습하려는 데이터와 유사한 도메인의 데이터를 통해 대부분의 특징을 학습한 후 학습하려는 데이터를 가지고 Adaptive Process를 진행하여 학습을 완료하는 것을 뜻한다. 새로운 Task나 enviroment에 대해서 학습에 활용할 수 있는 데이터가 부족할 때 활용도가 높다.

## 2) Content Loss
Content Loss란 A Neural Algorithm of Artistic Style에서 소개된 Loss로, Generated Image와 Content Image의 유사성을 높이는 역할을 한다.
Content Image의 Feature Map P, Generated Image의 Feature Map X에 대하여 Generator의 Layer마다 MSE를 구한 것으로, 다음과 같이 정의한다.

<img src="https://github.com/Team-Hauguin/AI_ART/blob/main/paper%20review/Nueral%20Head%20Reenactment%20With%20Latent%20Pose%20Descriptor/img/Content Loss.PNG" width="30%"></img>

본 논문에서는 Identity Encoder를 통과한 Embedding을 Content로 사용한다.

## 3) Adversarial Loss
Adversarail Loss란 Generative Adversarial Nets에서 소개된 Loss로, Generator는 Discriminator의 손실을 최대화하고, Discrimiinator는 자신의 손실을 최소화하게 설계된 Loss이다.
Generator를 훈련시킬 때는 오른쪽 항만 학습에 관여하고, Discriminator를 훈련시킬 때는 두 항 모두 관여한다.
Real Image x, Fake Image G(z)라고 할 때 아래와 같이 정의한다.

Loss(Generator) = log(1-D(G(z)))
Logg(Discriminator) = log(D(x)) - log(1-D(G(z))) 

<img src="https://github.com/Team-Hauguin/AI_ART/blob/main/paper%20review/Nueral%20Head%20Reenactment%20With%20Latent%20Pose%20Descriptor/img/Adversarial_Loss.PNG" width="90%">
</img>

## 4) Dice Coefficient Loss
Dice Coefficient Loss란 Image Segmentation 성능을 평가하는 Loss로, 두 대상 segmentation 영역 중 겹치는 영역을 각 영역의 합으로 나누는 Loss이며 다음과 같이 정의한다.

<img src="https://github.com/Team-Hauguin/AI_ART/blob/main/paper%20review/Nueral%20Head%20Reenactment%20With%20Latent%20Pose%20Descriptor/img/Dice_Loss.PNG" width="90%">
</img>

## 5) Landmark

<img src="https://github.com/Team-Hauguin/AI_ART/blob/main/paper%20review/Nueral%20Head%20Reenactment%20With%20Latent%20Pose%20Descriptor/img/Facial_Landmark.png" width="90%">
</img>

Landmark(=Facial Landmark)란 위 그림처럼 눈썹, 코, 입술과 같이 사람이라면 누구나 가지는 얼굴의 특징점을 의미한다.
하지만 Landmark Detection 학습을 위해서는 많은 양의 Annotation이 필요하고 눈동자의 방향 등을 추출하기 어려운 문제가 있어, Pose Encoder를 제안하여 이 문제를 해결한다.

# Method
## 전체구조
<img src="https://github.com/Team-Hauguin/AI_ART/blob/main/paper%20review/Nueral%20Head%20Reenactment%20With%20Latent%20Pose%20Descriptor/img/1.PNG" width="90%">
</img>

## 1) Identity encoder

## 2) Pose encoder

## 3) Style Based Generator

## 4) Discriminator

## 5) Loss

# Result
## 1) Accuracy

## 2) Error

## 3) Result Image
