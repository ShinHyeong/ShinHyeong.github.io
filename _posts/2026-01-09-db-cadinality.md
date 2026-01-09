---
title:  "[DB] 카디널리티(Cardinality)란 무엇이며 왜 알아야 할까?"
categories: Database
excerpt: "인덱스를 걸었는데 왜 여전히 Full Table Scan이 발생할까요? 인덱스 설계의 핵심 지표인 카디널리티(Cardinality)의 개념과 옵티마이저가 인덱스를 외면하게 되는 원리에 대해 알아봅시다."
description: "데이터베이스 성능 최적화의 필수 개념인 카디널리티(Cardinality)에 대해 알아봅시다. 인덱스가 있음에도 Full Table Scan이 발생하는 이유를 옵티마이저 비용 계산 관점에서 이해해봅시다.(Random I/O)"
tags: [카디널리티, Cardinality, 인덱스, Full Table Scan, 옵티마이저, 성능 최적화, 데이터베이스]
#redirect_from: #이전주소 입력
#search: false #만약 이 글이 검색되지 않기를 바란다면
#use_math: true #수식이 필요한 경우만 사용
---

# 데이터가 쌓일수록 왜 조회 성능이 느려지지?

<div class="story-box" markdown="1">

수백만건 데이터 속 원하는 정보를 찾기 위해 **하염없이 기다려본** 경험이 있으신가요? 쿼리 성능 저하의 원인은 다양하지만, 대부분 <strong class="highlight-text">비효율적인 데이터 조회방식</strong>에 있습니다.

</div>

## 범인은 Full Table Scan <span class="title-sub-desc">: 그래서 우리는 인덱스를 쓴다</span>

<div class="story-box" markdown="1">

DB가 특정 데이터를 찾기위해 테이블의 모든 행을 <strong class="highlight-text">하나씩 전부 확인</strong>하는 방식입니다. 수백만 권 책이 있는 도서관에서 원하는 책 한 권을 찾기 위해 모든 책의 제목을 일일이 확인하는 것과 같습니다. 이 방식의 문제는 **데이터가 많아질수록 성능은 기하급수적으로 저하**된다는 것입니다.

이런 비효율을 해결하기 위해 우리는 **인덱스**를 사용합니다. 인덱스는 DB의 목차같은 역할을 합니다. 인덱스를 사용하면 전체를 훑지않고 데이터가 있는 위치로 <strong class="highlight-text">바로 점프</strong>할 수 있어 검색 속도를 획기적으로 높일 수 있습니다.

</div>

## 그런데 왜 옵티마이저는 여전히 Full Table Scan을 선택하지?

