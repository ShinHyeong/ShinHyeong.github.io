---
title: "[AWS 해커톤] 깃허브 코드 기반 자소서 검증 서비스 'FactCheckAI' 개발 및 수상 후기"
categories: 회고
excerpt: "🏆 2025 AWS 해커톤 (GWNU X KNU) 우수상 수상 ! 아이디어 구체화부터 MVP 개발, 그리고 발표 전략의 아쉬움과 배운 점에 대해 회고했습니다."
description: "2025 AWS 해커톤 우수상 수상 프로젝트 'FactCheckAI' 회고. 기술 면접 준비의 불편함을 해소하기 위해 LLM을 활용한 자소서-코드 진위 검증 서비스를 개발했습니다. 아이디어 구체화부터 MVP 개발, 그리고 발표 전략의 아쉬움과 배운 점을 정리했습니다."
tags: [AWS, 해커톤, 수상, 수상작, 후기, 회고, 넥스트클라우드, nxtcloud, 강원대학교, 강원대, 모의면접]
#redirect_from: #이전주소 입력
#search: false #만약 이 글이 검색되지 않기를 바란다면
#use_math: true #수식이 필요한 경우만 사용
---

<img src="{{site.url}}/assets/images/2025-11-28-2025-aws-hackaton-retrospective/2025-aws-hackaton-info.jpeg" 
     alt="2025 AWS Hackathon Info" 
     style="display: block; margin: 2rem auto; max-width: 90%; height: auto;">

<div style="max-width: 800px; margin: 1.5rem auto 2.5rem auto; background: #f8fafc; border: 1px solid #e2e8f0; border-radius: 8px; padding: 1rem; text-align: center; color: #334155; font-family: -apple-system, sans-serif;">
  
  <div style="display: flex; flex-direction: column; gap: 0.5rem; justify-content: center; align-items: center;">
    
    <div style="font-size: 0.95rem; font-weight: 700; color: #1e293b; display: flex; align-items: center; gap: 6px;">
      <span style="font-size: 1.1rem;">🗓️</span> 2025.11.22 - 2025.11.23 <span style="font-weight: 400; color: #64748b; font-size: 0.85rem;">(무박 2일)</span>
    </div>
    
    <div style="width: 40px; height: 1px; background: #cbd5e1; margin: 0.2rem 0;"></div>

    <div style="font-size: 0.95rem; font-weight: 700;display: flex; align-items: center; gap: 6px;">
      <span style="font-size: 1.1rem;">✏️</span> 2025 강릉원주대학교 X 강원대학교 AWS 해커톤
    </div>

  </div>

</div>

# 1. 아이디어 구상 : '공급자'가 아닌 '사용자'의 시선으로

<div class="story-box" markdown="1">

솔직히 고백하자면, 해커톤 시작 전엔 내 머릿속엔 '수상실적' 네 글자뿐이었다. 하지만 해커톤 시작 전, 넥스트클라우드 최민철 멘토님의 말씀은 이런 내 마음을 완전히 바꾸었다.

> "’공급자'의 마음가짐으로 개발하면 안됩니다."

멘토님은 개발자가 실제 사용자의 환경(돈, 시간적 여유 등)을 고려하지 않고 기능 구현에만 매몰되는 현상을 지적하셨다. '자취생 레시피 앱'을 예로 들자면, 자취생들이 요리를 안 하는 건 레시피를 몰라서가 아니라, 돈과 여유가 없어서인데 개발자들은 "기능 좋은 앱을 만들었는데 왜 안 쓰지?"라며 유저 탓을 한다는 것이다. 이렇게 실제 통계와 유저의 상황을 보지 않고 반문하는 것은 <strong class="highlight-text">개발자의 아집이나 다름없다</strong>는 그 말이 마음을 울렸다.

이 조언을 기점으로 목표를 수정했다. 수상 여부를 떠나 <strong class="highlight-text">"내가 당장 필요하고, 쓰고 싶은 서비스"</strong>라는 본질에 집중하기로 했다. 그렇게 **FactCheckAI**를 구상하게 되었다. 

</div>

## <span class="title-with-dot">FactCheckAI</span> <span class="title-sub-desc">: 깃허브 코드 기반 자소서 검증 서비스</span>

<div class="story-box" markdown="1">

