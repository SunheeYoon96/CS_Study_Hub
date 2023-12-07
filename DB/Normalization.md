# 목표

데이터베이스 정규화에 대해 설명할 수 있는 능력을 갖자

이제 도부이결다조는 없다

## 예상 질문

- 제 1, 2, 3 정규화에 대해서 설명해주세요
- 정규화의 장단점에 대해서 설명해주세요

## 제 1 정규형

### 한 칸엔 하나의 데이터만

-싸피 테이블-

| 싸피번호 | 싸피이름 | todayAlgo |
| -------- | -------- | --------- |
| 1        | 성섭     | 실버3     |
| 2        | 선히     | 실버2     |
| 3        | 민섭     | 실버3     |
| 4        | 경       | 골드4     |
| 5        | 건       | 브론즈4   |

Q. 이 테이블에서, 만약 선히가 골드5 문제를 풀었다. 데이터를 어떻게 추가할래?

`**BAD**`

| 2   | 선히 | 실버2, 골드5 |
| --- | ---- | ------------ |

`**GOOD**`

| 2   | 선히 | 실버2 |
| --- | ---- | ----- |
| 2   | 선히 | 골드5 |

- BAD의 경우 단점
  1. 실버2 푼사람 찾기가 귀찮아짐
     - where todayAlgo = ‘실버2’ 이렇게 찾는게 아니라 where todayAlgo like ‘%실버2%’ 이렇게 고생할수도있음 성능저하 우려된다.
  2. todayAlgo 수정도 어려워지겠죠

따라서 GOOD CASE를 채택한다.

여기서 나온다. **한 칸엔 하나의 데이터만**

다른 말로, 복합 애트리뷰트, 다중값 애트리뷰트, 중첩 릴레이션 등 비 원자적인 애트리뷰트들을 허용하지 않는 릴레이션 형태라고 하는데 어떻게 기억하겠습니까 한칸에 하나만 저장한다고 기억하겠습니다 저는..

- 이렇게 만들어진 테이블을 **제 1정규형 테이블** 이라고 한다.

## 제 2 정규형

### 현재 테이블의 주제와 관련없는 컬럼을 다른 테이블로 빼버림

| 싸피번호 | 싸피이름 | todayAlgo | 획득경험치 |
| -------- | -------- | --------- | ---------- |
| 1        | 성섭     | 실버3     | s3         |
| 2        | 선히     | 실버2     | s2         |
| 3        | 민섭     | 실버3     | s3         |
| 4        | 경       | 골드4     | g4         |
| 5        | 건       | 브론즈4   | b4         |

Q. 만약에 실버3을 풀었을 때 s3가 아니고 ss3만큼의 경험치로 바꾸고 싶으면 어떻게하나요?

A. 그냥 다 s3찾아서 ss3로 바꿔도 되겠지만, 비효율적이잖아요? 그 방법을 이제 설명하겠습니다

싸피 테이블에, 몇 경험치를 얻든 크게 상관이 없다. todayAlgo에 종속되어있는 컬럼이다. 싸피번호, 싸피이름에는 독립적이다.

![Untitled](<images/Untitled%20(10).png>)

이미지처럼 todayAlgo를 싸피테이블과 경험치테이블을 연결하는 외래키로 설정할 수 있겠다.

싸피 테이블

| 싸피번호 | 싸피이름 | todayAlgo |
| -------- | -------- | --------- |
| 1        | 성섭     | 실버3     |
| 2        | 선히     | 실버2     |
| 3        | 민섭     | 실버3     |
| 4        | 경       | 골드4     |
| 5        | 건       | 브론즈4   |

경험치 테이블

| todayAlgo | 획득경험치 |
| --------- | ---------- |
| 실버3     | s3         |
| 실버2     | s2         |
| 골드4     | g4         |
| 브론즈4   | b4         |

> 이 작업을 통해, 경험치테이블의 데이터를 하나 바꾸면, 효율적으로 수정할 수 있게 되었다.

- 참고로, 제 1 정규화를 만족해야만 한다.
- 이렇게 만들어진 테이블을 **제 2정규형 테이블** 이라고 한다.

당연히 단점도 바로 나오겠죠?

- Q. 그래서 실버3경험치가 얼마에요?
- A. 음.. 잠시만요, 실버3 테이블보고, 외래키로 따라가서보면.. s3군요.

- 구조가 복잡해진다
- 조인 연산이 증가한다.
- 주의 : 시간에 대해서는 조심하자
  - 데이터를 최적화시켜서 빨라졌을수도 있고 많은 join연산으로 느려질수도있다.
  - 많은 조인이 발생한경우 **반정규화**에 대해 설명할 수 있다면, 시간과 관련된 이야기를 해도 좋을 것 같다.

## 제 3 정규형

### 일반 컬럼에만 종속된 컬럼은 다른 테이블로 뺀다.

미완 ,,