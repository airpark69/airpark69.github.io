---
title : Levenshtein 응용 - 음운 차이를 기준으로
date : 2022-12-18 00:27:00 +09:00
categories : [NLP, 파이썬]
tags : [Levenshtein, 동적 프로그래밍, 음운] 
---


## 한글의 초/중/종성(음운) 분리 이후 적용

### 한글의 Encoding

-   컴퓨터에는 각 글자에 대한 숫자가 정의되어 있으며 이를 Encoding이라 부른다

```python
for char in 'azAZ가힣ㄱㄴㅎㅏ':
    print('{} == {}'.format(char, ord(char)))

```

ord 함수는 해당 글자의 인코딩을 출력해준다.

결과 :

```python
a == 97
z == 122
A == 65
Z == 90
가 == 44032
힣 == 55203
ㄱ == 12593
ㄴ == 12596
ㅎ == 12622
ㅏ == 12623

```

```python
for idx in [97, 122, 65, 90, 44032, 55203]:
    print('{} == {}'.format(idx, chr(idx)))

```

반대로 chr 함수는 인코딩을 토대로 글자로 다시 변환해주는 역할을 한다.

결과 :

```python
97 == a
122 == z
65 == A
90 == Z
44032 == 가
55203 == 힣

```

다행히도, 한글에는 합성 규칙이 있으며 이에 따라서 초/중/종성의 인코딩 계산이 가능하다.

-   파이썬으로 구현한 코드

```python
kor_begin = 44032
kor_end = 55203
chosung_base = 588
jungsung_base = 28
jaum_begin = 12593
jaum_end = 12622
moum_begin = 12623
moum_end = 12643

chosung_list = [ 'ㄱ', 'ㄲ', 'ㄴ', 'ㄷ', 'ㄸ', 'ㄹ', 'ㅁ', 'ㅂ', 'ㅃ', 
        'ㅅ', 'ㅆ', 'ㅇ' , 'ㅈ', 'ㅉ', 'ㅊ', 'ㅋ', 'ㅌ', 'ㅍ', 'ㅎ']

jungsung_list = ['ㅏ', 'ㅐ', 'ㅑ', 'ㅒ', 'ㅓ', 'ㅔ', 
        'ㅕ', 'ㅖ', 'ㅗ', 'ㅘ', 'ㅙ', 'ㅚ', 
        'ㅛ', 'ㅜ', 'ㅝ', 'ㅞ', 'ㅟ', 'ㅠ', 
        'ㅡ', 'ㅢ', 'ㅣ']

jongsung_list = [
    ' ', 'ㄱ', 'ㄲ', 'ㄳ', 'ㄴ', 'ㄵ', 'ㄶ', 'ㄷ',
        'ㄹ', 'ㄺ', 'ㄻ', 'ㄼ', 'ㄽ', 'ㄾ', 'ㄿ', 'ㅀ', 
        'ㅁ', 'ㅂ', 'ㅄ', 'ㅅ', 'ㅆ', 'ㅇ', 'ㅈ', 'ㅊ', 
        'ㅋ', 'ㅌ', 'ㅍ', 'ㅎ']

jaum_list = ['ㄱ', 'ㄲ', 'ㄳ', 'ㄴ', 'ㄵ', 'ㄶ', 'ㄷ', 'ㄸ', 'ㄹ', 
              'ㄺ', 'ㄻ', 'ㄼ', 'ㄽ', 'ㄾ', 'ㄿ', 'ㅀ', 'ㅁ', 'ㅂ', 
              'ㅃ', 'ㅄ', 'ㅅ', 'ㅆ', 'ㅇ', 'ㅈ', 'ㅉ', 'ㅊ', 'ㅋ', 'ㅌ', 'ㅍ', 'ㅎ']

moum_list = ['ㅏ', 'ㅐ', 'ㅑ', 'ㅒ', 'ㅓ', 'ㅔ', 'ㅕ', 'ㅖ', 'ㅗ', 'ㅘ', 
              'ㅙ', 'ㅚ', 'ㅛ', 'ㅜ', 'ㅝ', 'ㅞ', 'ㅟ', 'ㅠ', 'ㅡ', 'ㅢ', 'ㅣ']

def compose(chosung, jungsung, jongsung):
    char = chr(
        kor_begin +
        chosung_base * chosung_list.index(chosung) +
        jungsung_base * jungsung_list.index(jungsung) +
        jongsung_list.index(jongsung)
    )
    return char

def decompose(c):
    if not character_is_korean(c):
        return None
    i = ord(c)
    if (jaum_begin <= i <= jaum_end):
        return (c, ' ', ' ')
    if (moum_begin <= i <= moum_end):
        return (' ', c, ' ')

    # decomposition rule
    i -= kor_begin
    cho  = i // chosung_base
    jung = ( i - cho * chosung_base ) // jungsung_base 
    jong = ( i - cho * chosung_base - jung * jungsung_base )    
    return (chosung_list[cho], jungsung_list[jung], jongsung_list[jong])

def character_is_korean(c):
    i = ord(c)
    return ((kor_begin <= i <= kor_end) or
            (jaum_begin <= i <= jaum_end) or
            (moum_begin <= i <= moum_end))

```

