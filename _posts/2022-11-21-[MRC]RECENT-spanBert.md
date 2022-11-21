---
title:  "[MRC]Relation Classification with Entity Type Restriction 논문 리뷰"
date: '2022-11-21 13:59:00 +09:00'
category: [논문, Moosic하게 Reading Comprehension]
tags: [NLP, 대회, RE, NER]
use_math: true
---

![](/assets/img/B/b0.png)

# 무식하게 논문읽기(MRC) REMC 편🔥

## 초록
관계 분류는 문장 안에서 두 속성 사이의 관계를 예측하는 문제를 다룬다. 존재하는 방법론 중 하나는 두 속성들 사이의 관계 후보군들 중에서 모든 관계를 고려하는 방법이 있다. 이러한 방법론은 속성 종류에 따른 관계 후보들의 **규칙**들을 무시한 채 관계 후보군 사이의 잘못된 관계를 예측하는 결과를 야기한다. 본 논문에서는 우리는 속성 타입을 끌어와서 관계 후보군을 제한하는 방식인  RElation Classification with ENtity Type restriction (RECENT)이라는 새로운 페러다임을 제시한다. 특히, 관계와 속성 타입의 상호 제한이 공식화되고 관계 분류에 도입된다. 게다가, 제안된 패러다임인 RECENT는 모델에 구애받지 않는다. 각각 두 개의 대표적인 모델 GCN과 SpanBERT를 기반으로, RECENT를 적용한 RECUNTGCN과 RECENTSpanBERT로 훈련시켰다. 표준 데이터 세트에 대한 실험 결과는 RECUNT가 GCN과 SpanBERT의 성능을 각각 6.9 및 4.4 F1 포인트 향상시킨다는 것을 나타낸다. 특히, RECENTSpanBERT는 TACRED에서 새로운 SOTA 달성한다.

> Abstract: Sentence-level relation extraction (RE) has a highly imbalanced data distribution that about 80% of data are labeled as negative, i.e., no relation; and there exist minority classes (MC) among positive labels; furthermore, some of MC instances have an incorrect label. Due to those challenges, i.e., label noise and low source availability, most of the models fail to learn MC and get zero or very low F1 scores on MCs. Previous studies, however, have rather focused on micro F1 scores and MCs have not been addressed adequately. To tackle high mis-classification errors for MCs, we introduce (1) a minority class attention module (MCAM), and (2) effective augmentation methods specialized in RE. MCAM calculates the confidence scores on MC instances to select reliable ones for augmentation, and aggregates MCs information in the process of training a model. Our experiments show that our methods achieve a state-of-the-art F1 scores on TACRED as well as enhancing minority class F1 score dramatically.