---
title : 파이썬 신뢰구간 구하기, Python confidence Interval
date : 2022-10-21 15:58:00 +09:00
categories : [IT개발, 통계학, 파이썬]
tags : [파이썬, 신뢰구간, python, confidence interval, 파이썬 신뢰구간] 
---
```python
import numpy as np
import math

def  confidence_95(i):

upper_cut = i.mean() + (1.96 * (np.std(inp) / math.sqrt(len(inp))))

lower_cut = inp.mean() - (1.96 * (np.std(inp) / math.sqrt(len(inp))))

return lower_cut, upper_cut
```
> 함수를 정의해서 직접 신뢰구간 95% 구간 공식을 통해 출력

numpy의 np.std()는 표준편차를 구하는 함수.

math.sqrt()는 받은 인자의 제곱근을 리턴한다.

```python
import numpy as np
import scipy.stats

def mean_confidence_interval(data, confidence=0.95):
    a = 1.0 * np.array(data)
    n = len(a)
    m, se = np.mean(a), scipy.stats.sem(a)
    h = se * scipy.stats.t.ppf((1 + confidence) / 2., n-1)
    return m, m-h, m+h
```

> 직접 함수를 정의해서 신뢰구간을 리턴하는 방식2

[scipy.stats.sem](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.sem.html) 함수는 평균의 표준 오차를 구해준다.

[scipy.stats.t.ppf](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.t.html) 함수는 퍼센트 포인트 함수로, 누적분포함수의 역함수이다.
0.975%에 해당하는 위치의 Z값(1.96)을 구해준다.





```python
import numpy as np, scipy.stats as st

st.t.interval(0.95, len(a)-1, loc=np.mean(a), scale=st.sem(a))
```

> scipy.stats.t.interval을 사용하는 조금 더 간단한 방식

scipy.stats.t.interval() 의 경우 매개변수로

신뢰구간 비율, 자유도(샘플사이즈 -1), 평균,  평균의 표준오차 를 받는다. 


```python
import statsmodels.stats.api as sms

sms.DescrStatsW(a).tconfint_mean()
```
> 가장 간단하다.

sms.DescrStatsW(Data).tconfint_mean()를 사용하는데

매개변수로 confidence를 받는데, 기본값으로는 0.05가 들어있다. 