<div style="max-width: 800px; margin: 0 auto;">
  <div style="display: flex; flex-wrap: wrap; justify-content: center; align-items: stretch; gap: 20px; font-family: 'Pretendard', sans-serif; margin: 30px 0;">
    
    <div style="flex: 1; min-width: 300px; border: 2px solid #ffeded; border-radius: 12px; padding: 25px 20px; background-color: #fffafb; display: flex; flex-direction: column; justify-content: space-between;">
      <div>
        <div style="text-align: center; color: #e74c3c; margin-top: 0; margin-bottom: 10px; font-size: 1.17em; font-weight: bold;">Full Table Scan</div>
        <p style="font-size: 0.9em; color: #666; text-align: center; margin-bottom: 20px;">모든 데이터를 하나씩 확인 (순차 검색)</p>
        <div style="display: flex; flex-direction: column; gap: 12px;">
          <div style="border: 1px solid #ddd; padding: 8px; background: white; border-left: 5px solid #e74c3c; display: flex; align-items: center; opacity: 0.5; font-size: 0.9em;">
            <span style="margin-right: 10px;">❌</span> ID: 1 | Name: Kim (실패)
          </div>
          <div style="text-align: center; color: #e74c3c; font-weight: bold; line-height: 1;">
            <span style="font-size: 0.8em; font-weight: normal; color: #999;">(...수많은 데이터 하나씩 스캔 중...)</span><br>
          </div>
          <div style="border: 2px solid #e74c3c; padding: 8px; background: #fff5f5; border-left: 5px solid #e74c3c; display: flex; align-items: center; font-weight: bold; font-size: 0.9em; box-shadow: 0 2px 5px rgba(231, 76, 60, 0.1);">
            <span style="margin-right: 10px;">🎯</span> ID: 111 | Name: Choi (발견!)
          </div>
        </div>
      </div>
      <div style="margin-top: 25px; text-align: center; font-weight: bold; color: #e74c3c; padding-top: 15px; border-top: 1px dashed #ffcdcd;">
        시간 복잡도: O(N)
      </div>
    </div>

    <div style="flex: 1; min-width: 300px; border: 2px solid #e8f5e9; border-radius: 12px; padding: 25px 20px; background-color: #f1f8e9; display: flex; flex-direction: column; justify-content: space-between;">
      <div>
        <div style="text-align: center; color: #2e7d32; margin-top: 0; margin-bottom: 10px; font-size: 1.17em; font-weight: bold;">Index Scan</div>
        <p style="font-size: 0.9em; color: #666; text-align: center; margin-bottom: 20px;">목차를 통해 해당 위치로 바로 점프 (이진 탐색 등)</p>
        <div style="display: flex; gap: 12px; align-items: center; justify-content: center; padding-top: 10px;">
          <div style="flex: 1; border: 1px dashed #2e7d32; padding: 10px; background: white; border-radius: 4px;">
            <div style="font-size: 0.75em; font-weight: bold; text-align: center; margin-bottom: 5px; color: #2e7d32;">[INDEX]</div>
            <div style="font-size: 0.8em; background: #f1f8e9; margin-bottom: 4px; padding: 2px;">Choi → P.11</div>
            <div style="font-size: 0.8em; opacity: 0.4;">Kim → P.1</div>
            <div style="font-size: 0.8em; opacity: 0.4;">Lee → P.2</div>
          </div>
          <div style="color: #2e7d32; font-size: 1.5em;">➔</div>
          <div style="flex: 1.2;">
            <div style="border: 2px solid #2e7d32; padding: 12px; background: white; border-radius: 8px; box-shadow: 2px 2px 8px rgba(0,0,0,0.05);">
              <strong style="font-size: 0.85em; color: #2e7d32;">Page 4</strong><br/>
              <span style="font-size: 0.85em;">ID: 111<br/>Name: <b>Choi</b></span>
            </div>
          </div>
        </div>
      </div>
      <div style="margin-top: 25px; text-align: center; font-weight: bold; color: #2e7d32; padding-top: 15px; border-top: 1px dashed #c8e6c9;">
        시간 복잡도: O(log N)
      </div>
    </div>

  </div>
</div>

<div class="story-box" markdown="1">

하지만 인덱스가 만능은 아닙니다. 분명 인덱스를 걸었음에도 DBMS 옵티마이저가 이를 무시하고, **여전히 Full Table Scan**을 선택하는 경우가 있습니다.

왜 이런 일이 발생할까요? 옵티마이저는 무조건 인덱스를 타는 게 아니라, 매번 <strong class="highlight-text">비용(Cost)</strong>을 계산해서 가장 효율적인 방법을 선택하기 때문입니다.

이때 옵티마이저가 **"인덱스 쓰는게 오히려 더 느리겠는데? 그냥 다 읽자!"**라고 판단하게 만드는 결정적인 기준이 바로 <strong class="highlight-text">카디널리티(Cardinality)</strong>입니다.

</div>

<br>

# 카디널리티 (Cardinality) <span class="title-sub-desc">: 값의 고유한 정도</span>

<div class="story-box" markdown="1">

카디널리티란 특정 칼럼에 포함된 값들의 <strong class="highlight-text">고유한 정도(Uniqueness)</strong>를 의미합니다.

</div>

