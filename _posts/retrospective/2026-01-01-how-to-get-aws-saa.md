---
title: "[자격증 취득] AWS SAA-C03 합격 후기"
categories: 회고
excerpt: "2주 동안 AWS SAA-C03 자격증을 준비하고 취득한 솔직한 후기입니다. 방대한 양의 공부의 부담감을 줄여준 공부법을 공유합니다."
description: "2주 동안 AWS SAA-C03 자격증을 준비하고 취득한 솔직한 후기입니다. 방대한 양의 공부의 부담감을 줄여준 공부법과 Gemini 활용법을 공유합니다.(AI 활용, 아키텍처 시각화)"
tags: [AWS, SAA, SAA-C03, 후기, 회고, 합격, 취득, 넥스트클라우드, nxtcloud, 강원대학교, 강원대, KNU]
#redirect_from: #이전주소 입력
#search: false #만약 이 글이 검색되지 않기를 바란다면
#use_math: true #수식이 필요한 경우만 사용
---

<img src="{{site.url}}/assets/images/2026-01-01-how-to-get-aws-saa/recruitment-announcement-for-aws-saa-training-program.jpeg" 
     alt="recruitment-announcement-for-aws-saa-training-program" 
     style="display: block; margin: 2rem auto; max-width: 50%; height: auto;">

<div class="story-box" markdown="1">

최근 학교에서 진행한 교육 과정을 통해 **AWS Certified Solutions Architect – Associate (SAA-C03)** 자격증을 취득하게 되었습니다. 준비 계기부터 합격까지, <strong class="highlight-text">2주간의 과정</strong>을 공유하고자 합니다.

</div>

# 1. 지원 동기

<div class="story-box" markdown="1">

## 미지의 영역으로 남겨두었던 '인프라'라는 숙제

예전에 해커톤에서 EC2로 서버를 배포해본 적이 있습니다. 하지만 당시 제 목표는 오로지 '배포 성공'이었고, 보안 그룹 설정이나 네트워크 구성이 왜 그렇게 되어야 하는지 깊게 고민하지 않았습니다. 그렇게 인프라 영역은 늘 <strong class="highlight-text">언젠가 공부해야 할 미지의 영역</strong>으로 남겨뒀습니다. 당장은 백엔드 지식을 습득하는 것이 더 우선이라고 생각하며 미뤄왔습니다.

하지만 최근 개인 프로젝트(쿠폰 어드민)를 진행하며 그 생각은 완전히 바뀌었습니다.

## "서버가 늘어나면, 업로드한 파일은 어디로 가야 하지?"

<div class="info-box">
<div style="border: 1px solid #e1e4e8; border-radius: 12px; padding: 30px 20px; background-color: #f8f9fa; font-family: sans-serif; margin: 20px 0;">
    <div style="display: flex; justify-content: center; align-items: stretch; gap: 30px; flex-wrap: wrap;">
        
        <div style="flex: 1 1 160px; max-width: 180px; border: 2px solid #007bff; border-radius: 8px; padding: 15px; background: white; display: flex; flex-direction: column; align-items: center; justify-content: flex-start;">
            <div style="font-size: 28px; margin-bottom: 5px;">🖥️</div>
            <div style="font-weight: bold; font-size: 14px; color: #333;">Server A</div>
            <div style="color: #6c757d; font-size: 11px; margin-bottom: 10px;">(기존 서버)</div>
            <div style="background: #e7f3ff; border: 1px dashed #007bff; border-radius: 4px; font-size: 11px; padding: 8px 5px; margin-top: auto; color: #007bff; width: 100%; text-align: center; box-sizing: border-box; font-weight: bold;">
                ✅ users.csv 있음
            </div>
        </div>

        <div style="flex: 1 1 160px; max-width: 180px; border: 2px solid #dc3545; border-radius: 8px; padding: 15px; background: white; display: flex; flex-direction: column; align-items: center; justify-content: flex-start;">
            <div style="font-size: 28px; margin-bottom: 5px; opacity: 0.7;">🖥️</div>
            <div style="font-weight: bold; font-size: 14px; color: #333;">Server B</div>
            <div style="color: #6c757d; font-size: 11px; margin-bottom: 10px;">(추가 서버 1)</div>
            <div style="background: #fff5f5; border: 1px solid #dc3545; border-radius: 4px; font-size: 11px; padding: 8px 5px; margin-top: auto; color: #dc3545; font-weight: bold; width: 100%; text-align: center; box-sizing: border-box;">
                ❌ 파일 없음
            </div>
        </div>
        
         <div style="flex: 1 1 160px; max-width: 180px; border: 2px solid #dc3545; border-radius: 8px; padding: 15px; background: white; display: flex; flex-direction: column; align-items: center; justify-content: flex-start;">
            <div style="font-size: 28px; margin-bottom: 5px; opacity: 0.7;">🖥️</div>
            <div style="font-weight: bold; font-size: 14px; color: #333;">Server C</div>
            <div style="color: #6c757d; font-size: 11px; margin-bottom: 10px;">(추가 서버 2)</div>
            <div style="background: #fff5f5; border: 1px solid #dc3545; border-radius: 4px; font-size: 11px; padding: 8px 5px; margin-top: auto; color: #dc3545; font-weight: bold; width: 100%; text-align: center; box-sizing: border-box;">
                ❌ 파일 없음
            </div>
        </div>
        
    </div>
