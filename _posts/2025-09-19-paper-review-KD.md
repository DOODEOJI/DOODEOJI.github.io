---
layout: post
title:  "[논문 정리] Distilling the Knowledge in a Neural Network"
date:   2025-09-19
categories: [NLP]
tags: [Knowledge Distillation]
paper_url : https://arxiv.org/pdf/1503.02531
---



# Introduction
* Cumbersome model → (distillate, transfer knowledge) small model for suitable
* 기존 대형 모델을 일반화 시키기 위해선 옳은 일반화 방향성에 대한 정보가 있어야 함.
* 하지만, 훈련 과정에서는 그런 정보를 알 수 없음
    * ⇒ **but, Distillation에서는 small model이 더 잘 일반화 될 수 있음**
* 대형 모델에서 생성된 soft target으로 학습 → 더 많은 정보를 얻을 수 있음
* **cf)** hard target `[0, 0, 1, 0, 0]` 정답 여부만, soft target `[0.1, 0.2, 0.5, 0.05, 0.15]` 확률 분포 값
* softmax 함수의 출력 값을 이용하면 cross-entropy cost function 에서는 영향력이 잘 반영되지 않을 수 있음 (확률 값이 0에 가깝다면)
* 따라서 softmax 함수의 출력 값이 아닌 입력 값(logits)을 활용하여 teacher와 student 간 squared difference를 활용하는 방법도 있음
* transfer set (small model의 train set)은
    1. unlabeled data 일수도 있고,
    2. 기존 training set을 활용할 수도 있음 (hard target 까지 활용하는 것)
* small model이 정확하게 분포를 맞히지 못해도 해당 분포의 방향으로 가는 것 만으로도 좋다

# Distillation
* softmax function의 temperature 값이 올라가면 더 부드러운 분포가 생성됨 (한쪽으로 치우쳐지지 않는) → 더 많은 정보를 학습할 수 있음
* ⇒ T가 커질 시 `logits/T` 의 값이 절대적으로 작아짐. 따라서, `e^(logits/T)`의 값 또한 작아져 각 값 간 차이가 적어짐. ⇒ 확률 분포가 극단적이지 않게 됨
* 학습 시에는 `T > 1`, 추론에서는 `T = 1` 사용해야함
* distilled model 에서는 soft target (큰 모델에서 사용한 T)과 hard target (T=1)을 가중 평균해서 사용함 + soft target에서 T를 증가시키면 gradient가 `1/T^2` 만큼 줄어들기 때문에 `T^2`를 곱해줘야함

## 2.1 Matching logits is a special case of distillation
* **high-T**: cost function이 logits 간 squared difference가 됨 → logit 맞히기에 집중
    * (+ 확률 분포가 평탄화 됨 ⇒ 학습 안정성, 간단한 cost function, negative class 정보 반영 가능)
* **low-T**: negative logits을 무시할 수 있음 (확률 분포가 0에 가까운 logits들) ⇒ noise 무시
* **but**, high-T에서 얻을 수 있었던 useful info를 잃음
* ⇒ 경험적으로 **intermediate-T**를 사용하는 게 낫더라

> **[2.1 추가 설명]**
>
<img src="/assets/images/2025-09/KD/2.1_math_1.png" width="550" alt="2.1">

>
> * C = cross-entropy cost function
> * z = distilled model’s logit
> * v = distilling model’s logit
>
> 테일러 전개를 사용하여 `e^z/T ≈ 1+z/T`로 근사 가능
> (z/T가 1보다 작을 때만 성립하므로 high-T여야함 ~ T > |z| )
>
<img src="/assets/images/2025-09/KD/2.1_math_2.png" width="550" alt="2.1">

>
> 따라서 식을 정리하면 다음과 같음.
> 또한, 수식의 간단함을 위해 zero-mean 가정 (`sum(z) = sum(v) = 0`)
> ~ 상대적인 차이는 같으므로 분포는 유지됨
>
<img src="/assets/images/2025-09/KD/2.1_math_3.png" width="550" alt="2.1">

>
> 따라서 식이 정리되고, 단순히 logit 간의 squared error로 근사할 수 있음

## Preliminary experiments on MNIST
* **teacher** : 2 hidden layers of 1200 hidden units on 60,000 training set with regularization
    * → 67 test errors
* **small net** : 2 hidden layers of 800 hidden units with no regularization
    * → 146 test erros
* **small net with soft targets with T = 20**
    * → **74 test errors**
* hidden layer ≥ 300 ⇒ T ≥ 8
* hidden layer = 30 ⇒ 2.5 ≤ T ≤ 4
* trasfer set에서 3을 아예 제거 → 1010개의 3 test set에서 133개만 오류
    * bias 3.5 increase 조정 시 14만 오류
    * transfer set에 7, 8만 있어도 (+bias 조정) 비교적 좋은 성능을 냄

## Experiments on speech recognition
* Ensemble of models → single distilled model
* 전체 문장이 아닌 frame 단위로 나눠서 cost function 계산
* **teacher** : 8 hidden layers of 2560 recified linear units and softmax layer with 14000 labels
    * (about 85M) (training DNN with 2000 hours and 550M examples)
