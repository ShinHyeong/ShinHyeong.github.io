---
title:  "[DB] 인덱스 자료구조로 B-Tree(혹은 B+Tree)를 사용하는 이유"
categories: Database
excerpt: "해시 인덱스는 O(1)인데, 왜 대부분의 DBMS는 B+Tree를 쓸까요? BST, AVL 트리와 비교하며 디스크 I/O와 트리 구조 관점에서 설명했습니다."
description: "해시 인덱스는 O(1)인데, 왜 대부분의 DBMS는 B+Tree를 쓸까요? 현대 데이터베이스 인덱스가 B-Tree와 B+Tree를 표준으로 사용하는 이유를 디스크 I/O, 페이지 단위 접근, 트리 높이 관점에서 설명했습니다. 또, 해시 인덱스, BST, AVL 트리의 한계와 비교하여 B+Tree가 범위 검색과 대용량 데이터에 적합한 이유를 정리했습니다."
tags: [인덱스, Index, 인덱스자료구조, 자료구조, B-Tree, B+Tree, 데이터베이스, Database]
#redirect_from: #이전주소 입력
#search: false #만약 이 글이 검색되지 않기를 바란다면
#use_math: true #수식이 필요한 경우만 사용
---

# 넘을 수 없는 물리적 장벽 : 메모리 vs. 디스크

<div style="max-width: 800px; margin: 0 auto; background-color: #f6fbfe; padding: 50px 20px; font-family: 'Malgun Gothic', 'Apple SD Gothic Neo', sans-serif; box-sizing: border-box; display: flex; align-items: center; justify-content: space-between; border-radius: 10px;">

    <div style="text-align: center; width: 25%;">
        <svg width="120" height="120" viewBox="0 0 100 100" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect x="20" y="30" width="40" height="40" rx="3" stroke="#0f4c75" stroke-width="3" fill="white"/>
            <text x="40" y="55" font-size="12" font-weight="bold" fill="#0f4c75" text-anchor="middle">CPU</text>
            <circle cx="20" cy="20" r="2" fill="#0f4c75"/><path d="M20 20 L25 30" stroke="#0f4c75" stroke-width="2"/>
            <circle cx="40" cy="15" r="2" fill="#0f4c75"/><path d="M40 15 L40 30" stroke="#0f4c75" stroke-width="2"/>
            <circle cx="60" cy="20" r="2" fill="#0f4c75"/><path d="M60 20 L55 30" stroke="#0f4c75" stroke-width="2"/>
            <circle cx="20" cy="80" r="2" fill="#0f4c75"/><path d="M20 80 L25 70" stroke="#0f4c75" stroke-width="2"/>
            <circle cx="40" cy="85" r="2" fill="#0f4c75"/><path d="M40 85 L40 70" stroke="#0f4c75" stroke-width="2"/>
            <circle cx="60" cy="80" r="2" fill="#0f4c75"/><path d="M60 80 L55 70" stroke="#0f4c75" stroke-width="2"/>
            <path d="M10 40 L20 40" stroke="#0f4c75" stroke-width="2" stroke-linecap="round"/>
            <path d="M10 50 L20 50" stroke="#0f4c75" stroke-width="2" stroke-linecap="round"/>
            <path d="M10 60 L20 60" stroke="#0f4c75" stroke-width="2" stroke-linecap="round"/>
            
            <rect x="70" y="15" width="15" height="70" rx="1" stroke="#0f4c75" stroke-width="3" fill="white"/>
            <rect x="74" y="20" width="7" height="10" fill="#0f4c75"/>
            <rect x="74" y="35" width="7" height="10" fill="#0f4c75"/>
            <rect x="74" y="50" width="7" height="10" fill="#0f4c75"/>
            <rect x="74" y="65" width="7" height="10" fill="#0f4c75"/>
            <line x1="60" y1="35" x2="70" y2="35" stroke="#0f4c75" stroke-width="2"/>
            <line x1="60" y1="50" x2="70" y2="50" stroke="#0f4c75" stroke-width="2"/>
            <line x1="60" y1="65" x2="70" y2="65" stroke="#0f4c75" stroke-width="2"/>
        </svg>
        <div style="font-size: 16px; font-weight: bold; color: #000; margin-top: 10px; white-space: nowrap;">
            메모리 접근: 나노초 (ns)
        </div>
    </div>

    <div style="flex-grow: 1; text-align: center; padding: 0 20px;">
        <div style="font-size: 32px; font-weight: 800; color: #d66415; margin-bottom: -15px;">
            &gt; 100,000배 차이
        </div>
        <svg width="100%" height="40" preserveAspectRatio="none" style="overflow: visible;">
            <defs>
                <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
                    <polygon points="0 0, 10 3.5, 0 7" fill="#eeb07f" />
                </marker>
            </defs>
            <line x1="0" y1="20" x2="100%" y2="20" stroke="#eeb07f" stroke-width="4" marker-end="url(#arrowhead)" />
        </svg>
    </div>

    <div style="text-align: center; width: 25%;">
        <svg width="120" height="120" viewBox="0 0 100 100" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect x="15" y="10" width="70" height="80" rx="4" stroke="#d66415" stroke-width="3" fill="white"/>
            <circle cx="20" cy="15" r="1.5" fill="#d66415"/>
            <circle cx="80" cy="15" r="1.5" fill="#d66415"/>
            <circle cx="20" cy="85" r="1.5" fill="#d66415"/>
            <circle cx="80" cy="85" r="1.5" fill="#d66415"/>
            
            <circle cx="50" cy="45" r="25" stroke="#d66415" stroke-width="3" fill="white"/>
            <circle cx="50" cy="45" r="8" stroke="#d66415" stroke-width="2"/>
            <circle cx="50" cy="45" r="2" fill="#d66415"/>
            
            <path d="M70 70 L55 55" stroke="#d66415" stroke-width="4" stroke-linecap="round"/>
            <circle cx="70" cy="70" r="5" fill="#d66415"/>
            
            <path d="M60 75 L80 75 L80 65" stroke="#d66415" stroke-width="2" fill="none"/>
        </svg>
        <div style="font-size: 16px; font-weight: bold; color: #000; margin-top: 10px; white-space: nowrap;">
            디스크 접근: 밀리초 (ms)
        </div>
    </div>