</div>
</div>

파일 업로드 API를 구현하다 문득 고민이 들었습니다. '지금은 서버가 한 대지만, 나중에 트래픽이 늘어나 <strong class="highlight-text">서버를 여러 대로 확장하게 되면</strong> 다른 서버에 저장된 파일을 어떻게 불러와야 하지?'

서버의 상태에 의존하지 않는 안정적인 환경을 만들기 위해 **외부 공용 스토리지**가 필요하다는 결론에 도달했고, 자연스럽게 AWS S3를 접하게 되었습니다. 이 과정에서 Presigned URL을 통해 무거운 파일 전송은 S3에 위임하고, 서버는 인증만 담당하여 최적화 시키는 구조를 접했습니다. <strong class="highlight-text">설계 하나로 서비스 전체 안정성이 높아지는 것</strong>을 직접 보며, 인프라 지식의 중요성을 실감할 수 있었습니다.

## "돌아가는 서버"를 넘어 "이유 있는 설계"로

저는 단순히 주어진 도구를 사용하는 수준에서 더 나아가고 싶었습니다. 앞으로 마주할 데이터베이스 병목 현상, 복잡한 네트워크 보안, 그리고 전역적인 서비스 가용성 확보까지. 수많은 변수 속에서 <strong class="highlight-text">"이것이 최선인가?"</strong>라는 질문에 스스로 답할 수 있는 능력을 갖추고 싶었습니다. 어떤 비즈니스 요구사항을 마주하더라도, 상황에 맞는 **기술적 근거**를 바탕으로 최선의 아키텍처를 고민해보고 싶었습니다.

그러던 중 학교에서 운영하는 AWS SAA 자격증 교육 프로그램 공고를 보고 망설임 없이 지원했습니다. 자격증 공부를 통해 AWS가 제시하는 **클라우드 설계 권장 사례(Best Practice)**를 학습하며, 가용성·비용·성능 사이의 <strong class="highlight-text">트레이드오프(Trade-off)</strong>를 고려해 최적의 구조를 찾아가는 사고 과정을 익히고 싶었습니다.

</div>

<br>

# 2. 공부법

<div style="display: flex; gap: 10px; justify-content: center; align-items: flex-start; margin: 2rem auto; max-width: 100%;">

  <img src="{{site.url}}/assets/images/2026-01-01-how-to-get-aws-saa/saa-textbook-by-nxtcloud.png" 
       alt="saa-textbook-by-nxtcloud" 
       style="width: 48%; height: auto; flex-shrink: 0;">

  <img src="{{site.url}}/assets/images/2026-01-01-how-to-get-aws-saa/certinavigator-by-nxtcloud.png" 
       alt="certinavigator-by-nxtcloud" 
       style="width: 48%; height: auto; flex-shrink: 0;">

</div>

<div class="story-box" markdown="1">

단순히 자격증만 딸 생각이었다면 3일컷도 가능했겠지만, 앞서 말한 개인적인 욕심 때문에 꼼꼼하게 학습하고 싶어, <strong class="highlight-text">매일 6~8시간씩 총 2주</strong>가 걸렸습니다.

- 교재: nxtcloud에서 제공한 노션 페이지
- 문제집 : nxtcloud에서 제공한 CertiNavigator (총 958문제의 덤프)
- 전략: 하루 80문제씩 풀이 + 모든 선지의 이유 분석.

