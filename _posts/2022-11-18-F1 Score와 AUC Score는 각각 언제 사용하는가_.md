---
title : F1 Score와 AUC Score는 각각 언제 사용하는가?
date : 2022-11-18 17:00:00 +09:00
categories : [통계학, 파이썬]
tags : [F1 Score, AUC] 
---

# F1 score
---

![Fbeta](https://user-images.githubusercontent.com/50907018/202724849-85a571ac-fc49-41c8-b9a5-179cf2b4f1d3.png)

정확도(precision)와 재현율(recall)을 조화평균으로 합친 값.

Beta에 해당하는 값은 recall에 대한 가중치와 같고 recall을 더 신경쓸수록 값을 높이면 된다.

```python
from sklearn.metrics import f1_score

y_pred_pos = y_pred > threshold # threshold는 임계점이다.
f1_score(y_true, y_pred_pos)
```

파이썬 sklearn.metrics를 이용하여 쉽게 구현할 수 있다.

F1 Score는 임계점에 따라서 다른 값이 나올 수 있다. 

## 언제 사용해야 할까?
---

-   Positive 집단에 집중하는 대부분의 이진 분류 문제에서 사용됩니다.

-  확실한 수치로 나타나는 결정 인자를 제공하기 때문에 **비즈니스 이해관계자를 설득하기에 좋습니다**. 머신 러닝의 핵심은 비즈니스 문제를 해결하는 것입니다.



# ROC AUC
---
![ROCcurve](https://user-images.githubusercontent.com/50907018/202729956-ab5430bd-7916-40fd-9bb9-806a6a68d9f5.png)

TPR과 FPR을 토대로 한 그래프.

F1 Score가 특정 임계점을 토대로 한 지표라면 

ROC의 선은 가능한 모든 임계점을 그래프 상에서 나타낸 것이다.

좌상단으로 곡선이 더 휘어질수록 좋은 성능을 가졌다고 판단한다.

그러나, 문제는 위 그래프의 검은선과 녹색선같은 경우에는 직관적으로 어느 것이 더 나은지 알아보기 힘들다.

이에 따라서 **AUC Score**를 활용한다.

이는 ROC 곡선의 하단 면적과 같다.

```python
from sklearn.metrics import roc_auc_score

roc_auc = roc_auc_score(y_true, y_pred_pos)
```

이 또한 파이썬으로 쉽게 출력 가능하다.

## 언제 사용해야 할까?
---

-   **예측치에 대한 순위를 매기는 것**에 집중하고 보정된 확률에 신경쓰지 않을 때 사용합니다.

(F1 Score와 비슷한 용도로 사용되지만, 상황에 따라 다르게 됩니다.)
-   **데이터의 불균형이 상당할 때는 사용하지 않습니다.** 고도로 불균형한 데이터셋에 대한 FPR은 다수의 TN으로 인해 낮아집니다.

-   Positive 집단과 Nagaive 집단에 대한 관심도가 비슷할 때 사용합니다.



출처 : https://neptune.ai/blog/f1-score-accuracy-roc-auc-pr-auc

더 많은 인사이트가 출처에 있습니다.
