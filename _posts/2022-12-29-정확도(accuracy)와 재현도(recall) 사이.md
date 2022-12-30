---
title : 정밀도(Precision)와 재현율(recall) 사이
date : 2022-12-29 15:39:00 +09:00
categories : [머신러닝, 파이썬]
tags : [accuracy, recall, 분류 문제, 평가지표] 
---

# 분류 문제 평가지표
---
- 대표적으로 Accuracy, Precision, Recall, F1, ROC-AUC 정도가 있다.
-  Accuracy, Precision, Recall, F1 다중분류 가능
- ROC-AUC 이진분류만 가능


## Accuracy(정확도)

- 전체 범주를 모두 바르게 맞춘 경우를 전체 수로 나눈 값

- (TP + TN) / Total
- 이것만 사용해서 평가 지표를 나타내는 경우도 있다.

## Precision (정밀도)

- **Positive로 예측**한 경우 중 올바르게 Positive를 맞춘 비율

- TP / (TP + FP)
- 무죄추정의 원칙
-> 확실한 증거 없이는 용의자를 범인이 아니라고 간주해야한다. 이럴 경우 정밀도가 중요해짐


## Recall (재현율)

- **실제 Positive**인 것 중 올바르게 Positive를 맞춘 것의 비율

- TP / (TP + FN)

- 코로나 진단 분류
-> 실제 환자를 놓치지 않고 격리를 시켜야 전염을 막기 때문에 재현율이 높아야 한다.



## F1-Score

- 정밀도와 재현율의 조화평균(harmonic mean)

> **왜 조화평균을 사용할까?**   
> 만약 산술평균으로 계산할 경우, 어느 한 쪽이 높은 값을 가지면 다른 한 쪽이 낮은 값을 가지게 되더라도 높은 쪽의 영향을 크게 받으므로 산술평균 값은 비교적 큰 값을 갖기 때문입니다. 즉, 값이 왜곡됩니다.

 반면에
> **조화평균은 어느 한 쪽이 낮으면, 값도 크게 낮아집니다.****

출처 : https://moons08.github.io/datascience/classification_score_basic/

- 정밀도와 재현율 둘 다 중요하기 때문에 사용되는 지표

- beta에 따라서 F2, F3로 다르게 사용 가능
-> beta 값이 높아질수록 Recall에 대한 가중치를 높게 둔다.
-> Recall이 높을수록 높은 점수를 받게 됨


## ROC - AUC(Receiver Operating Characteristic, Area Under the Curve)

![https://velog.velcdn.com/images%2Fhhhs101%2Fpost%2Fc1f8d734-8ef9-438f-bdaa-e697b8541f39%2Fimage.png](https://velog.velcdn.com/images%2Fhhhs101%2Fpost%2Fc1f8d734-8ef9-438f-bdaa-e697b8541f39%2Fimage.png)

- 여러 임계값에 대한 TPR(True Positive Rate, recall)과 FPR(False Positive Rate) 그래프

- 임계값에 따라서 정밀도와 재현율이 달라진다는 부분에서 이에 따라 이것을 고려한 성능 지표가 바로 ROC-AUC

### 유의할 부분
---

- ROC curve는 이진분류문제에서 사용

- 다중분류문제에서는 각 클래스를 이진클래스 분류문제로 변환(One Vs All)하여 사용

- 3-class(A, B, C) 문제 -> A vs (B,C), B vs (A,C), C vs (A,B) 로 나누어 수행



## 그래서 뭣이 중헌디?

---

![뭣이중헌디](https://user-images.githubusercontent.com/50907018/209842435-f2dc7667-5d10-4419-bf3f-77299389ac11.png)


아쉽게도 정답은 없다.

상황마다 다르기 때문이다.

다만, F-beta 에서 beta에 대한 가중치가 있다는 점을 기억하고 정밀도나 재현율 둘 중 어떤 점을 더 강조해야 할 지 고민하는 과정이 필요할 것이다.