<div style="max-width: 800px; margin: 0 auto; font-family: 'Pretendard', sans-serif;">
  <div style="display: flex; flex-wrap: wrap; justify-content: center; align-items: stretch; gap: 20px; margin: 30px 0;">
    
    <div style="flex: 1; min-width: 300px; border: 2px solid #e3f2fd; border-radius: 12px; padding: 25px 20px; background-color: #f8fbff; display: flex; flex-direction: column; justify-content: space-between;">
      <div>
        <div style="text-align: center; color: #1565c0; margin-bottom: 5px; font-size: 1.15em; font-weight: bold;">카디널리티가 높다</div>
        <div style="text-align: center; color: #1565c0; font-size: 0.9em; margin-bottom: 20px;">(High Cardinality)</div>
        
        <div style="display: flex; align-items: flex-end; gap: 4px; height: 80px; justify-content: center; margin-bottom: 25px; border-bottom: 2px solid #e0e0e0; padding-bottom: 5px;">
          <div style="width: 12px; height: 60%; background: #54a0ff; border-radius: 2px;"></div>
          <div style="width: 12px; height: 85%; background: #5f27cd; border-radius: 2px;"></div>
          <div style="width: 12px; height: 100%; background: #1dd1a1; border-radius: 2px;"></div>
          <div style="width: 12px; height: 50%; background: #ff6b6b; border-radius: 2px;"></div>
          <div style="width: 12px; height: 90%; background: #feca57; border-radius: 2px;"></div>
          <div style="width: 12px; height: 70%; background: #48dbfb; border-radius: 2px;"></div>
          <div style="width: 12px; height: 80%; background: #ff9ff3; border-radius: 2px;"></div>
        </div>

        <div style="font-size: 0.85em; line-height: 1.6; color: #444;">
          <div style="margin-bottom: 15px; padding: 12px; background: #fff; border-radius: 8px; border: 1px solid #d1e3f8; text-align: center;">
            중복도가 낮고 값이 고유함<br>
            <strong>→ 데이터를 거의 유일하게 식별 가능한 컬럼</strong>
          </div>
          <div style="text-align: center;">
            🪪 주민등록번호 | 📧 이메일 주소 | 👤 회원 ID
          </div>
        </div>
      </div>
      <div style="margin-top: 25px; text-align: center; font-weight: bold; color: #1565c0; padding: 10px; background: #e3f2fd; border-radius: 8px; font-size: 0.9em;">
        ✅ 인덱스 효율: 매우 좋음
      </div>
    </div>

    <div style="flex: 1; min-width: 300px; border: 2px solid #f5f5f5; border-radius: 12px; padding: 25px 20px; background-color: #fafafa; display: flex; flex-direction: column; justify-content: space-between;">
      <div>
        <div style="text-align: center; color: #d32f2f; margin-bottom: 5px; font-size: 1.15em; font-weight: bold;">카디널리티가 낮다</div>
        <div style="text-align: center; color: #d32f2f; font-size: 0.9em; margin-bottom: 20px;">(Low Cardinality)</div>
        
        <div style="display: flex; align-items: flex-end; gap: 4px; height: 80px; justify-content: center; margin-bottom: 25px; border-bottom: 2px solid #e0e0e0; padding-bottom: 5px;">
          <div style="width: 12px; height: 85%; background: #1565c0; border-radius: 2px;"></div>
          <div style="width: 12px; height: 85%; background: #cfd8dc; border-radius: 2px;"></div>
          <div style="width: 12px; height: 85%; background: #1565c0; border-radius: 2px;"></div>
          <div style="width: 12px; height: 85%; background: #cfd8dc; border-radius: 2px;"></div>
          <div style="width: 12px; height: 85%; background: #1565c0; border-radius: 2px;"></div>
          <div style="width: 12px; height: 85%; background: #cfd8dc; border-radius: 2px;"></div>
          <div style="width: 12px; height: 85%; background: #1565c0; border-radius: 2px;"></div>
        </div>

        <div style="font-size: 0.85em; line-height: 1.6; color: #444;">
          <div style="margin-bottom: 15px; padding: 12px; background: #fff; border-radius: 8px; border: 1px solid #eee; text-align: center;">
            중복도가 높고 값이 제한적<br>
            <strong>→ 몇 가지 정해진 값만 반복해서 나타나는 컬럼들</strong>
          </div>
          <div style="text-align: center;">
            🚻 성별 | 💳 결제 상태 | 📦 카테고리
          </div>
        </div>
      </div>
      <div style="margin-top: 25px; text-align: center; font-weight: bold; color: #757575; padding: 10px; background: #eeeeee; border-radius: 8px; font-size: 0.9em;">
        ⚠️ 인덱스 효율: 좋지 않음
      </div>
    </div>

  </div>
  
</div>

<div class="story-box" markdown="1">

- **카디널리티가 높다** : 전체 행 개수 대비 <strong class="highlight-text">중복되지 않는</strong> 값의 개수가 많다.
    - → 데이터를 **거의 유일하게 식별**할 수 있는 컬럼들
    
