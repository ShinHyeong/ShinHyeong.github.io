---
title:  "[DB] 카디널리티(Cardinality)란 무엇이며 왜 사용할까?"
categories: Database
excerpt: ""
description: ""
#redirect_from: #이전주소 입력
#search: false #만약 이 글이 검색되지 않기를 바란다면
#use_math: true #수식이 필요한 경우만 사용
---

# 내 쿼리는 왜 응답이 없을까

수백만건 데이터 속 원하는 정보를 찾기 위해 **하염없이 기다려본** 경험이 있으신가요? 이 문제에는 종종 비효율적인 데이터 조회방식이 있습니다.

## 범인은 Full Table Scan

DB가 특정 데이터를 찾기위해 테이블의 모든 행을 **하나씩 전부 확인**하는 방식입니다. 수백만 권 책이 있는 도서관에서 원하는 책 한 권을 찾기 위해 모든 책의 제목을 일일이 확인하는 것과 같습니다. 이 방식의 문제는 **데이터가 많아질수록 성능은 급격히 저하**된다는 것입다.

## 그래서 우리는 인덱스를 사용해요

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
        <p style="font-size: 0.9em; color: #666; text-align: center; margin-bottom: 20px;">목차를 통해 위치 바로 점프 (이진 탐색 등)</p>
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

인덱스는 DB의 목차같은 역할을 해서 Full Table Scan을 피하고 검색속도를 획기적으로 높여줍니다. 하지만 인덱스를 생성했음에도 DBMS가 인덱스를 사용하지 않고 **여전히 Full Table Scan을 사용하는 경우가 있습니다.**

왜 이런 일이 발생할까요?

# 카디널리티 (Cardinality)

인덱스 효율을 결정하는 열쇠, 카디널리티 때문인데요.

카디널리티가 무엇이냐면 특정 칼럼에 포함된 값들의 고유한 정도(Uniqueness)를 의미합니다.

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

- 카디널리티가 높다 : 전체 행 개수 대비 중복되지 않는 값의 개수가 많다.
    
    → 데이터를 **거의 유일하게 식별**할 수 있는 컬럼들
    
- 카디널리티가 낮다 : 전체 행 개수 대비 중복되는 값의 개수가 많다.
    
    → 몇 가지 **정해진 값만 반복**해서 나타나는 컬럼들


# 인덱스는 카디널리티가 높은 컬럼을 선택한다

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
            <div style="font-size: 0.8em; font-weight: bold; color: #2e7d32;">조회 결과 (소수)</div>
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

인덱스의 궁극적 목적은 **검색 범위를 빠르게 좁히는 것**입니다.

따라서 카디널리티가 **높은** 컬럼의 인덱스의 경우, 사용하여 필터링할 때 **소수의 행만** 남아 있기 때문에 매우 효율적입니다.

반면, 카디널리티가 **낮은** 컬럼의 인덱스의 경우, 사용하여 필터링하더라도 **여전히 많은 데이터**가 남아있어 DBMS 옵티마이저는 차라리 테이블 전체를 하나씩 스캔하는 것(Full Table Scan)이 더 빠르다고 판단할 수 있습니다.

## 그렇다면 카디널리티가 낮은 컬럼은 절대 인덱스를 걸면 안될까?

원칙적으로는 비효율적이지만, 반드시 그런 것은 아닙니다.

특정 조건하에서 카디널리티가 낮은 컬럼도 인덱스에서 중요한 역할을 수행할 수 있습니다.

### 복합인덱스 (Composite Index)

<div style="max-width: 800px; margin: 0 auto; font-family: 'Pretendard', sans-serif;">
  

  <div style="display: flex; flex-wrap: wrap; justify-content: center; align-items: stretch; gap: 20px; margin-bottom: 30px;">
    
    <div style="flex: 1; min-width: 340px; border: 2px solid #ffeded; border-radius: 12px; padding: 25px 20px; background-color: #fffafb; display: flex; flex-direction: column;">
      <div style="text-align: center; color: #d32f2f; margin-bottom: 25px; font-size: 1.1em; font-weight: bold;">
        ❌ 단독 인덱스
      </div>
      
      <div style="flex: 1; display: flex; flex-direction: column; align-items: center; justify-content: center;">
        <div style="border: 2px dashed #d32f2f; padding: 10px; border-radius: 8px; background: #fff; text-align: center; width: 80%; margin-bottom: 5px;">
          <span style="font-size: 1.2em;">💳</span> <strong>인덱스: 결제상태</strong><br>
          <span style="font-size: 0.8em; color: #d32f2f;">(카디널리티 낮음)</span>
        </div>
        
        <div style="position: relative; width: 60%; height: 60px; background: rgba(211, 47, 47, 0.1); border-left: 3px solid #d32f2f; border-right: 3px solid #d32f2f; display: flex; justify-content: center; align-items: center; margin-bottom: 20px;">
          <div style="position: absolute; top: -10px; color: #d32f2f; font-size: 1.5em;">⬇️</div>
          <div style="display: flex; flex-wrap: wrap; gap: 4px; width: 80%; justify-content: center;">
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
              <span style="font-size: 0.75em; color: #666;">(Low Card.)</span>
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
              <span style="font-size: 0.75em; color: #2e7d32;">(High Card.)</span>
            </div>
            <div style="color: #2e7d32; font-size: 1.5em; margin: 5px 0;">⬇️</div>
            <div style="display: flex; gap: 4px; justify-content: center;">
              <div style="width: 10px; height: 10px; border-radius: 50%; background: #1dd1a1; box-shadow: 0 0 5px rgba(29, 209, 161, 0.8);"></div>
              <div style="font-size: 0.85em; font-weight: bold; color: #2e7d32;">최종 결과</div>
            </div>
          </div>
        </div>

        <div style="font-size: 0.9em; color: #444; text-align: center; background: #fff; padding: 12px; border-radius: 8px; border: 1px solid #c8e6c9; width: 90%;">
          <strong>"검색 범위 극대화"</strong><br>
          '완료' 상태 중에서 '특정 일시'만<br>
          정확히 골라내어 성능이 비약적 향상
        </div>
      </div>
    </div>

  </div>

</div>

바로 복합인덱스인데요. 카디널리티가 낮은 칼럼도 단독으로 쓰일 때가 아닌, 다른 컬럼과 결합하여 복합 인덱스를 구성할 때는 필터링 성능을 크게 높일 수 있습니다.

ex. (`결제상태`, `결제일시`)

- `결제상태`라는 낮은 카디널리티 컬럼만으로는 비효율적이지만,
- `결제일시`라는 높은 카디널리티 칼럼과 결합하면, “완료상태 + 어제날짜” 같은 특정범위의 데이터를 매우 빠르게 찾을 수 있습니다.

## 반대로, 카디널리티가 높은데도 인덱스 효율이 나쁠 수 있다

그러나 카디널리티가 인덱스 성능의 유일한 척도는 아닙니다. 다음과 같은 특정 환경에서는 높은 카디널리티의 이점이 상쇄될 수 있습니다.

### 1) 테이블의 전체 데이터 양이 너무 적은 경우

이 경우엔 인덱스를 읽는 비용보다 차라리 전체 테이블을 스캔하는 것이 더 빠를 수 있습니다.

### 2) 컬럼의 데이터가 너무 길어 인덱스 크기가 비대해지는 경우

매우 긴 텍스트 칼럼 (ex. `VARCHAR(2000)`)

**인덱스 자체의 크기**가 너무 커져서 I/O 비용이 증가하고, 메모리 효율이 떨어져 오히려 성능 저하를 유발할 수 있습니다.