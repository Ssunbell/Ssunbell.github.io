---
title:  "[PE]sinusoidal add positional embedding"
date: '2023-01-23 16:59:00 +09:00'
category: [DeepLearning, PE]
tags: [Position Information, Positional Embedding, 포지션 임베딩]
use_math: true
---

![](/assets/img/B/b9.png){:width="100%" height="100%"}
*사인파 위치 인베딩*

# Attention is all you need
[Attention is all you need](https://arxiv.org/pdf/1706.03762.pdf) 논문은 NIPS 2017에 게재된 어텐션 구조에 대해 제안하는 논문이다. 오늘날 논문의 제목처럼 어텐션 구조가 쓰이지 않는 곳을 찾기 힘들어졌다. 자연어처리, 비전, 음성, 그래프 심지어는 시계열도 연구가 활발히 진행되고 있을 만큼 가히 ~~뉴진스~~ attention의 시대라고 볼 수 있다.

![](/assets/img/2023-01-23/2.gif){:width="100%" height="100%"}
*뉴진스는 이 논문을 분명 알고 있다.*

> 어텐션에서 사용하고 있는 positional embedding은 sinusoidal add positional embedding이다.

우리는 위치 임베딩에 대해서 알아볼 예정이므로 모델의 전체적인 구조나 다른 부분은 논문을 참고하기 바란다. 해당 논문의 저자는 어텐션 구조의 맹점인 순서 정보를 담지 못한다는 문제를 사인 함수와 코사인 함수를 이용한 위치 임베딩을 더함으로써 해결하였다.

## position embedding
### What is sinusoidal?
> sinusoidal add positional embedding에서 sinusoidal은 사인파를 의미한다.

어텐션 구조에서 순서 정보를 주기 위해서는 <font color='OrangeRed'>각 순서를 나타낼 수 있는 유일한 정보</font>를 담고 있어야 한다. 그렇다면 순서 정보를 담을 수 있는 가장 쉬운 방법은 무엇일까? 바로 번호를 메기는 것이다. 코딩에서는 이를 인덱스라 부르고 해당 인덱스로 순서 정보를 주면 될것이다.

PE\  =\  (1,2,3,\cdots ,n)

하지만 이는 n이 매우 커질 경우 순서 정보가 너무 커지는 바람에 우리의 원래 의도인 토큰간의 관계를 학습하는 어텐션 구조의 본질을 벗어나게 된다. 또한, n이 특정한 범위를 가지는 것이 아니기 때문에 모델의 일반화에 어렵다는 단점이 있다.


그렇다면 범위를 갖는 0 ~ 1 사이의 소수점으로 나타면 되지 않을까? 하지만 이것 또한 어렵다. 일반화에는 적합하나 input 길이가 1로 고정되기 때문에 원래의 input 길이를 알 수 없다.


논문 저자는 이를 해결하기 위해 다음과 같은 함수를 정의했다.

PE_{(pos,2i)}=sin(pos/10000^{2i/d_{model}})
PE_{(pos,2i+1)}=cos(pos/10000^{2i/d_{model}})

위의 함수는 사인과 코사인을 이용하여 순서 정보를 준다. 이를 통해 유일한 순서 정보를 줄 수 있다. 좀더 자세히 알아보자.

> 