</div>

데이터베이스의 인덱스는 대부분 디스크(HDD/SSD)에 저장됩니다. 디스크 접근(I/O)은 메모리 접근보다 수만 배는 느리기 때문에, CPU가 아무리 빨라도 <strong class="highlight-text">디스크에서 데이터를 읽어오는 시간</strong>이 **전체 성능**을 좌우합니다.

⇒ 목표 : 그래서 우리는 <strong class="highlight-text">디스크에 접근하는 횟수</strong>를 어떻게든 **최소화**해야합니다.

## 후보1 : 이론상 가장 빠른 해시인덱스

<img src="{{site.url}}/assets/images/2026-01-18-why-db-index-uses-btree-and-bplustree/hash-index-disadvantage.png" 
     alt="hash-index-disadvantage" 
     style="display: block; margin: 2rem auto; max-width: 70%; height: auto;">

해시자료구조는 특정 값을 찾는 동등비교(=)연산에서 O(1)의 속도를 보장하기 때문에 이론상 가장 빠릅니다.

그러나, <strong class="highlight-text">데이터가 정렬되어 있지 않아</strong> **범위검색(<, >, BETWEEN)이 불가능**합니다. 따라서 범용적인 DB 인덱스로는 한계가 명확합니다.

## 후보2 : 그렇다면 정렬된 구조인 BST(이진탐색트리)는?

<img src="{{site.url}}/assets/images/2026-01-18-why-db-index-uses-btree-and-bplustree/bst-disadvantage.png" 
     alt="bst-disadvantage" 
     style="display: block; margin: 2rem auto; max-width: 90%; height: auto;">

그렇다면 정렬된 구조인 이진탐색트리는 어떨까요?

- 왼쪽 서브트리 : 루트보다 작은 값
- 오른쪽 서브트리 : 루트보다 큰 값

이 구조덕분에 데이터 탐색과 범위 검색이 모두 가능합니다.

