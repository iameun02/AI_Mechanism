<b>[Activation]</b>

수치미분의 연산시간 문제를 해결하기 위해,
미분을 Backpropagation으로 풀었을 때,
또다른 문제가 존재 하게된다.

바로 Vanishing Gradient 발생한다는 것이다.
Vanishing Gradient 는 경사가 없다는 말로 
(오차항 에대한 파라미터의 편미분 값 : 경사) 
더이상 학습이 되지않는 것을 의미하며 더이상 파라미터 업데이트가 안되게 된다.

이는 시그모이드 (0~ 1) 사이를 계속 곱해주다보니 0에 수렴하기 때문에 발생하게 된다. <br>
(시그모이드 미분값 : 0~ 0.25 사이) <br>
즉, 이 문제는 히든레이어가 깊어 질수록 발생하는 문제이다.

이를 해결하기 위해선
원인 : 시그모이드의 미분 값이 0에 가까워서 이기 때문에
시그모이드 대신 다른 활성함수를 쓰는 방법으로 해결된다.
이는 다양한 activation 활용으로 발전하게 된다.

[Tanh] 
 - Activation Function : -1 ~ 1
 - Derivative 0~1
 - 미분값이 0 이되면 똑같이 경사 없어지는 단점이 존재

[ReLU]
  - Activation Function : 0 ~ ∞
  - Derivative 0~1
  - 미분값이 0 이되면 똑같이 경사 없어지는 단점이 존재
  - 꺾인 지점에서 미분이 안되기 때문에 한계 존재

[Leaky ReLU]
  - 조금 꺽어서 0에서 -로 만들어줌, 연속성을 보여함


[élu] 
  - 지수함수를 통해 곡선으로 만듬
  - 0 지점도없어지고, 꺾인 지점도 없어짐

------------------------------------

<b>[Optimization method_ 'solver'] </b>

경사하강법에 기능을 추가적으로 더해줘서 학습률을 효과적으로 변경해보는 방법!

𝒘 = 𝒘− 𝛄(𝞉 𝒆𝒓𝒓𝒐𝒓/ 𝞉 𝒘) [기본식]

#1. Stochastic Gradient Descent (확률적 경사하강)
- 전체데이터를 사용하는거 대신에 확률적으로 추출된 일부데이터(mini_size)를 여러번 학습
  Batch size : mini_size
- 결과는 Batch 결과에 수렴, 왜쓰느냐 계산속도가 훨씬 빠름
  경사하강식이 그대로임, 입력데이터가 바뀌는 것 <br>
  𝒘 = 𝒘− 𝛄(𝞉 𝒆𝒓𝒓𝒐𝒓/ 𝞉 𝒘) <br>
- 이미지 데이터는 기본적으로 용량이 크기 때문에, <br>
  Batch size 를 작게해서 한정된 메모리공간에서 작업할수있도록 함
- OOM error(Out Of Memory) → batch size를 작게해준다
- 성능 그닥,, 성능보단 속도에 집중 <br>


#2. Momentum
 - 이동과정에 '관성'을 가중치로 반영
 - 과거 이동방향과 이동하려는 방향의 벡터를 연산해서 이동폭을 보정
   𝒘 = 𝒘− 𝛄(𝞉 𝒆𝒓𝒓𝒐𝒓/ 𝞉 𝒘) + 𝒎*𝒅𝒘 
   𝒅𝒘 = 𝒘₀ - 𝒘₁

#3. Adaptive
  - 학습횟수에따라 Learning rate를 고정시키지 말고 적응시켜나갔으면 좋겠다.
  - 학습률을 조절 : 학습률을 점차 작게 한다.
  - 학습률 감쇠식 추가
     - 𝛄 = 𝛄 /(1+ 𝐩*𝐧) → 작어짐      
       - (𝐧 : 학습횟수 , 𝐩 : 0-1 사이의 값으로 만들어줌)
     - 𝐠  = 𝐠  + (𝞉 𝒆𝒓𝒓𝒐𝒓/ 𝞉 𝒘)²
     - 𝛄 (작아짐 ) / √𝐠 (커짐) : 결국 작아짐
     - 𝒘 = 𝒘 − 𝛄/ √𝐠 (𝞉 𝒆𝒓𝒓𝒐𝒓/ 𝞉 𝒘)

#3-1 RMSProp : Adaptive 모델에 g 제곱합을 지수평균으로 대체한 것 <br>


#4. Adam 
- #1 + #2 + #3
 - RMSProp 과 Momentum 장점을 합침
 - Momentum 과 같이 radient 지수평균을 저장
 - RMSProp 과 같이 gradient 제곱값의 지수평을 저장
 - 디폴트 옵션 Learning rate = 0.001 (adativ 적용되어있으니 점점 작아짐) → Optimizer를 통해 학습률 조절가능

