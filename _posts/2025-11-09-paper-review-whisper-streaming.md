---
layout: post
title:  "[논문 정리] Whisper Streaming: Real-time Transcription via Local Agreement"
date:   2025-11-09
categories: [Speech]
tags: [Whisper, Speech Recognition, Real-time]
---

## Abstract
* whisper → whisper-streaming (using local agreement)

<br>

## Introduction
* whisper는 offline data만 처리 가능 (processing time 제약이 없음)
* real-time 구현하는 naive한 방법 : 30초씩 끊어서 처리
    > → latency가 너무 길고, segment 사이에 단어가 끊기면 처리 불가

* 저자들은 **LocalAgreement** 사용한 whisper-streaming 제안
    > → latency 3.3초대로 줄임

<br>

## Background

### Whisper
* 기존 모델보다 4배 빠르다고 알려진 개조된 **faster-whisper** 사용 (+ `large-v2` 모델 사용)
* 백본으로 whisper 사용했지만 word-level timestamp랑 punctuation 제공하는 다른 모델도 사용 가능

### Streaming
* T시점 이전 출력 시퀀스들을 기반으로 T시점 이전의 소스 시퀀스에 대한 출력_T 를 산출하는 Task

### LocalAgreement
* 시점 간 동일하게 2회 겹치는 buffer content에 대해 confirm
* confirm 하는 구간에 온점 존재하면 trim하고 버퍼에 남겨놓음
* buffer confirm 시 이전 context와 비교하여 변경된 부분이 있어도 반영하지 않음

<br>

## Whisper-Streaming

### Update loop
* **MinChunkSize** - iter마다 chunk 얼마나 처리할건지
    > → latency와 quality 결정

### Audio buffer
* whisper는 30초 단위 문장으로 학습 / timestamp, punctuation 제공
* Update마다 들어온 input buffer에 넣고 whisper로 처리
* 버퍼에는 항상 새로운 문장이 들어오게끔 처리 (whisper 로직에 맞게)
* timestamp of last word에 confirmed 마킹
    > → 다음 update에서 buffer에 있는 내용 모두 whisper 다시 돌림 (다시 돌려서 변경된 부분은 무시함)

### Skipping the confirmed part
* output 출력 시 들어온 단어의 timestamp가 마지막으로 confirm된 timestamp와 1초 이내의 간격이면 중복 검사 필요
* 들어온 단어의 n-gram (n = 1-5)과 confirmed output과 비교하여 같으면 skip함.

**[예시]**
```text
T-1 시점 output: 나는 학교에 갔다
T시점 output : (나는 학교에 갔다)가 집에 갔다