---
layout: post
title:  "[논문 정리] A Survey on Knowledge Distillation of Large Language Models"
date:   2025-09-19
categories: [NLP]
tags: [Knowledge Distillation, Large Language Models]
paper_url : https://arxiv.org/pdf/2402.13116
---
## 2.1 overview
### 2.1 Comparing Traditional Recipe
* 과거에는 그저 teacher 모델의 정답을 복제하는 수준
* 최근에는 LLM 프롬프트를 활용하여 teacher 모델의 reasoning이나 thought process를 복제한다.

## 2.2 Relation to Data Augmentation
* LLM을 활용한 KD에서는 specific한 domain에 맞는 context-rich한 data를 augmentation함.
* Dataset을 diversity와 specificity 측면에서 풍부하게 사용함으로써 student가 teacher 모델의 understanding이나 cognitive strategies를 체화할 수 있게 됨.
* +적은 dataset이나 computing resources에서도 충분히 잘 작동하게 됨

## 2.3 Survey Scope
* KD에서 파생 → Skill Distillation, Verticalization Distillation
* SD (= skill enhancement) == thinking patterns, persona/preference modeling and value alignment (가치 판단) 등의 영역을 LLM이 배울 수 있게
* VD (= domain-specific applications) == 특정 domain 지식을 주입

## 2.4 Distillation Pipeline in LLM era
* 단어 elicitation 끌어내기
<img src="/assets/images/2025-09/KD_with_LLM/elicitation.png" width="700" alt="elicitation">
* teacher 모델에서 skill/domain tuning, alignment를 한 후 그에 맞는 dataset을 입력. 프롬프트를 통해 teacher의 단순히 정답 뿐만 아니라 추론, 사고방식 (chain of thought)을 student 에게 주입

**Target Skill or Domain Streering Teacher LLM**
1. Seed knowledge as Input
2. Generation of Distillation Knowledge
3. Training the Student Model with a Specific Learning Object

# 3. Knowledge Distillation Algorithms
⇒ Knowledge + Distillation

## 3.1 Knowledge

### 3.1.1 Labeling
<img src="/assets/images/2025-09/KD_with_LLM/labeling.png" width="700" alt="labeling">
* Instruction I (LLM에게 지시)
    * (e.g. “You must generate a detailed and long answer.” or “explain like I’m five, think step-by-step”)
* Demonstration c (output을 내기 위한 x,y 쌍 제공)
    * ⇒ LLM의 CoT (chain-of-thought), PoT (program-of-thought) 끌어내기 가능
* Quora나 Stack overflow 같은 곳에서 query를 가져오기도 함
* 두 teacher model 간 대화도 위의 인간의 query와 상응하는 dialogue 생성 가능

### 3.1.2 Expansion
<img src="/assets/images/2025-09/KD_with_LLM/expansion.png" width="700" alt="expansion">
* labeling → lack of scale and variety of the input data (← privacy of the data)
* input x도 demonstration을 통해 LLM이 직접 생성
* 생성된 expansion input은 또 다시 demonstration pool로 추가됨
* Evol-Instruct method (instruction 자체도 LLM이 expansion)
    * Difficulty (e.g. rewriting the question to be more complex)
    * Diversity (e.g. generatingmore long-tailed instructions)
    * ⇒ 비슷하지만 의미, 맥락적으로 약간은 다른 dataset 생성하면서 여러 task에서 성능을 높일 수 있음
    * but, 초기 seed demon과 teacher LLM 성능에 과도하게 의존
    * ⇒ bias를 만들어 낼 수 있음

### 3.1.3 Data Curation
<img src="/assets/images/2025-09/KD_with_LLM/data_curation.png" width="700" alt="data_curation">
* Labeling : seed가 task dataset → potential noise, dirty data
* Expansion : seed demonstration에서 generate → homogeneous data
* ⇒ meta information을 seed로 입력
* UltraChat ←They collect extensive meta-information across three domains: Questions about the World, Creation and Generation, and Assistance on Existing Materials.
* 여러 document나 open-source dataset에서 meta-info 수집
* meta-info는 구체적인 demon이나 instruction이 아닌 task와 관련된 일반적인 정보들

### 3.1.4 Feature
<img src="/assets/images/2025-09/KD_with_LLM/feature.png" width="700" alt="feature">
* Black-box 모델로 distillation → White-box로 distillation
* output probability distribution : 확률 분포도 함께 제공
* intermediate layer : task에 적합한 중간 층만 distillation
* self-generated sequence ← feedback : student가 먼저 생성한 후 teacher가 피드백
* quantization of LLM : LLM 양자화로 loss는 최소화하면서 원본 output distribution은 유지
* Multi-teacher/ ensemble : output distribution 앙상블
    * → Black-box 모델에선 적용 불가, 성능이 block-box 기반 distillation 보다 떨어짐

### 3.1.5 Feedback
<img src="/assets/images/2025-09/KD_with_LLM/feedback.png" width="700" alt="feedback">
* Teacher가 student 모델의 생성값을 피드백
    * 랭킹 , 평가
    * 직접 고쳐주기 (자신의 출력물 기반 or GT 기반)

### 3.1.6 self-knowledge
<img src="/assets/images/2025-09/KD_with_LLM/self-knowledge.png" width="700" alt="self-knowledge">
* Self-Instruct ~ data Expansion
* Self-Align ~ context-distillation (produce indepth and detailed response)
* RLCD ~ aligned, unaligned output 둘 다 사용
* filtering method - Impossible Distillation, LMSI
* Reinforced Self-Training (ReST) (iteration)
    * grow : multiple output
    * improve : ranking or filtering
* Self-Play, Self-Rewarding etc.

## 3.2 Distillation

### 3.2.1 Supervised Fine-Tuning
or Sequence-Level KD (SeqKD)
* effective for black-box LLM
* teacher가 생성한 sequence의 likelihood를 최대화

### 3.2.2 Divergence and Similarity
from white-box teacher LLM
* minimizing divergence in probability distributions
* enhancing the similarity of hidden states