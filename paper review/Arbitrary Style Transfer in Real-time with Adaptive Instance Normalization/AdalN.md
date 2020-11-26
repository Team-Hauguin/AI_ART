# Arbitrary Style Transfer in Real-time with Adaptive Instance Normalization
## Introduction

참고 사이트

* 논문저자 github사이트 https://github.com/xunhuang1995/AdaIN-style
* [Arbitrary Style Transfer in Real-time with Adaptive Instance Normalization](https://towardsdatascience.com/fast-and-arbitrary-style-transfer-40e29d308dd3)

Gatys et al.은 DNN을 통해 이미지의 content와 style feature를 추출하고,
Arbitrary(임의)의 이미지에서 추출된 style feature를 병합하는 **Style transfer**를 제시하였습니다.
그러나 Style transfer는 **연산속도가 매우 느리다는 단점**이 있습니다. 

이 논문에서는 이미지를 **실시간(그만큼 빠른)** 으로 임의의 style로 바꾸는 방식을 제안하고자 합니다. 
이 방식의 핵심은 __adaptive instance normalization(AdaIN)__ 입니다. 
AdaIN 레이어는 Content feature의 평균과 분산을 Style feature의 평균과 분산으로 정렬하는 기능을 갖습니다. 
이 방식은 사전에 학습할 필요가 없고 매우 빠릅니다. 

## Background
AdaIN은 Feed-forward style transfer에서 매우 효과적인 Instance Normalization(IN)에서 영감을 얻었습니다.

### Batch Normalization
* [Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift](https://arxiv.org/abs/1502.03167)

Ioffe and Szegedy가 소개한 batch normalization(BN)은 feature의 통계량을 normalizing하여 feed-forward networks의 학습을 매우 쉽게 만들었습니다.
BN레이어는 원래 discriminative networks의 학습을 가속화 하기 위해 만들어졌으나 generative image modeling에도 효과가 있다고 알려져 있습니다. 

BN은 주어진 batch x에 대해 각각의 feature channel의 평균과 분산을 정규화합니다. 
![BN](https://user-images.githubusercontent.com/8110442/100293752-0c39a080-2fc8-11eb-8dba-bc4bc4352572.png)

### Instance Normalization
원래의 style transfer에서는 각각의 convolutinal layer에 BN layer를 포함하고 있었으나,
Ulyanov et al.가 단순히 BN을 IN으로 바꾸는 것으로 놀라운 향상을 이끌어냇습니다. 

![IN](https://user-images.githubusercontent.com/8110442/100295018-d1d20280-2fcb-11eb-8e76-1b1d7aa4c60a.png)

### Conditional Instance Normalization
Dumoulin et al.은 알파인 parameter인 gamma, beta를 학습하는 대신에 다른 parameter set을 학습하는 conditional instance normalization (CIN)을 제안했습니다. 
![CIN](https://user-images.githubusercontent.com/8110442/100295468-085c4d00-2fcd-11eb-8e44-92f79efcfd3c.png)

훈련과정에서 style 이미지는 고정된 이미지 셋(실험에서는 s=32)에서 random하게 선택됩니다.
그 다음 content이미지는 CIN에서 사용된 parameter에 의한 스타일 전송 네트워크에 의해 처리됩니다. 
놀랍게도 네트워크는 동일한 컨볼루션 매개 변수, 다른 affine parameter를 사용하여 완전히 다른 스타일의 이미지를 생성할 수 있습니다. 

하지만 CIN layer가 포함된 네트워크는 정규화 layer가 없는 네트워크와 비교하여 2*F*S 추가 매개변수가 필요합니다.(F:네트워크 Feature map수)
스타일 수에 따라 추가 paramaeter 수가 선형적으로 늘어나기 때문에 많은 스타일을 모델링하기 위해 확장하는것이 어렵고 또한 재학습이 없는 임의의 style적용에 맞지 않는 방법입니다.  

## Interpreting Instance Normalization

IN가 크게 성공했지만 Style transfer에서 잘 작동하는 이유는 명확하지 않습니다. 
Ulyanov et al.[52] 가 제시한 IN의 성공은 content 이미지의 대조에 대한 불변성때문입니다. IN은 Feature공간에서 발생하므로 픽셀공간에서 나타나는 단순한 대비 정규화보다 더 큰 영향을 미칩니다. 더 놀라운것은 IN의 affine parameters는 output이미지의 스타일을 완전히 변경할 수 있다는 것입니다. 

DNN의 convolutional feature 통계량은 이미지의 Style을 포착할 수 있는것을 잘 알려져 있습니다.
반면 Gatys et al.은 2차 통계량을 최적화 목표로 사용하였습니다. Li et al.은 최근 채널 별 평균 및 분산을 포함한 다른 많은 통계를 일치시키는 것이 스타일 전달에 효과적이라는 것을 보여주었습니다. 
이런 관찰에 영향을 받아 **Instance Normalization** 은 통계량을(평균과 분산) 정규화하여 스타일 정규화를 수행한다고 주장합니다. 
그리고 DNN은 이미지 descriptor 역할을 수행하지만 또한 이미지를 생성할때 style을 제어할 수도 있다고 생각했습니다. 

single-style trasfer를 위한 texture networks를 향상시키기 위해서 IN, BN을 활용한 Code를 실행했습니다. 
예상했듯이 IN 변환이 BN모델보다 더 빨랐습니다. 
우리는 모든 학습이미지를 luminace channel에서 histogram equlization에 의한 같은 대조에 의해서 정규화했습니다. 

![IN2](https://user-images.githubusercontent.com/8110442/100295022-d4345c80-2fcb-11eb-949c-27d391862ee7.png)

Fig.1 (b)에서 보여지듯이, IN 여전히 효율적입니다. Ulyanov et al.의 설명이 불완전함을 나타냅니다. 
우리의 가정을 증명하기 위해 우리는 모든 학습 이미지를 같은 스타일로 정규화하고 Pretrained된 stlye transfer 네트워크를 제공했다. 
Fig1.1 (c)에 따르면 이미지가 이미 style normalized되어 있을대 IN에 의한 개선은 훨씬 더 작아집니다. 
남이있는 차이는 style normalization[24] 완벽하지 않다는 사실로 설명 할 수 있습니다. 
또한 스타일 정규화 된 이미지에대해 훈련된 BN이 있는 모델은 원본 이미지에 대해 IN이 있는 모델만큼 빠르게 수렴할 수 있습니다. 
우리의 결과는 IN이 style nomalization처럼 수행한다는 사실을 나타낸다. 

## Adaptive Instance Normalization


IN을 약간 변형한 AdaIN을 소개합니다. AdaIN은 단순히 Content input의 평균과 분산을 Style input의 평균과 분산으로 맞추도록 조절합니다.
실험을 통해서 AdaIN이 효과적으로 병합하는 것을 확인하였습니다. 



## Experimental Setup
## Training
## Result


  
1. Item 1
1. Item 2
1. Item 3
   1. Item 3a
   1. Item 3b
  
  

As Kanye West said:

> We're living the future so
> the present is our past.

I think you should use an
`<addr>` element here instead.

```javascript
function fancyAlert(arg) {
  if(arg) {
    $.facebox({div:'#foo'})
  }
}
```

    function fancyAlert(arg) {
      if(arg) {
        $.facebox({div:'#foo'})
      }
    }
    
    
def foo():
    if not bar:
        return True

- [x] @mentions, #refs, [links](), **formatting**, and <del>tags</del> supported
- [x] list syntax required (any unordered or ordered list supported)
- [x] this is a complete item
- [ ] this is an incomplete item

First Header | Second Header
------------ | -------------
Content from cell 1 | Content from cell 2
Content in the first column | Content in the second column