- **카디널리티가 낮다** : 전체 행 개수 대비 <strong class="highlight-text">중복</strong>되는 값의 개수가 많다.
    - → 몇 가지 **정해진 값만 반복**해서 나타나는 컬럼들

</div>

<br>

# 카디널리티가 낮다 = 옵티마이저가 인덱스 선택 안 할 확률이 높다

<div style="max-width: 800px; margin: 0 auto; font-family: 'Pretendard', sans-serif;">
  <div style="display: flex; flex-wrap: wrap; justify-content: center; align-items: stretch; gap: 20px; margin: 30px 0;">
    
    <div style="flex: 1; min-width: 340px; border: 2px solid #e8f5e9; border-radius: 12px; padding: 25px 20px; background-color: #f1f8e9; display: flex; flex-direction: column; justify-content: space-between;">
      <div>
        <div style="text-align: center; color: #2e7d32; margin-bottom: 5px; font-size: 1.15em; font-weight: bold;">높은 카디널리티 인덱스</div>
        <div style="text-align: center; color: #666; font-size: 0.85em; margin-bottom: 20px;">(예: 주민번호, ID)</div>
        
        <div style="position: relative; height: 135px; display: flex; flex-direction: column; align-items: center; margin-bottom: 5px;">
          <div style="display: flex; gap: 3px; margin-bottom: 5px;">
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #ff6b6b;"></div>
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #54a0ff;"></div>
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #1dd1a1;"></div>
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #feca57;"></div>
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #5f27cd;"></div>
          </div>
          <div style="width: 80px; height: 50px; background: #c8e6c9; clip-path: polygon(0% 0%, 100% 0%, 65% 100%, 35% 100%); border-bottom: 3px solid #2e7d32;"></div>
          <div style="width: 18px; height: 25px; border-left: 2px solid #2e7d32; border-right: 2px solid #2e7d32;"></div>
          <div style="display: flex; gap: 4px; margin-top: 8px;">
            <div style="width: 10px; height: 10px; border-radius: 50%; background: #1dd1a1; box-shadow: 0 0 5px rgba(29, 209, 161, 0.8);"></div>
          </div>
        </div>

        <div style="font-size: 0.9em; line-height: 1.6; color: #444; text-align: center; background: #fff; padding: 12px; border-radius: 8px; border: 1px solid #c8e6c9;">
          <strong>"필터링 성능이 우수함"</strong><br>
          조회 시 검색 범위가 즉시 줄어들어<br>
          최소한의 작업으로 데이터 발견
        </div>
      </div>
      <div style="margin-top: 20px; text-align: center; font-weight: bold; color: #2e7d32; padding: 10px; background: #e8f5e9; border-radius: 8px; font-size: 0.9em;">
        🚀 결과: 매우 빠른 검색 속도
      </div>
    </div>

    <div style="flex: 1; min-width: 340px; border: 2px solid #ffeded; border-radius: 12px; padding: 25px 20px; background-color: #fffafb; display: flex; flex-direction: column; justify-content: space-between;">
      <div>
        <div style="text-align: center; color: #d32f2f; margin-bottom: 5px; font-size: 1.15em; font-weight: bold;">낮은 카디널리티 인덱스</div>
        <div style="text-align: center; color: #666; font-size: 0.85em; margin-bottom: 20px;">(예: 성별, 결제여부)</div>
        
        <div style="position: relative; height: 135px; display: flex; flex-direction: column; align-items: center; margin-bottom: 5px;">
          <div style="display: flex; gap: 3px; margin-bottom: 5px;">
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #1565c0;"></div>
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #1565c0;"></div>
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #cfd8dc;"></div>
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #1565c0;"></div>
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #cfd8dc;"></div>
          </div>
          <div style="width: 100px; height: 50px; background: #ffcdd2; clip-path: polygon(0% 0%, 100% 0%, 85% 100%, 15% 100%); border-bottom: 3px solid #d32f2f;"></div>
          <div style="width: 70px; height: 25px; border-left: 2px solid #d32f2f; border-right: 2px solid #d32f2f; background: rgba(211, 47, 47, 0.05);"></div>
          <div style="display: flex; flex-wrap: wrap; gap: 3px; width: 60px; justify-content: center; margin-top: 5px;">
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #1565c0;"></div>
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #1565c0;"></div>
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #1565c0;"></div>
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #1565c0;"></div>
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #1565c0;"></div>
          </div>
        </div>

        <div style="font-size: 0.9em; line-height: 1.6; color: #444; text-align: center; background: #fff; padding: 12px; border-radius: 8px; border: 1px solid #ffcdd2;">
          <strong>"필터링 성능이 낮음"</strong><br>
          인덱스를 타도 남는 데이터가 너무 많음<br>
          결국 대량의 행을 추가로 확인해야 함
        </div>
      </div>
      <div style="margin-top: 20px; text-align: center; font-weight: bold; color: #d32f2f; padding: 10px; background: #ffebee; border-radius: 8px; font-size: 0.9em;">
        🐢 결과: Full Table Scan이 더 빠를 수도 있음
      </div>
    </div>

  </div>