나는 기술면접을 종종 볼 기회가 있어 LLM으로 면접 예상 질문을 뽑아서 연습한다. 그런데 단순 CS 지식 확인 면접말고 포트폴리오로 진행되는 면접은 매번 여러 개의 코드를 일일이 복사해야하고 LLM 프롬프트 조건을 상세하게 입력해야 한다는 게 매우 번거로웠었다. 그래서 이 불편함을 내가 직접 해결해보고자 이 아이디어를 생각하게 되었다.

간단하게 설명하자면, 다음과 같은 핵심 기능을 제공한다.

<div class="info-box">
<div style="display: flex; flex-wrap: wrap; gap: 10px; margin-bottom: 1.5rem;">
    
    <div style="flex: 1; min-width: 200px; background: #f8fafc; border: 1px solid #e2e8f0; border-radius: 8px; padding: 1rem; text-align: center; color: #1e293b; font-weight: 700; box-shadow: 0 1px 2px rgba(0,0,0,0.05);">
      🔍<br>진위 판독
    </div>

    <div style="flex: 1; min-width: 200px; background: #f8fafc; border: 1px solid #e2e8f0; border-radius: 8px; padding: 1rem; text-align: center; color: #1e293b; font-weight: 700; box-shadow: 0 1px 2px rgba(0,0,0,0.05);">
      🤖<br>맞춤형 질문 생성
    </div>

    <div style="flex: 1; min-width: 200px; background: #f8fafc; border: 1px solid #e2e8f0; border-radius: 8px; padding: 1rem; text-align: center; color: #1e293b; font-weight: 700; box-shadow: 0 1px 2px rgba(0,0,0,0.05);">
      💬<br>꼬리 질문 심층 면접
    </div>

  </div>

  <div style="color: #334155; line-height: 1.7; margin-bottom: 2rem; padding: 0 5px;">
    <p style="margin-bottom: 0.8rem;">
      <strong>🔍 진위 판독</strong>
      <p style="padding-left:2rem;">"자소서엔 있는데 코드엔 없다?"<br>지원자의 주장과 깃허브 코드를 대조해 구현 여부를 4단계로 꼼꼼하게 검증함.</p>
    </p>
    <p style="margin-bottom: 0.8rem;">
      <strong >🤖 맞춤형 질문 생성</strong><br>
      <p style="padding-left:2rem;">단순한 공통 질문이 아니라, JD(직무 기술서)와 제출된 코드를 정밀 분석해<br>면접관이 던질 법한 날카로운 질문을 생성함.</p>
    </p>
    <p style="margin-bottom: 0;">
      <strong>💬 심층 면접 시뮬레이션</strong><br>
      <p style="padding-left:2rem;">단답형 답변으로 끝나지 않도록, 꼬리물기 질문을 계속 던져<br>실전 압박 면접을 완벽하게 대비할 수 있음.</p>
    </p>
  </div>
  
</div>

<div style="display: flex; flex-wrap: wrap; gap: 10px;">
  
  <a href="https://github.com/saa-hackathon-2025/factcheck" target="_blank" style="flex: 1; min-width: 250px; text-decoration: none; display: flex; align-items: center; justify-content: space-between; background: #fff; border: 1px solid #cbd5e1; border-radius: 6px; padding: 0.7rem 1rem; transition: background 0.2s;">
    <div style="display: flex; flex-direction: column;">
      <span style="font-size: 0.9rem; font-weight: 700; color: #1e293b;">🚀 Final Version</span>
      <span style="font-size: 0.75rem; color: #64748b;">최종 제출 버전 (Submission)</span>
    </div>
    <div style="font-size: 1rem; color: #94a3b8;">↗</div>
  </a>

</div>
</div>

<br>

# 2. 잘했던 점(Keep) : 과거의 실패를 '철저한 준비'로

<div class="story-box" markdown="1">

팀 내 아이디어 회의 끝에, 2가지 후보로 추려졌다.

