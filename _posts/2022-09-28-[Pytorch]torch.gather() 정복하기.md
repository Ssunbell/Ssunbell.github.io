---
title:  "[Pytorch]torch.gather 정복하기(+예제)"
date: '2022-09-28 16:59:00 +09:00'
category: [Pytorch, torch 기본]
tags: [네이버, 부스트캠프, 네이버부스트캠프, AI Tech, 부스트캠프 AI Tech 4기, torch, Pytorch, gather, reshape, view]
use_math: true
---
![](https://user-images.githubusercontent.com/97590480/192314341-3ac916f5-4acb-4c84-83be-8c87543701e8.png)

## torch.gather()란?

> 우선 [파이토치 공식문서](https://pytorch.org/docs/stable/generated/torch.gather.html#torch.gather)를 확인해보자.

![](https://user-images.githubusercontent.com/97590480/192312877-72ed96eb-162a-4ca6-939f-3e62bfd466e2.png)
*파이토치 gather 함수 정의*

정의가 매우 짧게 설명되어 있다. 그렇기 때문에 이해하기 어렵다고 생각한다. 정의부터 천천히 살펴보자.

> 차원(dimension)으로 정의되는 축(axis)을 따라 값(values)을 모아준다(Gathers values along an axis specified by dim).

뭔가 값을 **모아준다**는 의미로부터 우리는 **<font color='OrangeRed'>인덱싱</font>**과 관련이 있는 함수라는 것을 대충 파악할 수 있다.

![](/assets/img/Pororo/angry.png){: width="30%" height="30%"}
*이해가 안되도 천천히 가보자*

> 파라미터 값은 어떤걸 받는지 살펴보자.

1. input (Tensor) – 인덱싱을 할 원본 데이터(Tensor 객체)*(the source tensor)*
   - 말 그대로 원본 데이터를 의미한다. 당연히 텐서로 넣어줘야 한다.
2. dim (int) – 인덱싱을 진행할 차원(dims) 혹은 축(axis)*(the axis along which to index)*
   - 인덱싱을 진행할 차원을 의미한다. 밑에서 예제를 보면서 더 상세히 이해해보자.
3. index (LongTensor) – 모아줄 요소들의 인덱스 값을 포함하는 indices(여기서는 텐서)*(the indices of elements to gather)*
   - 해당 차원에서 인덱싱 값을 모아두는 것을 indices라고 보통 부르는데, 이 indices 텐서를 의미한다.
   - 이 또한 dim과 같이 이해해줘야 하므로 예제를 보며 이해해보자.

> 예제 1) 2차원 tensor

```python
import torch

t = torch.tensor([[1, 2, 3],
                  [4, 5, 6],
                  [7, 8, 9]])

## dim = 0일때,
torch.gather(t, 0, torch.tensor([[0, 0, 0],
                                 [0, 0, 0],
                                 [0, 0, 0]]))
>>>
tensor([[1, 2, 3],
        [1, 2, 3],
        [1, 2, 3]])

## dim = 1일때,
torch.gather(t, 1, torch.tensor([[0, 0, 0],
                                 [0, 0, 0],
                                 [0, 0, 0]]))
tensor([[1, 1, 1],
        [4, 4, 4],
        [7, 7, 7]])
```

위의 예시를 보면 dim이 0과 1일때, 결과가 다른 것을 확인할 수 있다. 위의 결과만 확인해보면
1. dim = 0일때, <font color='OrangeRed'>1행</font>([1, 2, 3])에 대해서만 인덱싱이 일어난 것을 알 수 있다.
2. dim = 1일때, <font color='OrangeRed'>각 행</font>의 첫번째 인덱스(1,4,7)에 대해서만 인덱싱이 일어난 것을 알 수 있다.

이걸로 미루어 짐작해보면, dim은 똑같은 인덱스 번호여도 보는 **각도**가 다르다는 것을 알 수 있다.

> 여기서 공식 문서를 다시 한 번 보자.

![](/assets/img/2022-09-27/2.png)
*파이토치 공식문서*

3차원 텐서에서는 위의 예시와 같이 dim에 해당하는 인덱싱 번호에 indices가 들어가게 된다.


아직 3차원은 어려우니 2차원인 위의 예시로 설명하자면,

```python
# input
t = torch.tensor([[1, 2, 3],
                  [4, 5, 6],
                  [7, 8, 9]])
# dim = 0 일때,
tensor([[1, 2, 3],
        [1, 2, 3],
        [1, 2, 3]])
# dim = 1 일때,
tensor([[1, 1, 1],
        [4, 4, 4],
        [7, 7, 7]])
```

1. dim = 0 일때 인덱싱은 `out[1][0] = input[indices[1][0]][0]`로 구성된다.
   1. indices[1][0]은 3개의 [0,0,0] 중에서 **두번째**를 의미하고, 그 중에서 **첫번째**를 의미하므로 0을 의미한다.
   2. input[indieces[1][0]][0]은 input[0][0]이므로 1을 의미한다.
   3. out[1][0]은 두번째 행 첫번째 열을 의미하므로 해당 칸에 **1**이 들어가게 된다.
2. dim = 1 일때 인덱싱은 `out[1][0] = input[1][indices[1][0]]`로 구성된다.
   1. indices[1][0]은 3개의 [0,0,0] 중에서 **두번째**를 의미하고, 그 중에서 **첫번째**를 의미하므로 0을 의미한다.
   2. input[1][indices[1][0]]은 input[1][0]이므로 4를 의미한다.
   3. output[1][0]은 두번째 행 첫번째 열을 의미하므로 해당 칸에 **4가 들어가게 된다.

공식문서의 설명에 따라 우리는 왜 값이 달라지는지 이해했다.

![](/assets/img/Pororo/IdonKnow.png){: width="40%" height="40%"}
*이해했다...*