</div>

<div class="story-box" markdown="1">

인덱스의 궁극적 목적은 <strong class="highlight-text">검색 범위를 좁혀서 Random I/O 비용을 줄이는</strong> 것입니다.

따라서 카디널리티가 **높은** 컬럼의 경우(중복이 없으면), 인덱스를 타는 순간 검색 범위가 아주 좁게 좁혀집니다. 몇 번만 점프(Random I/O)만 하면 되기 때문에 옵티마이저는 인덱스를 선택합니다.

반면, 카디널리티가 **낮은** 컬럼의 경우(중복이 많으면), 인덱스를 타더라도 여전히 수십만 건이 남습니다. 수십만 번 점프해야하기 때문에, 옵티마이저는 인덱스 대신 <strong class="highlight-text">Full Table Scan을(Sequential I/O) 선택</strong>합니다. 보통 데이터베이스 엔진은 인덱스를 통해 읽어야 할 데이터가 **전체 데이터의 약 10%~25%**를 넘어가면, 인덱스가 있어도 사용하지 않고 Full Table Scan을 선택합니다.

</div>

<br>

## [심화] 카디널리티가 낮아도 인덱스 쓰게 만드는 법 <span class="title-sub-desc">: 복합인덱스 (Composite Index)</span>

<div class="story-box" markdown="1">

그렇다면 카디널리티가 낮은 컬럼은 무조건 인덱스에서 제외할까요?

그렇지 않습니다. **복합인덱스**를 활용하면 이야기가 달라집니다.

</div>