그러나, 데이터가 **정렬된 순서로 입력**되면 **트리가 한쪽으로** 치우칩니다. **균형이 깨진 트리는 사실상 연결리스트(Linked List)**와 같아져 조회 성능이 최악의 경우 **O(n)**으로 저하됩니다. 이러한 특성때문에 DB 인덱스로 사용하기엔 리스크가 큽니다.

## 후보3: 스스로 균형을 잡는 트리 AVL은 어때? (레드블랙트리)

<img src="{{site.url}}/assets/images/2026-01-18-why-db-index-uses-btree-and-bplustree/avl_disadvantage.png" 
     alt="avl-disadvantage" 
     style="display: block; margin: 2rem auto; max-width: 80%; height: auto;">

균형이진트리는 데이터 삽입/삭제시 **스스로 구조를 재조정**하여 항상 균형을 유지합니다. 따라서 BST의 최악의 경우인 O(n)을 방지하고, 어떤 상황에서도 O(log n)의 조회 성능을 보장합니다.

하지만 균형이 잡혀도 자식노드가 2개뿐인 이진트리는 데이터 양이 많아질수록 필연적으로 **트리의 높이가 매우 깊어집니다.**

> 트리의 높이 = 조회 작업에 필요한 최대 디스크 I/O 횟수

수억 개의 데이터가 있다면, 트리의 높이는 수십 레벨에 달할 수 있고, 이는 곧 **수십 번의 디스크 접근**을 의미합니다. 여전히 너무 느립니다.

# B-Tree : 높이 대신 너비를 택하다

<img src="{{site.url}}/assets/images/2026-01-18-why-db-index-uses-btree-and-bplustree/btree_vs_binary_tree_height_width.png" 
     alt="btree_vs_binary_tree_height_width" 
     style="display: block; margin: 2rem auto; max-width: 100%; height: auto;">

B-Tree란 **하나의 노드에 여러 개의 키**를 저장하는 **다진 트리**입니다.(Multi-way Tree)

하나의 노드에 많은 데이터를 담기 때문에 **트리의 높이를 극적으로 낮춥니다**. 따라서 수억 개의 데이터도 단 3~4 레벨의 높이로 표현할 수 있으므로, <strong class="highlight-text">몇 번의 디스크 I/O</strong>만으로 **조회 작업을 완료**할 수 있습니다.

또, DB는 데이터를 페이지 또는 블록 단위로 읽어옵니다. B-Tree **노드 크기**를 이 <strong class="highlight-text">"페이지"</strong> 단위에 맞추면, 한번의 디스크 I/O로 노드의 **여러 키 값**을 가져올 수 있습니다. 이는 <strong class="highlight-text">캐시 적중률</strong>을 높이고 조회 성능을 극대화합니다.

## 모든 노드가 데이터를 갖는다

<img src="{{site.url}}/assets/images/2026-01-18-why-db-index-uses-btree-and-bplustree/btree_internal_and_leaf_node_data_storage.png" 
     alt="btree_internal_and_leaf_node_data_storage" 
     style="display: block; margin: 2rem auto; max-width: 100%; height: auto;">

키(Key)와 해당 데이터(Data)가 모든 노드(루트,브랜치,리프)에 저장될 수 있습니다. 따라서 리프 노드까지 가지 않아도, 운좋으면  **중간 노드에서 조회 작업이 끝**날 수 있습니다. 그러나 이것이 항상 장점은 아닙니다.

# B+Tree : B-Tree의 단점 보완한 구조

<img src="{{site.url}}/assets/images/2026-01-18-why-db-index-uses-btree-and-bplustree/btree_to_bplustree_leaf_linking.png" 
     alt="btree_to_bplustree_leaf_linking" 
     style="display: block; margin: 2rem auto; max-width: 100%; height: auto;">

B+Tree는 B-Tree의 장점을 계승하고, DB 환경에서 최적화된 개선을 가진 자료구조입니다.

1. **데이터는 오직 리프 노드에만** 저장한다. **브랜치** 노드는 경로탐색을 위한 **인덱스** 역할로만 수행한다.
2. 모든 리프 노드가 **연결리스트(Linked List)**로 연결되어있다.