위 코드는 합성 / 분해를 구현한 코드로

여기서 분해 부분을 살펴보자면

```python
 # decomposition rule
    i -= kor_begin
    cho  = i // chosung_base
    jung = ( i - cho * chosung_base ) // jungsung_base 
    jong = ( i - cho * chosung_base - jung * jungsung_base )  

```

-   초/중/종성으로 분해하기 위해서는 ord 로 글자를 숫자로 변형한 뒤, 완전 한글의 시작값, 44032 를 빼줍니다. 이 값을 A 라고 한다면
-   A를 초성의 기본값 (588)으로 나눠주면 그 몫이 초성 list의 index가 되고, 그 나머지를 B라고 합니다
-   B를 중성의 기본값 (28) 로 나눠주면 그 몫이 중성 list 의 index 가 되고, 그 나머지를 C라고 합니다.
-   C가 종성 list의 index가 됩니다.
-   합성은 이 과정의 역순입니다.

## 이를 활용하여 ****초/중/종성 분리를 적용한 Levenshtein distance****

```python
def jamo_levenshtein(s1, s2, debug=False):
    if len(s1) < len(s2):
        return jamo_levenshtein(s2, s1, debug)

    if len(s2) == 0:
        return len(s1)

    def substitution_cost(c1, c2):
        if c1 == c2:
            return 0
        return levenshtein(decompose(c1), decompose(c2))/3

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
            print(['%.3f'%v for v in current_row[1:]])

        previous_row = current_row

    return previous_row[-1]

```

substitution_cost 부분을 살펴보면 초/중/종성의 비교를 적용했다는 것을 알 수 있습니다.

```python
def substitution_cost(c1, c2):
        if c1 == c2:
            return 0
        return levenshtein(decompose(c1), decompose(c2))/3

```

-   글자의 substituion 과정에서 다를 경우에만 초/중/종성의 비교를 통하여 비용을 계산하고 이를 3으로 나눠줍니다.
-   이는 수정 비용의 기본값이 1인 상황에서 초/중/종성의 비교는 음운 하나하나를 글자로 가정하고 levenshtein 계산을 하기 때문에 0~3의 비용이 나오기 때문입니다.
-   따라서 개별 단어의 비용을 다르게 할 경우 이 수치도 조정을 해줘야 합니다.

예시

```python
s1 = '호랑나비'
s2 = '호랑나빙'
print(jamo_levenshtein(s1, s2, debug=True))

```

결과:

```python
['0.000', '1.000', '2.000', '3.000']
['1.000', '0.000', '1.000', '2.000']
['2.000', '1.000', '0.000', '1.000']
['3.000', '2.000', '1.000', '0.333']
0.3333333333333333

```

호랑나비 → 호랑나빙 비교에서 비와 빙의 수정 비용이 0.333이 나왔음을 알 수 있습니다.

음운 하나의 차이를 글자 전체의 수정으로 인식하지 않게 되었습니다.

### 다른 방법으로 구현

```python
s1 = '호랑나비'
s2 = '호랑나빙'

s1_ = ''.join([comp for c in s1 for comp in decompose(c)])
s2_ = ''.join([comp for c in s2 for comp in decompose(c)])

print(s1_) 
print(s2_) 
print(levenshtein(s1_, s2_)/3)

```

결과:

```python
ㅎㅗ ㄹㅏㅇㄴㅏ ㅂㅣ
ㅎㅗ ㄹㅏㅇㄴㅏ ㅂㅣㅇ
0.3333333333333333

```


출처 : https://lovit.github.io/nlp/2018/08/28/levenshtein_hangle/