* **ensemble** : randomly initialized 된 10개의 모델 ensemble (train set 다르게 바꾸는 건 효과 X)
* **distilled**: T = [1, 2, 5, 10] (2가 최적), hard taget cross-entropy에는 가중치 0.5 적용
* T = 1로 설정하고 distillation 수행하면 성능 향상은 제한적임

<img src="/assets/images/2025-09/KD/ex_results.png" width="550" alt="experiments_results">

# Training ensembles of specialists on very big datasets
* Ensemble 병렬 학습→ but, 추론 시 computation 속도가 느림
    * ⇒ 추론에 distilled model 사용하여 해결 가능
* 병렬 학습 가능해도 애초에 모델이 크고, 데이터가 크면 학습량이 어마어마해짐
    * ⇒ 모델을 조그맣게 쪼개서 ensemble → but, 과적합 되기 쉬움
    * ⇒ soft target 사용으로 과적함 방지

## 5.1 The JFT dataset
* Google dataset (100M labeled images with 15000 labels)
* 여러 모델 복사본(replica)이 같은 training set의 다른 mini-batch 학습 후 업데이트 한 가중치를 중앙 서버로 보내서 중앙 서버에서 파라미터 업데이트 후 replica에 보내줌
* 하나의 replica 내부에서도 뉴런을 여러 부분으로 쪼개서 다른 코어에 배치
* ⇒ 이렇게 병렬화 해도 훈련에 6개월 걸렸음

## 5.2 Specialist Models
* cumbersom model = ensemble of one generalist + many specialist model
* specialist의 softmax에서는 필요 없는 class를 ‘쓰레기통 class’로 한 번에 묶으면 차원 줄어듦
* overfitting을 막기 위해, specialist의 초기 가중치는 generalist의 초기 가중치로 설정하고,
* train set은 50% special set + 50% random set으로 설정
* train set의 bias를 조정하기 위해 specialist class가 과대 샘플된 비율만큼 dustbin class의 logit을 조절할 수 있음

## 5.3 Assigning classes to specialists
* generalist가 자주 혼동하는 class들을 묶어주는 것이 좋음 → 혼동 행렬로 clustering 할 수 있지만 정답 label이 필요함
* **정답 label 없이도 clustering**
    * generalist의 예측 분포에 covariance matrix 적용시켜서 clustering
    * 예측 분포에서 covariance가 높게 나타나는 것은 두 클래스를 혼동하고 있다는 것
* 저자는 on-line version of K-means 사용했다고 함
* (몇몇 clustering algorithm 사용해봤는데 결과는 비슷)

## 5.4 Performing inference with ensembles of specialists
* 입력 들어오면 generalist 예측에서 가장 높은 n개 클래스 선택 (저자: n=1)
* 선택된 클래스이 specialist의 클래스 그룹에 속하면 해당 specialist를 활성화
* 최종 확률 분포 q를 계산하기 위해 KL divergence를 최소화
* specialist에서 역시 관심 없는 클래스는 dustbin 클래스로 합침

<img src="/assets/images/2025-09/KD/5.4.png" width="550" alt="5.4">

> **[KL divergence 추가 설명]**
>
<img src="/assets/images/2025-09/KD/KD.png" width="550" alt="KD">

> * P는 실제 분포, Q는 비교할 분포
> * p와 q가 비슷할 수록 log 값이 작아지고 KLD 값이 작아짐
> * p = q 인 상황이 0으로 최소
> * 따라서 위 식은 generalist와 specialist의 분포를 모두 고려하여 가장 분포가 비슷한 최종 분포 q를 만들어내는 것
> * 해당 식은 closed solution이 없기에 `q = softmax(z)`로 두고 gradient descent 한다고 함

## 5.5 Results
* specialist는 각자 개별 학습이 가능하고 학습이 훨씬 빠르다
* 어떤 class는 여러 specialist에 포함될 수 있음
* 많은 specialist가 특정 class를 cover할 수록 성능은 좋아진다
* specialist는 병렬화가 쉬워 확장성이 좋다

<img src="/assets/images/2025-09/KD/final_results.png" width="550" alt="final_results">

# Soft Targets as Regularizers
* 3%의 dataset만 사용하여 모델 돌렸을 때 soft target이 훨씬 overfitting이 덜 된다
* 심지어는 early-stopping도 사용하지 않았는데 100% dataset 모델과 비슷한 성능으로 수렴했다

## 6.1 Using soft targets to prevent specialists from overfitting
* specialist는 자신이 관심 있는 class만 학습 → overfitting 되기 쉬움
* → (dustbin으로 한번에 처리 말고) soft target으로 관심 없는 class에 대한 정보도 제공

# Relationship to Mixtures of Experts
* MoE 방식은 specialist와 유사하지만, 병렬 학습이 어렵고 큰 데이터셋에서는 사용하기 어렵다
* (MoE - multiple expert가 존재하고 gating network가 어떤 expert를 사용할지 확률적으로 결정)

# Discussion
* Ensemble이나 큰 데이터 셋에서 distillation은 효과적
* 특정 class에 대한 data가 없어도 잘 분류해냄
* ensemble 모델의 정보를 잘 받아서 deploy (inference)에 용이함
    * (ensemble은 추론에 많은 시간 소요)
* large dataset에서 highly confusable cluster를 뽑아내어 specialist model을 구축할 수 있었음
    * (large model → generalist + specialists)
* specialists → single large net으로 복원은 시도해보지 않았음