---
title:  "[NLP]BERT vs RoBERTa 비교하기"
date: '2022-10-01 23:59:00 +09:00'
category: [모델탐구, RoBERTa]
tags: [NLP, 대회, 모두의 말뭉치]
use_math: true
---

## BERT 모델과 RoBERTa 모델 비교하기

> What is BERT?

BERT 모델은 NLP 분야에서는 혁신을 불러온 대규모 사전학습 모델이다. 구글에서 만든 이 BERT 모델은 나중에 다뤄보도록 하겠다.

## RoBERTa

> What is ROBERTa?

RoBERTa 모델은 **R**obustly **O**ptimizerd **BERT** **a**pporach 모델의 줄임말이다. Robust라는 의미는 우리말로 번역하면 '강건한'이라는 말을 많이 쓴다. 좀더 직관적으로 이해하자면 <font color='OrangeRed'>'어떤 상황에서든 비슷하게 잘 적용되는'</font>의 의미로 받아들여도 좋다. ~~통계학에서는 이상치에 영향을 받지 않는다는 의미이다.~~


Robustly Opimized이므로 어떤 상황에서든 잘 적용되게 BERT를 최적화한 모델이라는 의미에서 RoBERT이고, approach는 왜 있는지는 개인적으로는 잘 모르겠지만, 추측컨데 BERT모델을 변형해서 만들었기 때문에 저자들의 겸손한 표현이지 않나 싶다.

![](/assets/img/Pororo/humble.png){: width="50%" height="50%"}
*짜식.. 겸손하긴 ㅎ*

## BERT and ROBERTa comparison

> BERT와 RoBERTa의 비교

BERT의 문제점을 보완하고자 RoBERTa가 나왔고, 실제로 RoBERTa를 썼을 경우 더 좋은 성능을 내는 경우가 종종 있다. 우선 표로 대략적인 차이점을 확인하고 좀더 자세히 씹고 뜯고 맛보고 즐겨보자.

|비교|BERT|RoBERTa|
|------|---|---|
|출처|Google|Meta|
|마스킹|Static masking/substitution|Dynamic masking/substitution|
|입력데이터|Inputs are two concatenated document segments|Inputs are sentence sequences that may span document boundaries|
|다음문장예측|Next Sentence Prediction|No NSP|
|batch_size|256|2,000|
|토큰화|Word-piece tokenization|Character-level byte-pair encoding|
|데이터|BookCorpus and English Wikipedia|BookCorpus, CC-New, OpneWebText, Stories|
|시간길이|1M(1,000,000) steps|500K(500,000) steps|
|최적화|Train on short sequences first|Train only on full-length sequences|

### 1. Static vs Dynamic
> 정적 마스킹 vs 동적 마스킹

1. BERT
   - BERT의 가장 큰 특징은 <font color='OrangeRed'>어떤 특정 부분의 단어를 "MASK"라는 토큰으로 대체</font>하여 이 `<MASK>`의 원래 단어를 예측하는 학습 방법을 진행한는 것이다.
   - BERT에서는 학습 데이터를 4개의 복사본으로 만들어 각각 다른 단어를 마스킹하여 학습을 진행한다.
   - 이때, <font color='OrangeRed'>정적</font>이라는 표현은 이러한 복사본이 **그대로 재사용**된다는 점이다.

2. ROBERTa
   - ROBERTa는 4개의 복사본을 만들어 각각 다른 마스킹을 진했했다는 것은 BERT와 동일하다.
   - 하지만, 복사본을 그대로 사용한 BERT와 다르게, **랜덤**하게 진행하여 다른 방식으로 학습이 진행되도록 만들었다.
   - 이러한 방식을 여기서는 <font color='OrangeRed'>동적 마스킹</font>이라는 표현을 사용한다.

### 2. two concatenated document segments vs sentence sequences
> 두 문서를 합친 데이터 vs 문장을 계속 이어붙인 데이터

1. BERT
   - BERT에서는 두 문장을 합쳐서 훈련을 진행한다. 예를들어, `I love Pororo.`, `I love Loopy, too,`라는 두개의 문장이 있다면, `I love Pororo. I love Loopy, too.`의 합친(concatenated) 문장을 사용한다.
   - 아래 공식문서 중에서 주황색 박스를 보면 위의 텐서가 **합쳐진 문장**, 아래가 **합쳐진 문장의 id**를 의미하므로 0번째 문서와 1번째 문서가 합쳐진 것을 볼 수 있다.

![](/assets/img/2022-10-03/1.png)
*텐서플로우 공식문서*

2. RoBERTa
   - RoBERTa에서는 4가지 방식(SEGMENT-PAIR+NSP, SENTENCE-PAIR+NSP, FULL-SENTENCES, DOC-SENTENCE)의 성능을 비교하여 결과적으로는 DOC-SENTENCES이 좋은 성능을 보였다고 한다.
   - 하지만, FULL-SENTENCES와 가변적인 배치 사이즈의 차이만 존재하기 때문에 편의성을 위해 FULL-SENTENCES를 사용한다고 논문에서 말한다.
   - 그러면 FULL-SENTENCES의 설명을 한번 이해해보자.