<div style="font-family: 'Apple SD Gothic Neo', sans-serif; font-size: 0.8rem; letter-spacing: -0.03em; line-height: 1.4; max-width: 800px; margin: 1.5rem auto; color: #334155;">

  <div style="background: #f8fafc; border: 1px solid #e2e8f0; border-radius: 0.6rem; padding: 0.8rem;">
    <div style="font-weight: 800; color: #1e293b; margin-bottom: 0.8rem; text-align: center; font-size: 0.9rem;">
      ⚖️ 아이디어 선정 기준 비교
    </div>
    
    <div style="display: grid; grid-template-columns: 1.2fr 1fr 1fr; gap: 0.4rem; text-align: center; align-items: stretch;">
      
      <div style="font-weight: 700; color: #64748b; font-size: 0.75rem; padding-bottom: 0.4rem; border-bottom: 2px solid #cbd5e1;">아이디어</div>
      <div style="font-weight: 700; color: #64748b; font-size: 0.75rem; padding-bottom: 0.4rem; border-bottom: 2px solid #cbd5e1;">이전 수상 사례</div>
      <div style="font-weight: 700; color: #64748b; font-size: 0.75rem; padding-bottom: 0.4rem; border-bottom: 2px solid #cbd5e1;">13시간 내 구현</div>

      <div style="font-size: 0.75rem; color: #334155; padding: 0.8rem 0.4rem; display: flex; align-items: center; justify-content: center; flex-direction: column; border-radius: 4px 0 0 4px;">
        <strong>사기탐지 AI</strong>
        <span style="color:#64748b; font-size:0.9em; margin-top:2px;">(이미지, 피싱)</span>
      </div>
      <div style="font-size: 0.75rem; color: #475569; padding: 0.8rem 0.4rem; display: flex; align-items: center; justify-content: center;">
        🟢 많음 (6건)
      </div>
      <div style="font-size: 0.75rem; color: #475569; padding: 0.8rem 0.4rem; display: flex; align-items: center; justify-content: center; border-radius: 0 4px 4px 0;">
        🟢 이전 해커톤 수상 사례가<br>많으니 가능할지도..?
      </div>

      <div style="font-size: 0.75rem; color: #1e3a8a; padding: 0.8rem 0.4rem; background: #eff6ff; display: flex; align-items: center; justify-content: center; flex-direction: column; font-weight: 700; border-radius: 4px 0 0 4px;">
        자소서 검증
        <span style="color:#3b82f6; font-size:0.9em; margin-top:2px; font-weight:400;">(내 아이디어)</span>
      </div>
      <div style="font-size: 0.75rem; color: #1e40af; padding: 0.8rem 0.4rem; background: #eff6ff; display: flex; align-items: center; justify-content: center; font-weight: 500;">
        🔺 적음 (2건)
      </div>
      <div style="font-size: 0.75rem; color: #1e40af; padding: 0.8rem 0.4rem; background: #eff6ff; display: flex; align-items: center; justify-content: center; font-weight: 800; border-radius: 0 4px 4px 0;">
        🔺 미지수 (애매함)
      </div>

    </div>
  </div>
</div>

사기 탐지 AI는 구현이 쉽지만, 이미 대기업도 개발중인 서비스라 차별성이 부족했다. 내 아이디어는 매력적이라는 평을 받았지만 **'13시간 내 구현 가능성'**이 미지수였다. 나 스스로도 13시간 내로 구현이 가능할지 확신이 없었다. 

2년 전, ‘2023 SW중심대학 해커톤’이라는 전국 단위 해커톤에 참가했을 때 악몽이 떠올랐다. 아이디어 스펙을 욕심낸 탓에 개발 시간이 부족했고, 그 시간을 벌기 위해 무리하게 밤을 새우다 보니 컨디션 난조로 개발 속도가 오히려 더뎌지는 악순환을 겪었다. 아이디어를 축소시켜 겨우 기능은 구현했지만 발표 무대에서는 체력 저하로 준비한 만큼의 역량을 보여주지 못해 크게 아쉬움이 남았었다. 이번 해커톤은 <strong class="highlight-text">기술적 불확실성을 미리 제거</strong>함으로써 절대 무리하고 싶지 않았다. 해커톤을 즐기고 싶었다.

그래서 나는 <strong class="highlight-text">가장 구현이 까다로운 핵심 기능들을 미리 프로토타이핑</strong>해봄으로써 나 스스로도 확신을 얻고, 팀원 또한 설득시키고자 했다. 다행히 프로토타이핑은 성공했고 그렇게 나의 아이디어가 최종 선정되었다. 

