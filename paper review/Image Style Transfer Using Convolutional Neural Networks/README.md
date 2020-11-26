# Image Style Transfer Using Convolutional Neural Networks, Leon A. Gatys

## Code & Results
- https://github.com/NoPainNoCode/Neural_Style_Transfer
- 구현 코드는 위 url의 style_transfer.ipynb를 참고하면 된다.
- result 폴더에 100 epoch마다 target image를 저장하도록 했다. 따라서 source image의 texture가 어떻게 target image에 tranfer되어 가는지 그 양상을 확인할 수 있다.

## Introduction
저자들은 'Neural Algorithm of Artistic Style'이라는 새로운 방법론을 논문을 통해 제시하며 이 알고리즘을 통해서 'image style transfer'를 기존의 방법들보다 더 효과적으로 수행할수 있다고 주장한다.
'image style transfer'란 아래 그림처럼 source image(or style reference image)의 texture 정보만을 가져와서 target image(or base image)에 합성시키되 target image의 semantic content는 보존하는 것을 의미한다.
논문의 저자들은 VGG-Netowork의 'conv1_2', 'conv2_2', 'conv3_2', 'conv4_2 그리고 'conv5_2' layer들의 정보를 사용했는데, Network의 higher layer로 갈수록 detail한 pixel 정보는 lost해가는 반면(=원래 사진이 가진 style이나 texture는 잃어 가는 반면), high-level content(=집모양과 같은 object의 shape)는 보존한다는 것을 알아냈다.
![Figure1](https://user-images.githubusercontent.com/54407983/100335252-1daa9900-3018-11eb-80d7-0de32b765836.jpeg)

## Deep image representations
- 16개의 Convolutional layer와 5개의 pooling layer로 구성된 VGG19 network을 가져와서 fully connected layer를 제거하고(feature extraction용으로만 사용할 것이기 때문에 분류를 위한 FC layer는 제거)
feature extraction용으로 사용했고, 원래 VGG19 network에서는 pooling을 maximum pooling operation을 쓰도록 되어 있는데 이것을 average pooling으로 변경하면 좀 더 appealing한 result가 나오는 것을 발견했다고 한다.

- 보통 딥러닝 모델을 학습시킨다고 하면, 모델의 weights를 loss function이 minimize 하는 방향으로 every step마다 오류를 역전파해가면서 weights를 변경해가면서 학습이 진행된다.
그런데 이 논문에서는 Network의 weights를 변경해가는 것이 아니고, generated_image. 즉 image 자체의 구성을 변경해가면서 최종 원하는 output의 image를 만들어 나간다.

- 학습과정은 어느 방향으로 이루어지는 것일까를 생각해보면 아래식과 같다. distance() 함수는 쉽게 표현하고자 사용한 표기일 뿐 L2 Norm 등으로 대체될 수 있는 함수이다.
즉 우리가 원하는 최종 아웃풋인 generated_image(=combination image)는 style 정보는 reference_image(=source image)에서 가져와야하고 content 정보는 original_image(=base image)에서 가져와야 하기 때문에 아래와 같이 loss fucntion을 구성한뒤 training이 진행됨에 따라서 loss를 minimize해가면 된다.

- minimize(loss) = distance[style(reference_image) - style(generated_image)] + distance[content(original_image)-content(generated_image)]

### Content representation
아래 content의 Loss를 구하는 식의 구성을 보면, base_image의 feature map인 p와 generated_image의 feature map인 x와의 거리를 L2 loss를 이용하여 구함을 볼 수 있다.
그리고 x의 feature map인 F_ij를 조금 변화시켜봤을때 content Loss가 어떻게 변화하는지를 측정하고 있다.
![Figure2](https://user-images.githubusercontent.com/54407983/100339088-c0fdad00-301c-11eb-8234-e6f4564ffda4.jpeg)

### Style representation
reference_image로부터 style 정보를 추출하기 위해서 Gram matrix를 이용한다.
Gram matrix는 서로 다른 feature map간에 correlation을 측정하기 위해 사용된다.
Gram maxrix를 구하는 과정인 아래 그림의 내요에 대해서 설명해 보면,
특정 layer의 feature map size가 (400, 599, 64)라고 가정하면 이 숫자들은 (width, height, channel)의 정보를 의미하는데, (64, 400, 599)처럼 channel first로 변경후 width와 height를 곱해서 정보를 표현한는 것으로 다시 변경하면 (64, 239600)이 된다. 이것이 논문에서 말하는 F_ik이다.
![Figure3](https://user-images.githubusercontent.com/54407983/100340189-2736ff80-301e-11eb-9b95-ab63bfe72d54.jpeg)

지금까지 살펴본 generated image의 content 정보를 어떻게 표현할 것인지와 style 정보를 어떻게 표현하 것인지를 전체 bird view 관점에서 살펴보면 아래 그림과 같다.

### Style transfer

## Result
### Trade-off between content and style matching
### Effect of different layers of the Convolutional Neural Network
### Initialisation of gradient descent
### Photorealistic style transfer

## Discussion