<div style="max-width: 800px; margin: 0 auto; font-family: 'Pretendard', sans-serif;">
  

  <div style="display: flex; flex-wrap: wrap; justify-content: center; align-items: stretch; gap: 20px; margin-bottom: 30px;">
    
    <div style="flex: 1; min-width: 340px; border: 2px solid #ffeded; border-radius: 12px; padding: 25px 20px; background-color: #fffafb; display: flex; flex-direction: column;">
      <div style="text-align: center; color: #d32f2f; margin-bottom: 25px; font-size: 1.1em; font-weight: bold;">
        ❌ 단독 인덱스
      </div>
      
      <div style="flex: 1; display: flex; flex-direction: column; align-items: center; justify-content: center;">
        <div style="border: 2px dashed #d32f2f; padding: 10px; border-radius: 8px; background: #fff; text-align: center; width: 80%; margin-bottom: 5px;">
          <span style="font-size: 1.2em;">💳</span> <strong>인덱스: 결제상태</strong><br>
          <span style="font-size: 0.8em; color: #d32f2f;">(Low Cardinality)</span>
        </div>
        
        <div style="position: relative; width: 60%; height: 60px; background: rgba(211, 47, 47, 0.1); border-left: 3px solid #d32f2f; border-right: 3px solid #d32f2f; display: flex; justify-content: center; align-items: center; margin-bottom: 20px;">
          <div style="position: absolute; top: -10px; color: #d32f2f; font-size: 1.5em;">⬇️</div>
          <div style="display: flex; flex-wrap: wrap; gap: 4px; width: 80%; justify-content: center; margin-top:20px;">
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #d32f2f;"></div>
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #d32f2f;"></div>
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #d32f2f;"></div>
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #d32f2f;"></div>
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #d32f2f;"></div>
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #d32f2f;"></div>
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #d32f2f;"></div>
            <div style="width: 8px; height: 8px; border-radius: 50%; background: #d32f2f;"></div>
          </div>
        </div>

        <div style="font-size: 0.9em; color: #444; text-align: center; background: #fff; padding: 12px; border-radius: 8px; border: 1px solid #ffcdd2; width: 90%;">
          <strong>"데이터가 여전히 너무 많음"</strong><br>
          '완료' 상태만 걸러내도 수만 건이 남아<br>
          결국 대량의 행을 추가 확인해야 함
        </div>
      </div>
    </div>

    <div style="flex: 1; min-width: 340px; border: 2px solid #e8f5e9; border-radius: 12px; padding: 25px 20px; background-color: #f1f8e9; display: flex; flex-direction: column;">
      <div style="text-align: center; color: #2e7d32; margin-bottom: 25px; font-size: 1.1em; font-weight: bold;">
        ✅ 복합 인덱스
      </div>
      
      <div style="flex: 1; display: flex; flex-direction: column; align-items: center; justify-content: center;">
        <div style="display: flex; width: 100%; justify-content: space-around; align-items: flex-start; margin-bottom: 20px;">
          <div style="display: flex; flex-direction: column; align-items: center; width: 45%;">
            <div style="border: 2px dashed #2e7d32; padding: 8px; border-radius: 6px; background: #fff; text-align: center; width: 100%; font-size: 0.9em;">
              <span style="font-size: 1.1em;">💳</span> <strong>1차 필터: 상태</strong><br>
              <span style="font-size: 0.75em; color: #666;">(Low Cardinality)</span>
            </div>
            <div style="color: #2e7d32; font-size: 1.5em; margin: 5px 0;">⬇️</div>
            <div style="display: flex; gap: 3px; flex-wrap: wrap; justify-content: center; opacity: 0.6;">
              <div style="width: 6px; height: 6px; border-radius: 50%; background: #2e7d32;"></div>
              <div style="width: 6px; height: 6px; border-radius: 50%; background: #2e7d32;"></div>
              <div style="width: 6px; height: 6px; border-radius: 50%; background: #2e7d32;"></div>
              <div style="width: 6px; height: 6px; border-radius: 50%; background: #2e7d32;"></div>
              <div style="width: 6px; height: 6px; border-radius: 50%; background: #2e7d32;"></div>
            </div>
          </div>

          <div style="color: #2e7d32; font-size: 2em; align-self: center;">➔</div>

          <div style="display: flex; flex-direction: column; align-items: center; width: 45%;">
            <div style="border: 2px solid #2e7d32; padding: 8px; border-radius: 6px; background: #fff; text-align: center; width: 100%; font-size: 0.9em; font-weight: bold;">
              <span style="font-size: 1.1em;">📅</span> <strong>2차 필터: 일시</strong><br>
              <span style="font-size: 0.75em; color: #2e7d32;">(High Cardinality)</span>
            </div>
            <div style="color: #2e7d32; font-size: 1.5em; margin: 5px 0;">⬇️</div>
            <div style="display: flex; gap: 4px; justify-content: center;">
              <div style="width: 10px; height: 10px; border-radius: 50%; background: #1dd1a1; box-shadow: 0 0 5px rgba(29, 209, 161, 0.8);"></div>
            </div>
          </div>
        </div>

        <div style="font-size: 0.9em; color: #444; text-align: center; background: #fff; padding: 12px; border-radius: 8px; border: 1px solid #c8e6c9; width: 90%;">
          <strong>"검색 범위 극대화"</strong><br>
          '완료' 상태 중에서 '특정 일시'만<br>
          골라내어 성능이 비약적 향상
        </div>
      </div>
    </div>

  </div>

</div>

<div class="story-box" markdown="1">

카디널리티가 낮은 칼럼이라도, <strong class="highlight-text">다른 칼럼과 결합</strong>하면 강력한 필터가 될 수 있습니다.

ex. (`결제상태`, `결제일시`)

- `결제상태`라는 낮은 카디널리티 컬럼만으로는 비효율적이지만,
- `결제일시`라는 높은 카디널리티 칼럼과 결합하면, “완료상태 + 어제날짜” 같은 특정범위의 데이터를 매우 빠르게 찾을 수 있습니다.

</div>

<br>

## ⚠️ [예외] 카디널리티가 높은데도 인덱스 안 쓰는 경우 <span class="title-sub-desc">: 비용(Cost)의 역설</span>

<div class="story-box" markdown="1">

그러나 카디널리티가 인덱스 성능의 유일한 척도는 아닙니다. 

