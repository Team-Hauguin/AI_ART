# Image Style Transfer Using Convolutional Neural Networks, Leon A. Gatys

## Code & Results
- https://github.com/NoPainNoCode/Neural_Style_Transfer
- 구현 코드는 위 url의 style_transfer.ipynb를 참고하면 된다.
- result 폴더에 100 epoch마다 target image를 저장하도록 했다. 따라서 source image의 texture가 어떻게 target image에 tranfer되어 가는지 그 양상을 확인할 수 있다.

## Introduction
저자들은 'Neural Algorithm of Artistic Style'이라는 새로운 방법론을 논문을 통해 제시하며 이 알고리즘을 통해서 'image style transfer'를 기존의 방법들보다 더 효과적으로 수행할수 있다고 주장한다.
'image style transfer'란 아래 그림처럼 source image(or style reference image)의 texture 정보만을 가져와서 target image(or base image)에 합성시키되 target image의 semantic content는 보존하는 것을 의미한다.
![Figure1](https://user-images.githubusercontent.com/54407983/100335252-1daa9900-3018-11eb-80d7-0de32b765836.jpeg)

## Deep image representations
16개의 Convolutional layer와 5개의 pooling layer로 구성된 VGG19 network을 가져와서 fully connected layer를 제거하고(feature extraction용으로만 사용할 것이기 때문에 분류를 위한 FC layer는 제거)
feature extraction용으로 사용했고, 원래 VGG19 network에서는 pooling을 maximum pooling operation을 쓰도록 되어 있는데 이것을 average pooling으로 변경하면 좀 더 appealing한 result가 나오는 것을 발견했다고 한다.
### Content representation
### Style representation
### Style transfer

## Result
### Trade-off between content and style matching
### Effect of different layers of the Convolutional Neural Network
### Initialisation of gradient descent
### Photorealistic style transfer

## Discussion