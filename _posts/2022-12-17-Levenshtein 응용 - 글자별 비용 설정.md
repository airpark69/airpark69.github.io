---
title : Levenshtein 응용 - 글자별 비용 설정
date : 2022-12-17 23:31:00 +09:00
categories : [NLP, 파이썬]
tags : [Levenshtein, 동적 프로그래밍] 
---

## 글자마다 비용을 다르게 적용(User define Cost)
---

-   특정 글자를 대체 할 경우, 그에 대한 비용은 다른 글자와 다르도록 지정

```python
def levenshtein(s1, s2, cost=None, debug=False):
    if len(s1) < len(s2):
        return levenshtein(s2, s1, debug=debug)

    if len(s2) == 0:
        return len(s1)

    if cost is None:
        cost = {}

    # changed
    def substitution_cost(c1, c2):
        if c1 == c2:
            return 0
        return cost.get((c1, c2), 1)

    previous_row = range(len(s2) + 1)
    for i, c1 in enumerate(s1):
        current_row = [i + 1]
        for j, c2 in enumerate(s2):
            insertions = previous_row[j + 1] + 1
            deletions = current_row[j] + 1
            # Changed
            substitutions = previous_row[j] + substitution_cost(c1, c2)
            current_row.append(min(insertions, deletions, substitutions))

        if debug:
            print(current_row[1:])

        previous_row = current_row

    return previous_row[-1]

```

여기서 추가된 부분은

```python
def substitution_cost(c1, c2):
        if c1 == c2:
            return 0
        return cost.get((c1, c2), 1)

```

이 부분으로 cost 라는 딕셔너리 자료형에 대하여 get으로 키 값을 받아온다.

cost에는 (c1, c2)라는 튜플 형식의 key가 지정되어 있어야 하며,

get 함수 기능상 key가 없을 경우 두 번째 인자가 기본값으로 반환된다.

즉, (c1, c2) 라는 key가 있을 경우, 그에 해당하는 값을 반환하고 없으면 1을 반환한다.

```python
substitutions = previous_row[j] + substitution_cost(c1, c2)

```

위의 함수를 적용하여 substitutions 비용에 대한 계산을 변경해준다.

cost 적용 X → 결과

```python
s1 = '아이쿠야'
s2 = '아이쿵야'
levenshtein(s1, s2, debug=True)

```

```python
[0, 1, 2, 3]
[1, 0, 1, 2]
[2, 1, 1, 2]
[3, 2, 2, 1]

1

```

Cost 적용 O → 결과

```python
cost = {('쿠', '쿵'):0.1}
s1 = '아이쿠야'
s2 = '아이쿵야'
levenshtein(s1, s2, cost, debug=True)

```

```python
[0, 1, 2, 3]
[1, 0, 1, 2]
[2, 1, 0.1, 1.1]
[3, 2, 1.1, 0.1]

0.1

```

## 이용되는 부분

### 대화 데이터 서비스

-   일반적인 문자와 다르게 “대화” 에는 발음에 따라서 음운변동이 생긴다.
-   초성이 된소리가 되는 경우 (우리말 표준발음법에 ‘-ㄹ’ 뒤에 연결되는 ‘ㄱ, ㄷ, ㅂ, ㅅ, ㅈ’은 된소리로 발음한다는 규정)

→ 더 열심히 할게요 (할께요 라고 발음됨)

-   종성에 ‘ㅇ’ 받침이 추가되는 경우

→ 뭐행? (뭐해 - 뭐행) 같은 대화체

-   종성에 ‘ㅁ’ 받침이 추가되는 경우(음슴체)

→ 뭐함? 저기있음

**

> 의미는 같으나 단어 자체가 다른 경우 비용을 줄여서 최소 비용을 낮게 설정할 수 있다.

**

출처 : https://lovit.github.io/nlp/2018/08/28/levenshtein_hangle/
