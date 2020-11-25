# Perceptual Losses for Real-Time Style Transfer and Super-Resolution
Justin Johnson, Alexandre Alahi, Li Fei-Fei, ArXiv, 2016년

[논문링크](https://arxiv.org/pdf/1603.08155.pdf)

## Summary
- 본 논문은 Style transfer와 Super resolution에서 Per-pixel loss functions 대신 __Perceptual loss functions__ 사용을 제안한다.
  - Per-pixel loss functions : 개별 픽셀 값을 기준으로 두 이미지를 비교 (Gatys Style transfer)
  - Perceptual loss functions : ImageNet과 같은 데이터셋으로 사전 훈련된 Convolution Neural Network로 두 이미지를 비교

## Method
![이미지01](https://user-images.githubusercontent.com/13701781/100174687-3cbe0380-2f10-11eb-8239-61d0569ae2dc.png)

- Image Transform Network
  - Deep residual convolutional neural network
  - Residual blocks와 stride로 Downsampling과 Upsampling을 수행
  - 이미지(x)가 입력되면 이미지(ŷ)를 출력(사이즈는 동일)

- Loss Network(Φ)
  - ImageNet Dataset으로 Pretrain된 VGG-16 Network
  - Feature reconstruction loss
  ![이미지03](https://user-images.githubusercontent.com/13701781/100180093-d12d6380-2f1a-11eb-802d-bfc6f8fd0824.png)
    - 출력 이미지(ŷ)와 레이어 'relu3_3'를 Euclidean distance로 계산 
  - Style reconstruction loss
  ![이미지04](https://user-images.githubusercontent.com/13701781/100180096-d4285400-2f1a-11eb-9017-da26ceaab271.png)
    - 출력 이미지(ŷ)와 레이어 'relu1_2','relu2_2','relu3_3','relu4_3'를 Gram matrices의 Frobenius norm으로 계산
  - Total reconstruction loss
    - Feature reconstruction loss + Style reconstruction loss + Total variation regularization(for denosing)

## Result
- Style Transfer
- Super Resolution