## ① 간단한 시나리오 → AI를 활용한 키워드 공식 만들기

SAA 문제는 시나리오가 길기에 자칫하면 핵심 요구사항을 놓치기 쉬웠습니다. 긴 지문을 반복해서 읽다 보니 집중력은 금방 떨어질 것 같았고, 줄어들지 않는 남은 문제 양을 보며 이대로는 공부 의지가 꺾일 것 같은 불안감이 들었습니다. 

이를 극복하기 위해, 저는 Gemini에게 <strong class="highlight-text">"이 문제에서 정답이 될 수밖에 없는 결정적 키워드를 뽑아줘"</strong>라고 요청했고, 특정 상황(예: 실시간 처리, 비용 절감, 가용성 보장 등)에 맞는 **서비스 매칭 공식**을 만들었습니다.

<div style="font-family: 'Pretendard', -apple-system, BlinkMacSystemFont, system-ui, Roboto, sans-serif; display: flex; flex-direction: column; align-items: center; gap: 16px; margin: 2.5rem 0; width: 100%;">
  
  <div style="display: flex; align-items: center; gap: 18px; flex-wrap: wrap; justify-content: center;">
    <div style="background: #f8fafc; border: 1px solid #e2e8f0; color: #475569; padding: 6px 12px; border-radius: 6px; font-weight: 600; font-size: 0.85rem; white-space: nowrap;">
      자주 액세스하지 않는 데이터
    </div>
    
    <span style="color: #cbd5e1; font-weight: bold; font-size: 1rem;">+</span>
    
    <div style="background: #f8fafc; border: 1px solid #e2e8f0; color: #475569; padding: 6px 12px; border-radius: 6px; font-weight: 600; font-size: 0.85rem; white-space: nowrap;">
      비용 효율적인 저장
    </div>
    
    <span style="color: #94a3b8; font-weight: bold; font-size: 1.2rem;">→</span>
    
    <div style="background: #334155; color: white; padding: 7px 16px; border-radius: 6px; font-weight: 700; font-size: 0.9rem; min-width: 120px; text-align: center; box-shadow: 0 2px 4px rgba(0,0,0,0.1);">
      S3 Glacier
    </div>
  </div>

  <div style="display: flex; align-items: center; gap: 18px; flex-wrap: wrap; justify-content: center;">
    <div style="background: #f8fafc; border: 1px solid #e2e8f0; color: #475569; padding: 6px 12px; border-radius: 6px; font-weight: 600; font-size: 0.85rem; white-space: nowrap;">
      읽기 성능 향상
    </div>
    
    <span style="color: #cbd5e1; font-weight: bold; font-size: 1rem;">+</span>
    
    <div style="background: #f8fafc; border: 1px solid #e2e8f0; color: #475569; padding: 6px 12px; border-radius: 6px; font-weight: 600; font-size: 0.85rem; white-space: nowrap;">
      관리형 DB
    </div>
    
    <span style="color: #94a3b8; font-weight: bold; font-size: 1.2rem;">→</span>
    
    <div style="background: #334155; color: white; padding: 7px 16px; border-radius: 6px; font-weight: 700; font-size: 0.9rem; min-width: 120px; text-align: center; box-shadow: 0 2px 4px rgba(0,0,0,0.1);">
      RDS Read Replica
    </div>
  </div>

</div>

이렇게 키워드를 매칭한 공식을 수십 개 만들어 두니, 길고 복잡한 문제도 핵심만 빠르게 간파할 수 있었습니다.

## ② 복잡한 시나리오 → 문제 상황(아키텍처) 시각화

<img src="{{site.url}}/assets/images/2026-01-01-how-to-get-aws-saa/visualizing-the-problem-architecture-by-gemini.png" 
     alt="visualizing-the-problem-architecture-by-gemini" 
     style="display: block; margin: 2rem auto; max-width: 90%; height: auto;">

단순히 키워드만으로는 해결되지 않는 문제들도 있었습니다. 특히 여러 서비스가 복잡하게 얽힌 시나리오에서는 글만 읽어서는 전체 구조를 파악하기 어려웠습니다. 

이에 실제 실무에서 마주할 수 있는 시나리오라고 생각하며, 문제에서 묘사하는 <strong class="highlight-text">인프라 구조를 직접 손으로 그려보며</strong> 감을 익혔습니다. 그리고 제가 상황을 올바르게 이해했는지 확인하기 위해, Gemini에게 시각화를 부탁한 후 비교해보며 부족한 부분을 채워 나갔습니다.

