---
title:  "[MRC]ERNIE-Layout: Layout Knowledge Enhanced Pre-training for Visually-rich Document Understanding 논문 리뷰"
date: '2022-12-10 13:59:00 +09:00'
category: [논문, Moosic하게 Reading Comprehension]
tags: [NLP, 대회, RE, NER]
use_math: true
---

![](/assets/img/B/b0.png)

# 무식하게 논문읽기(MRC) ERNIE-Layout 편🔥

## 논문과 관련된 정보들
- 제출 시기 : 2022년 10월 12일 / 마지막 수정 날짜 : 2022년 10월 14일
- EMNLP 2022에서 Computation and Language(cs.CL) 주제로 Accept된 논문입니다.
- 멀티모달과 관련된 논문으로 표나 그래프 같은 Document를 어떻게 하면 더 잘 이해하도록 학습시킬지 사전학습과 관련된 논문입니다.
- 제출 당시 key Information Extraction(FUNSD, CORD, SROIE, Kleister-NDA)과 Document Question Answering(DocVQA)에서 SOTA를 달성하였습니다.

## [초록](https://arxiv.org/pdf/2210.06155.pdf)

> Recent years have witnessed the rise and success of pre-training techniques in visually-rich document understanding. However, most existing methods lack the systematic mining and utilization of layout-centered knowledge, leading to sub-optimal performances. In this paper, we propose ERNIE-Layout, a novel document pre-training solution with layout knowledge enhancement in the whole workflow, to learn better representations that combine the features from text, layout, and image. Specifically, we first rearrange input sequences in the serialization stage, and then present a correlative pre-training task, reading order prediction, to learn the proper reading order of documents. To improve the layout awareness of the model, we integrate a spatial-aware disentangled attention into the multi-modal transformer and a replaced regions prediction task into the pre- training phase. Experimental results show that ERNIE-Layout achieves superior performance on various downstream tasks, setting new state- of-the-art on key information extraction, document image classification, and document question answering datasets. The code and models are publicly available at [PaddleNLP1](https://github.com/PaddlePaddle/PaddleNLP/tree/develop/model_zoo/ernie-layout).

줄글이 아닌 표나 그래프 등의 시각적 정보가 있는 문서의 이해를 위한 사전학습 기술은 최근들어 급격히 발달하고 성공했다는 것이 명백해 보입니다. 하지만, 지금까지의 방법론들은 **레이아웃 중심**의 지식을 발굴하거나 활용하는 방식이 체계적이지 않아서 최적의 성능보다 부족한 성능을 보여줍니다. 본 논문에서, 우리는 전체 작업 흐름에서 레이아웃 지식을 강조하여 높여주어서 사전학습을 진행하는 방식인 ERNIE-Layout이라는 새로운 문서에 관한 사전학습 방식을 제안합니다. 이러한 방식은 **텍스트, 레이아웃 그리고 이미지로부터 추출한 특징들을 결합**하여 더 나은 표현을 학습시키기 위함입니다. 특히, 우리는 먼저 **순서화 단계(serialization)에서 입력 시퀀스를 재배치**시킵니다. 그리고 문서의 적절한 순서를 학습하기 위해 읽는 **순서 예측(reading order prediction)**이라는 사전학습과 관련있는 태스크를 제시합니다. 모델에게 레이아웃의 인식률을 향상시키기 위해, 우리는 멀티모달 트랜스포머의 어텐션을 **공간적인 요소분해(spatial-aware disentagled) 어텐션으로 대체**하여 다운스트림 태스크인 지역 예측(region prediction) 태스크를 사전학습 단계로 통합시켰습니다. 다양한 다운스트림 실험을 한 결과 좋은 성능을 달성할 수 있었고, 실험 핵심 정보 추출, 문서 이미지 분류 그리고 문서 질의응답 데이터셋에서 SOTA를 달성했습니다. 코드와 모델은 [PaddleNLP1](https://github.com/PaddlePaddle/PaddleNLP/tree/develop/model_zoo/ernie-layout)에서 자유롭게 보실 수 있습니다.

## 필요한 배경지식

> 레이아웃

논문에서 말하는 레이아웃은 **텍스트 정보가 있는 영역**을 의미합니다.

> 


## 내용 요약

## 결론 및 비판