---
title : CrossValidation, holdout, K-fold
date : 2023-01-11 18:15:00 +09:00
categories : [머신러닝, 파이썬]
tags : [CV, CrossValidation, HoldOut, K-fold] 
---


# CrossValidation(CV)
---

- 데이터셋을 나눠서 훈련과 테스트를 실시하여 과적합을 피하는 기법

- 데이터셋이 큰 경우와 작은 경우 방법이 나뉘게 된다.

- CV는 **시계열 데이터**에는 적합한 방법이 아니다.





## 데이터셋이 큰 경우
- 사용하는 모델이 훈련하기 위한 데이터가 충분할 경우, 데이터셋이 크다고 할 수 있다.


### Hold-out 기법 사용

- **훈련/검증/테스트 세트** 로 나눠서 사용

- 통상적으로 6 : 2 : 2 정도의 비율

- 데이터셋이 훈련에 충분해야함

```python
from sklearn.model_selection import train_test_split
```
위 라이브러리를 활용하면 쉽게 구현 가능


---

## 데이터셋이 작은 경우

- 보통 수 천개 정도의 데이터셋이거나, 모델이 학습하기에 데이터가 부족한 경우를 말한다.


### K-fold 기법 사용

![](https://camo.githubusercontent.com/1cb4da520a32f6800363ab103ae8fee4ea96a8993dfdf23e80caf77b16ba08d3/68747470733a2f2f73656261737469616e72617363686b612e636f6d2f696d616765732f626c6f672f323031362f6d6f64656c2d6576616c756174696f6e2d73656c656374696f6e2d70617274332f6b666f6c642e706e67)

- 데이터셋을 훈련/테스트 데이터로 분리한 뒤

- 훈련 데이터를 K 개로 나눠서 평가 데이터와 훈련 데이터를 바꿔가며 모델의 결과를 보는 방법

```python
from sklearn.model_selection import cross_val_score
```

위 라이브러리를 사용하거나, 특정 모델에는 자체적으로 CV 기능이 있는 경우가 있다.