## 1) 리프노드에만 저장, 브랜치 노드는 인덱스로만 → 너비는 더 넓고 높이는 더 낮게!

<img src="{{site.url}}/assets/images/2026-01-18-why-db-index-uses-btree-and-bplustree/btree_vs_bplustree_internal_node_structure.png" 
     alt="btree_vs_bplustree_internal_node_structure" 
     style="display: block; margin: 2rem auto; max-width: 100%; height: auto;">

브랜치 노드에 데이터를 저장하지 않으니, 동일한 노드 크기(페이지 크기)에 더 많은 키(경로 정보)를 저장할 수 있습니다.

따라서 노드가 더 많이 분기해서 트리의 너비는 넓어지고, 높이는 더 낮아집니다. 이는 <strong class="highlight-text">디스크 I/O 횟수가 더 줄어들 수 있음</strong>을 의미합니다.

## 2) 리프 노드 연결리스트로 순차 스캔 → 범위 검색 성능 UP!

<img src="{{site.url}}/assets/images/2026-01-18-why-db-index-uses-btree-and-bplustree/btree_range_query_traversal.png" 
     alt="btree_range_query_traversal" 
     style="display: block; margin: 2rem auto; max-width: 100%; height: auto;">

모든 리프 노드들이 포인터로 서로 연결된 Linked List 구조를 가집니다. 이 구조 덕분에 B-Tree처럼 **트리를 다시 올라가지 않고**, <strong class="highlight-text">리프노드만 순차적으로 탐색</strong>하면 되므로, 훨씬 **범위 탐색**과 **정렬** 성능이 극대화됩니다.

ex. WHERE age BETWEEN 25 AND 50

- 범위의 **시작 값(25)를 찾아** 리프노드로 이동한다. (B-Tree)
- 트리를 다시 탐색하지 않고 **연결된 리프노드를 따라 순차적으로 스캔**한다(→30→45→50)

## 거의 모든 현대 DBMS 표준 : MySQL InnoDB 엔진 예시

<img src="{{site.url}}/assets/images/2026-01-18-why-db-index-uses-btree-and-bplustree/mysql-innodb-dbms-structure.png" 
     alt="mysql-innodb-dbms-structure" 
     style="display: block; margin: 2rem auto; max-width: 90%; height: auto;">

- 프라이머리 인덱스(클러스터형 인덱스) : 리프노드에 실제 데이터 **레코드 전체**가 저장됩니다.
- 세컨더리 인덱스(보조 인덱스) : 리프노드에는 데이터 주소 대신 **레코드의 PK** 값이 저장됩니다. 따라서 세컨더리 인덱스로 조회하면, 먼저 PK 값을 찾은 후, 이 PK 값으로 프라이머리 인덱스를 한번 더 조회해야 실제 데이터에 접근할 수 있습니다.

# 마무리

