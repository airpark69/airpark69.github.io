---
title : online_incremental_learning으로 해커톤대회에서의 문제점 해결-1
date : 2023-02-01 19:11:00 +09:00
categories : [머신러닝, 파이썬]
tags : [online learning, incremental learning, **continual learning**, lifelong learning] 
---

# 앞선 문제~

---

- 다중분류에서 향후 추가될 수 있을 class에 대한 대응 방안이 필요했다

- 이에 대한 대책으로 회귀 문제로의 변환을 생각해보았으나, NLP 문제를 회귀로 접근하게 될 경우 숫자로 변환한 라벨을 다시 자연어로 변환할 때의 문제점이 꽤나 복잡하다
> 이쪽으로 향후에 더 공부할 기회가 있으면 좋을 것 같다

---

## 해결 방안

- 딱 키워드가 생각나지 않던 도중에 이런저런 기법들을 공부하다가 발견하게 되었다.


### **Lifelong Learning** / **Continual learning**


위의 키워드로 관련 기법을 찾아볼 수 있고, 간단히 요약하자면 하나의 모델이 계속해서 학습하며 **추가적인 Task와 Class를 반영할 수 있도록 하는 것**이다.

전이학습에서 포인트를 잡았던 모델의 "재사용" 에 대한 부분과 비슷하다고 생각할 수도 있다.

### 전이학습의 문제점

- Catastrophic forgetting (Semantic Draft)

- 다른 Task를 위해서 가중치를 바꾸다보면 Fine Tuning을 위해서 상관관계에 대한 이해가 없기 때문에 기존의 Task를 제대로 수행하지 못하게 된다는 것이다

### 이에 대한 해결책이 Continual learning

---

### 표준화를 ML으로 접근한다는 방법에 대한 해결책

결과적으로 표준화 결과에 대한 다중분류의 접근에서 문제가 될 수 있는 부분 중 하나에 대한 해결책은 찾게 된 것이다. (필연적인 Class의 추가)

다른 하나의 문제점은 해당 모델이 예측한 출력값이 다른 모델의 입력값으로 사용되었을 때의 결과를 보장할 수 없다는 것이다.

- 이 부분에 대해서는 대략적으로 모델 내의 워크플로우가 입력값 -> 표준화 Task -> 목표   Task -> 출력값 으로 하나로 통일된 형태를 시도해보면 좋을 것 같았다.

- 역전파의 가중치 업데이트가 표준화 Task와 목표 Task에 대해서 둘 다 적용된다면 어떨까? 이러한 연구도 있는지 찾아봐야 할 것 같다.


# Keras와 **Creme**을 활용한 incremental learning

---

### 해당 과정의 목표점

- **Keras + pre-trained CNNs** 을 사용하여  이미지 데이터셋에서 특성 추출

- **Creme**을 활용하여 RAM이 감당하기 힘든 크기의 데이터셋을 이용하기

- 추출된 특성이 메모리에 비해서 너무 커도 훈련시킬 수 있고, 그 중 일부만 사용해도 정확도가 높을 수 있다는 것을 확인하기



### 사용 데이터셋

- [Kaggle’s Dogs vs. Cats dataset](https://www.kaggle.com/c/dogs-vs-cats)
---

## 해당 기법의 사용 이유

![](https://b2633864.smushcdn.com/2633864/wp-content/uploads/2019/06/incremental_learning_vis.jpg?lossy=1&strip=1&webp=1)

> incremental learning의 원리를 이미지로 나타낸 것

image data, text data, audio data, or numerical/categorical data 중 그 어떤 것을 사용한다고 해도 결국 데이터셋의 크기로 인하여 문제가 일어날 가능성이 높다

이에 따라

- Amazon, NewEgg 등등 을 사용

- RAM을 업그레이드

- [AWS](https://pyimagesearch.com/2017/09/20/pre-configured-amazon-aws-deep-learning-ami-with-python/) or [Azure](https://pyimagesearch.com/2018/03/21/my-review-of-microsofts-deep-learning-virtual-machine/)같은 클라우드 서비스를 이용

하는 정도의 방법이 있다.

이러한 방법들은 충분히 합리적이지만 가장 먼저 시도해볼 방법이 바로 incremental learning 이다.

### 다음 게시물에서 계속~

## 참고자료


https://pyimagesearch.com/2019/06/17/online-incremental-learning-with-keras-and-creme/
