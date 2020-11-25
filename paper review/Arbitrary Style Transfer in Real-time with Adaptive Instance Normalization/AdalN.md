# Arbitrary Style Transfer in Real-time with Adaptive Instance Normalization
## Introduction

참고 사이트

* https://github.com/xunhuang1995/AdaIN-style
* https://towardsdatascience.com/fast-and-arbitrary-style-transfer-40e29d308dd3

Gatys et al는 DNN을 통해 이미지의 content와 style feature를 추출하고,
Arbitrary(임의)의 이미지에서 추출된 content와 style feature를 병합하는 **Style transfer**를 제시하였습니다.
그러나 Style transfer는 연산속도가 매우 느리다는 단점이 있습니다. 

이 논문에서는 실시간으로(그만큼 빠른) 이미지를 arbitrary style로 바꾸는 방식을 제안하고자 합니다. 
이 방식의 핵심은 __adaptive instance normalization(AdaIN)__ 입니다. 
AdaIN 레이어는 Content feature의 평균과 분산을 Style feature의 평균과 분산으로 정렬하는 기능을 갖습니다. 
이 방식은 사전에 학습할 필요가 없고 매우 빠릅니다. 

## Background
우리의 방식은 Feed-forward style transfer에서 매우 효과적인 Instance Normalization(IN)에서 영감을 얻었습니다.
IN을 약간 변형한 AdaIN을 소개합니다. AdaIN은 단순히 Content input의 평균과 분산을 Style input의 평균과 분산으로 맞추도록 조절합니다.
실험을 통해서 AdaIN이 효과적으로 병합하는 것을 확인하였습니다. 


## Interpreting Instance Normalization
## Adaptive Instance Normalization
## Experimental Setup
## Training
## Result


# This is an <h1> tag
## This is an <h2> tag
###### This is an <h6> tag

*This text will be italic*
_This will also be italic_

**This text will be bold**
__This will also be bold__

_You **can** combine them_

* Item 1
* Item 2
  * Item 2a
  * Item 2b
  
1. Item 1
1. Item 2
1. Item 3
   1. Item 3a
   1. Item 3b
   
![GitHub Logo](/images/logo.png)
Format: ![Alt Text](url)


http://github.com - automatic!
[GitHub](http://github.com)

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