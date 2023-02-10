---
title : AutoEncoder와 Semi-Supervised Learning을 통한 분류
date : 2023-02-10 11:05:00 +09:00
categories : [머신러닝, 파이썬]
tags : [AE, AutoEncoder, 특성추출, Semi-Supervised Learning] 
---

# AutoEncoder
---

![Building Autoencoders in Keras](https://blog.keras.io/img/ae/autoencoder_schema.jpg)

상세한 내용은 검색할 경우 굉장히 많이 나온다.

요약하자면, 원본 이미지의 특성을 보존하여 입력값을 해당 특성에 맞게 변환하는 것이다.

## 활용 방안

---

- 이상 탐지

- 차원 축소

등이 있는데, 여기서는 차원 축소에 대해서만 집중해볼 것이다.


## 차원 축소

---

- 불필요한 특성을 제거함으로 차원의 저주를 해결하며 학습 시간이나 모델 성능을 좋게 만드는 방법

- 주의할 점은 중요한 특성에 대한 구분 기준에서 인간이 담당할 경우 실수할 확률이 있다는 것이다.


AutoEncoder의 강점은 원본에 대한 복원이며, 이에 따라서 여러 학습 데이터셋들에서 추출한 해당 Class의 특성을 추출하게 된다.

잘 학습된 AE에서 Encoder만을 사용할 경우, 입력값이 해당 가중치를 통과하면 필요한 특성들에 대한 값으로 출력된다.

즉, PCA같은 용도로 사용할 수 있는 것이다.

## **Semi-Supervised Learning**

- AE를 활용하여 **Unsupervised Learning**을 통해 데이터셋을 구분, Label을 지정

- Classifier를 통하여 해당 데이터셋을 학습시켜 **Supervised Learning**


## Kaggle 예제

https://www.kaggle.com/code/shivamb/semi-supervised-classification-using-autoencoders/notebook


>해당 방법이 효과적일 수 있는 몇 가지 조건은 기억해두자

- Label 1에 해당하는 값이 **0.17 %**에 불과한 극도로 불균형한 데이터셋

-> 지도학습으로는 언더샘플링을 시도해야 하는데, 이 경우 정확도가 떨어짐

![](https://www.kaggleusercontent.com/kf/9665973/eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI4Q0JDLUhTMjU2In0..UD5f-UaXGGw2xckNtG6BZQ.vbYBf07TlvuzzK-65h7LXHmXHYGrmwyoFkluF0baXE1YaMV-eeOM93eytwLu6cEjBOSmi5UizGlyIEYBEmooeRKUV-EUqtNeCoSnvbs7d63j-Tw5KwbbXJfkIXjo0HruoGH4ulA0sRYqDZMBviPl3t7CIwMulBA4Jug0qfCMDp0fgK7EPbv9ccYbTrcVCF65Wzczfm5kBOn1MFB8FLMwDJ0ST1mlCPeR0fIZMFqrjuwyUQOeKjty_DkgeXcICFPqpVWvvTSLgiwhWenMjzuhdUDG6ePfzhhbPrxuzAOzAgXkA3RNHw-cBxupt-4hVAnu5Zf7Du_uGrpqG33gu6PTNN1UA2tCxNhAo0UapOkIi8kbskSaDJFJEHKrkiNOhjUouHWCuPtSrqvzSdYGebEzgwgoy4WCq-jv1-tv1PBnuiicDXwsnWvFKTEiIkBth9qf9f1_Ei4QhgtxTPOUWqcKA46bTJZtdROjVMyuTuYKvcwgCbFapk1KcYr--yn9DGSl331lWORnrtLosD3OdWhRudxvHWC-_mObarJDpdEnZ84I4wJ4GdwrgJvtzRNPpuscjrd5uJud1exhZ0wcUQA3MIhEztuHU35uLfZWDFnlosK2UL8DG-UU2yHT829TwNWcSmrQ5VIBKopYxbYebh5WgXPhPDosfnXEE4m_L7iEPEdLp1Gff8s60yUwlO4uqjQ_.9ittR-pC5bvDRW2taHe2Dw/__results___files/__results___7_0.png)

- Class 간의 유사도가 상당히 큰 문제 + 이진분류

-> T-SNE를 통한 시각화 결과만 봐도 비슷한 관계에 해당하는 데이터가 많다는게 보인다(우측 하단 꼬리 부분)


-> 이유는 단순한데, AutoEncoder 특성상 여러 Class를 학습시킬 수는 없다. 학습 과정에서도 Label값을 따로 넣어주지 않기 때문



## 생각할 점

---

데이터의 관계, 문제의 종류에 따라서 더 효율적인, 더 효과적인 접근 방법이 다양하게 존재한다.

해당 문제에 대한 전문가가 되어서 모델을 직접 설계하고 이론을 만들어내는 사람도 대단하다.

하지만, 현실에서의 여러 문제에 대해서 실질적으로 해결 방안을 내놓으려면 다양한 방법들을 활용하는 사람도 필요할 것이다.