옵티마이저는 카디널리티가 높아도 **전체 비용(Cost)** 관점에서 종합적으로 계산하여 최적의 경로를 선택하기 때문입니다. 따라서 다음과 같은 특정 환경에서는 높은 카디널리티의 이점이 <strong class="highlight-text">상쇄</strong>될 수 있습니다.

</div>

<div style="max-width: 800px; margin: 0 auto; font-family: 'Pretendard', sans-serif;">
  
  <div style="display: flex; flex-wrap: wrap; justify-content: center; align-items: stretch; gap: 20px; margin-bottom: 30px;">
    
    <div style="flex: 1; min-width: 340px; border: 2px solid #ed8936; border-radius: 12px; padding: 25px 20px; background-color: #fffaf0; display: flex; flex-direction: column;">
      <div style="text-align: center; color: #c05621; margin-bottom: 20px; font-size: 1.1em; font-weight: bold;">
        ⏱️ 1) 전체 데이터 수가 너무 적을 때
      </div>
      
      <div style="flex: 1; display: flex; flex-direction: column; justify-content: center;">
        <div style="display: flex; flex-direction: column; gap: 15px; margin-bottom: 20px;">
          <div>
            <div style="font-size: 0.85em; font-weight: bold; color: #718096; margin-bottom: 5px;">A. 인덱스 스캔 방식 (느림)</div>
            <div style="display: flex; height: 26px; border-radius: 13px; overflow: hidden; background: #e2e8f0; border: 1px solid #cbd5e0;">
              <div style="flex: 3; background: #ed8936; display: flex; align-items: center; justify-content: center; color: white; font-size: 0.75em; font-weight: bold;">인덱스 트리 탐색 (준비)</div>
              <div style="flex: 1; background: #48bb78; display: flex; align-items: center; justify-content: center; color: white; font-size: 0.75em;">✓</div>
            </div>
          </div>
          <div>
            <div style="font-size: 0.85em; font-weight: bold; color: #2f855a; margin-bottom: 5px;">B. Full Table Scan 방식 (빠름 🚀)</div>
            <div style="display: flex; width: 45%; height: 26px; border-radius: 13px; overflow: hidden; background: #f0fff4; border: 1px solid #c6f6d5;">
              <div style="flex: 1; background: #48bb78; display: flex; align-items: center; justify-content: center; color: white; font-size: 0.75em; font-weight: bold;">바로 읽기 완료!</div>
            </div>
          </div>
        </div>

        <div style="font-size: 0.9em; color: #2d3748; text-align: center; background: #fff; padding: 12px; border-radius: 8px; border: 1px solid #ed8936; box-shadow: 0 2px 4px rgba(0,0,0,0.05);">
          <strong>준비 과정이 더 오래 걸림</strong><br>
          데이터가 적으면 인덱스를 거치는 오버헤드가<br>
          실제 데이터를 읽는 시간보다 큼
        </div>
      </div>
    </div>

    <div style="flex: 1; min-width: 340px; border: 2px solid #ed8936; border-radius: 12px; padding: 25px 20px; background-color: #fffaf0; display: flex; flex-direction: column;">
      <div style="text-align: center; color: #c05621; margin-bottom: 20px; font-size: 1.1em; font-weight: bold;">
        🐘 2) 칼럼 값 자체가 너무 비대할 때
      </div>
      
      <div style="flex: 1; display: flex; flex-direction: column; justify-content: center; align-items: center;">
        <div style="width: 100%; border: 1px solid #cbd5e0; background: #fff; border-radius: 6px; overflow: hidden; margin-bottom: 15px; box-shadow: 0 3px 6px rgba(0,0,0,0.08);">
            <div style="display: flex; background: #edf2f7; border-bottom: 1px solid #cbd5e0; font-size: 0.85em; font-weight: bold; color: #4a5568;">
                <div style="padding: 10px; width: 50px; text-align: center; border-right: 1px solid #cbd5e0;">ID</div>
                <div style="padding: 10px; flex: 1;">LongTextCol (인덱스 대상 컬럼)</div>
            </div>
            <div style="display: flex; background: #fffaf0; border-bottom: 2px solid #ed8936; font-size: 0.85em;">
                 <div style="padding: 10px; width: 50px; text-align: center; border-right: 1px solid #ed8936; font-weight: bold; color: #c05621; background: #feebc8;">N</div>
                 <div style="padding: 10px; flex: 1; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; color: #7b341e; font-family: 'Courier New', monospace; font-weight: bold;">
                    "이_컬럼에는_엄청나게_긴_텍스트_데이터가_들어가서_인덱스_키가_매우_커집니다...(생략)"
                 </div>
            </div>
             <div style="text-align: center; font-size: 0.75em; color: #718096; padding: 6px; background: #f7fafc; font-style: italic;">(실제로는 수천 자가 넘어가는 데이터)</div>
        </div>

        <div style="font-size: 0.9em; color: #2d3748; text-align: center; background: #fff; padding: 12px; border-radius: 8px; border: 1px solid #ed8936; box-shadow: 0 2px 4px rgba(0,0,0,0.05);">
          <strong>I/O 효율 급격히 저하</strong><br>
          인덱스 한 페이지에 담기는 정보량이 줄어,<br>
          디스크 접근 횟수 증가 및 메모리 낭비
        </div>
      </div>
    </div>

  </div>
