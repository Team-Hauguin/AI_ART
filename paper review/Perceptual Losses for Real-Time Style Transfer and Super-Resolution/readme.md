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
    - 출력 이미지(ŷ)와 레이어 'relu3_3'를 Euclidean distance로 계산
![이미지02](https://user-images.githubusercontent.com/13701781/100180396-6af51080-2f1b-11eb-8a01-7122d0bef0c6.png)
  - Style reconstruction loss
    - 출력 이미지(ŷ)와 레이어 'relu1_2','relu2_2','relu3_3','relu4_3'를 Gram matrices의 Frobenius norm으로 계산
![이미지03](https://user-images.githubusercontent.com/13701781/100180398-6c263d80-2f1b-11eb-8c59-538f363bfdc6.png)
  - Total reconstruction loss
    - Feature reconstruction loss + Style reconstruction loss + Total variation regularization(for denosing)

## Result
- Style Transfer
  ![이미지04](https://user-images.githubusercontent.com/13701781/100181197-118de100-2f1d-11eb-9684-904e49938188.png)

- Super Resolution
  ![이미지05](https://user-images.githubusercontent.com/13701781/100181194-10f54a80-2f1d-11eb-84ef-e6dc40e7d128.png)
