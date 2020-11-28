# Arbitrary Style Transfer in Real-time with Adaptive Instance Normalization
## 1. Introduction

참고 사이트

* [ [논문사이트] Arbitrary Style Transfer in Real-time with Adaptive Instance Normalization](https://arxiv.org/abs/1703.06868)
* [논문저자 github사이트] https://github.com/xunhuang1995/AdaIN-style
* [ [참고페이지] Arbitrary Style Transfer in Real-time with Adaptive Instance Normalization](https://towardsdatascience.com/fast-and-arbitrary-style-transfer-40e29d308dd3)

Gatys et al.은 DNN을 통해 이미지의 content와 style feature를 추출하고,
Arbitrary(임의)의 이미지에서 추출된 style feature를 병합하는 **Style transfer**를 제시하였습니다.
그러나 Style transfer는 *연산속도가 매우 느리다는 단점* 이 있습니다. 

이 논문에서는 이미지를 *실시간(그만큼 빠른)* 으로 임의의 style로 바꾸는 방식을 제안하고자 합니다. 
이 방식의 핵심은 __adaptive instance normalization(AdaIN)__ 입니다. 
AdaIN 레이어는 Content feature의 평균과 분산을 Style feature의 평균과 분산으로 정렬하는 기능을 갖습니다. 
이 방식은 사전에 학습할 필요가 없고 매우 빠릅니다. 

## 2. Background
AdaIN은 Feed-forward style transfer에서 매우 효과적인 Instance Normalization(IN)에서 영감을 얻었습니다.

### 2.1 Batch Normalization
* [ [논문 사이트] Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift](https://arxiv.org/abs/1502.03167)

Ioffe and Szegedy가 소개한 batch normalization(BN)은 feature의 통계량을 normalizing하여 feed-forward networks의 학습을 매우 쉽게 만들었습니다.
BN레이어는 원래 discriminative networks의 학습을 가속화 하기 위해 만들어졌으나 generative image modeling에도 효과가 있다고 알려져 있습니다. 

BN은 주어진 batch x에 대해 각각의 feature channel의 평균과 분산을 정규화합니다. 
![BN](https://user-images.githubusercontent.com/8110442/100293752-0c39a080-2fc8-11eb-8dba-bc4bc4352572.png)

### 2.2 Instance Normalization
* [ [논문 사이트] Instance Normalization: The Missing Ingredient for Fast Stylization](https://arxiv.org/abs/1607.08022)



원래의 style transfer에서는 각각의 convolutinal layer에 BN layer를 포함하고 있었으나,
Ulyanov et al.가 단순히 BN을 IN으로 바꾸는 것으로 놀라운 향상을 이끌어냇습니다. 

![IN](https://user-images.githubusercontent.com/8110442/100295018-d1d20280-2fcb-11eb-8e76-1b1d7aa4c60a.png)

### 2.3 Conditional Instance Normalization
Dumoulin et al.은 알파인 parameter인 gamma, beta를 학습하는 대신에 다른 parameter set을 학습하는 conditional instance normalization (CIN)을 제안했습니다. 
![CIN](https://user-images.githubusercontent.com/8110442/100295468-085c4d00-2fcd-11eb-8e44-92f79efcfd3c.png)

훈련과정에서 style 이미지는 고정된 이미지 셋(실험에서는 s=32)에서 random하게 선택됩니다.
그 다음 content이미지는 CIN에서 사용된 parameter에 의한 스타일 전송 네트워크에 의해 처리됩니다. 
놀랍게도 네트워크는 동일한 컨볼루션 매개 변수, 다른 affine parameter를 사용하여 완전히 다른 스타일의 이미지를 생성할 수 있습니다. 

하지만 CIN layer가 포함된 네트워크는 정규화 layer가 없는 네트워크와 비교하여 2*F*S 추가 매개변수가 필요합니다.(F:네트워크 Feature map수)
스타일 수에 따라 추가 paramaeter 수가 선형적으로 늘어나기 때문에 많은 스타일을 모델링하기 위해 확장하는것이 어렵고 또한 재학습이 없는 임의의 style적용에 맞지 않는 방법입니다.  

### 2.4 Interpreting Instance Normalization

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
우리의 가정을 증명하기 위해 우리는 모든 학습 이미지를 같은 스타일로 정규화하고 Pretrained된 stlye transfer 네트워크를 제공했습니다. 
Fig1.1 (c)에 따르면 이미지가 이미 style normalized되어 있을대 IN에 의한 개선은 훨씬 더 작아집니다. 
남이있는 차이는 style normalization[24] 완벽하지 않다는 사실로 설명 할 수 있습니다. 
또한 스타일 정규화 된 이미지에대해 훈련된 BN이 있는 모델은 원본 이미지에 대해 IN이 있는 모델만큼 빠르게 수렴할 수 있습니다. 
우리의 결과는 IN이 style nomalization처럼 작동한다는 사실을 나타냅니다.

## 3. Adaptive Instance Normalization

IN이 affine 매개 변수에 의해 지정된 단일 스타일로 입력을 정규화하는 경우, 아핀 변환을 사용하여 임의로 지정된 스타일에 적응시킬 수 있습니까? 
여기서는 Adaptive Instance Normalization(AdaIN)라고하는 IN에 대한 간단한 확장을 제안합니다. 
AdaIN은 content input x와 style input y를 수신하고 x의 채널 별 평균과 분산을 y와 일치하도록 정렬합니다. 
BN, IN 또는 CIN과 달리 AdaIN에는 학습 가능한 아핀 매개 변수가 없습니다. 
대신 스타일 입력에서 affine 매개 변수를 계산합니다.

![adain](https://user-images.githubusercontent.com/8110442/100517490-293fc080-31ce-11eb-9d32-e144d49db7e1.PNG)

여기서 우리는 단순히 σ(y)로 정규화 된 콘텐츠 입력의 크기를 조정하고 µ(y)로 이동합니다. IN과 유사하게 이러한 통계는 여러 공간 위치에서 계산됩니다. 
직관적으로 특정 스타일의 브러시 스트로크를 감지하는 기능 채널을 고려해 보겠습니다. 이러한 종류의 획이있는 스타일 이미지는이 기능에 대해 높은 평균 활성화를 생성합니다. 
AdaIN이 생성 한 출력은 콘텐츠 이미지의 공간 구조를 유지하면서이 기능에 대해 동일한 높은 평균 활성화를 갖습니다. 브러시 스트로크 기능은 [10]과 유사하게 피드 포워드 디코더를 사용하여 이미지 공간으로 반전 될 수 있습니다. 이 기능 채널의 분산은 더 미묘한 스타일 정보를 인코딩 할 수 있으며, 이는 AdaIN 출력 및 최종 출력 이미지로도 전송됩니다.

간단히 말해 AdaIN은 기능 통계, 특히 채널 별 평균 및 분산을 전송하여 기능 공간에서 스타일 전송을 수행합니다. AdaIN 레이어는 [6]에서 제안한 스타일 스왑 레이어와 유사한 역할을합니다. 스타일 스왑 작업은 시간과 메모리를 많이 소모하지만 AdaIN 레이어는 IN 레이어만큼 간단하여 계산 비용이 거의 추가되지 않습니다.



## 4. Experimental Setup
### 4.1 Architecture
![adain2](https://user-images.githubusercontent.com/8110442/100517567-c0a51380-31ce-11eb-9265-af159ea41d18.PNG)

위의 style transfer network은 제안된 AdaIN layer를 기반으로 생성되었습니다.  
이 모델의 앞부분 Encoder는 VGG-19를 통해 미리 학습된 layer인 simple encoder-decoder architecture를 가져왔습니다.
그 후 content, style이미지를 feature 공간에서 encoding하고 우리는 feature map을 adaIN layer에 의해서 content feature map의 평균과 분산을 style feature map의 그것으로 정렬시키고 target feature map을 만들었습니다. 

![adain3](https://user-images.githubusercontent.com/8110442/100520621-03241b80-31e2-11eb-82f8-0e40075f757d.png)

* T : style transfer network
* t : target feature maps
* c : content image
* s : style image

랜덤하게 초기화된 decoder g는 map t를 이미지 공간으로 학습된다. 그리고 style이미지를 생성합니다.

### 4.2 Training
이 논문에서는 [6]의 설정에 따라 콘텐츠 이미지로 MS-COCO [36]를 사용하고 WikiArt [39]에서 주로 수집 한 그림 데이터 세트를 스타일 이미지로 사용하여 네트워크를 훈련시켰습니다. 각각의 데이터 셋은 80,000개의 샘플을 학습시켰습니다. adam optimizer, batch-size = 8를 사용했습니다. 
첫번째로 가로세로비율을 유지하며 짧은 쪽의 이미지사이즈를 512 조정하고, 256 * 256 영역을 랜덤하게 잘랐습니다. 
네트워크는 fully convolutional이므로 모든 크기의 이미지에 적용할 수 있습니다. 

![adain4](https://user-images.githubusercontent.com/8110442/100520623-06b7a280-31e2-11eb-9fb0-78e4285cd0e5.png)

[51, 11, 52]방식과 유사하게 미리 학습된 VGG19을 사용했고 content loss와 style loss의 조합으로 손실함수를 계산했습니다. 
content loss는 target feature와 output image와의 유클리드 거리를 사용했습니다. 
AdaIN layer는 오직 스타일 feature의 평균과 분산을 전달시키므로 style loss는 이 통계량에만 일치합니다. 일반적으로 사용되는 gram matrix가 비슷한 손실을 생성할 수 있지만 개념적으로 더 간결하기 때문에 IN통계와 일치했습니다.(Li et al. [33])
VGG-19레이어 내부의 각 φi는 style loss를 계산합니다. 이 실험에서는 relu1 1, relu2 1, relu3 1, relu4 1 layers를 동일한 가중치로 사용했습니다. 

## 5. Result

![adain5](https://user-images.githubusercontent.com/8110442/100520624-08816600-31e2-11eb-9539-fd972e2910a6.png)

결과 비교를 위해서 3가지 방식의 style transfer와 비교하였습니다. 
1. the flexible but slow optimization-based method [16]
2. the fast feed-forward method restricted to a single style [52]
3. the flexible patch-based method of medium speed [6]

기타 : default configurations.

**1. Qualitative Examples**  

5번째 행의 예시와 같이 몇가지 케이스에서는 [16], [52]방식과 비교했을때 품질이 조금 떨어지는 것으로 보입니다. speed, flecibility, quality에서 trade-off가 있기 때문에 충분히 발생가능합니다.  
[6]과 비교해보면 AdaIN 방식은 좀더 충실하게 스타일을 전송하는것으로 보입니다. 
마지막 예제에서 각 content-patch를 가장 가까운 style-patch와 일치하도록 시도하는 것에서 [6]의 한계를 명확하게 볼 수 있습니다. 
대부분의 content-patch가 타겟style의 대표스타일이 아닌 지역적인 스타일과 match되면 스타일 전송은 실패했다고 여겨집니다.
따라서 일부경우 [6]이 더 매력적인 결과를 생성할 수 있지만 대표스타일과 통계를 일치시키는 것이 더 일반적인(_안정적인_) 솔루션이라고 이야기하고 있습니다.  

**2. Quantitative evaluations**

AdaIN 방식은 빠른속도와 유연성 vs 어느정도의 품질(저하) 가 trade-off로 작용하고 있습니다. 그렇다면 품질저하의 정도는 얼마나 될까요? 
이를 비교하기 위해 최적화를 기반으로하는 [16], fast single-style transfer [52] 와 스타일 손실 측면에서 비교했습니다. 

![adain6](https://user-images.githubusercontent.com/8110442/100520628-09b29300-31e2-11eb-9a35-0849d7a62822.png)

손실은 [52, 16]과 동일하게 산출되었습니다. 
AdaIN 방식과 [52]의 방식은 최적화 50~100 반복에서 유사한 스타일 손실을 얻습니다. 이는 AdaIN 방식의 강력한 일반화 능력을 보여줍니다. 또한 스타일 손실은 원본 콘텐츠 이미지보다 훨씬 작습니다. 

**3. Speed analysis**

대부분의 계산시간은 content encoding, style encoding, and decoding하는데 소비합니다(각각 1/3정도). 
비디오 처리같은 일부 애플리케이션 시나리오에서 스타일 이미지는 한번만 인코딩 되어야 하며 AdaIN은 저장된 스타일 통계를 사용하여 모든 후속 이미지를 처리할 수 있습니다. 
스타일 인코딩 시간을 제외하고 알고리즘은 256*256, 512*512 이미지에 대해 각각 56 및 12FPS로 실행되므로 사용자가 업로드한 임의의 스타일을 실시간으로 처리 할 수 있습니다. 
[16]보다 약 3배 빠르며 [6]보다 1-2배 빠릅니다. 

**4.to high resolution style images** 

게다가 AdaIN 방식은 몇 가지 스타일로 제한되는 feed-forward 방법과 비슷한 속도를 냅니다.[52, 11] 이 방식이 처리 시간이 약간 더 긴 것은 VGG 기반 네트워크 때문입니다. 보다 효율적인 아키텍쳐를 사용하면 속도를 더 향상시킬 수 있습니다. 임의의 스타일에 적용 할 수 있는 다른 기존 알고리즘보다 훨씬 빠르면서도 적은수의 스타일로 제한된 방법과 비슷한 속도를 보입니다. 
