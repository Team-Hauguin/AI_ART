# Introduction 
## 1) Visual Attribute Transfer란?
하나의 이미지를 다른 color, tone, texture의 style로 바꿔주는걸 말한다. 
예를들어, 같은 scene을 묘사한경우 그림이나 스케치를 사진으로 변경하거나 반대 경우 모두를 포함한다


# Motivation

## 1) Analogy with Bidirectional Constraints
유사한 appearance를 갖고 있거나, 구조적으로 동일한 위치에 있는 양방향의 제약조건을 가진 analogy를 확인할 수 있다. 

## 2) Reconstruction using CNN

## 3) Deep Patchmatch

# Deep Image Analogy

## 1) System Pipeline
전체 system pipeline은 그림과 같다. input image A와 B'을 Preprocessing 과정을 통해 layer를 나누게 되고 각 layer에서 3~5 순서대로 진행되게 된다. 

## 2) Preprocessing
Input 이미지는 A와 B'으로 imagenet으로 pretrained 된 VGG19를 통과하여 Feature map을 얻게된다. 
첫번째로 layer 5에서 A'(5), A(5) 그리고 B(5), B'(5)는 구조적으로 유사함을 이용할 예정이므로 동일하다고 보고 Layer 4의 A'(4), B(4)를 생성하게 된다. 
> A'(5) = A(5) , B(5) = B'(5)

## 3) Nearest-Neighbor Field Search

## 4) Latent Image Recontruction 