<div style="max-width: 800px; margin: 20px auto; font-family: 'Pretendard', 'Segoe UI', Roboto, sans-serif; overflow-x: auto;">
  <div style="min-width: 750px; display: flex; align-items: center; justify-content: space-between; padding: 20px 10px; background-color: #fdfdfd; border: 1px solid #eee; border-radius: 15px;">
    
    <div style="flex: 1; display: flex; flex-direction: column; align-items: center;">
      <div style="width: 100%; height: 90px; padding: 10px; background-color: #fff0f0; border: 2px solid #ff4d4d; border-radius: 10px; display: flex; flex-direction: column; justify-content: center; text-align: center; box-shadow: 0 2px 4px rgba(0,0,0,0.05);">
        <span style="font-size: 0.85em; font-weight: bold; color: #d93025; margin-bottom: 4px;">[문제]</span>
        <span style="font-size: 0.9em; font-weight: bold; line-height: 1.2;">느린<br>디스크 I/O</span>
      </div>
    </div>

    <div style="padding: 0 5px; color: #ccc; font-weight: bold;">&rarr;</div>

    <div style="flex: 1; display: flex; flex-direction: column; align-items: center;">
      <div style="width: 100%; height: 90px; padding: 10px; background-color: #f8f9fa; border: 1.5px solid #dee2e6; border-radius: 10px; display: flex; flex-direction: column; justify-content: center; text-align: center;">
        <span style="font-size: 0.85em; font-weight: bold; color: #495057; margin-bottom: 4px;">해시 인덱스</span>
        <span style="font-size: 0.8em; color: #d93025; line-height: 1.2;">범위 검색<br>불가능</span>
      </div>
    </div>

    <div style="padding: 0 5px; color: #ccc; font-weight: bold;">&rarr;</div>

    <div style="flex: 1; display: flex; flex-direction: column; align-items: center;">
      <div style="width: 100%; height: 90px; padding: 10px; background-color: #f8f9fa; border: 1.5px solid #dee2e6; border-radius: 10px; display: flex; flex-direction: column; justify-content: center; text-align: center;">
        <span style="font-size: 0.85em; font-weight: bold; color: #495057; margin-bottom: 4px;">BST</span>
        <span style="font-size: 0.8em; color: #d93025; line-height: 1.2;">불균형 시<br>O(N) 위험</span>
      </div>
    </div>

    <div style="padding: 0 5px; color: #ccc; font-weight: bold;">&rarr;</div>

    <div style="flex: 1; display: flex; flex-direction: column; align-items: center;">
      <div style="width: 100%; height: 90px; padding: 10px; background-color: #f8f9fa; border: 1.5px solid #dee2e6; border-radius: 10px; display: flex; flex-direction: column; justify-content: center; text-align: center;">
        <span style="font-size: 0.85em; font-weight: bold; color: #495057; margin-bottom: 4px;">균형 트리</span>
        <span style="font-size: 0.8em; color: #d93025; line-height: 1.2;">높은 트리<br>I/O 부담</span>
      </div>
    </div>

    <div style="padding: 0 5px; color: #ccc; font-weight: bold;">&rarr;</div>

    <div style="flex: 1; display: flex; flex-direction: column; align-items: center;">
      <div style="width: 100%; height: 90px; padding: 10px; background-color: #e7f3ff; border: 1.5px solid #007bff; border-radius: 10px; display: flex; flex-direction: column; justify-content: center; text-align: center;">
        <span style="font-size: 0.85em; font-weight: bold; color: #0056b3; margin-bottom: 4px;">B-Tree</span>
        <span style="font-size: 0.8em; color: #0056b3; line-height: 1.2;">낮은 높이<br>I/O 최소화</span>
      </div>
    </div>

    <div style="padding: 0 5px; color: #ccc; font-weight: bold;">&rarr;</div>

    <div style="flex: 1; display: flex; flex-direction: column; align-items: center;">
      <div style="width: 100%; height: 90px; padding: 10px; background-color: #e6ffed; border: 2px solid #28a745; border-radius: 10px; display: flex; flex-direction: column; justify-content: center; text-align: center; box-shadow: 0 4px 8px rgba(40,167,69,0.15);">
        <span style="font-size: 0.85em; font-weight: bold; color: #1e7e34; margin-bottom: 4px;">B+Tree</span>
        <span style="font-size: 0.8em; font-weight: bold; color: #1e7e34; line-height: 1.2;">범위 검색<br>최적화</span>
      </div>
    </div>

  </div>
</div>

DB 인덱스의 자료구조는 “어떻게 <strong class="highlight-text">디스크 접근을 최소화</strong> 할 것인가”에 대한 답을 찾아온 과정이었습니다. 

B+Tree는 다음과 같은 특징으로 이 문제에 대한 최적의 답을 제시합니다.

- 낮고 넓은 트리 : **최소한의 디스크 I/O**로 조회성능 높임
- 리프 노드 연결 : 압도적인 **범위 검색** 성능
- 구조적 일관성 : BST와 달리, 예측 가능하고 안정적인 성능

DB 인덱스 성능의 핵심은 알고리즘이 아니라 **디스크 I/O**입니다.

이것이 MySQL, PostgreSQL, Oracle 등 대부분의 현대 DBMS가 B+Tree를 표준으로 채택한 이유입니다.