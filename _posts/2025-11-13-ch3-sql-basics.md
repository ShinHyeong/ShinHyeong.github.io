---
title:  "Ch3. SQL 기본"
categories: sqld
#redirect_from: #이전주소 입력
#search: false #만약 이 글이 검색되지 않기를 바란다면
#use_math: true #수식이 필요한 경우만 사용
---

# 3.1. 관계형 데이터베이스 개요

- **E.F. Codd** 박사의 **정규화** 이론에 따라 **데이터 일관성** 문제를 **근본적 해결**한 DB 시스템
- 2차원 테이블 : 행(Row) + 열(Column)
    - 스키마 → 칼럼 헤더
    - 필드(속성) → 열(Column)
    - 레코드(튜플) → 행(Row)
- SQL이라는 공통 질의 언어를 정의해 데이터 조회, 가공, 추출 가능

---

# 3.3. 헷갈리는 함수 정리

## 3.3.1. TRIM : 공백/문자열 제거

```sql
TRIM ( [[LEADING/TRAILING/**BOTH**] [문자열/**공백**] FROM] [컬럼명] )
```

- `LTRIM`, `RTRIM` 도 비슷하게

## 3.3.4. 변환함수

- 암시적 형변환 : 그 결과를 명확하게 예측할 수 없을 때가 많음, 성능저하, 에러발생
- 따라서 가급적 명시적 형변환을 사용한다

## 3.3.5. NULL 관련함수

### 1) `NVL`(=`ISNULL`) : NULL인 데이터는 어떤 값으로 처리할지

```sql
NVL( [칼럼], [바꿀 데이터] )
```

- 1번째 인자 ≠ NULL : 그 인자 그대로 반환
- 1번째 인자 = NULL : 2번째 인자
- ex. NVL(COMM, 0) : COMM이 NULL인 것들은 0 처리

### 2) `NULLIF` : isSame(칼럼1, 칼럼2)→ 같으면 NULL, 다르면 칼럼1 리턴

```sql
NULLIF( [칼럼1], [칼럼2] )
```

- 칼럼1=칼럼2 : return NULL
- 칼럼1≠칼럼2 : return 칼럼1

### 3) `COALESCE` : NOT NULL인 첫 인자 반환

```sql
COALESCE( [칼럼1], [칼럼2], [칼럼3], ... )
```

## 3.3.6. `CASE` (=`DECODE`)

```sql
CASE [칼럼]
WHEN [조건1] THEN [바꿀값A]
WHEN [조건2] THEN [바꿀값B]
WHEN [조건3] THEN [바꿀값C]
ELSE [바꿀값N]
END
```

```sql
DECODE( [칼럼], [조건1], [바꿀값A], [조건2], [바꿀값B], [조건3], [바꿀값C], ... )
```