이 과정은 단순히 정답을 맞히는 것을 넘어, 앞으로 문제를 읽을 때마다 <strong class="highlight-text">시스템이 어떻게 구성되고 데이터가 어떻게 흐르는지</strong> 직관적으로 이해하는 데 큰 도움이 되었습니다. 또 각 서비스가 해결하려는 문제 상황을 명확하게 이해하고 나니, 그냥 무작정 서비스 이름과 기능을 외우는 것보다, <strong class="highlight-text">서비스의 존재 이유와 쓰임새</strong>가 훨씬 더 명확하게 와닿았습니다.

## ③ 오답노트 : 모든 선지의 근거 찾기

<img src="{{site.url}}/assets/images/2026-01-01-how-to-get-aws-saa/rationale-analysis-for-all-options-by-gemini.png" 
     alt="rationale-analysis-for-all-options-by-gemini" 
     style="display: block; margin: 2rem auto; max-width: 90%; height: auto;">

<img src="{{site.url}}/assets/images/2026-01-01-how-to-get-aws-saa/mistake-review-notes-for-aws-saa.png" 
     alt="mistake-review-notes-for-aws-saa" 
     style="display: block; margin: 2rem auto; max-width: 90%; height: auto;">

단순히 답을 맞히는 게 아니라, 왜 A는 답이 되고 B는 답이 될 수 없는지 <strong class="highlight-text">모든 선지의 근거</strong>를 분석하는 과정을 거쳤습니다. 막히는 부분은 Gemini의 상세한 풀이를 참고했고, 그 과정에서 새롭게 알게 된 내용은 즉시 오답노트에 기록했습니다. 

매일 6~8시간이 소요될 만큼 공부 시간이 길어졌지만, 각 서비스 간 <strong class="highlight-text">미묘한 차이를 정확히 변별하는 힘</strong>을 기를 수 있었습니다. 덕분에 학습이 누적될수록 오답노트에 적을 내용은 줄어들었고, 문제 푸는 속도는 자연스럽게 빨라졌습니다.

## ④ 시험 D-1 : 총 정리

시험 전날에는 새로운 지식을 더하진 않고, **지금까지 학습한 내용**을 완벽히 내 것으로 만드는데 집중했습니다.

### 오답노트 복습 - 공식 최종 보완

<img src="{{site.url}}/assets/images/2026-01-01-how-to-get-aws-saa/identifying-weaknesses-through-mistake-review-notes-by-gemini.png" 
     alt="identifying-weaknesses-through-mistake-review-notes-by-gemini" 
     style="display: block; margin: 2rem auto; max-width: 90%; height: auto;">

2주간 꾸준히 작성한 오답노트는 저의 **약점이 집약된 데이터베이스**라고 생각했습니다. 누적된 오답노트 내용을 Gemini에게 보여주며 <strong class="highlight-text">"내 취약점을 분석하고, 어떤 개념을 보강해야 하는지 알려줘"</strong>라고 요청했습니다. 이를 통해 자주 혼동하는 개념이나 놓치는 부분을 다시 한번 정확히 정리하며 공식을 보완했습니다.

### 전체 인프라 관점에서의 큰 그림 그려보기

