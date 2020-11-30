# Unpaired Image-to-Image Translation Using Cycle-Consistent Adversarial Networks 

# 1) Introduction  
  이 논문에서는 한 도메인의 이미지 데이터를 다른 도메인으로 image translation을 할때 항상 매칭이 되는 paried data가 있어야만 하는 
  Pix2Pix의 한계점을 해결하고자 한다. 기존의 픽셀에서 다른 픽셀로 바꾸는 모델인 Pix2Pix에서는 신발의 외곽선 이미지로 구체적인 신발의 
  이미지를 생성하고 싶다면 학습 단계에서 서로 매칭이 되는 paired image data가 존재해야한다. 하지만 스타일을 바꾸는 image translation에서 
  항상 매칭이 되는 대규모의 paired data를 구하기가 어렵기 때문에, CycleGAN을 통해 쌍을 이루지 않는 unpaired data set으로도 학습을 가능하게 하고자 한다. 

![image](https://user-images.githubusercontent.com/62173633/100192575-94229a80-2f35-11eb-9c6e-ac0fa01a55a5.png)

# 2) Formulation 
  먼저 이 논문에서는 두가지의 loss function을 제안하고 있다. 기존의 Pix2Pix에서는 Generator가 input과 output의 차이를 최소화로 학습하게 하는 모델로 
  L1 loss와 GAN loss를 사용했다. 그러나 CycleGAN에서는 Lgan(G(x),y)의 adversarial loss의 형태는 유지하되, L1 loss가 아닌 원래의 input data으로 
  최대한 복구가 가능하도록 하는 다른 loss function을 추가하기로 한다. 

### -Adversarial Loss 
![image](https://user-images.githubusercontent.com/62173633/100192458-5887d080-2f35-11eb-9fd0-53e6b74a4243.png)

![image](https://user-images.githubusercontent.com/62173633/100561577-a1a49f80-32fc-11eb-9192-5b845a09bc50.png)

   CycleGAN에서는 2개의 Generator와 2개의 Discriminator를 사용한다. 도메인 X로부터 G라는 Generator를 통해 Y도메인으로 변환하고 또 반대로 
   F라는 Generator를 통해 다시 도메인X로 변환한다. 여기서 존재하는 Dx와 Dy는 각각의 도메인을 위한 Discriminator로서 서로 속이도록 학습하여 
   더 진짜를 만드는 적대적 학습이 이루어진다. 
   
    
### -Cycle Consistency Loss 

![image](https://user-images.githubusercontent.com/62173633/100191490-8cfa8d00-2f33-11eb-8be6-fd1fef1210e5.png)

![image](https://user-images.githubusercontent.com/62173633/100561588-a8331700-32fc-11eb-801a-19f8dde6e3ff.png)

![image](https://user-images.githubusercontent.com/62173633/100566356-21386b80-3309-11eb-9351-1f6d645a16a8.png)
### ![image](https://user-images.githubusercontent.com/62173633/100566365-25fd1f80-3309-11eb-9e06-8a3a38564134.png)

   이 논문에서 가장 말하고자 하는 것은 G와 F Generator를 통해 서로 다른 도메인으로 보내진 데이터가 이전의 데이터로 다시 돌아와야 한다는
   cycle consistency를 설명한다. 즉, 서로 변환을 하는 과정에서 각각의 Generator를 통해 변환 시킨것을 다시 자기 자신과 pixel-wise loss를 
   걸어주는 것을 Cycle Consistency Loss라고 한다. 


![image](https://user-images.githubusercontent.com/62173633/100561741-06f89080-32fd-11eb-8527-d741cb53c6a4.png)

![image](https://user-images.githubusercontent.com/62173633/100561747-0b24ae00-32fd-11eb-9d94-1d2554956940.png)

   따라서, 전반적인 틀은 Pix2Pix와 같지만 2개의 Generator, 2개의 Discriminator를 사용해 X,Y 도메인으로 서로 변환이 가능하며 추가적으로 
   Cycle Consistency Loss를 사용한다는 점이 연구의 특징이다. 

# 3) Implementation 
### -Network Architecture 

# 4) Results 

![image](https://user-images.githubusercontent.com/62173633/100192472-60e00b80-2f35-11eb-9c3e-91edddd9b284.png)