이 선택은 의외로 우리 팀의 **강력한 심리적 안전장치**가 되기도 했다. 해커톤 초반, 다른 팀들이 기술 스택을 정하고 아이디어 브레인 스토밍을 하느라 우왕좌왕할 때, 우리는 빠르게 핵심 기능을 개발하고, QA와 서비스의 완성도를 높이는 데 온전히 집중할 수 있었다. <strong class="highlight-text">과거의 실패가 전화위복이 되어 우리 팀의 큰 경쟁력이 된 순간이었다.</strong>

</div>

<div class="icon-box">
  <div style="display: flex; flex-wrap: wrap; gap: 10px;">
    
    <a href="https://github.com/ShinHyeong/FactCheckAi" target="_blank" 
   style="flex: 1; min-width: 250px; text-decoration: none; display: flex; align-items: center; justify-content: space-between; border: 1px solid #cbd5e1; border-radius: 6px; padding: 0.7rem 1rem; transition: all 0.2s ease-in-out;">
      <div class="btn-text-group" style="display: flex; flex-direction: column;">
        <span class="btn-title" style="font-size: 0.9rem; font-weight: 700; color: #1e293b;">🧪 MVP Version</span>
        <span class="btn-sub" style="font-size: 0.75rem; color: #64748b;">초기 프로토타입 (Prototype)</span>
      </div>
      <div class="btn-icon" style="font-size: 1rem; color: #94a3b8;">↗</div>
    </a>

  </div>
</div>

<br>

# 3. 아쉬웠던 점(Problem) : 말 대신 PPT에 친절함을

<img src="{{site.url}}/assets/images/2025-11-28-2025-aws-hackaton-retrospective/2025-aws-hackaton-speech.jpeg" 
     alt="2025 AWS Hackathon Speech" 
     style="display: block; margin: 2rem auto; max-width: 70%; height: auto;">

<div class="story-box" markdown="1">

그렇다고 모든 과정이 계획대로 흘러간 것은 아니었다. 앞서 말한 선택 덕분에 핵심 기능 구현은 예상보다 훨씬 빠르게 끝났다. 그러나 발표 준비 과정에서 완벽히 모든 것을 설명하려고 했다. 아이디어 멘토링 시간에 날카로운 질문을 받고 난 후, 나의 설명이 부족하여 우리 서비스의 **핵심 타겟과 차별점이 충분히 전달되지 못할까봐** 불안해졌기 때문이다.

그래서 나는 초등학생이 들어도 단번에 이해할 수 있도록, **대본을 더 세세하고 꼼꼼하게 풀어썼다.** 원래는 새벽 3시에 컨디션 조절을 위해 잠들 계획이었으나, 이 방대해진 대본을 다듬느라 결국 밤을 새우고 말았다. 그래도 내용을 온전히 전달할 수만 있다면 컨디션이 나빠지는 것은 감수할 수 있었다.

하지만 다음 날 현장에서 주어진 발표 시간은 단 **6분**이었다. 리허설을 해보면서 준비한 내용을 다 설명할 시간은 없음을 직감했다. 결국 밤새 준비한 대본의 절반 이상을 즉석에서 생략하고 넘어가야 했다. 나의 설명이 중간중간 생략되자 심사위원과 청중의 시선은 자연스럽게 발표 자료(PPT)로 쏠렸다.

발표가 끝난 직후, 발표 준비 전략에 대한 아쉬움이 크게 남았다. 발표시간이 생각보다 짧게 주어질 수 있다는 변수를 생각하지 못했다. 대본에 담으려던 그 친절한 설명을 PPT 안에 녹여냈어야 했다. <strong class="highlight-text">설명이 빨라지거나 생략되는 변수 속에서도, PPT만 보면 의문점이 해결될 수 있도록</strong> 말이다.

PPT는 핵심만 간결하게 만들었는데, 정작 준비한 설명을 다 못 하게 되니 불친절한 자료가 되어버린 것 같았다. 제한된 시간 내에 청중을 설득해야 할 경우, 현장 상황에 따라 생략될 수 있는 구두 설명보다는, <strong class="highlight-text">청중이 언제든 눈으로 다시 확인할 수 있는 PPT 자체에 상세한 맥락을 담아두는 것</strong>이 훨씬 안전하고 효과적인 전략임을 깨달았다.

</div>

<br>

# 4. 깨달음(Insight) : '진심'이 만든 '비즈니스 경쟁력’