<div class="info-box">
<div style="width: 800px; margin: 0 auto; font-family: 'Pretendard', -apple-system, sans-serif; letter-spacing: -0.02em; color: #374151;">
  
  <div style="display: flex; align-items: stretch; gap: 6px; margin-bottom: 8px;">
    
    <div style="flex: 1; background: #f9fafb; border: 1px solid #e5e7eb; border-radius: 6px; padding: 10px 4px; text-align: center; box-shadow: 0 1px 2px rgba(0,0,0,0.03);">
      <div style="font-weight: 800; font-size: 0.8rem; color: #111827; margin-bottom: 4px;">0. 가속</div>
      <div style="font-size: 0.7rem; color: #6b7280; line-height: 1.4; height: 32px; display: flex; flex-direction: column; justify-content: center;">
        Route 53<br>CloudFront
      </div>
      <div style="font-size: 0.65rem; color: #111827; font-weight: 700; margin-top: 6px; border-top: 1px solid #f3f4f6; padding-top: 4px;">접속 경로 가속</div>
    </div>

    <div style="align-self: center; color: #d1d5db; font-size: 12px;">▶</div>

    <div style="flex: 1; background: #f9fafb; border: 1px solid #e5e7eb; border-radius: 6px; padding: 10px 4px; text-align: center; box-shadow: 0 1px 2px rgba(0,0,0,0.03);">
      <div style="font-weight: 800; font-size: 0.8rem; color: #111827; margin-bottom: 4px;">1. 기초</div>
      <div style="font-size: 0.7rem; color: #6b7280; line-height: 1.4; height: 32px; display: flex; flex-direction: column; justify-content: center;">
        VPC · IAM<br>GW · EP
      </div>
      <div style="font-size: 0.65rem; color: #111827; font-weight: 700; margin-top: 6px; border-top: 1px solid #f3f4f6; padding-top: 4px;">네트워크 망 구축</div>
    </div>

    <div style="align-self: center; color: #d1d5db; font-size: 12px;">▶</div>

    <div style="flex: 1; background: #f9fafb; border: 1px solid #e5e7eb; border-radius: 6px; padding: 10px 4px; text-align: center; box-shadow: 0 1px 2px rgba(0,0,0,0.03);">
      <div style="font-weight: 800; font-size: 0.8rem; color: #111827; margin-bottom: 4px;">2. 실행</div>
      <div style="font-size: 0.7rem; color: #6b7280; line-height: 1.4; height: 32px; display: flex; flex-direction: column; justify-content: center;">
        EC2 · ASG<br>EBS · Fargate
      </div>
      <div style="font-size: 0.65rem; color: #111827; font-weight: 700; margin-top: 6px; border-top: 1px solid #f3f4f6; padding-top: 4px;">연산 자원 생성</div>
    </div>

    <div style="align-self: center; color: #d1d5db; font-size: 12px;">▶</div>

    <div style="flex: 1; background: #f9fafb; border: 1px solid #e5e7eb; border-radius: 6px; padding: 10px 4px; text-align: center; box-shadow: 0 1px 2px rgba(0,0,0,0.03);">
      <div style="font-weight: 800; font-size: 0.8rem; color: #111827; margin-bottom: 4px;">3. 저장</div>
      <div style="font-size: 0.7rem; color: #6b7280; line-height: 1.4; height: 32px; display: flex; flex-direction: column; justify-content: center;">
        S3 · RDS<br>DynamoDB
      </div>
      <div style="font-size: 0.65rem; color: #111827; font-weight: 700; margin-top: 6px; border-top: 1px solid #f3f4f6; padding-top: 4px;">데이터 저장소</div>
    </div>
  </div>

  <div style="display: flex; align-items: stretch; gap: 6px;">
    
    <div style="flex: 1; background: #f9fafb; border: 1px solid #e5e7eb; border-radius: 6px; padding: 10px 4px; text-align: center; box-shadow: 0 1px 2px rgba(0,0,0,0.03);">
      <div style="font-weight: 800; font-size: 0.8rem; color: #111827; margin-bottom: 4px;">4. 연계</div>
      <div style="font-size: 0.7rem; color: #6b7280; line-height: 1.4; height: 32px; display: flex; flex-direction: column; justify-content: center;">
        Lambda · ALB<br>SQS · SNS
      </div>
      <div style="font-size: 0.65rem; color: #111827; font-weight: 700; margin-top: 6px; border-top: 1px solid #f3f4f6; padding-top: 4px;">서비스 연결 최적화</div>
    </div>

    <div style="align-self: center; color: #d1d5db; font-size: 12px;">▶</div>

    <div style="flex: 1; background: #f9fafb; border: 1px solid #e5e7eb; border-radius: 6px; padding: 10px 4px; text-align: center; box-shadow: 0 1px 2px rgba(0,0,0,0.03);">
      <div style="font-weight: 800; font-size: 0.8rem; color: #111827; margin-bottom: 4px;">5. 관리</div>
      <div style="font-size: 0.7rem; color: #6b7280; line-height: 1.4; height: 32px; display: flex; flex-direction: column; justify-content: center;">
        CloudWatch<br>CloudTrail
      </div>
      <div style="font-size: 0.65rem; color: #111827; font-weight: 700; margin-top: 6px; border-top: 1px solid #f3f4f6; padding-top: 4px;">상태 감시 및 기록</div>
    </div>

    <div style="align-self: center; color: #d1d5db; font-size: 12px;">▶</div>

    <div style="flex: 1; background: #f3f4f6; border: 1px solid #d1d5db; border-radius: 6px; padding: 10px 4px; text-align: center; box-shadow: 0 1px 2px rgba(0,0,0,0.03);">
      <div style="font-weight: 800; font-size: 0.8rem; color: #111827; margin-bottom: 4px;">6. 보안</div>
      <div style="font-size: 0.7rem; color: #4b5563; line-height: 1.4; height: 32px; display: flex; flex-direction: column; justify-content: center;">
        KMS · WAF<br>Secrets Mgr
      </div>
      <div style="font-size: 0.65rem; color: #111827; font-weight: 700; margin-top: 6px; border-top: 1px solid #d1d5db; padding-top: 4px;">인프라 보안 강화</div>
    </div>

    <div style="align-self: center; color: #d1d5db; font-size: 12px;">▶</div>

    <div style="flex: 1; background: #ffffff; border: 1px dashed #d1d5db; border-radius: 6px; padding: 10px 4px; text-align: center; display: flex; align-items: center; justify-content: center;">
      <div style="font-weight: 700; font-size: 0.75rem; color: #9ca3af;">Architecture<br>Complete</div>
    </div>

  </div>
