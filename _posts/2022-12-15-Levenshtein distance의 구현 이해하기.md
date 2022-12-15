---
title : Levenshtein distance의 구현 이해하기
date : 2022-12-15 23:10:00 +09:00
categories : [NLP, 파이썬]
tags : [Levenshtein, 동적 프로그래밍] 
---

# 단어 간 거리

## 1. 의미적 거리

-   의미 간의 유사성을 측정
-   word embedding과 같은 방법으로 학습 가능
-   Word2Vec은 의미적 유사성을 벡터로 표현, 그러나 형태적 유사성을 나타내진 못함
-   현재 활용 가능한 방법이 있으며, 성능이 좋은 편

## 유사성 지표 - cosine distance

## 2. 형태적 거리

-   말 그대로 형태적인 유사성을 측정
-   생각해볼 만한 부분이 많은 분야고, 현재 목표로 먼저 공부할 부분

## 유사성 지표 - string distance

-   Jaro-winkler, Levenshtein

→ string을 이용하여 정의

-   Hamming, Cosine, TF-IDF distance

→ string을 벡터로 표현한 후 거리를 정의


# Levenshtein distance

-   string s1 → s2로 교정하는데 드는 최소 횟수를 두 string간의 거리로 정의
-   오탈자 교정에 자주 이용됨
-   별명은 edit distance

## 알고리즘 설명

-   Delete
-   Insert
-   Substitution
-   이 중 선택되는 방법 1회당 1회의 변환을 했다고 본다.

예시)

s1 : 꿈을꾸는아이

s2 : 아이오아이

s1이 s2로 바뀌기 위한 최소 횟수는 위의 방법들을 통해서 변환하기 까지 걸리는 횟수이다.

s1의 “꿈” 을 지우고 “아” 를 입력할 경우

지우고 (Delete) 입력하기 (Insert) 라는 2회 과정을 거치기 때문에 단순히 대체하기 (Substitution)보다 비용이 많이 든다.

따라서 “꿈” 을 “아” 로 대체하는 방향이 선택된다.

### Levenshetin distance의 목표

위의 알고리즘으로 **가장 적은 비용이 드는 수정 방법을 찾는 것**


## 동적 프로그래밍 (Dynamic Programming) 을 통한 구현

-   동적프로그래밍은 전체 문제를 작은 문제의 집합으로 정의
-   작은 문제를 반복적으로 풀어서 전체 문제를 해결

```python
def levenshtein(s1, s2, debug=False):
    if len(s1) < len(s2):
        return levenshtein(s2, s1, debug)

    if len(s2) == 0:
        return len(s1)

    previous_row = range(len(s2) + 1)
    for i, c1 in enumerate(s1):

        current_row = [i + 1]
        for j, c2 in enumerate(s2):
            insertions = previous_row[j + 1] + 1
            deletions = current_row[j] + 1
            substitutions = previous_row[j] + (c1 != c2)
            current_row.append(min(insertions, deletions, substitutions))

        if debug:
            print(current_row[1:])

        previous_row = current_row

    return previous_row[-1]

```

```python
s1 = '데이터마이닝'
s2 = '데이타마닝'

print(levenshtein(s1, s2, True))

```

결과값은

[0, 1, 2, 3, 4] 
[1, 0, 1, 2, 3] 
[2, 1, 1, 2, 3] 
[3, 2, 2, 1, 2] 
[4, 3, 3, 2, 2] 
[5, 4, 4, 3, 2] 
 2

가 나오며, 이를 통해서 최소 변경횟수는 2라는 것을 알 수 있다.

위의 함수에서 유의할 점은 초기값에 해당하는 previous_row가 [0 ~ s2의 길이 + 1] 이라는 것이다.

또한 가장 첫번째 열에 해당하는 값은 1부터 시작해서 하나씩 오르는 값으로 고정된다.

[0, 1, 2, 3, 4] 
[1, 0, 1, 2, 3] 
[2, 1, 1, 2, 3] 
[3, 2, 2, 1, 2] 
[4, 3, 3, 2, 2] 
[5, 4, 4, 3, 2]

그리하여 결과값인 이것은 사실은

**[0, 1, 2, 3, 4, 5]**
**[1,** 0, 1, 2, 3, 4]
**[2,** 1, 0, 1, 2, 3] 
**[3,** 2, 1, 1, 2, 3] 
**[4,** 3, 2, 2, 1, 2] 
**[5,** 4, 3, 3, 2, 2] 
**[6**, 5, 4, 4, 3, 2]

이러한 형태로 이루어져 있으며 이 상태에서 Levenshtein distance를 계산한다.

문자에 해당하는 부분을 숫자로 치환한 표 형식이라고 볼 수 있다.

알고리즘에 따라서 좌상단에서 시작하여 오른쪽 아래의 값을 가장 최소의 비용을 구하여 넣은 뒤에 마지막으로 우상단에 위치한 값을 최소 횟수라고 판단하는게 이 Levenshtein distance를 활용한 방법의 요약이다.

이 방법이 동적 프로그래밍에 해당하는 이유는 **기존의 단어와 비교할 단어를 토대로 비용을 계산**하며 이 비용을 저장한 뒤에 순서대로 기존 단어와 비교할 단어를 묶어서 계속해서 비용을 구하는 것을 반복한다.

가령 위의 예시에서 데이터마이닝 / 데이타마닝 의 경우

1번째 루프

데 → 데 (0회 변경)

데 → 데이 (1회 삽입)

데 → 데이타 (2회 삽입)

데 → 데이타마 (3회 삽입)

데 → 데이타마닝 (4회 삽입)

01222

2번째 루프

데이 → 데 (1회 삭제)

데이 → 데이(0회 변경)

데이 → 데이타(1회 삽입)

데이 → 데이타마(2회 삽입)

데이 → 데이타마닝(3회 삽입)

10123

의 상황으로 이를 반복하여 결론적으로

마지막 루프의 마지막 행인

…

…

데이터마이→데이타마닝(2회 변경)

데이터마이닝 → 데이타마닝(2회 변경)

에 해당하는 우리가 풀고자 하는 문제까지 도달하는 방법이다.

즉, 작은 문제(단어 하나와 단어 2개의 비교) 를 통하여 큰 문제(문자열 간의 비교)에 도달하여 풀어가는 것이다.



참고 : https://lovit.github.io/nlp/2018/08/28/levenshtein_hangle/