> Each input is packed with full sentences sampled contiguously from one or more documents, such that the total length is at most 512 tokens. Inputs may cross document boundaries. When we reach the end of one doc- ument, we begin sampling sentences from the next document and add an extra separator token between documents. We remove the NSP loss(출처: [해당논문](https://arxiv.org/abs/1907.11692))

```
각각의 입력 데이터는 하나 이상의 문서에서 연속적으로 샘플링된 전체 문장으로 채워지며,
이러한 입력 데이터의 총 길이는 최대 512 토큰을 넘지 않는다.
입력 데이터는 문서의 경계를 넘을 수 있다.
한 문서의 끝에 도달하면 다음 문서의 문장을 가져와 샘플링하기 시작하고
문서 사이에 문서와 문서를 구분해주는 토큰(<SEP>)을 추가한다.
```

- 좀더 직관적으로 설명하자면 BERT에서는 보여준 **두 문서만** 합친 입력 데이터를 사용하는 반면,
- RoBERTa는 제한된 길이 내에서 2개 혹은 그 이상이 될 수도 있는 문서들을 한데 모아서 학습을 시킨다.

![](/assets/img/2022-10-03/2.png){: width="70%" height="70%"}
*입력 데이터 들어가요 뿌뿌~*

### 3. Next Sentence Prediction vs No NSP
> 다음 문장 예측

BERT에서는 마스킹을 예측(Auto-Encoding)하는 것뿐만 아니라 다음 문장을 예측(AutoRegressive)하는 것도 같이 포함되어 있지만, RoBERTa에서는 해당 예측은 진행하지 않았다.

![](/assets/img/2022-10-03/3.png)
*이 다음에 없어질 것을 예측해보자 (AutoRegressive)*

### 4. batch_size
> 256 배치 vs 2,000 배치

BERT는 256 사이즈의 배치 사이즈를 설정했지만, RoBERTa에서는 2000의 꽤나 큰 배치 사이즈를 설정했다. 다음은 논문 내용이다.

> Past work in Neural Machine Translation has shown that training with very large mini-batches can both improve optimization speed and end-task performance when the learning rate is increased appropriately (Ott et al., 2018). Recent work has shown that BERT is also amenable to large batch training (You et al., 2019)(출처: [해당논문](https://arxiv.org/abs/1907.11692)).

높은 배치 사이즈가 속도 측면과 학습률의 적절한 조정이 가능하다면 성능 또한 좋다는 의견에 따라 ROBERTa는 배치 사이즈를 높게 설정했다.

### 5. Tokenization
> 토큰화 방법

1. BERT
    - BERT에서는 BPE라는 알고리즘이 <font color='OrangeRed'>빈도수</font>에 기반하여 가장 많이 등장한 쌍을 병합하는 방법론을 변형하여 Corpus의 <font color='OrangeRed'>우도(Likelihood)값을 최대화(argmax)</font>하는 쌍을 병합한다.
    - 구글에서는 이를 `WordPiece Tokenizer`라는 토큰화 기법을 BERT에 사용한다.

2. RoBERTa
    - RoBERTa에서는 BPE Tokenizer를 사용하며, 빈도수 기반의 방법론을 사용한다.
    - 이러한 BPE Tokenizer는 openAI에서 GPT에 적용되는 토크나이저와 동일한 토크나이저를 사용한다.

### 6. Data
> 데이터 크기

데이터는 다다익선이므로 보통 초기 모델보다 데이터의 크기가 증가하는 경향을 보인다. 따라서, RoBERTa에서는 BERT보다는 많고 다양한 데이터를 사용했다.

### 7. timeSteps
> 시간길이

- 기본적으로 RNN 계열이기 때문에 `t기`의 시간을 나타내는 Step의 개념이 있다.
- BERT의 Step이 RoBERTa보다 2배 긴 것을 확인할 수 있지만, 배치 사이즈를 고려하면 RoBERTa(2,000 x 500M)가 전체적으로는 더 긴 timeStep을 가지고 있다.

![](/assets/img/2022-10-03/4.png)
*이러한 모델를 Sequence 모델이라고 한다*

### 8. Optimization process to training, first
> 훈련 최적화

- BERT에서는 첫번째 훈련에서 짧은 문장으로 훈련을 진행함으로써 훈련을 최적화하는 것이 도움이 된다고 하지만, RoBERTa에서는 해당 과정을 생략했다.

## 마무리
우리는 BERT와 RoBERTa 모델을 비교하여 알아보았다. 정리를 해보자면

- Dynamic masking
- setence sequences
- No SEP
- BPE Tokenizer

등이 RoBERTa의 큰 특징으로 요약할 수 있다.

![](/assets/img/Pororo/yeah.png){: width="30%" height="30%"}
*예~*