</div>

### 1) 테이블의 전체 데이터 양이 너무 적은 경우

<div class="story-box" markdown="1">

전체 데이터 수가 너무 적으면 인덱스를 사용하는 것 자체가 오히려 **시간 낭비**가 될 수 있습니다. 전체 데이터 수가 5개인 테이블에서 특정 단어를 찾는다고 가정해 봅시다. 인덱스 페이지를 열어 위치를 확인하고 다시 테이블을 보는 것보다, 그냥 처음부터 끝까지 훑어보는 게 훨씬 빠르겠죠?

인덱스를 읽고 주소를 찾아가는 <strong class="highlight-text">준비 단계 비용</strong>이, 테이블을 그냥 다 읽어버리는 비용보다 크기 때문에 옵티마이저는 인덱스를 무시하게 됩니다.

</div>

### 2) 데이터의 길이가 너무 긴 경우

<div class="story-box" markdown="1">

`VARCHAR(2000)`처럼 매우 긴 텍스트 컬럼에 인덱스를 걸면, **인덱스 자체의 크기가 비대**해집니다. DB는 인덱스를 **페이지**라는 일정한 크기 단위로 나누어 관리하는데, 데이터가 길어지면 다음과 같은 문제가 발생합니다.

- <strong class="highlight-text">I/O 비용 증가</strong> : 한 페이지에 담을 수 있는 인덱스 정보가 몇 개 안되다보니, 평소라면 1개만 읽어도 될 것을 5개, 10개씩 읽어야 합니다. 결국 디스크를 읽는 횟수(I/O)가 늘어나 속도가 느려집니다.
- <strong class="highlight-text">메모리 효율 저하</strong> : 이렇게 무거워진 인덱스들이 메모리 버퍼 공간을 많이 차지하게 됩니다. 따라서 정작 자주 쓰이는 다른 데이터들이 메모리에 올라오지 못하고 밀려나게 되면서 전체적인 DB 성능이 떨어지는 원인이 됩니다.

</div>

---

# 카디널리티를 고려하며 인덱스를 쓰자

<div class="story-box" markdown="1">
데이터가 쌓일수록 인덱스는 성능을 높여주는 좋은 도구입니다.

하지만 무조건 인덱스를 사용하는 것이 높은 성능을 보장하는 것은 아닙니다. <strong class="highlight-text">카디널리티가 낮은 칼럼</strong>에 무작정 걸어버리먼 오히려 성능에 악영향을 줍니다. 인덱스 주소를 확인하고 실제 데이터를 하나하나 찾아가는 <strong class="highlight-text">Random I/O 비용</strong>이 테이블 전체를 훑는 비용보다 크기 때문입니다.

결국 중요한 것은 단순히 인덱스를 '만드는 것'이 아니라, <strong class="highlight-text">옵티마이저가 기꺼이 선택할 수밖에 없는 인덱스</strong>를 <strong class="highlight-text">'설계하는 것'</strong>입니다. 카디널리티로 데이터 분포도를 파악하면 전략적인 인덱스를 설계할 수 있습니다. 오늘 배운 카디널리티의 원리와 I/O 비용의 관계를 바탕으로, 여러분의 쿼리가 가장 효율적인 길을 찾아갈 수 있도록 전략적인 인덱스를 설계해 보시길 바랍니다.

</div>