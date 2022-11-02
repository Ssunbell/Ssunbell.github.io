---
title:  "[MRC]무식하게 논문읽기 SMART-Loss"
date: '2022-10-30 16:59:00 +09:00'
category: [논문, Moosic하게 Reading Comprehension]
tags: [NLP, STS, 유사도]
use_math: true
published: true
---

> SMART: Robust and Efficient Fine-Tuning for Pre-trained Natural Language Models through Principled Regularized Optimization

# Abstract
- 전이 학습(Transfer learning)은 자연어 처리 영역을 근복적으로 바꿔놓은 방법론입니다. 많은 SOTA 모델은 우선 대규모 텍스트 말뭉치를 사전학습한 다음 downstream 작업을 위한 미세조정(fine-tuning)의 흐름을 따라갑니다. 그러나 너무나도 복잡한 사전학습 모델에 비해 제한된 크기의 데이터로 dwonstream을 실시하려는 문제로 인해 downstream 작업을 파인튜닝하는 과정에서 오버피팅이 발생하고 처음보는 데이터에 일반화(generalize)하지 못하는 문제가 발생합니다. 이러한 문제를 해결하기 위해, 우리는 더 나은 일반화 성능을 얻기 위해 강건하고(robust) 효율적인 사전학습 모델의 파인튜닝 학습 방식을 제안합니다. 학습 방식은 다음 두가지로 이루어져있습니다. 1. 모델의 복잡한 방식을 효율적으로 다뤄주는 Smoothness-inducing 규제. 2. 신뢰 구간 방식과 공격적인 업데이트를 방지하는 Bregman proximal point optimization. 우리의 실험은 GLUE, SNLI, SciTail과 ANLI의 자연어처리 영역에서 SOTA를 달성했습니다. 더불어, GLUE에서는 T5모델보다 더 높은 성능을 보였습니다.