</div>
</div>

1장부터 16장까지의 개별 서비스를 넘어, 이들이 어떻게 <strong class="highlight-text">유기적으로 연결</strong>되어 하나의 완성된 인프라를 구성하는지 큰 그림을 그리며 정리했습니다. 이를 통해 **이름이 비슷해서 헷갈리는 개념**(예: Global Accelerator vs Transfer Accelerator) 또한 정리하며 확실히 구분할 수 있었습니다.

</div>

<br>

# 3. 느낀점 <span class="title-sub-desc">: 자격증, 그 이상의 가치를 얻으며</span>

<img src="{{site.url}}/assets/images/2026-01-01-how-to-get-aws-saa/saa-certi.png" 
     alt="saa-certi" 
     style="display: block; margin: 2rem auto; max-width: 90%; height: auto;">

<div class="story-box" markdown="1">

자격증을 준비하며, 단순히 AWS 서비스의 종류를 익히는 데 그치지 않고, 백엔드 개발자로서 한 단계 더 넓은 시야를 갖게 되었습니다. 구체적으로는 다음과 같은 점들을 얻을 수 있었습니다.

* **기술적 근거 제시 능력** : 이전에는 막연했던 기술 선택의 기준이 명확해졌습니다. 예를 들어, 이전에는 "빠른 데이터 조회"라는 요구사항에 Redis 캐시만을 떠올렸다면, 지금은 상황에 맞춰 **DAX(DynamoDB Accelerator)**를 도입할지, **ElastiCache(Redis/Memcached)**를 배치할지, 혹은 **RDS의 Read Replica**를 확장할지 등 <strong class="highlight-text">비용과 성능 사이의 트레이드오프(Trade-off)</strong>를 고려하며 기술적 근거를 제시할 수 있게 되었습니다.

* **성능 중심에서 고가용성과 내결함성을 고려하는 설계로** : 이전에는 주로 "어떻게 하면 더 빠른 응답 속도를 낼까?"와 같은 성능 최적화에만 집중해 왔습니다. 하지만 이번 과정을 통해 아무리 빠른 시스템이라도 장애 상황에서 버티지 못한다면 그 가치가 퇴색된다는 점을 배웠습니다. 특히 **SQS(Simple Queue Service)**를 활용해 컴포넌트 간의 의존성을 줄이고, 갑작스러운 트래픽 폭증이나 장애 상황에서도 요청을 유실하지 않는 <strong class="highlight-text">내결함성 설계의 중요성</strong>을 체감했습니다. 이제는 단순히 처리 속도뿐만 아니라, Multi-AZ 배포나 버퍼링 구조를 통해 어떤 상황에서도 <strong class="highlight-text">'중단 없이 지속되는 서비스'</strong>를 만들기 위한 아키텍처를 함께 고민하게 되었습니다.

저처럼 인프라 공부를 미뤄웠던 백엔드 개발자분이 있다면, 자격증 취득 겸 한번 도전 해보시는 것을 추천합니다. 내가 짠 코드가 돌아가는 <strong class="highlight-text">'서버 너머의 세상'</strong>을 이해하는 것만으로도, 개발을 바라보는 시야가 훨씬 넓어지는 것을 느끼실 수 있을 것입니다.

</div>