# Arbitrary Style Transfer in Real-time with Adaptive Instance Normalization
## Introduction

참고 사이트

* 논문저자 github사이트 https://github.com/xunhuang1995/AdaIN-style
* [Arbitrary Style Transfer in Real-time with Adaptive Instance Normalization](https://towardsdatascience.com/fast-and-arbitrary-style-transfer-40e29d308dd3)

Gatys et al는 DNN을 통해 이미지의 content와 style feature를 추출하고,
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
Ulyanov et al가 단순히 BN을 IN으로 바꾸는 것으로 놀라운 향상을 이끌어 냇습니다. 




### Conditional Instance Normalization


IN을 약간 변형한 AdaIN을 소개합니다. AdaIN은 단순히 Content input의 평균과 분산을 Style input의 평균과 분산으로 맞추도록 조절합니다.
실험을 통해서 AdaIN이 효과적으로 병합하는 것을 확인하였습니다. 



## Interpreting Instance Normalization
## Adaptive Instance Normalization
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
