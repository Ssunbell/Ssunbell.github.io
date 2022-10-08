---
title:  "[NLP]다국어의 저주(curse of multilinguality)와 Capacity Dilution Trade-off"
date: '2022-10-02 23:59:00 +09:00'
category: [NLP, 용어정리]
tags: [NLP, XLM-RoBERTa, XLM-R, curse of multilinguality]
use_math: true
---

![](/assets/img/Pororo/edi.png){: width="40%" height="40%"}
*즐거운 용어 파헤치기~*

#  👀 용어 파헤치기

## 1. 다국어의 저주(curse of multilinguality)

다국어의 저주란 모델의 용량이 고정이라고 가정할때, 더 많은 언어를 추가할수록 다국어 언어 모델에서 희귀한(low-resource) 언어의 성능이 어느 수준까지 상승하다가 단일 및 다국어 모델의 성능이 전체적으로 떨어지는 것을 말한다. XLM-R 모델(2020)에서 처음 이 개념을 제시했다.

![](/assets/img/2022-10-02/resource.png){: width=70%" height="70%"}
*high-resource and low-resource language(출처: Unsupervised Cross-lingual Representation Learning at Scale)*

> 위의 그래프를 보면 파란색이 스와힐리어(동아프리카쪽 언어)처럼 영어나 중국어에 비해 해당 데이터가 많지 않은 언어를 희귀한 언어(low-resource language), 해당 데이터가 많은 언어를 희귀하지 않은 언어(high-resource language)라고 정의한다.

1. 언어를 추가했을 때, 아래 그래프처럼 성능이 증가하다가 <font color='OrangeRed'>언어의 수가 너무 많이 증가하게 되면 해당 언어들의 성능이 되려 처음보다 떨어진다.</font>
2. 자세히보면 언어가 7개일 때보다 100개일때 희귀한 언어의 성능이 더 떨어지는 것을 볼 수 있다.
3. 동시에 희귀하지 않은 언어 같은 경우에는 언어의 수가 많아질수록 **지속적으로 성능이 떨어지는** 것을 볼수 있으며, 전체적인 성능도 **지속적으로 떨어지는 것**을 볼 수 있다.
4. 결국 상충관계(trade-off)가 존재하기 때문에 목적에 따라 학습 언어의 갯수를 조정해줘야 한다.

![](/assets/img/2022-10-02/curse.png){: width=70%" height="70%"}
*출처: Unsupervised Cross-lingual Representation Learning at Scale*

## 2. Positive Transfer and Capacity Dilution trade-off

> 여기서 용량(Capacity)은 모델에서 파라미터의 수를 의미한다. 이러한 용량은 메모리나 속도에 의해 제한을 받는다.

1. 긍정효과 전이학습과 용량 희석 간의 상충관계는 전이학습 간섭 상충관계(transfer-interference trade-off)라고도 한다.
2. 적은 데이터를 가진 희귀한(low-resource) 언어는 비슷한 많은 데이터를 가진(high-resource) 언어를 추가함으로써 성능을 개선할 수는 있지만, 언어당 용량이 감소하는 문제에 의해 <font color='OrangeRed'>전체적인 성능이 저하하는 문제</font>가 발생한다.
3. 위의 그래프를 보면 파란색 막대와 주황색 막대가 희석되어 회색의 막대를 구성하는 것을 볼 수 있다.


## 정리
결국 데이터가 없는 언어의 성능 향상을 원한다면

1. **전체적인 성능**를 우선 포기해야 하고
2. 언어의 수가 증가할수록 성능이 떨어지므로 **언어의 갯수 제한**

이라는 선택의 문제가 발생하게 된다.