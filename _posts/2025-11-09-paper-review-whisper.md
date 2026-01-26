---
layout: post
title:  "[논문 정리] Whisper: Robust Speech Recognition via Large-Scale Weak Supervision"
date:   2025-11-09
categories: [Paper Review]
tags: [Whisper, Speech Recognition]
---

## Introduction
* **Wav2Vec** → unsupervised learning
* decoder로 text 뽑아내기 힘드며, 그러기 위해선 fine-tuning 필요함
* ML에서 학습 데이터 셋 분포에서 벗어난 분포의 데이터 셋에 대해선 성능이 좋지 않음
* 특정 데이터셋에선 성능이 무지막지하게 좋아도 다른 데이터셋에선 X
    > ⇒ supervised fine-tuning된 decoder 없이도 out of the box 한 데이터까지 잘 다루는 시스템이 필요

* supervised learning에서는 high-quality audio 데이터가 모자른 상황이다
* 680,000 시간의 weakly supervised set을 모았다
    > ⇒ zero-shot에도 괜찮은 성능 + dataset-specific fine-tuning 필요없음
    > ⇒ multilingual, multitask를 위한 데이터셋도 포함되어 있음

<br>

## Approach

### 2.1 Data Processing
* data pre-processing 아주 minimal하게
* raw text를 예측하도록 거의 standardization 안함 (구두점 없애기, 소문자화 등등)
* inverse text normalization의 필요성을 없앰 (다시 구두점 만들거나 인간이 자연스럽게 받아들이게 수정하는 과정 필요없이 바로 결과물로 나옴)
* 인터넷에서 transcript랑 짝으로 오디오 데이터셋 만듦 (다양한 분포 반영 가능 - 녹음 환경, 언어, 발화자 등등)
* audio 다양성은 도움되지만, transcript는 그다지
* raw dataset의 trascript에서 별로 좋지 않은 data 들 존재
    > ⇒ filtering method로 거름

* 많은 인터넷의 transcript가 기존 ASR로 만들어진 결과이고, ML이 만든 결과와 인간이 만들어낸 data가 섞이면 성능 오히려 하락함
    > ⇒ heuristics 써서 이런거 감지해서 제거했다

* 기존 ASR로 생성된 transcript는 punctuation이나 formatting (문단 구분), stylistic aspect (capitalization) 같은 세밀한 부분 잡지 못함
* all-uppercase, lowercase 된 데이터나 comma가 전혀 등장하지 않는 데이터 등 인간이 만들어낸 자막이 아닐 경우가 큰 데이터들을 단순하고 rule-based 규칙들로 제거했음
* 오디오 - 자막이 언어 매칭 되는 경우에만 해당 데이터 사용
* X → en 의 경우에는 번역 데이터셋으로 추가
* 유사한 문장들은 제거
* 30초 단위의 segment로 끊어서 학습
* 오디오가 없는 부분도 VAD 학습을 위해 제외하지 않고 넣었음
* 초기 학습 이후 error rate를 기반으로 low-quality data는 제거했음 (misaligned transcript, heuristic으로는 잡히지 않은 데이터들 등)
* 학습과 평가 과정에서 trascript가 겹치는 부분 제거

### 2.2 Model
* 데이터셋의 성능 향상 효과를 위해 기존에 존재하는 transformer 모델 구조 차용
* data는 16kHZ, 80-channel log-Mel spectrogram으로 변환 (25ms window/ 10ms stride)
* zero-mean 되게 -1~1로 scale
* input → **2 Convolution layer + GELU** → **sinusoidal positioning embedding** → **encoder/ decoder** (같은 block 수)
* tokenizer는 GPT-2에서 사용한 거
* 다국어용은 vocabulary로 refit해서

### 2.3 Multitask Format
* 오디오 듣고 predicting 하는 거 이외에도 VAD, speaker diarization (화자 구분), inverse text normalization (나온 결과를 자연스럽게)
* 개별적인 모델 구조로 두면 굉장히 복잡해지기에 하나의 단일 모델로 이 모든 걸 해결
* one-to-many 구조이므로 task를 구분지을 수 있는 것 필요
* decoder input token으로 이걸 구분
* 인코더 출력 뿐만 아니라 그 전의 history of text (맥락)까지 가지고 함께 출력함

* 애매한 길이로 짤린 오디오도 출력 가능
* 확률적으로 이전 text도 같이 넣어줌

**[Tokens]**
* `startoftranscript` - start token (language 종류) (VoxLingual07 model로 뽑아낸 language)
* `nospeech` - 음성 없을 경우 모델이 해당 token을 뱉게 학습
* `transcribe` / `translate` - task token
* `notimestamps` - timestamps 유무
* relative(상대) 시간: 절대 시각(파일 00:01:21.34)이 아니라, “현재 오디오 세그먼트의 시작점”을 0으로 두고 그에 대한 상대 시간을 예측합니다.
* 20ms 단위 양자화(quantization): 모든 시간을 가장 가까운 20ms(0.02초) 격자에 올림/내림해 표현합니다. Whisper의 내부 해상도가 20ms라 여기에 맞춰요. → 1초당 50칸.
* 어휘에 타임스탬프 토큰 추가: 20ms마다 하나씩 대응하는 특수 토큰을 어휘에 넣고, 디코더가 일반 단어 토큰 사이에 타임스탬프 토큰을 내보내도록 학습합니다.
* `endoftranscript` - finish token

* previous context text는 오류 계산에 포함 X
* 학습은 모든 token을 예측하는 task

### 2.4 Training Details
* parameter 설정 값
* 이름을 예측하려는 문제가 있어서 이름 제외