<img src="{{site.url}}/assets/images/2025-11-28-2025-aws-hackaton-retrospective/2025-aws-hackaton-final-rank.png" 
     alt="2025 AWS Hackathon Final Rank" 
     style="display: block; margin: 2rem auto; max-width: 70%; height: auto;">

<div class="story-box" markdown="1">

발표에 개인적인 아쉬움이 남았지만, 서비스 자체의 완성도와 차별점은 충분히 전달되었다. 결과적으로 우리 팀은 3등(우수상)이라는 값진 성과를 거두었다. 수상할 수 있었던 이유는 우리가 이 서비스의 <strong class="highlight-text">잠재적 고객</strong>이었기 때문이라고 생각한다.

아이디어 멘토링 시간, 멘토님들은 데모 영상을 보자 "지원자뿐만 아니라 채용 담당자 입장에서도 탐낼 만한 아이디어"라며 가능성을 먼저 인정해 주셨다. 하지만 칭찬도 잠시, 곧이어 서비스의 본질을 꿰뚫는 날카로운 질문이 시작되었다.

> "굳이 이 서비스 안 쓰고 ChatGPT 쓰면 될 것 같은데.. 뭐가 달라요?"
> <br>"이 서비스를 정말로 사람들이 결제할까요? 수익화 전략은 뭐에요?"

쏟아지는 질문에 긴장은 되었지만 막힘없이 답변했다. 직접 서비스를 이용할 당사자의 입장에서, 기존 서비스와는 무엇이 달라서 꼭 써야만 하는지, 그리고 어느 지점에서 기꺼이 지갑을 열게 될지를 누구보다 현실적으로 알고 있었기 때문이다.

하지만 동시에 멘토님들의 질문을 통해, **우리 서비스의 차별점이 충분히 전달되지 않았음**을 깨달았다. 나는 당황하는 대신 부족했던 설명을 보충하여 차분히 다시 전달했고, 그제야 멘토님은 고개를 끄덕이셨다. 우리는 이 과정을 놓치지 않고, 다음날 최종 발표에서 논리를 탄탄하게 보강하는 재료로 삼았다.

이런 과정을 거치며 비로소 우리가 만든 서비스의 강점이 더 또렷해졌다. 그리고 단순히 아이디어를 설명하는 수준을 넘어 설득력 있는 형태로 다듬어졌다. 우리는 심사위원들의 공감을 이끌어내며 '우수상'이라는 결실을 맺을 수 있었다. 

</div>

<img src="{{site.url}}/assets/images/2025-11-28-2025-aws-hackaton-retrospective/2025-aws-hackaton-team-photo.jpeg" 
     alt="2025 AWS Hackathon Team Photo" 
     style="display: block; margin: 2rem auto; max-width: 70%; height: auto;">

<div class="story-box" markdown="1">

이번 해커톤의 가장 큰 수확은 수상 실적 그 자체가 아니다. "어차피 내가 사용할 서비스니까, 기왕 만드는 거 제대로 만들어보자”는 마음으로 치열하게 고민했을 뿐인데, <strong class="highlight-text">감사하게도 수상이라는 결과가 뒤따라왔다.</strong> 이번 수상은 진심을 다해 문제에 파고들면 성과는 자연스럽게 따라온다는 사실을 일깨워 준 뜻깊은 경험이었다.

나는 앞으로도 <strong class="highlight-text">'내 코드가 누군가의 문제를 실질적으로 해결해주고 있는지'</strong> 끊임없이 되물으며, 계속해서 성장해 나갈 것이다.

</div>

  <div class="icon-box">
    <div style="display: flex; flex-wrap: wrap; gap: 10px;">
      
      <a href="https://mandusitstudy.tistory.com/532" target="_blank" style="flex: 1; min-width: 250px; text-decoration: none; display: flex; align-items: center; justify-content: space-between; background: #fff; border: 1px solid #cbd5e1; border-radius: 6px; padding: 0.7rem 1rem; transition: background 0.2s;">
        <div style="display: flex; flex-direction: column;">
          <span style="font-size: 0.9rem; font-weight: 700; color: #1e293b;">📝 다른 팀원의 후기도 궁금하다면</span>
          <span style="font-size: 0.75rem; color: #64748b;"></span>
        </div>
        <div style="font-size: 1rem; color: #94a3b8;">↗</div>
      </a>

    </div>
  </div>


<br>