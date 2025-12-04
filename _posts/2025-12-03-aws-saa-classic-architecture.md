---
title: "Chapter1. Classic Architecture"
categories: AWS-SAA
excerpt: "고가용성과 확장성을 갖춘 서비스를 배포하는 가장 오래된 방법에 대해 알아보자"
#redirect_from: #이전주소 입력
#search: false #만약 이 글이 검색되지 않기를 바란다면
#use_math: true #수식이 필요한 경우만 사용
---

# 1. 서버

## 1) Elastic Compute Cloud(EC2)

### 무엇이든 뭔가 하려면 일단 컴퓨터가 필요하다.

우리가 컴퓨터를 사용하는 목적은 다양하지만, 개발자에게 있어 컴퓨터는 곧 **서비스를 제공하기 위한 도구**{: .highlight-text }입니다. 단순히 게임을 하거나 동영상을 시청하는 것이 아니라(소비), 무언가를 사용자에게 전달하는 역할을 수행하는 것이죠.
{: .story-box }

<div class="info-box">
  <div style="max-width: 800px; margin: 0 auto; background-color: #f8fafc; padding: 1.5rem; border-radius: 1rem; border: 1px solid #e2e8f0; font-family: sans-serif; color: #333; box-sizing: border-box;">
    
    <h4 style="text-align: center; margin-top: 0; font-size: 1.2rem; font-weight: 800; color: #1e293b; margin-bottom: 1.5rem;">
      <i class="fas fa-cloud" style="color: #f97316; margin-right: 0.5rem;"></i>
      EC2 = 우리가 빌린 '웨이터(Server)'
    </h4>

    <div style="display: flex; flex-wrap: wrap; justify-content: center; align-items: center; gap: 0.8rem;">

      <div style="flex: 1; min-width: 200px; display: flex; flex-direction: column; gap: 0.8rem;">
        <div style="background: white; padding: 0.8rem; border-radius: 0.75rem; border: 1px solid #e2e8f0; box-shadow: 0 2px 4px rgba(0,0,0,0.05); position: relative;">
          <span style="position: absolute; top: -0.5rem; left: 0.8rem; background: #64748b; color: white; padding: 0.1rem 0.5rem; border-radius: 4px; font-size: 0.65rem; font-weight: 700;">비유</span>
          <div style="display: flex; align-items: center; justify-content: space-between; margin-top: 0.3rem;">
            <div style="text-align: center;">
              <span style="font-size: 1.8rem;">🙋‍♂️</span><br>
              <span style="font-size: 0.7rem; font-weight: 700; color: #64748b;">손님</span>
            </div>
            <div style="color: #cbd5e1; font-size: 1rem;">
              <i class="fas fa-arrow-right"></i>
            </div>
            <div style="text-align: center;">
              <span style="font-size: 1.8rem;">🗣️</span><br>
              <span style="font-size: 0.7rem; font-weight: 700; color: #1e293b;">주문</span>
            </div>
          </div>
        </div>

        <div style="background: white; padding: 0.8rem; border-radius: 0.75rem; border: 1px solid #e2e8f0; box-shadow: 0 2px 4px rgba(0,0,0,0.05); position: relative;">
          <span style="position: absolute; top: -0.5rem; left: 0.8rem; background: #3b82f6; color: white; padding: 0.1rem 0.5rem; border-radius: 4px; font-size: 0.65rem; font-weight: 700;">실제</span>
          <div style="display: flex; align-items: center; justify-content: space-between; margin-top: 0.3rem;">
            <div style="text-align: center;">
              <span style="font-size: 1.8rem;">💻</span><br>
              <span style="font-size: 0.7rem; font-weight: 700; color: #64748b;">사용자</span>
            </div>
            <div style="color: #cbd5e1; font-size: 1rem;">
              <i class="fas fa-arrow-right"></i>
            </div>
            <div style="text-align: center;">
              <span style="font-size: 1.8rem;">🖱️</span><br>
              <span style="font-size: 0.7rem; font-weight: 700; color: #1e293b;">클릭(요청)</span>
            </div>
          </div>
        </div>
      </div>

      <div style="flex: 0 0 auto; z-index: 10;">
        <div style="background: linear-gradient(135deg, #fff7ed 0%, #ffedd5 100%); padding: 1rem; border-radius: 1rem; border: 2px solid #f97316; box-shadow: 0 10px 25px -5px rgba(249, 115, 22, 0.4); text-align: center; width: 170px; display: flex; flex-direction: column; justify-content: center; align-items: center; position: relative; box-sizing: border-box;">

          <div style="position: relative; display: inline-block; margin-bottom: 0.3rem;">
            <i class="fas fa-server" style="font-size: 3.5rem; color: #fdba74;"></i>
            <div style="position: absolute; bottom: -5px; right: -8px; background: white; border-radius: 50%; padding: 4px; box-shadow: 0 2px 4px rgba(0,0,0,0.1);">
                <span style="font-size: 2rem; line-height: 1;">🤵</span>
            </div>
          </div>

          <h4 style="margin: 0.3rem 0 0; font-size: 1.1rem; font-weight: 800; color: #c2410c;">AWS EC2</h4>
          <span style="font-size: 0.75rem; font-weight: 600; color: #ea580c; background: rgba(255,255,255,0.6); padding: 0.2rem 0.5rem; border-radius: 99px; margin-top: 0.3rem; display: inline-block;">
            = 서버 (가상 PC)
          </span>

          <i class="fas fa-chevron-right" style="position: absolute; left: -14px; top: 50%; transform: translateY(-50%); font-size: 1.2rem; color: #f97316;"></i>
          <i class="fas fa-chevron-right" style="position: absolute; right: -14px; top: 50%; transform: translateY(-50%); font-size: 1.2rem; color: #3b82f6;"></i>
        </div>
      </div>

      <div style="flex: 1; min-width: 200px; display: flex; flex-direction: column; gap: 0.8rem;">
        <div style="background: white; padding: 0.8rem; border-radius: 0.75rem; border: 1px solid #e2e8f0; box-shadow: 0 2px 4px rgba(0,0,0,0.05); position: relative;">
          <span style="position: absolute; top: -0.5rem; right: 0.8rem; background: #64748b; color: white; padding: 0.1rem 0.5rem; border-radius: 4px; font-size: 0.65rem; font-weight: 700;">비유</span>
          <div style="display: flex; align-items: center; justify-content: space-between; margin-top: 0.3rem;">
            <div style="text-align: center;">
              <span style="font-size: 1.8rem;">🍲</span><br>
              <span style="font-size: 0.7rem; font-weight: 700; color: #1e293b;">음식</span>
            </div>
            <div style="color: #cbd5e1; font-size: 1rem;">
              <i class="fas fa-arrow-right"></i>
            </div>
            <div style="text-align: center;">
              <span style="font-size: 1.8rem;">😋</span><br>
              <span style="font-size: 0.7rem; font-weight: 700; color: #64748b;">서빙 완료</span>
            </div>
          </div>
        </div>

        <div style="background: white; padding: 0.8rem; border-radius: 0.75rem; border: 1px solid #e2e8f0; box-shadow: 0 2px 4px rgba(0,0,0,0.05); position: relative;">
          <span style="position: absolute; top: -0.5rem; right: 0.8rem; background: #3b82f6; color: white; padding: 0.1rem 0.5rem; border-radius: 4px; font-size: 0.65rem; font-weight: 700;">실제</span>
          <div style="display: flex; align-items: center; justify-content: space-between; margin-top: 0.3rem;">
            <div style="text-align: center;">
              <span style="font-size: 1.8rem;">🎬</span><br>
              <span style="font-size: 0.7rem; font-weight: 700; color: #1e293b;">영상 파일</span>
            </div>
            <div style="color: #cbd5e1; font-size: 1rem;">
              <i class="fas fa-arrow-right"></i>
            </div>
            <div style="text-align: center;">
              <span style="font-size: 1.8rem;">📺</span><br>
              <span style="font-size: 0.7rem; font-weight: 700; color: #64748b;">화면 재생</span>
            </div>
          </div>
        </div>
      </div>

    </div>

    <div style="text-align: center; margin-top: 1.5rem; background-color: #fff; padding: 1rem; border-radius: 0.5rem; border: 1px solid #e2e8f0;">
      <p style="margin: 0; font-size: 0.9rem; line-height: 1.6; color: #475569;">
        우리가 AWS에서 빌리는 <strong>EC2 인스턴스</strong>가 바로<br>
        주문(요청)을 처리하고 음식(결과)을 가져다주는 <strong>웨이터(Server)</strong> 역할을 합니다.
      </p>
    </div>

  </div>
</div>

<div class="story-box" markdown="1">
**'서버(Server)'**라는 의미를 식당으로 비유를 들어보겠습니다.

1. **주문 (요청)**: 식당에 방문한 손님(사용자)이 웨이터(Server)에게 음식을 주문합니다.
   - 예시: 넷플릭스 앱에서 **'오징어 게임 시즌2 1화'**를 **클릭(요청)**합니다.
2. **서빙 (제공)**: 웨이터는 주문받은 음식을 주방에서 가져와 손님에게 **서빙(제공)**합니다.
   - 예시: 요청을 받은 서버는 저장된 영상 파일을 사용자에게 **스트리밍(제공)**합니다.

AWS의 **Elastic Compute Cloud (EC2)**는 바로 이 '서버' 역할을 하는 가상 컴퓨터를 클라우드 환경에서 빌려 쓰는 서비스입니다. 즉, 넷플릭스 사용자의 요청에 따라 영상을 찾아 서빙해 주는 **'웨이터'**를 AWS에서 고용하는 것과 같습니다.

EC2는 본질적으로 컴퓨터이기 때문에 다음과 같은 특징을 가집니다.

- 🖥️ **컴퓨터로서의 기본 기능**{: .highlight-text } : 일반 PC처럼 CPU, RAM, 스토리지, 네트워크, IP 등을 가짐
- 🛡️ **'서버'로서의 특화 기능**{: .highlight-text } : 서비스 제공을 위해 보안(Security), 비용 관리, 개발 편의 사항 등이 각별하게 신경 써져 있음
</div>

<br>

### 주요 연관 기능

EC2는 혼자서 독립적으로 존재하기보다, AWS의 생태계 안에서 다른 핵심 서비스들과 유기적으로 연결될 때 비로소 **안정적인 서비스 운영**{: .highlight-text }이 가능해집니다.
{: .story-box }

<div class="info-box">
    
    <div style="padding: 0 1.5rem; display: flex; gap: 1rem; flex-wrap: wrap; justify-content: center;">

      <div style="flex: 1; min-width: 140px; background-color: #ffffff; padding: 1rem; border-radius: 0.5rem; border: 1px solid #e2e8f0; box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05); text-align: center; display: flex; flex-direction: column; align-items: center;">
        <i class="fas fa-server" style="font-size: 2rem; color: #94a3b8; margin-bottom: 0.5rem;"></i>
        <h4 style="margin: 0.2rem 0; font-size: 1rem; font-weight: 700; color: #1e293b;">EC2</h4>
        <span style="font-size: 0.85rem; color: #64748b;">가상 머신</span>
      </div>

      <div style="flex: 1; min-width: 140px; background-color: #ffffff; padding: 1rem; border-radius: 0.5rem; border: 1px solid #e2e8f0; box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05); text-align: center; display: flex; flex-direction: column; align-items: center;">
        <i class="far fa-hdd" style="font-size: 2rem; color: #94a3b8; margin-bottom: 0.5rem;"></i>
        <h4 style="margin: 0.2rem 0; font-size: 1rem; font-weight: 700; color: #1e293b;">EBS</h4>
        <span style="font-size: 0.85rem; color: #64748b;">데이터 저장</span>
      </div>

      <div style="flex: 1; min-width: 140px; background-color: #ffffff; padding: 1rem; border-radius: 0.5rem; border: 1px solid #e2e8f0; box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05); text-align: center; display: flex; flex-direction: column; align-items: center;">
        <i class="fas fa-network-wired" style="font-size: 2rem; color: #94a3b8; margin-bottom: 0.5rem;"></i>
        <h4 style="margin: 0.2rem 0; font-size: 1rem; font-weight: 700; color: #1e293b;">ELB</h4>
        <span style="font-size: 0.85rem; color: #64748b;">부하 분산</span>
      </div>

      <div style="flex: 1; min-width: 140px; background-color: #ffffff; padding: 1rem; border-radius: 0.5rem; border: 1px solid #e2e8f0; box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05); text-align: center; display: flex; flex-direction: column; align-items: center;">
        <i class="fas fa-layer-group" style="font-size: 2rem; color: #94a3b8; margin-bottom: 0.5rem;"></i>
        <h4 style="margin: 0.2rem 0; font-size: 1rem; font-weight: 700; color: #1e293b;">ASG</h4>
        <span style="font-size: 0.85rem; color: #64748b;">자동 확장</span>
      </div>

    </div>
</div>

<div class="story-box" markdown="1">
가장 대표적으로 함께 사용되는 기능들은 다음과 같습니다.

1.  **EBS (Elastic Block Store)**
    - EC2가 연산(CPU, RAM)을 담당하는 두뇌라면, EBS는 데이터를 영구적으로 보관하는 **저장소(SSD/HDD)**{: .highlight-text }입니다.
    - EC2 인스턴스를 중지하거나 종료해도 중요한 데이터가 사라지지 않도록 별도로 저장하는 '외장 하드'와 같은 역할을 합니다.

2.  **ELB (Elastic Load Balancing)**
    - 맛집에 손님이 몰리면 웨이터 한 명으로는 감당이 안 되듯, 접속자가 폭주하면 서버 한 대로는 부족할 수 있습니다.
    - ELB는 들어오는 트래픽(손님)을 여러 대의 EC2 인스턴스에 골고루 나눠주는 **부하 분산 장치(매니저)**{: .highlight-text } 역할을 수행합니다.

3.  **ASG (Auto Scaling Group)**
    - 서비스 사용량은 항상 일정하지 않습니다. 낮에는 많고 새벽에는 적을 수 있습니다.
    - ASG는 트래픽 양에 맞춰 EC2 인스턴스의 개수를 자동으로 늘리거나(Scale-out) 줄여주는(Scale-in) **자동 확장 기능**{: .highlight-text }입니다. 이를 통해 비용 효율성을 극대화할 수 있습니다.
</div>

<br>

### 인스턴스 유형

<div class="story-box" markdown="1">
EC2 인스턴스를 생성하려고 보면 `t3.small`, `m5.large` 같은 암호 같은 이름들을 마주하게 됩니다. 하지만 이 이름에는 규칙이 있습니다. 바로 **[패밀리] + [세대] + [크기]** 순서입니다.

- **패밀리(Family):** 인스턴스의 특성 (앞글자 알파벳)
- **세대(Generation):** 숫자가 높을수록 최신 모델
- **크기(Size):** CPU와 메모리의 용량 (nano < micro < small < medium < large ...)

가장 중요한 것은 **'맨 앞글자(패밀리)'**가 무엇이냐에 따라 사용 목적이 완전히 달라진다는 점입니다.
</div>

<div class="info-box">
  <div style="padding: 0 1.5rem; border-radius: 1rem; max-width: 800px; margin: 0 auto;">

    <div style="background-color: #f8fafc; border-radius: 0.5rem; padding: 0.8rem; margin-bottom: 1.5rem; display: flex; justify-content: center; align-items: center; gap: 0.5rem; font-size: 0.85rem;">
      <div style="text-align: center;">
        <span style="background: #eff6ff; color: #2563eb; border: 1px solid #bfdbfe; padding: 0.1rem 0.4rem; border-radius: 3px; font-weight: 700;">t</span>
        <span style="font-size: 0.75rem; color: #64748b; margin-left: 0.2rem;">클래스(패밀리)</span>
      </div>
      <span style="color: #cbd5e1; font-weight: 700;">•</span>
      <div style="text-align: center;">
        <span style="background: #f0fdf4; color: #16a34a; border: 1px solid #bbf7d0; padding: 0.1rem 0.4rem; border-radius: 3px; font-weight: 700;">3</span>
        <span style="font-size: 0.75rem; color: #64748b; margin-left: 0.2rem;">세대</span>
      </div>
      <span style="color: #cbd5e1; font-weight: 700;">•</span>
      <div style="text-align: center;">
        <span style="background: #faf5ff; color: #9333ea; border: 1px solid #e9d5ff; padding: 0.1rem 0.4rem; border-radius: 3px; font-weight: 700;">small</span>
        <span style="font-size: 0.75rem; color: #64748b; margin-left: 0.2rem;">크기</span>
      </div>
    </div>

    <div style="display: flex; flex-direction: column; gap: 0.6rem;">

      <div style="background-color: #fff; border: 1px solid #e2e8f0; border-radius: 0.5rem; padding: 0.8rem; display: flex; align-items: flex-start; gap: 0.8rem;">
        <div style="background-color: #eff6ff; width: 36px; height: 36px; display: flex; align-items: center; justify-content: center; border-radius: 0.3rem; color: #2563eb; font-weight: 800; font-size: 1.1rem; flex-shrink: 0;">m</div>
        <div style="flex: 1; min-width: 0;">
          <div style="display: flex; align-items: center; gap: 0.4rem; margin-bottom: 0.3rem; white-space: nowrap; overflow: hidden;">
            <span style="font-weight: 800; font-size: 0.9rem; color: #1e293b;">범용</span>
            <span style="font-size: 0.75rem; color: #64748b;">(General Purpose)</span>
            <span style="font-size: 0.65rem; color: #1e40af; background: #dbeafe; padding: 0.1rem 0.3rem; border-radius: 3px; font-weight: 700;">Hint: Modest</span>
          </div>
          <div style="font-size: 0.8rem; line-height: 1.6; color: #475569;">
            컴퓨팅/메모리/네트워크 <strong>균형</strong>
            <span style="background:#f1f5f9; padding:0 0.3rem; border-radius:3px; color:#475569; margin-left:0.2rem;">웹 서버</span>
            <span style="background:#f1f5f9; padding:0 0.3rem; border-radius:3px; color:#475569;">코드 저장소</span>
          </div>
        </div>
      </div>

      <div style="background-color: #fff; border: 1px solid #e2e8f0; border-radius: 0.5rem; padding: 0.8rem; display: flex; align-items: flex-start; gap: 0.8rem;">
        <div style="background-color: #fff7ed; width: 36px; height: 36px; display: flex; align-items: center; justify-content: center; border-radius: 0.3rem; color: #ea580c; font-weight: 800; font-size: 1.1rem; flex-shrink: 0;">c</div>
        <div style="flex: 1; min-width: 0;">
          <div style="display: flex; align-items: center; gap: 0.4rem; margin-bottom: 0.3rem; white-space: nowrap; overflow: hidden;">
            <span style="font-weight: 800; font-size: 0.9rem; color: #1e293b;">컴퓨팅 최적화</span>
            <span style="font-size: 0.75rem; color: #64748b;">(Compute)</span>
            <span style="font-size: 0.65rem; color: #9a3412; background: #ffedd5; padding: 0.1rem 0.3rem; border-radius: 3px; font-weight: 700;">Hint: Computing</span>
          </div>
          <div style="font-size: 0.8rem; line-height: 1.6; color: #475569;">
            <strong>고성능 프로세서</strong> 필요 작업
            <span style="background:#f1f5f9; padding:0 0.3rem; border-radius:3px; color:#475569; margin-left:0.2rem;">배치 처리</span>
            <span style="background:#f1f5f9; padding:0 0.3rem; border-radius:3px; color:#475569;">고성능 웹서버</span>
            <span style="background:#f1f5f9; padding:0 0.3rem; border-radius:3px; color:#475569;">미디어/AI</span>
          </div>
        </div>
      </div>

      <div style="background-color: #fff; border: 1px solid #e2e8f0; border-radius: 0.5rem; padding: 0.8rem; display: flex; align-items: flex-start; gap: 0.8rem;">
        <div style="background-color: #f0fdf4; width: 36px; height: 36px; display: flex; align-items: center; justify-content: center; border-radius: 0.3rem; color: #16a34a; font-weight: 800; font-size: 1.1rem; flex-shrink: 0;">r</div>
        <div style="flex: 1; min-width: 0;">
          <div style="display: flex; align-items: center; gap: 0.4rem; margin-bottom: 0.3rem; white-space: nowrap; overflow: hidden;">
            <span style="font-weight: 800; font-size: 0.9rem; color: #1e293b;">메모리 최적화</span>
            <span style="font-size: 0.75rem; color: #64748b;">(Memory)</span>
            <span style="font-size: 0.65rem; color: #166534; background: #dcfce7; padding: 0.1rem 0.3rem; border-radius: 3px; font-weight: 700;">Hint: RAM</span>
          </div>
          <div style="font-size: 0.8rem; line-height: 1.6; color: #475569;">
            <strong>빠른 성능</strong> 및 메모리 내 대규모 데이터 처리
            <span style="background:#f1f5f9; padding:0 0.3rem; border-radius:3px; color:#475569; margin-left:0.2rem;">실시간 빅데이터</span>
            <span style="background:#f1f5f9; padding:0 0.3rem; border-radius:3px; color:#475569;">고성능 DB</span>
          </div>
        </div>
      </div>

      <div style="background-color: #fff; border: 1px solid #e2e8f0; border-radius: 0.5rem; padding: 0.8rem; display: flex; align-items: flex-start; gap: 0.8rem;">
        <div style="background-color: #faf5ff; width: 36px; height: 36px; display: flex; align-items: center; justify-content: center; border-radius: 0.3rem; color: #9333ea; font-weight: 800; font-size: 0.9rem; flex-shrink: 0;">i,d</div>
        <div style="flex: 1; min-width: 0;">
          <div style="display: flex; align-items: center; gap: 0.4rem; margin-bottom: 0.3rem; white-space: nowrap; overflow: hidden;">
            <span style="font-weight: 800; font-size: 0.9rem; color: #1e293b;">스토리지 최적화</span>
            <span style="font-size: 0.75rem; color: #64748b;">(Storage)</span>
            <span style="font-size: 0.65rem; color: #6b21a8; background: #f3e8ff; padding: 0.1rem 0.3rem; border-radius: 3px; font-weight: 700;">Hint: I/O, Data</span>
          </div>
          <div style="font-size: 0.8rem; line-height: 1.6; color: #475569;">
            높은 디스크 <strong>I/O 처리량</strong> 필요
            <span style="background:#f1f5f9; padding:0 0.3rem; border-radius:3px; color:#475569; margin-left:0.2rem;">NoSQL/SQL DB</span>
            <span style="background:#f1f5f9; padding:0 0.3rem; border-radius:3px; color:#475569;">트랜잭션</span>
            <span style="background:#f1f5f9; padding:0 0.3rem; border-radius:3px; color:#475569;">DW</span>
          </div>
        </div>
      </div>

      <div style="background-color: #fff; border: 1px solid #e2e8f0; border-radius: 0.5rem; padding: 0.8rem; display: flex; align-items: flex-start; gap: 0.8rem;">
        <div style="background-color: #ecfeff; width: 36px; height: 36px; display: flex; align-items: center; justify-content: center; border-radius: 0.3rem; color: #0891b2; font-weight: 800; font-size: 1.1rem; flex-shrink: 0;">t</div>
        <div style="flex: 1; min-width: 0;">
          <div style="display: flex; align-items: center; gap: 0.4rem; margin-bottom: 0.3rem; white-space: nowrap; overflow: hidden;">
            <span style="font-weight: 800; font-size: 0.9rem; color: #1e293b;">버스팅 퍼포먼스</span>
            <span style="font-size: 0.75rem; color: #64748b;">(Burstable)</span>
            <span style="font-size: 0.65rem; color: #0e7490; background: #cffafe; padding: 0.1rem 0.3rem; border-radius: 3px; font-weight: 700;">Hint: Turbo/Tiny</span>
          </div>
          <div style="font-size: 0.8rem; line-height: 1.6; color: #475569;">
            기본 성능 + 필요 시 <strong>버스트(가속)</strong>
            <span style="background:#f1f5f9; padding:0 0.3rem; border-radius:3px; color:#475569; margin-left:0.2rem;">개발/테스트</span>
            <span style="background:#f1f5f9; padding:0 0.3rem; border-radius:3px; color:#475569;">소규모 웹/DB</span>
            <span style="background:#f1f5f9; padding:0 0.3rem; border-radius:3px; color:#475569;">마이크로서비스</span>
          </div>
        </div>
      </div>

    </div>
  </div>

</div>

일반적으로 입문자가 프리티어로 접하는 **t 시리즈**는 평소에는 적절한 성능을 유지하다가, 트래픽이 몰릴 때 일시적으로 성능을 높일 수 있어 비용 효율적입니다. 실제 서비스 성격에 맞춰 적절한 패밀리를 선택하는 것이 클라우드 비용 절감의 시작입니다.
{: .story-box }

<br>

### EC2 인스턴스 구매 옵션

<div class="story-box" markdown="1">
EC2는 사용 목적과 기간에 따라 다양한 요금제를 제공합니다. 마치 호텔을 예약할 때 "하루만 묵을지", "1년 회원권을 끊을지", "땡처리 방을 잡을지" 고민하는 것과 비슷합니다.

가장 기본이 되는 두 가지 방식부터 살펴보겠습니다.
</div>

#### 1. 기본 구매 방식 (단기/유동적)

대부분의 입문자가 처음 접하는 방식입니다.
{: .story-box }

<div class="info-box">
<div style="background-color: #f8fafc; padding: 1.5rem; border-radius: 0.75rem; border: 1px solid #e2e8f0; ">
  <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 1rem;">
    
    <div style="background: white; border-radius: 0.5rem; border: 1px solid #e2e8f0; overflow: hidden; display: flex; flex-direction: column;">
      <div style="background: #3b82f6; padding: 0.8rem; color: white; display: flex; align-items: center; justify-content: space-between;">
        <span style="font-weight: 700;">온디맨드 (On-Demand)</span>
        <i class="fas fa-clock"></i>
      </div>
      <div style="padding: 1rem; flex: 1; display: flex; flex-direction: column; justify-content: space-between;">
        <div>
          <div style="background: #eff6ff; border-radius: 0.3rem; padding: 0.5rem; margin-bottom: 0.8rem; font-size: 0.9rem; color: #1e3a8a; text-align: center;">
            🏨 <strong>"제값 내고 하루만 묵을게요"</strong>
          </div>
          <ul style="margin: 0; padding-left: 1.2rem; color: #475569; font-size: 0.85rem; line-height: 1.6;">
            <li>가장 일반적인 요금제</li>
            <li>초 단위 과금 (쓴 만큼만 냄)</li>
            <li>약정 X, 선불 X</li>
          </ul>
        </div>
        <div style="margin-top: 1rem; text-align: center; font-size: 0.8rem; color: #64748b; background: #f1f5f9; padding: 0.3rem; border-radius: 4px;">
           👉 <strong>개발, 테스트, 단기 작업</strong> 추천
        </div>
      </div>
    </div>

    <div style="background: white; border-radius: 0.5rem; border: 1px solid #e2e8f0; overflow: hidden; display: flex; flex-direction: column;">
      <div style="background: #f97316; padding: 0.8rem; color: white; display: flex; align-items: center; justify-content: space-between;">
        <span style="font-weight: 700;">스팟 (Spot)</span>
        <span style="background: #fff; color: #ea580c; font-size: 0.75rem; padding: 0.1rem 0.5rem; border-radius: 4px; font-weight: 800;">최대 90% 할인</span>
      </div>
      <div style="padding: 1rem; flex: 1; display: flex; flex-direction: column; justify-content: space-between;">
        <div>
          <div style="background: #fff7ed; border-radius: 0.3rem; padding: 0.5rem; margin-bottom: 0.8rem; font-size: 0.9rem; color: #9a3412; text-align: center;">
            🏨 <strong>"빈방 땡처리 경매"</strong>
          </div>
          <ul style="margin: 0; padding-left: 1.2rem; color: #475569; font-size: 0.85rem; line-height: 1.6;">
            <li>AWS의 남는 자원을 싸게 이용</li>
            <li><strong>단, 더 비싼 입찰자가 오면 뺏김</strong></li>
            <li>언제든 중단될 수 있음</li>
          </ul>
        </div>
        <div style="margin-top: 1rem; text-align: center; font-size: 0.8rem; color: #64748b; background: #f1f5f9; padding: 0.3rem; border-radius: 4px;">
           👉 <strong>데이터 분석, 배치 처리</strong><br>(중단돼도 괜찮은)
        </div>
      </div>
    </div>

  </div>
</div>
</div>

#### 2. 장기 약정 할인 (1년 / 3년 계약)

서비스를 장기적으로 운영할 계획이라면, 미리 약정을 걸어 비용을 크게 아낄 수 있습니다. AWS는 최근 **Savings Plans**를 강력하게 권장하는 추세입니다.
{: .story-box }

<div class="info-box">
  <div style="background-color: #f8fafc; padding: 1.5rem; border-radius: 0.75rem; border: 1px solid #e2e8f0; ">
  
  <div style="text-align: center; margin-bottom: 1.5rem;">
    <span style="background: #0f172a; color: white; padding: 0.4rem 1rem; border-radius: 99px; font-size: 0.9rem; font-weight: 700;">
      <i class="fas fa-handshake" style="margin-right: 0.3rem;"></i> 최대 72% 할인
    </span>
    <p style="margin-top: 0.5rem; color: #64748b; font-size: 0.9rem;">
      "1년/3년 동안 꾸준히 쓸 테니 깎아주세요" (전체 선불 시 할인율 최대)
    </p>
  </div>

  <div style="display: flex; gap: 1rem; flex-wrap: wrap; margin-bottom: 1.5rem;">
    <div style="flex: 1; min-width: 280px; background: white; border: 1px solid #cbd5e1; border-radius: 0.5rem; overflow: hidden;">
      <div style="background: #eff6ff; color: #1e40af; padding: 0.6rem; text-align: center; font-weight: 700; border-bottom: 1px solid #bfdbfe;">
        1. 예약 인스턴스 (RI)
      </div>
      <div style="padding: 1rem;">
        <p style="font-size: 0.85rem; color: #475569; margin-bottom: 0.8rem; text-align: center;">
          특정 <strong>기기 자체</strong>를 예약
        </p>
        <div style="background: #f1f5f9; padding: 0.5rem; border-radius: 4px; font-size: 0.8rem; color: #334155; margin-bottom: 0.5rem;">
          <strong>• 표준 (Standard):</strong> 변경 불가 / 할인율 높음 (72%)
        </div>
        <div style="background: #fff; border: 1px solid #e2e8f0; padding: 0.5rem; border-radius: 4px; font-size: 0.8rem; color: #334155;">
          <strong>• 전환형 (Convertible):</strong> 패밀리, OS 변경 가능 / 할인율 낮음 (66%)
        </div>
      </div>
    </div>

    <div style="flex: 1; min-width: 280px; background: white; border: 2px solid #34d399; border-radius: 0.5rem; overflow: hidden; position: relative;">
      <div style="position: absolute; top: 0; right: 0; background: #34d399; color: white; font-size: 0.7rem; font-weight: 800; padding: 0.2rem 0.5rem; border-bottom-left-radius: 6px;">추천 ✨</div>
      <div style="background: #ecfdf5; color: #065f46; padding: 0.6rem; text-align: center; font-weight: 700; border-bottom: 1px solid #6ee7b7;">
        2. Savings Plans (SP)
      </div>
      <div style="padding: 1rem;">
        <p style="font-size: 0.85rem; color: #475569; margin-bottom: 0.8rem; text-align: center;">
          시간당 <strong>약정 금액($)</strong>을 채우면 됨
        </p>
        <div style="background: #f1f5f9; padding: 0.5rem; border-radius: 4px; font-size: 0.8rem; color: #334155; margin-bottom: 0.5rem;">
          <strong>• EC2 SP:</strong> 특정 패밀리/리전 고정 (최대 72%)
        </div>
        <div style="background: #d1fae5; border: 1px solid #34d399; padding: 0.5rem; border-radius: 4px; font-size: 0.8rem; color: #064e3b; font-weight: 700;">
          • Compute SP: EC2 + Fargate + Lambda + SageMaker ✅
        </div>
      </div>
    </div>
  </div>
</div>
</div>

<div class="story-box" markdown="1">
**💡 어떤 Savings Plan을 선택해야 할까요?**

- **EC2만** 사용하고, **리전/패밀리 변경 계획이 없다** <i class="fas fa-arrow-right" style="font-size:0.7rem; margin:0 5px;"></i> **EC2 Instance SP** (할인율 ⬆️)
- **Lambda, Fargate** 등을 섞어 쓰거나, **인스턴스 유형을 자주 바꾼다** <i class="fas fa-arrow-right" style="font-size:0.7rem; margin:0 5px;"></i> **Compute SP** (유연성 ⬆️)
</div>

#### 3. 특수 목적 (규제 및 라이선스)

보안 규정이나 라이선스 문제로 하드웨어를 단독으로 써야 할 때 사용하는 옵션입니다.
{: .story-box }

<div class="info-box">
<div style="max-width: 800px; margin: 0 auto; background-color: #f8fafc; padding: 1.5rem; border-radius: 0.75rem; border: 1px solid #e2e8f0; margin-bottom: 2rem;">
  
  <div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 0.8rem; margin-bottom: 2rem;">
    
    <div style="background: white; padding: 0.8rem; border-radius: 0.5rem; border: 1px solid #e2e8f0; display: flex; flex-direction: column;">
      <div style="margin-bottom: 0.5rem; text-align: center;">
        <strong style="font-size: 0.9rem; color: #1e293b;">🏢 전용 호스트<br><span style="font-size: 0.75rem; color: #64748b;">(Dedicated Host)</span></strong>
      </div>
      <div style="background: #eff6ff; padding: 0.5rem; border-radius: 4px; color: #1e40af; font-size: 0.8rem; margin-bottom: 0.8rem; text-align: center; font-weight: 700; height: 100%; display: flex; align-items: center; justify-content: center;">
        "건물 통째로 매입"
      </div>
      <div style="font-size: 0.75rem; color: #475569; line-height: 1.5; text-align: center;">
        물리 서버 제어권 보유<br>
        <span style="color: #dc2626; font-weight: 700;">가장 비쌈</span>
      </div>
    </div>

    <div style="background: white; padding: 0.8rem; border-radius: 0.5rem; border: 1px solid #e2e8f0; display: flex; flex-direction: column;">
      <div style="margin-bottom: 0.5rem; text-align: center;">
        <strong style="font-size: 0.9rem; color: #1e293b;">🔒 전용 인스턴스<br><span style="font-size: 0.75rem; color: #64748b;">(Dedicated Instance)</span></strong>
      </div>
      <div style="background: #f0fdf4; padding: 0.5rem; border-radius: 4px; color: #166534; font-size: 0.8rem; margin-bottom: 0.8rem; text-align: center; font-weight: 700; height: 100%; display: flex; align-items: center; justify-content: center;">
        "전용 층 사용"
      </div>
      <div style="font-size: 0.75rem; color: #475569; line-height: 1.5; text-align: center;">
        하드웨어 격리 보장<br>
        <span style="color: #64748b;">배치 제어 불가</span>
      </div>
    </div>

    <div style="background: white; padding: 0.8rem; border-radius: 0.5rem; border: 1px solid #e2e8f0; display: flex; flex-direction: column;">
      <div style="margin-bottom: 0.5rem; text-align: center;">
        <strong style="font-size: 0.9rem; color: #1e293b;">📅 용량 예약<br><span style="font-size: 0.75rem; color: #64748b;">(Capacity Res.)</span></strong>
      </div>
      <div style="background: #fff7ed; padding: 0.5rem; border-radius: 4px; color: #9a3412; font-size: 0.8rem; margin-bottom: 0.8rem; text-align: center; font-weight: 700; height: 100%; display: flex; align-items: center; justify-content: center;">
        "자리 미리 찜"
      </div>
      <div style="font-size: 0.75rem; color: #475569; line-height: 1.5; text-align: center;">
        확실한 인스턴스 확보<br>
        <span style="color: #ea580c; font-weight: 700;">미사용 시에도 과금</span>
      </div>
    </div>

  </div>

  <div style="background: white; border-radius: 0.5rem; border: 1px solid #cbd5e1; overflow: hidden; width: 100%; box-sizing: border-box;">
    <div style="background: #334155; color: white; padding: 0.6rem; text-align: center; font-size: 0.9rem; font-weight: 700;">
      📝 한눈에 외우기 (Cheat Sheet)
    </div>
    <div style="overflow-x: auto; width: 100%;">
      <table style="width: 100%; min-width: 600px; border-collapse: collapse; font-size: 0.8rem; text-align: center; table-layout: fixed;">
        <thead>
          <tr style="background: #f1f5f9; color: #475569;">
            <th style="padding: 0.6rem; border-bottom: 2px solid #e2e8f0; width: 20%;">구분</th>
            <th style="padding: 0.6rem; border-bottom: 2px solid #e2e8f0; width: 26.6%;">전용 호스트<br>(Host)</th>
            <th style="padding: 0.6rem; border-bottom: 2px solid #e2e8f0; width: 26.6%;">전용 인스턴스<br>(Instance)</th>
            <th style="padding: 0.6rem; border-bottom: 2px solid #e2e8f0; width: 26.6%;">용량 예약<br>(Capacity)</th>
          </tr>
        </thead>
        <tbody style="color: #334155;">
          <tr>
            <td style="padding: 0.6rem; border-bottom: 1px solid #e2e8f0; font-weight: 700; background: #f8fafc; word-break: keep-all;">
              하드웨어 공유
            </td>
            <td style="padding: 0.6rem; border-bottom: 1px solid #e2e8f0; color: #dc2626; font-weight: 700;">X</td>
            <td style="padding: 0.6rem; border-bottom: 1px solid #e2e8f0; color: #dc2626; font-weight: 700;">X</td>
            <td style="padding: 0.6rem; border-bottom: 1px solid #e2e8f0; color: #64748b;">(상관없음)</td>
          </tr>
          <tr>
            <td style="padding: 0.6rem; border-bottom: 1px solid #e2e8f0; font-weight: 700; background: #f8fafc; word-break: keep-all;">
              배치 제어<br><span style="font-weight:400; font-size:0.7em">(Placement)</span>
            </td>
            <td style="padding: 0.6rem; border-bottom: 1px solid #e2e8f0;">
              <span style="background: #dcfce7; color: #166534; padding: 0.1rem 0.4rem; border-radius: 99px; font-weight: 700; display: inline-block;">가능 (O)</span>
            </td>
            <td style="padding: 0.6rem; border-bottom: 1px solid #e2e8f0; color: #94a3b8;">
              불가능 (X)
            </td>
            <td style="padding: 0.6rem; border-bottom: 1px solid #e2e8f0; color: #94a3b8;">
              -
            </td>
          </tr>
          <tr>
            <td style="padding: 0.6rem; border-bottom: 1px solid #e2e8f0; font-weight: 700; background: #f8fafc; word-break: keep-all;">
              라이선스(BYOL)<br><span style="font-weight:400; font-size:0.7em">(Compliance)</span>
            </td>
            <td style="padding: 0.6rem; border-bottom: 1px solid #e2e8f0;">
              <span style="background: #dcfce7; color: #166534; padding: 0.1rem 0.4rem; border-radius: 99px; font-weight: 700; display: inline-block;">가능 (O)</span>
            </td>
            <td style="padding: 0.6rem; border-bottom: 1px solid #e2e8f0; color: #94a3b8;">
              불가능 (X)
            </td>
            <td style="padding: 0.6rem; border-bottom: 1px solid #e2e8f0; color: #94a3b8;">
              -
            </td>
          </tr>
          <tr>
            <td style="padding: 0.6rem; font-weight: 700; background: #f8fafc; word-break: keep-all;">
              과금/약정 특징
            </td>
            <td style="padding: 0.6rem; font-size: 0.75rem; word-break: keep-all;">
              서버 단위 과금
            </td>
            <td style="padding: 0.6rem; font-size: 0.75rem; word-break: keep-all;">
              인스턴스 과금<br>(+$2/Region)
            </td>
            <td style="padding: 0.6rem; font-size: 0.75rem; word-break: keep-all;">
              시간 약정 없음<br>(단기 사용 가능)
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</div>
</div>

<div style="margin-bottom: 1.5rem; ">
    <h4 style="margin: 0; color: #1e293b;font-weight: 800;">✔️ 놓치기 쉬운 핵심 디테일 3가지 (Deep Dive)</h4>
  </div>

  <div class="deep-dive-list">

    <div class="dd-item">
      <div class="dd-header">
        <span class="dd-title">1. 전용 인스턴스: 배치가 바뀐다?</span>
        <span class="dd-badge red">함정 주의</span>
      </div>
      <p class="dd-content">
        "나만 쓰는 하드웨어"는 맞지만, <span class="code-span">중지(Stop) 후 시작(Start)</span>하면 다른 기계로 이사 갈 수 있습니다.
        <span class="sub-text">👉 따라서 <strong>배치 제어(Placement Control)</strong>가 불가능합니다. (원하는 랙에 고정 불가)</span>
      </p>
    </div>

    <div class="dd-item">
      <div class="dd-header">
        <span class="dd-title">2. 용량 예약 + 할인 조합법</span>
        <span class="dd-badge blue">Tip</span>
      </div>
      <p class="dd-content">
        용량 예약 자체는 <span class="code-span">할인 0% (정가)</span>입니다. 하지만 다른 할인과 합체가 가능합니다.
        <span class="sub-text highlight-box">🎯 (용량 예약으로 자리 확보) + (RI/SP로 요금 할인) = 완벽 조합</span>
      </p>
    </div>

    <div class="dd-item">
      <div class="dd-header">
        <span class="dd-title">3. 전용 호스트: 내 라이선스 쓰기(BYOL)</span>
      </div>
      <p class="dd-content">
        물리 서버 전체를 점유하므로 기존 온프레미스에서 쓰던 기업용 라이선스를 그대로 가져올 수 있습니다.
        <span class="sub-text">👉 기준: <strong>소켓(Socket)당, 코어(Core)당, VM당</strong> 라이선스</span>
      </p>
    </div>

  </div>

<br>

### 배치 그룹 전략(Placement Groups)

인스턴스 물리적 배치 전략
{: .story-box }

<div class="info-box">

<div style="padding:0 1rem; border-radius: 0.75rem; font-size: 0.85rem; letter-spacing: -0.03em;">

  <div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 1rem;">

    <div style="background: white; border: 1px solid #e9d5ff; border-radius: 0.5rem; overflow: hidden; display: flex; flex-direction: column;">
      <div style="background: #faf5ff; padding: 0.6rem; border-bottom: 1px solid #e9d5ff; text-align: center;">
        <i class="fas fa-th-large" style="color: #9333ea; font-size: 1.2rem; margin-bottom: 0.3rem;"></i>
        <div style="font-weight: 800; color: #6b21a8; font-size: 0.95rem;">클러스터 (Cluster)</div>
        <span style="font-size: 0.7rem; color: #7e22ce; background: #e9d5ff; padding: 0.1rem 0.4rem; border-radius: 3px; font-weight: 700;">단일 AZ / 초고속</span>
      </div>

      <div style="padding: 0.8rem; background: #fff; display: flex; justify-content: center; border-bottom: 1px dashed #f3e8ff;">
        <div style="border: 2px solid #d8b4fe; border-radius: 6px; padding: 6px; background-color: #faf5ff;">
          <div style="font-size: 0.6rem; color: #a855f7; text-align: center; margin-bottom: 2px; font-weight: 700;">Rack 1</div>
          <div style="display: grid; grid-template-columns: repeat(4, 1fr); gap: 2px;">
            <div style="width: 10px; height: 10px; background: #a855f7; border-radius: 2px;"></div>
            <div style="width: 10px; height: 10px; background: #a855f7; border-radius: 2px;"></div>
            <div style="width: 10px; height: 10px; background: #a855f7; border-radius: 2px;"></div>
            <div style="width: 10px; height: 10px; background: #a855f7; border-radius: 2px;"></div>
            <div style="width: 10px; height: 10px; background: #a855f7; border-radius: 2px;"></div>
            <div style="width: 10px; height: 10px; background: #a855f7; border-radius: 2px;"></div>
            <div style="width: 10px; height: 10px; background: #a855f7; border-radius: 2px;"></div>
            <div style="width: 10px; height: 10px; background: #a855f7; border-radius: 2px;"></div>
            <div style="width: 10px; height: 10px; background: #a855f7; border-radius: 2px;"></div>
            <div style="width: 10px; height: 10px; background: #a855f7; border-radius: 2px;"></div>
            <div style="width: 10px; height: 10px; background: #a855f7; border-radius: 2px;"></div>
            <div style="width: 10px; height: 10px; background: #a855f7; border-radius: 2px;"></div>
            <div style="width: 10px; height: 10px; background: #a855f7; border-radius: 2px;"></div>
            <div style="width: 10px; height: 10px; background: #a855f7; border-radius: 2px;"></div>
            <div style="width: 10px; height: 10px; background: #a855f7; border-radius: 2px;"></div>
            <div style="width: 10px; height: 10px; background: #a855f7; border-radius: 2px;"></div>
          </div>
        </div>
      </div>

      <div style="padding: 0.6rem; flex: 1; display: flex; flex-direction: column; justify-content: space-between;">
        <ul style="margin: 0; padding-left: 0.8rem; color: #475569; line-height: 1.4; font-size: 0.8rem; margin-bottom: 0.5rem;">
          <li>단일 AZ 내 인스턴스를 <strong>밀집(뭉침)</strong>시켜 지연 시간 최소화</li>
        </ul>
        <div style="background: #f3f4f6; border-radius: 0.3rem; padding: 0.4rem;">
          <strong style="color: #4b5563; font-size: 0.75rem; display: block; margin-bottom: 0.2rem;">📌 용도</strong>
          <p style="margin: 0; font-size: 0.75rem; color: #64748b; line-height: 1.3;">
            HPC, 머신러닝 등 노드 간 통신 속도가 중요한 작업
          </p>
        </div>
      </div>
    </div>

    <div style="background: white; border: 1px solid #fed7aa; border-radius: 0.5rem; overflow: hidden; display: flex; flex-direction: column;">
      <div style="background: #fff7ed; padding: 0.6rem; border-bottom: 1px solid #fed7aa; text-align: center;">
        <i class="fas fa-arrows-alt-h" style="color: #ea580c; font-size: 1.2rem; margin-bottom: 0.3rem;"></i>
        <div style="font-weight: 800; color: #9a3412; font-size: 0.95rem;">분산 (Spread)</div>
        <span style="font-size: 0.7rem; color: #c2410c; background: #ffedd5; padding: 0.1rem 0.4rem; border-radius: 3px; font-weight: 700;">하드웨어 분리 / 안전</span>
      </div>

      <div style="padding: 0.8rem; background: #fff; display: flex; justify-content: center; gap: 0.5rem; border-bottom: 1px dashed #ffedd5;">
        <div style="border: 2px solid #fdba74; border-radius: 4px; padding: 4px; width: 20px; display: flex; flex-direction: column; align-items: center; justify-content: center; height: 50px; background: #fff7ed;">
          <div style="width: 12px; height: 12px; background: #f97316; border-radius: 2px;"></div>
          <span style="font-size: 0.5rem; color: #f97316; margin-top: auto;">R1</span>
        </div>
        <div style="border: 2px solid #fdba74; border-radius: 4px; padding: 4px; width: 20px; display: flex; flex-direction: column; align-items: center; justify-content: center; height: 50px; background: #fff7ed;">
          <div style="width: 12px; height: 12px; background: #f97316; border-radius: 2px;"></div>
          <span style="font-size: 0.5rem; color: #f97316; margin-top: auto;">R2</span>
        </div>
        <div style="border: 2px solid #fdba74; border-radius: 4px; padding: 4px; width: 20px; display: flex; flex-direction: column; align-items: center; justify-content: center; height: 50px; background: #fff7ed;">
          <div style="width: 12px; height: 12px; background: #f97316; border-radius: 2px;"></div>
          <span style="font-size: 0.5rem; color: #f97316; margin-top: auto;">R3</span>
        </div>
      </div>

      <div style="padding: 0.6rem; flex: 1; display: flex; flex-direction: column; justify-content: space-between;">
        <ul style="margin: 0; padding-left: 0.8rem; color: #475569; line-height: 1.4; font-size: 0.8rem; margin-bottom: 0.5rem;">
          <li style="margin-bottom: 0.3rem;">각 인스턴스를 <strong>서로 다른 하드웨어(Rack)</strong>에 배치</li>
          <li>AZ당 <strong>최대 7개</strong> 제한</li>
        </ul>
        <div style="background: #f3f4f6; border-radius: 0.3rem; padding: 0.4rem;">
          <strong style="color: #4b5563; font-size: 0.75rem; display: block; margin-bottom: 0.2rem;">📌 특징</strong>
          <p style="margin: 0; font-size: 0.75rem; color: #64748b; line-height: 1.3;">
            하드웨어 장애 시 다른 인스턴스 영향 X (격리)
          </p>
        </div>
      </div>
    </div>

    <div style="background: white; border: 1px solid #bbf7d0; border-radius: 0.5rem; overflow: hidden; display: flex; flex-direction: column;">
      <div style="background: #f0fdf4; padding: 0.6rem; border-bottom: 1px solid #bbf7d0; text-align: center;">
        <i class="fas fa-layer-group" style="color: #16a34a; font-size: 1.2rem; margin-bottom: 0.3rem;"></i>
        <div style="font-weight: 800; color: #166534; font-size: 0.95rem;">파티션 (Partition)</div>
        <span style="font-size: 0.7rem; color: #15803d; background: #dcfce7; padding: 0.1rem 0.4rem; border-radius: 3px; font-weight: 700;">대규모 분산 / 빅데이터</span>
      </div>

      <div style="padding: 0.8rem; background: #fff; display: flex; justify-content: center; gap: 0.3rem; border-bottom: 1px dashed #bbf7d0;">
        <div style="border: 1px dashed #86efac; border-radius: 4px; padding: 3px; background: #f0fdf4;">
          <div style="font-size: 0.5rem; color: #16a34a; text-align: center; margin-bottom: 2px;">Partition 1</div>
          <div style="display: flex; gap: 2px;">
             <div style="border: 1px solid #86efac; padding: 1px; width: 14px; display: flex; flex-direction: column; align-items: center; gap: 1px;">
                <div style="width: 8px; height: 8px; background: #22c55e; border-radius: 1px;"></div>
                <div style="width: 8px; height: 8px; background: #22c55e; border-radius: 1px;"></div>
             </div>
             <div style="border: 1px solid #86efac; padding: 1px; width: 14px; display: flex; flex-direction: column; align-items: center; gap: 1px;">
                <div style="width: 8px; height: 8px; background: #22c55e; border-radius: 1px;"></div>
             </div>
          </div>
        </div>
        <div style="border: 1px dashed #86efac; border-radius: 4px; padding: 3px; background: #f0fdf4;">
          <div style="font-size: 0.5rem; color: #16a34a; text-align: center; margin-bottom: 2px;">Partition 2</div>
          <div style="display: flex; gap: 2px;">
             <div style="border: 1px solid #86efac; padding: 1px; width: 14px; display: flex; flex-direction: column; align-items: center; gap: 1px;">
                <div style="width: 8px; height: 8px; background: #22c55e; border-radius: 1px;"></div>
             </div>
             <div style="border: 1px solid #86efac; padding: 1px; width: 14px; display: flex; flex-direction: column; align-items: center; gap: 1px;">
                <div style="width: 8px; height: 8px; background: #22c55e; border-radius: 1px;"></div>
                <div style="width: 8px; height: 8px; background: #22c55e; border-radius: 1px;"></div>
             </div>
          </div>
        </div>
      </div>

      <div style="padding: 0.6rem; flex: 1; display: flex; flex-direction: column; justify-content: space-between;">
        <ul style="margin: 0; padding-left: 0.8rem; color: #475569; line-height: 1.4; font-size: 0.8rem; margin-bottom: 0.5rem;">
          <li>파티션(랙 집합) 단위로 분산하여 하드웨어 공유 X</li>
          <li>단일 AZ 내 <strong>수백 개 EC2</strong> 확장</li>
        </ul>
        <div style="background: #f3f4f6; border-radius: 0.3rem; padding: 0.4rem;">
          <strong style="color: #4b5563; font-size: 0.75rem; display: block; margin-bottom: 0.2rem;">📌 적합한 작업</strong>
          <p style="margin: 0; font-size: 0.75rem; color: #64748b; line-height: 1.3;">
            Hadoop, Kafka, Cassandra (분산 처리)
          </p>
        </div>
      </div>
    </div>

  </div>
</div>
</div>

<div style="margin-bottom: 1.5rem;">
  <h4 style="margin: 0; color: #1e293b; font-weight: 800;">✔️ 배치 그룹 선택 가이드 (Deep Dive)</h4>
</div>

<div class="deep-dive-list">

  <div class="dd-item">
    <div class="dd-header">
      <span class="dd-title">1. 클러스터(Cluster)는 오직 '단일 AZ'</span>
    </div>
    <p class="dd-content">
      클러스터는 최고의 네트워크 속도를 위해 물리적으로 가까이 뭉쳐야 하므로 <span class="code-span">여러 AZ에 걸쳐 생성할 수 없습니다.</span>
      <span class="sub-text">👉 반면, <strong>분산(Spread)</strong>과 <strong>파티션(Partition)</strong> 그룹은 여러 AZ에 걸쳐서 배치 가능합니다.</span>
    </p>
  </div>

  <div class="dd-item">
    <div class="dd-header">
      <span class="dd-title">2. 분산(Spread) vs 파티션(Partition) 구분법</span>
      <span class="dd-badge blue">Tip</span>
    </div>
    <p class="dd-content">
      둘 다 하드웨어를 분리하지만, <span class="code-span">'규모'</span>가 다릅니다.
      <span class="sub-text">
        • 분산: 인스턴스 단위 격리 (AZ당 7개 제한) → <strong>중요 DB</strong><br>
        • 파티션: 그룹 단위 격리 (수백 개 가능) → <strong>Hadoop, Kafka</strong>
      </span>
    </p>
  </div>

  <div class="dd-item">
    <div class="dd-header">
      <span class="dd-title">3. 실행 중인 인스턴스는 이동 불가</span>
    </div>
    <p class="dd-content">
      이미 잘 돌아가고 있는 인스턴스를 나중에 배치 그룹으로 옮길 수는 없습니다.
      <span class="sub-text">👉 <strong>AMI(이미지)를 생성</strong>한 뒤, 배치 그룹을 지정하여 <strong>새 인스턴스로 다시 시작(Launch)</strong>해야 합니다.</span>
    </p>
  </div>

</div>

<br>

### 보안그룹(Security Group)

<div class="story-box" markdown="1">
<h4 style="color: #1e293b; font-weight: 800; margin-bottom: 1rem;">EC2의 가상 방화벽, 보안 그룹(Security Group)</h4>

보안 그룹은 EC2 인스턴스를 보호하기 위한 **가상 방화벽**{: .highlight-text }입니다. 인스턴스로 들어오는 트래픽(Inbound)과 나가는 트래픽(Outbound)을 철저하게 검사하여 허용된 요청만 통과시킵니다.
</div>

<div class="info-box">

<div style="background-color: white; margin-top: 2rem; margin-bottom: 2rem; font-size: 0.9rem; letter-spacing: -0.03em; line-height: 1.5; max-width: 820px; margin-left: auto; margin-right: auto;">

    <div style="background-color: #f8fafc; border: 1px solid #cbd5e1; border-radius: 0.8rem; padding: 1.5rem; margin-bottom: 2rem; position: relative;">

        <h4 style="text-align: center; margin: 0 0 1.5rem 0; color: #334155; font-size: 1.1rem;">
            🏰 <strong>이중 보안 시스템</strong>으로 이해하기
        </h4>

        <div style="position: relative; padding: 2rem; background: #fff; border: 2px dashed #94a3b8; border-radius: 1rem; margin-bottom: 1rem;">

            <div style="position: absolute; top: -12px; left: 20px; background: #94a3b8; color: white; padding: 0.2rem 0.8rem; border-radius: 99px; font-size: 0.8rem; font-weight: 700;">
                1차 관문: 네트워크 ACL (NACL)
            </div>

            <div style="position: absolute; top: 10px; right: 20px; color: #64748b; font-size: 0.75rem;">
                🏢 <strong>아파트 정문 (서브넷 수준)</strong>
            </div>

            <div style="border: 2px solid #3b82f6; background: #eff6ff; border-radius: 0.8rem; padding: 2rem 1.5rem 1.5rem 1.5rem; position: relative; margin-top: 1rem;">

                <div style="position: absolute; top: -12px; left: 20px; background: #3b82f6; color: white; padding: 0.2rem 0.8rem; border-radius: 99px; font-size: 0.8rem; font-weight: 700;">
                    2차 관문: 보안 그룹 (SG)
                </div>

                <div style="position: absolute; top: 10px; right: 20px; color: #1e40af; font-size: 0.75rem;">
                    🚪 <strong>우리집 현관문 (인스턴스 수준)</strong>
                </div>

                <div style="background: white; border: 1px solid #bfdbfe; border-radius: 0.5rem; padding: 1rem; text-align: center; display: flex; align-items: center; justify-content: center; gap: 0.5rem;">

                    <i class="fas fa-server" style="font-size: 1.5rem; color: #f97316;"></i>

                    <div>
                        <strong style="color: #1e293b; display: block;">EC2 인스턴스</strong>
                        <span style="font-size: 0.7rem; color: #64748b;">(가장 소중한 자산)</span>
                    </div>

                </div>

            </div>

        </div>

        <p style="text-align: center; font-size: 0.85rem; color: #475569; margin: 0;">
            트래픽은 <strong>NACL(서브넷)</strong>을 먼저 통과한 뒤, <strong>보안 그룹(인스턴스)</strong>을 통과해야 EC2에 도달합니다.
        </p>

    </div>
</div>

<div class="story-box" markdown="1">
처음 보안 그룹을 생성하면 다음과 같은 **초기 상태(Default)**를 가집니다.

- <strong style="color: #1e293b;">인바운드(Inbound):</strong> <span style="color: #ef4444; font-weight: 700;">모두 차단 (Deny All)</span> <br> <span style="color: #64748b;">→ 규칙을 추가하지 않으면 아무도 들어올 수 없습니다. (암묵적 차단)</span>
- <strong style="color: #1e293b;">아웃바운드(Outbound):</strong> <span style="color: #22c55e; font-weight: 700;">모두 허용 (Allow All)</span> <br> <span style="color: #64748b;">→ 인스턴스에서 외부로 나가는 통신은 기본적으로 다 열려 있습니다.</span>
</div>

<div class="info-box">

<div style="background-color: white; margin-top: 2rem; margin-bottom: 2rem; font-family: 'Apple SD Gothic Neo', sans-serif; font-size: 0.85rem; letter-spacing: -0.03em; line-height: 1.4; max-width: 820px; margin-left: auto; margin-right: auto;">

            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 1rem;">

                <div style="border: 2px solid #3b82f6; border-radius: 0.8rem; overflow: hidden; display: flex; flex-direction: column;">
                    <div style="background: #eff6ff; padding: 0.6rem; text-align: center; border-bottom: 1px solid #3b82f6;">
                        <div style="font-weight: 800; color: #1e40af; font-size: 1rem;">보안 그룹 (SG)</div>
                        <div style="font-size: 0.75rem; color: #60a5fa;">"똑똑한 경호원 (Stateful)"</div>
                    </div>

                    <div style="padding: 0.8rem; flex: 1; display: flex; flex-direction: column; gap: 0.8rem;">

                        <div style="background: #fff; border: 1px solid #bfdbfe; border-radius: 0.5rem; padding: 0.5rem; text-align: center;">
                            <div style="font-size: 0.75rem; font-weight: 700; color: #1e40af; margin-bottom: 0.3rem;">위치: 인스턴스 수준</div>
                            <div style="display: inline-block; position: relative;">
                                <i class="fas fa-server" style="font-size: 2rem; color: #3b82f6;"></i>
                                <i class="fas fa-shield-alt" style="position: absolute; bottom: -5px; right: -5px; font-size: 1rem; color: #1e40af; background: white; border-radius: 50%;"></i>
                            </div>
                            <div style="font-size: 0.7rem; color: #64748b; margin-top: 0.2rem;">개별 인스턴스 방어</div>
                        </div>

                        <div style="background: #f0f9ff; border: 1px dashed #3b82f6; border-radius: 0.5rem; padding: 0.5rem; text-align: center;">
                            <strong style="color: #0369a1; display: block; font-size: 0.75rem; margin-bottom: 0.3rem;">🧠 상태 저장 (Stateful)</strong>
                            <div style="display: flex; align-items: center; justify-content: center; gap: 0.3rem; font-size: 0.7rem; color: #475569;">
                                <span>In</span>
                                <i class="fas fa-long-arrow-alt-right" style="color: #3b82f6;"></i>
                                <span style="background: white; border: 1px solid #cbd5e1; padding: 0.1rem 0.3rem; border-radius: 3px; font-weight: 700;">기억</span>
                                <i class="fas fa-long-arrow-alt-right" style="color: #3b82f6;"></i>
                                <span>Out(자동)</span>
                            </div>
                        </div>

                        <div style="background: #f8fafc; padding: 0.6rem; border-radius: 0.5rem; border: 1px solid #e2e8f0;">
                            <strong style="display: block; font-size: 0.75rem; color: #1e293b; margin-bottom: 0.5rem; text-align: center;">📋 규칙 작동 원리</strong>
                            <div style="display: flex; flex-direction: column; gap: 0.5rem;">

                                <div style="display: flex; align-items: center; gap: 0.5rem; background: white; padding: 0.4rem; border-radius: 4px; border: 1px solid #cbd5e1;">
                                    <div style="background: #dcfce7; color: #166534; padding: 0.1rem 0.3rem; border-radius: 3px; font-weight: 700; font-size: 0.65rem; white-space: nowrap;">허용(Allow)만</div>
                                    <i class="fas fa-random" style="color: #94a3b8; font-size: 0.8rem;"></i>
                                    <div style="font-size: 0.7rem; color: #475569;">규칙 <strong>동시 평가</strong> (순서 X)</div>
                                </div>

                                <div style="display: flex; align-items: center; gap: 0.5rem; background: white; padding: 0.4rem; border-radius: 4px; border: 1px solid #cbd5e1;">
                                    <div style="display: flex; gap: 2px;">
                                        <span style="background: #e2e8f0; color: #475569; padding: 0.1rem 0.3rem; border-radius: 3px; font-size: 0.65rem; font-weight: 700;">IP</span>
                                        <span style="background: #dbeafe; color: #1e40af; padding: 0.1rem 0.3rem; border-radius: 3px; font-size: 0.65rem; font-weight: 700;">SG-ID</span>
                                    </div>
                                    <i class="fas fa-arrow-right" style="color: #94a3b8; font-size: 0.8rem;"></i>
                                    <div style="font-size: 0.7rem; color: #475569;">IP 및 <strong>다른 SG 참조</strong> 가능</div>
                                </div>

                                <div style="display: flex; align-items: center; gap: 0.5rem; background: white; padding: 0.4rem; border-radius: 4px; border: 1px solid #cbd5e1;">
                                    <i class="fas fa-edit" style="color: #3b82f6; font-size: 0.8rem; width: 15px; text-align: center;"></i>
                                    <div style="font-size: 0.7rem; color: #475569;">
                                        <strong>시작 / 수정 시</strong> 적용
                                    </div>
                                </div>

                            </div>
                        </div>

                    </div>
                </div>

                <div style="border: 2px solid #94a3b8; border-radius: 0.8rem; overflow: hidden; display: flex; flex-direction: column;">
                    <div style="background: #f1f5f9; padding: 0.6rem; text-align: center; border-bottom: 1px solid #94a3b8;">
                        <div style="font-weight: 800; color: #475569; font-size: 1rem;">네트워크 ACL (NACL)</div>
                        <div style="font-size: 0.75rem; color: #64748b;">"원칙적인 경비원 (Stateless)"</div>
                    </div>

                    <div style="padding: 0.8rem; flex: 1; display: flex; flex-direction: column; gap: 0.8rem;">

                        <div style="background: #fff; border: 1px solid #cbd5e1; border-radius: 0.5rem; padding: 0.5rem; text-align: center;">
                            <div style="font-size: 0.75rem; font-weight: 700; color: #475569; margin-bottom: 0.3rem;">위치: 서브넷 수준</div>
                            <div style="display: inline-block; border: 2px dashed #94a3b8; padding: 0.3rem; border-radius: 0.4rem;">
                                <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 2px;">
                                    <i class="fas fa-server" style="font-size: 0.8rem; color: #cbd5e1;"></i>
                                    <i class="fas fa-server" style="font-size: 0.8rem; color: #cbd5e1;"></i>
                                    <i class="fas fa-server" style="font-size: 0.8rem; color: #cbd5e1;"></i>
                                    <i class="fas fa-server" style="font-size: 0.8rem; color: #cbd5e1;"></i>
                                </div>
                            </div>
                            <div style="font-size: 0.7rem; color: #64748b; margin-top: 0.2rem;">서브넷 전체 방어</div>
                        </div>

                        <div style="background: #f8fafc; border: 1px dashed #94a3b8; border-radius: 0.5rem; padding: 0.5rem; text-align: center;">
                            <strong style="color: #475569; display: block; font-size: 0.75rem; margin-bottom: 0.3rem;">🤖 비상태 (Stateless)</strong>
                            <div style="display: flex; align-items: center; justify-content: center; gap: 0.3rem; font-size: 0.7rem; color: #475569;">
                                <span>In</span>
                                <i class="fas fa-long-arrow-alt-right" style="color: #94a3b8;"></i>
                                <span style="background: white; border: 1px solid #cbd5e1; padding: 0.1rem 0.3rem; border-radius: 3px; font-weight: 700;">망각</span>
                                <i class="fas fa-long-arrow-alt-right" style="color: #94a3b8;"></i>
                                <span>Out(재검사)</span>
                            </div>
                        </div>

                        <div style="background: #f8fafc; padding: 0.6rem; border-radius: 0.5rem; border: 1px solid #e2e8f0;">
                            <strong style="display: block; font-size: 0.75rem; color: #1e293b; margin-bottom: 0.5rem; text-align: center;">📋 규칙 작동 원리</strong>
                            <div style="display: flex; flex-direction: column; gap: 0.5rem;">

                                <div style="display: flex; align-items: center; gap: 0.5rem; background: white; padding: 0.4rem; border-radius: 4px; border: 1px solid #cbd5e1;">
                                    <div style="display: flex; flex-direction: column; gap: 2px;">
                                        <span style="background: #dcfce7; color: #166534; padding: 0.1rem 0.3rem; border-radius: 3px; font-weight: 700; font-size: 0.65rem; white-space: nowrap;">허용(Allow)</span>
                                        <span style="background: #fee2e2; color: #991b1b; padding: 0.1rem 0.3rem; border-radius: 3px; font-weight: 700; font-size: 0.65rem; white-space: nowrap;">거부(Deny)</span>
                                    </div>
                                    <i class="fas fa-sort-numeric-down" style="color: #94a3b8; font-size: 0.8rem;"></i>
                                    <div style="font-size: 0.7rem; color: #475569;">번호 <strong>순서대로</strong> 평가 (직렬)</div>
                                </div>

                                <div style="display: flex; align-items: center; gap: 0.5rem; background: white; padding: 0.4rem; border-radius: 4px; border: 1px solid #cbd5e1;">
                                    <div style="display: flex; gap: 2px;">
                                        <span style="background: #e2e8f0; color: #475569; padding: 0.1rem 0.3rem; border-radius: 3px; font-size: 0.65rem; font-weight: 700;">IP Only</span>
                                    </div>
                                    <i class="fas fa-arrow-right" style="color: #94a3b8; font-size: 0.8rem;"></i>
                                    <div style="font-size: 0.7rem; color: #475569;">특정 <strong>IP 주소만</strong> 참조 가능</div>
                                </div>

                                <div style="display: flex; align-items: center; gap: 0.5rem; background: white; padding: 0.4rem; border-radius: 4px; border: 1px solid #cbd5e1;">
                                    <i class="fas fa-bolt" style="color: #eab308; font-size: 0.8rem; width: 15px; text-align: center;"></i>
                                    <div style="font-size: 0.7rem; color: #475569;">
                                        <strong>즉시 적용</strong>
                                    </div>
                                </div>

                            </div>
                        </div>

                    </div>
                </div>
            </div>

        </div>
    </div>
</div>

<div class="info-box">
  
  <div style="margin-bottom: 1.5rem;">
    <h4 style="margin: 0; color: #1e293b; font-weight: 800;">✔️ 보안 그룹 vs NACL 실전 포인트 (Deep Dive)</h4>
  </div>

  <div class="deep-dive-list">

    <div class="dd-item">
      <div class="dd-header">
        <span class="dd-title">1. "상태 저장(Stateful)"의 진짜 의미</span>
        <span class="dd-badge blue">핵심</span>
      </div>
      <p class="dd-content">
        보안 그룹은 들어오는 요청(Inbound)을 허용하면, 그 응답으로 나가는 트래픽(Outbound)은 <span class="code-span">규칙과 상관없이 자동으로 허용</span>됩니다. (왕복 티켓)
        <span class="sub-text">👉 반면 <strong>NACL은 Stateless</strong>이므로, 들어올 때 허용했어도 나가는 규칙(Outbound)이 없으면 응답이 차단됩니다.</span>
      </p>
    </div>

    <div class="dd-item">
      <div class="dd-header">
          <span class="dd-title">2. 해커 IP를 차단하려면? 오직 NACL</span>
      </div>
      <p class="dd-content">
          보안 그룹은 <strong>'허용(Allow)' 규칙만</strong> 존재하며, 규칙에 없는 트래픽은 <span class="code-span">암묵적으로 모두 차단</span>됩니다. "특정 IP만 막겠다(Deny)"는 설정은 불가능합니다.
          
          <span class="sub-text gray-box">
              • 특정 공격자 IP 차단(Deny) → <strong>NACL 사용</strong><br>
              • 서비스 포트 개방(Allow) → <strong>보안 그룹 사용</strong>
          </span>
      </p>
    </div>

    <div class="dd-item">
      <div class="dd-header">
        <span class="dd-title">3. 보안 그룹은 '신분증', NACL은 '지명수배'</span>
        <span class="dd-badge pink">비유</span>
      </div>
      <p class="dd-content">
        보안 그룹은 <span class="code-span">다른 보안 그룹 ID를 참조</span>할 수 있습니다. "웹 서버 그룹(A)에서 오는 트래픽만 DB 그룹(B)이 허용한다"는 식의 유연한 설정이 가능합니다.
        <span class="sub-text">👉 반면 NACL은 오직 <strong>IP 대역(CIDR)</strong>으로만 규칙을 설정할 수 있습니다.</span>
      </p>
    </div>

  </div>
</div>

<br>

### User Data

<div class="info-box">
<div style="background-color: white; margin-top: 2rem; margin-bottom: 2rem; font-family: 'Apple SD Gothic Neo', sans-serif; font-size: 0.85rem; letter-spacing: -0.03em; line-height: 1.4; max-width: 800px; margin-left: auto; margin-right: auto; color: #334155;">

  <div style="display: flex; gap: 0.5rem; align-items: stretch; margin-bottom: 2rem;">

    <div style="flex: 1; border: 1px solid #e2e8f0; border-radius: 0.8rem; overflow: hidden; display: flex; flex-direction: column; min-width: 0;">
      <div style="background: #f1f5f9; padding: 0.6rem; text-align: center; border-bottom: 1px solid #e2e8f0;">
        <div style="font-weight: 800; color: #475569; font-size: 0.9rem;">1. 인스턴스 시작</div>
        <div style="font-size: 0.7rem; color: #94a3b8;">(Trigger)</div>
      </div>
      <div style="padding: 1rem; flex: 1; display: flex; flex-direction: column; align-items: center; justify-content: center; background: white;">
        <div style="background: #f8fafc; border: 1px solid #cbd5e1; width: 50px; height: 50px; border-radius: 50%; display: flex; align-items: center; justify-content: center; margin-bottom: 0.5rem;">
          <i class="fas fa-power-off" style="font-size: 1.5rem; color: #475569;"></i>
        </div>
        <div style="text-align: center; font-size: 0.8rem; color: #334155; font-weight: 700;">
          서버 전원 ON
        </div>
      </div>
    </div>

    <div style="display: flex; align-items: center; justify-content: center; color: #cbd5e1;">
      <i class="fas fa-chevron-right" style="font-size: 1.2rem;"></i>
    </div>

    <div style="flex: 1.2; border: 1px solid #e2e8f0; border-radius: 0.8rem; overflow: hidden; display: flex; flex-direction: column; min-width: 0;">
      <div style="background: #f1f5f9; padding: 0.6rem; text-align: center; border-bottom: 1px solid #e2e8f0;">
        <div style="font-weight: 800; color: #475569; font-size: 0.9rem;">2. User Data 실행</div>
        <div style="font-size: 0.7rem; color: #94a3b8;">(Script)</div>
      </div>
      <div style="padding: 0.8rem; flex: 1; display: flex; flex-direction: column; background: white; justify-content: center;">
        <div style="background: #1e293b; border-radius: 0.4rem; padding: 0.6rem; color: #e2e8f0; font-family: monospace; font-size: 0.7rem; margin-bottom: 0.5rem; position: relative; border: 1px solid #334155;">
          <div style="color: #60a5fa; margin-bottom: 4px;">>_ #!/bin/bash</div>
          <div>apt-get update -y</div>
          <div>yum install -y httpd</div>
          <div style="color: #94a3b8;">...running...</div>
        </div>
        <div style="display: flex; justify-content: center; gap: 0.3rem;">
          <span style="background: #f1f5f9; color: #475569; padding: 0.1rem 0.4rem; border-radius: 3px; font-size: 0.7rem; font-weight: 700; border: 1px solid #e2e8f0;">Root 권한</span>
          <span style="background: #f1f5f9; color: #475569; padding: 0.1rem 0.4rem; border-radius: 3px; font-size: 0.7rem; font-weight: 700; border: 1px solid #e2e8f0;">단 1회</span>
        </div>
      </div>
    </div>

    <div style="display: flex; align-items: center; justify-content: center; color: #cbd5e1;">
      <i class="fas fa-chevron-right" style="font-size: 1.2rem;"></i>
    </div>

    <div style="flex: 1.2; border: 1px solid #e2e8f0; border-radius: 0.8rem; overflow: hidden; display: flex; flex-direction: column; min-width: 0;">
      <div style="background: #f1f5f9; padding: 0.6rem; text-align: center; border-bottom: 1px solid #e2e8f0;">
        <div style="font-weight: 800; color: #475569; font-size: 0.9rem;">3. 부트스트랩 완료</div>
        <div style="font-size: 0.7rem; color: #94a3b8;">(Result)</div>
      </div>
      <div style="padding: 0.8rem; flex: 1; display: flex; align-items: center; gap: 0.8rem; background: white;">
        <div style="position: relative; flex-shrink: 0;">
           <div style="background: #f0fdfa; border: 1px solid #ccfbf1; border-radius: 0.5rem; width: 46px; height: 46px; display: grid; grid-template-columns: 1fr 1fr; align-items: center; justify-items: center; padding: 2px;">
             <i class="fas fa-couch" style="color: #0d9488; font-size: 0.7rem;"></i>
             <i class="fas fa-tv" style="color: #0d9488; font-size: 0.7rem;"></i>
             <i class="fas fa-bed" style="color: #0d9488; font-size: 0.7rem;"></i>
             <i class="fas fa-wifi" style="color: #0d9488; font-size: 0.7rem;"></i>
           </div>
           <i class="fas fa-check-circle" style="position: absolute; bottom: -4px; right: -4px; color: #0d9488; background: white; border-radius: 50%; font-size: 0.9rem;"></i>
        </div>
        <div style="display: flex; flex-direction: column; gap: 0.2rem;">
          <div style="font-size: 0.75rem; color: #334155;"><strong>풀옵션 입주</strong> 완료</div>
          <div style="height: 1px; background: #e2e8f0; width: 100%; margin: 2px 0;"></div>
          <div style="font-size: 0.7rem; color: #64748b;">업데이트 OK</div>
          <div style="font-size: 0.7rem; color: #64748b;">설정 OK</div>
        </div>
      </div>
    </div>

  </div>

  <div style="background: #f8fafc; border: 1px solid #e2e8f0; border-radius: 0.8rem; padding: 1.2rem; text-align: center;">
    
    <div style="display: inline-block; background: white; border: 1px dashed #94a3b8; padding: 0.8rem; border-radius: 0.5rem;">
      <span style="color: #475569; font-size: 0.9rem;">
        "EC2에서 <strong style="color: #3b82f6;">Bootstrap(행위)</strong>을 하기 위해<br>
        사용하는 기능이 바로 <strong style="color: #f59e0b;">User Data(도구)</strong>이다."
      </span>
    </div>

    <div style="margin-top: 1rem; display: flex; justify-content: center; gap: 0.5rem; font-size: 0.8rem; color: #64748b;">
      <div style="display: flex; align-items: center; gap: 5px;">
        <i class="fas fa-check-circle" style="color: #10b981;"></i> AWS: <strong>User Data</strong>
      </div>
      <div style="width: 1px; background: #cbd5e1;"></div>
      <div style="display: flex; align-items: center; gap: 5px;">
        <i class="fas fa-check-circle" style="color: #10b981;"></i> Azure: <strong>Custom Data</strong>
      </div>
      <div style="width: 1px; background: #cbd5e1;"></div>
      <div style="display: flex; align-items: center; gap: 5px;">
        <i class="fas fa-check-circle" style="color: #10b981;"></i> GCP: <strong>Startup Script</strong>
      </div>
    </div>
    <div style="margin-top: 0.5rem; font-size: 0.75rem; color: #94a3b8;">
      (클라우드마다 이름은 다르지만, 부트스트랩 기능은 모두 동일합니다)
    </div>

  </div>

</div>
</div>

<div class="story-box" markdown="1">
📦 **User Data (사용자 데이터)**{: .highlight-text } : EC2 서버가 켜지자마자 실행되는 할 일 목록 (Shell Script)<br>
🚀 **OS의 부트스트랩**{: .highlight-text } : 서버 시작 시 특정 명령을 자동으로 실행하는 행위
</div>

<br>

### 탄력적 IP(Elastic IP / EIP)

<div class="story-box" markdown="1">
EC2를 생성할 때 할당받는 일반 공인 IP는 인스턴스를 **중지(Stop)하고 다시 시작(Start)하면 IP 주소가 변경**{: .highlight-text }됩니다. 서버의 주소가 계속 바뀐다면 사용자가 접속할 수 없겠죠? 이를 해결하기 위해 고정된 IP 주소인 **탄력적 IP(Elastic IP)**를 사용합니다.
</div>

<div class="info-box">
<div style="margin-top: 2rem; margin-bottom: 2rem; font-family: 'Apple SD Gothic Neo', sans-serif; font-size: 0.9rem; letter-spacing: -0.03em; line-height: 1.4; max-width: 820px; margin-left: auto; margin-right: auto;">

  <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; margin-bottom: 1rem;">
    
    <div style="border: 1px dashed #94a3b8; border-radius: 0.6rem; padding: 1rem; background: #f8fafc; display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center;">
      <strong style="color: #64748b; font-size: 0.9rem; margin-bottom: 0.5rem;">일반 Public IP</strong>
      
      <div style="display: flex; align-items: center; gap: 0.5rem; margin-bottom: 0.5rem;">
        <div style="position: relative;">
          <i class="fas fa-server" style="font-size: 2rem; color: #cbd5e1;"></i>
          <i class="fas fa-sync-alt" style="position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); color: #ef4444; font-size: 1rem; background: rgba(255,255,255,0.8); border-radius: 50%;"></i>
        </div>
        <i class="fas fa-arrow-right" style="color: #94a3b8;"></i>
        <div style="display: flex; flex-direction: column; font-family: monospace; font-size: 0.8rem;">
          <span style="color: #94a3b8; text-decoration: line-through;">1.2.3.4</span>
          <span style="color: #ef4444; font-weight: 700;">5.6.7.8 (New)</span>
        </div>
      </div>
      
      <div style="font-size: 0.8rem; color: #475569; background: white; padding: 0.3rem 0.6rem; border-radius: 4px; border: 1px solid #e2e8f0;">
        EC2 인스턴스를 중지 후 재시작<br>→ <strong>Public IP가 변경</strong>
      </div>
    </div>

    <div style="border: 2px solid #3b82f6; border-radius: 0.6rem; padding: 1rem; background: #eff6ff; display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center;">
      <strong style="color: #1e40af; font-size: 0.9rem; margin-bottom: 0.5rem;">탄력적 IP (EIP)</strong>

      <div style="display: flex; align-items: center; gap: 0.5rem; margin-bottom: 0.5rem;">
        <i class="fas fa-server" style="font-size: 2rem; color: #3b82f6;"></i>
        <div style="height: 2px; width: 20px; background: #3b82f6;"></div>
        <div style="background: #1e3a8a; color: white; padding: 0.3rem 0.6rem; border-radius: 4px; font-family: monospace; font-size: 0.85rem; font-weight: 700; display: flex; align-items: center; gap: 5px;">
          <i class="fas fa-map-pin" style="color: #fbbf24;"></i> 54.123.x.x
        </div>
      </div>

      <div style="font-size: 0.8rem; color: #1e3a8a;">
        인스턴스에 <strong>고정된 공용 IP</strong>가 필요할 경우
      </div>
    </div>

  </div>
</div>
</div>

<div class="story-box" markdown="1" style="margin-bottom: 0rem;">
EIP는 단순히 '고정 IP'를 제공하는 것 이상의 의미가 있습니다. 인스턴스에 장애가 발생했을 때, EIP 주소의 연결을 **건강한 인스턴스로 즉시 재연결(Remapping)**{: .highlight-text }하여 서비스 중단을 마스킹할 수 있습니다.

하지만 IPv4 주소는 전 세계적으로 부족한 자원입니다. AWS는 사용자가 IP를 낭비하지 않도록 독특한 과금 정책을 가지고 있습니다.
</div>
<div class="info-box" style="margin-bottom:2.5rem">
  <div style="display: grid; grid-template-columns: 1fr 2fr; gap: 1rem;">
    <div style="background: white; border: 1px solid #e2e8f0; border-radius: 0.6rem; padding: 0.8rem; display: flex; align-items: center; justify-content: center; gap: 0.5rem;">
      <div style="text-align: center;">
        <i class="fas fa-server" style="color: #64748b;"></i>
        <div style="font-size: 0.75rem; color: #64748b;">1 인스턴스</div>
      </div>
      <i class="fas fa-long-arrow-alt-right" style="color: #3b82f6;"></i>
      <div style="text-align: center;">
        <i class="fas fa-tag" style="color: #3b82f6;"></i>
        <div style="font-size: 0.75rem; color: #1e40af; font-weight: 700;">1 EIP</div>
      </div>
    </div>
    <div style="background: #fff7ed; border: 1px solid #fed7aa; border-radius: 0.6rem; padding: 0.8rem; display: flex; align-items: center; gap: 1rem;">
      <div style="background: #fff; width: 40px; height: 40px; border-radius: 50%; display: flex; align-items: center; justify-content: center; border: 2px solid #f97316; flex-shrink: 0;">
        <i class="fas fa-dollar-sign" style="color: #f97316; font-size: 1.2rem;"></i>
      </div>
      <div style="font-size: 0.8rem; color: #9a3412; line-height: 1.3;">
        <strong>⚠️ 비용 주의:</strong> 릴리즈(반납) 하지 않으면 소유<br>
        → 연결 안 된(미연결) EIP도 <strong>비용 발생</strong>
      </div>
    </div>
  </div>
</div>

<div style="margin-bottom: 1.5rem;">
  <h4 style="margin: 0; color: #1e293b; font-weight: 800;">✔️ EIP 아키텍처 패턴 (Deep Dive)</h4>
</div>

<div class="deep-dive-list">

  <div class="dd-item">
    <div class="dd-header">
      <span class="dd-title">1. 장애 조치용으로 사용 (Masking Failure)</span>
    </div>
    <p class="dd-content">
      소프트웨어적인 문제로 인스턴스가 먹통이 되었을 때, 엔지니어가 EIP의 연결 대상을 <span class="code-span">대기 중인(Standby) 인스턴스로 즉시 변경</span>하여 서비스 다운타임을 최소화할 수 있습니다.
    </p>
  </div>

  <div class="dd-item">
    <div class="dd-header">
      <span class="dd-title">2. 되도록 EIP 대신 ELB를 쓰세요</span>
      <span class="dd-badge green">권장</span>
    </div>
    <p class="dd-content">
      EIP는 리전당 <strong>기본 5개로 제한</strong>(Soft Limit)되어 있습니다. 확장성 있는 서비스를 위해서는 고정 IP(EIP)에 의존하기보다, <span class="code-span">로드 밸런서(ELB)의 DNS 주소</span>를 사용하는 것이 훨씬 좋은 아키텍처입니다.
    </p>
  </div>

</div>

<br>

### AMI(Amazon Machine Image)

<div class="story-box" markdown="1">
컴퓨터를 새로 살 때마다 윈도우를 설치하고, 한글/오피스를 깔고, 환경 설정을 다시 하는 것은 매우 번거롭습니다. 만약 내 컴퓨터의 현재 상태를 **그대로 복제해서 '틀(Mold)'로 만들어둔다면** 어떨까요?

**AMI(Amazon Machine Image)**가 바로 그 역할을 합니다. EC2 인스턴스를 실행하기 위해 필요한 운영체제, 애플리케이션, 설정값 등을 모두 담고 있는 **'마스터 이미지'**입니다.
</div>

<div class="info-box">
<div style="background-color: white; margin-bottom: 2rem; font-family: 'Apple SD Gothic Neo', sans-serif; font-size: 0.85rem; letter-spacing: -0.03em; line-height: 1.4; max-width: 820px; margin-left: auto; margin-right: auto; color: #334155;">
  <div style="display: flex; gap: 1rem; border: 2px solid #cbd5e1; border-radius: 0.8rem; padding: 1.5rem 1rem; margin-bottom: 1rem; align-items: center; background: #f8fafc;">  
    <div style="text-align: center; min-width: 100px;">
      <i class="fas fa-save" style="font-size: 2.5rem; color: #475569; margin-bottom: 0.5rem;"></i>
      <div style="font-weight: 800; font-size: 0.9rem; color: #1e293b;">AMI</div>
      <div style="font-size: 0.7rem; color: #64748b;">(붕어빵 틀)</div>
    </div>
    <div style="flex: 1; border-left: 2px dashed #cbd5e1; padding-left: 1rem;">
      <div style="font-weight: 800; color: #334155; font-size: 0.95rem; margin-bottom: 0.5rem;">
        EC2 인스턴스를 찍어내는 <span style="color: #2563eb;">완벽한 설계도</span>
      </div>
      <div style="display: flex; gap: 0.3rem; margin-bottom: 0.8rem; flex-wrap: wrap;">
        <span style="background: white; border: 1px solid #94a3b8; padding: 0.2rem 0.5rem; border-radius: 4px; font-size: 0.75rem; color: #475569;">운영 체제</span>
        <span style="background: white; border: 1px solid #94a3b8; padding: 0.2rem 0.5rem; border-radius: 4px; font-size: 0.75rem; color: #475569;">설치된 프로그램</span>
        <span style="background: white; border: 1px solid #94a3b8; padding: 0.2rem 0.5rem; border-radius: 4px; font-size: 0.75rem; color: #475569;">권한/설정</span>
      </div>
      <div style="font-size: 0.8rem; color: #475569;">
        <i class="fas fa-check-circle" style="color: #10b981; margin-right: 5px;"></i>
        이 이미지 하나만 있으면 똑같은 컴퓨터를 100대든 1000대든 즉시 생성 가능
      </div>
    </div>
  </div>
  <div style="display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 0.8rem;">
    <div style="background: white; border: 1px solid #e2e8f0; border-radius: 0.6rem; padding: 1rem 0.5rem; text-align: center;">
      <div style="color: #f59e0b; margin-bottom: 0.3rem;"><i class="fas fa-users" style="font-size: 1.2rem;"></i></div>
      <strong style="display: block; font-size: 0.85rem; color: #1e293b; margin-bottom: 0.2rem;">1. 공용 AMI</strong>
      <div style="font-size: 0.75rem; color: #64748b;">AWS 기본 제공<br>(Linux, Windows 등)</div>
    </div>
    <div style="background: white; border: 1px solid #e2e8f0; border-radius: 0.6rem; padding: 1rem 0.5rem; text-align: center;">
      <div style="color: #3b82f6; margin-bottom: 0.3rem;"><i class="fas fa-user-cog" style="font-size: 1.2rem;"></i></div>
      <strong style="display: block; font-size: 0.85rem; color: #1e293b; margin-bottom: 0.2rem;">2. 사용자 정의 AMI</strong>
      <div style="font-size: 0.75rem; color: #64748b;">내가 직접 세팅하고<br>저장한 나만의 이미지</div>
    </div>
    <div style="background: white; border: 1px solid #e2e8f0; border-radius: 0.6rem; padding: 1rem 0.5rem; text-align: center;">
      <div style="color: #10b981; margin-bottom: 0.3rem;"><i class="fas fa-shopping-cart" style="font-size: 1.2rem;"></i></div>
      <strong style="display: block; font-size: 0.85rem; color: #1e293b; margin-bottom: 0.2rem;">3. 마켓플레이스</strong>
      <div style="font-size: 0.75rem; color: #64748b;">검증된 기업의 솔루션<br>(구매하여 사용)</div>
    </div>
  </div>
</div>
</div>

<div class="story-box" markdown="1">
가장 많이 사용하는 방식은 **[사용자 정의 AMI]**입니다. EC2를 하나 띄워서 필요한 세팅을 완벽하게 끝낸 후, 그것을 본떠서 이미지를 만듭니다. 이때 **데이터 무결성(Data Integrity)**{: .highlight-text }을 위해 인스턴스를 잠시 멈추는 것이 권장됩니다.
</div>

<div class="info-box">
<div style="background-color: white; margin-bottom: 2rem; font-family: 'Apple SD Gothic Neo', sans-serif; font-size: 0.85rem; letter-spacing: -0.03em; line-height: 1.4; max-width: 820px; margin-left: auto; margin-right: auto; color: #334155;">
  <div style="border: 2px solid #475569; border-radius: 0.8rem; overflow: hidden;">
    <div style="background: #475569; color: white; padding: 0.6rem; text-align: center; font-weight: 800; font-size: 0.9rem;">
      🛠️ AMI 생성 및 활용 과정
    </div>
    <div style="display: flex; align-items: stretch; background: white; padding: 1.5rem 1rem; gap: 0.5rem; justify-content: space-between;">
      <div style="flex: 1; display: flex; flex-direction: column; align-items: center; text-align: center;">
        <div style="background: #eff6ff; color: #3b82f6; width: 40px; height: 40px; border-radius: 50%; display: flex; align-items: center; justify-content: center; margin-bottom: 0.5rem; border: 1px solid #bfdbfe;">
          <strong style="font-size: 0.9rem;">1</strong>
        </div>
        <div style="font-weight: 700; font-size: 0.8rem; color: #334155; margin-bottom: 0.2rem;">원본 세팅</div>
        <div style="font-size: 0.7rem; color: #64748b;">EC2 시작 후<br>SW 설치/설정</div>
      </div>
      <div style="display: flex; align-items: center; color: #cbd5e1;"><i class="fas fa-chevron-right"></i></div>
      <div style="flex: 1; display: flex; flex-direction: column; align-items: center; text-align: center;">
        <div style="background: #fef2f2; color: #ef4444; width: 40px; height: 40px; border-radius: 50%; display: flex; align-items: center; justify-content: center; margin-bottom: 0.5rem; border: 1px solid #fecaca;">
          <strong style="font-size: 0.9rem;">2</strong>
        </div>
        <div style="font-weight: 700; font-size: 0.8rem; color: #334155; margin-bottom: 0.2rem;">인스턴스 중지</div>
        <div style="font-size: 0.7rem; color: #64748b;">파일 손상 방지<br>(권장 사항)</div>
      </div>
      <div style="display: flex; align-items: center; color: #cbd5e1;"><i class="fas fa-chevron-right"></i></div>
      <div style="flex: 1; display: flex; flex-direction: column; align-items: center; text-align: center;">
        <div style="background: #f0fdf4; color: #10b981; width: 40px; height: 40px; border-radius: 50%; display: flex; align-items: center; justify-content: center; margin-bottom: 0.5rem; border: 1px solid #bbf7d0;">
          <strong style="font-size: 0.9rem;">3</strong>
        </div>
        <div style="font-weight: 700; font-size: 0.8rem; color: #334155; margin-bottom: 0.2rem;">AMI 생성</div>
        <div style="font-size: 0.7rem; color: #64748b;">스냅샷 자동 생성<br>및 이미지 등록</div>
      </div>
      <div style="display: flex; align-items: center; color: #cbd5e1;"><i class="fas fa-chevron-right"></i></div>
      <div style="flex: 1; display: flex; flex-direction: column; align-items: center; text-align: center;">
        <div style="background: #fffbeb; color: #f59e0b; width: 40px; height: 40px; border-radius: 50%; display: flex; align-items: center; justify-content: center; margin-bottom: 0.5rem; border: 1px solid #fde68a;">
          <strong style="font-size: 0.9rem;">4</strong>
        </div>
        <div style="font-weight: 700; font-size: 0.8rem; color: #334155; margin-bottom: 0.2rem;">대량 배포</div>
        <div style="font-size: 0.7rem; color: #64748b;">AMI를 사용하여<br>새 인스턴스 시작</div>
      </div>
    </div>
  </div>
</div>
</div>

<div class="story-box" markdown="1"  style="margin-bottom:0">
<h4 style="color: #1e293b; font-weight: 800; margin-bottom: 1rem;">Q. User Data도 초기 설정을 하잖아요? 뭐가 달라요?</h4>

맞습니다. 둘 다 인스턴스를 처음 시작할 때 환경을 구성하는 방법입니다. 가장 큰 차이는 **"설정을 언제 하느냐(Timing)"**입니다.
쉽게 비유하자면 **냉동 피자(AMI)**와 **밀키트(User Data)**의 차이와 같습니다.
</div>

<div class="info-box">
  <div style="font-family: 'Apple SD Gothic Neo', sans-serif; font-size: 0.8rem; letter-spacing: -0.03em; line-height: 1.4; margin-left: auto; margin-right: auto; color: #334155;">
  <div style="background: #f8fafc; border: 1px solid #e2e8f0; border-radius: 0.6rem; padding: 0.8rem;">
    <div style="font-weight: 800; color: #1e293b; margin-bottom: 0.6rem; text-align: center; font-size: 0.85rem;">
      ⚖️ 기술적 차이점 요약
    </div>    
    <div style="display: grid; grid-template-columns: repeat(4, 1fr); gap: 0.4rem; text-align: center;">
      <div style="font-weight: 700; color: #64748b; font-size: 0.75rem; padding-bottom: 0.3rem; border-bottom: 1px solid #cbd5e1;">구분</div>
      <div style="font-weight: 700; color: #7c3aed; font-size: 0.75rem; padding-bottom: 0.3rem; border-bottom: 1px solid #a78bfa;">AMI</div>
      <div style="font-weight: 700; color: #2563eb; font-size: 0.75rem; padding-bottom: 0.3rem; border-bottom: 1px solid #93c5fd;">User Data</div>
      <div style="font-weight: 700; color: #059669; font-size: 0.75rem; padding-bottom: 0.3rem; border-bottom: 1px solid #6ee7b7;">추천</div>
      <div style="font-size: 0.75rem; color: #334155; padding: 0.3rem 0; border-bottom: 1px solid #e2e8f0;">설정 시점</div>
      <div style="font-size: 0.75rem; color: #475569; padding: 0.3rem 0; border-bottom: 1px solid #e2e8f0; background: #faf5ff;">Baking (생성)</div>
      <div style="font-size: 0.75rem; color: #475569; padding: 0.3rem 0; border-bottom: 1px solid #e2e8f0; background: #eff6ff;">Boot (부팅)</div>
      <div style="font-size: 0.7rem; color: #94a3b8; padding: 0.3rem 0; border-bottom: 1px solid #e2e8f0;">-</div>
      <div style="font-size: 0.75rem; color: #334155; padding: 0.3rem 0; border-bottom: 1px solid #e2e8f0;">부팅 속도</div>
      <div style="font-size: 0.75rem; color: #7c3aed; font-weight: 700; padding: 0.3rem 0; border-bottom: 1px solid #e2e8f0; background: #faf5ff;">빠름 🚀</div>
      <div style="font-size: 0.75rem; color: #94a3b8; padding: 0.3rem 0; border-bottom: 1px solid #e2e8f0; background: #eff6ff;">느림 🐢</div>
      <div style="font-size: 0.7rem; color: #64748b; padding: 0.3rem 0; border-bottom: 1px solid #e2e8f0;">설치 많으면 AMI</div>
      <div style="font-size: 0.75rem; color: #334155; padding: 0.3rem 0;">업데이트</div>
      <div style="font-size: 0.75rem; color: #94a3b8; padding: 0.3rem 0; background: #faf5ff;">어려움</div>
      <div style="font-size: 0.75rem; color: #2563eb; font-weight: 700; padding: 0.3rem 0; background: #eff6ff;">쉬움</div>
      <div style="font-size: 0.7rem; color: #64748b; padding: 0.3rem 0;">잦은 변경 User Data</div>
    </div>
  </div>
</div>
</div>

<div style="margin-bottom: 1.5rem;">
  <h4 style="margin: 0; color: #1e293b; font-weight: 800;">✔️ 실전 운영 및 시험 포인트 (Deep Dive)</h4>
</div>

<div class="deep-dive-list">

  <div class="dd-item">
    <div class="dd-header">
      <span class="dd-title">1. AMI는 리전(Region)에 종속됩니다.</span>
    </div>
    <p class="dd-content">
      서울 리전(ap-northeast-2)에서 만든 AMI는 버지니아 리전(us-east-1)에서 바로 보이지 않습니다. 다른 리전에서 사용하려면 <span class="code-span">AMI 복사(Copy AMI)</span> 기능을 통해 해당 리전으로 이미지를 복제해야 합니다.
    </p>
  </div>

  <div class="dd-item">
    <div class="dd-header">
      <span class="dd-title">2. 실무 권장: 하이브리드 전략 (Golden Image)</span>
      <span class="dd-badge green">Best Practice</span>
    </div>
    <p class="dd-content">
      어느 하나만 쓰기보다는 둘을 섞어 쓰는 것이 좋습니다.

       <div style="text-align: center; margin-top:1rem; margin-bottom:1rem;">
          <div style="display: inline-block; background: #fff; border: 1px dashed #cbd5e1; border-radius: 0.6rem; padding: 0.8rem; width: 100%; box-sizing: border-box;">
            <strong style="color: #059669; font-size: 0.85rem;">💡 실무 Best Practice (하이브리드 전략)</strong>
            <div style="display: flex; flex-wrap: wrap; align-items: center; justify-content: center; gap: 0.5rem; margin-top: 0.5rem;">
              <div style="background: #f5f3ff; color: #7c3aed; padding: 0.2rem 0.5rem; border-radius: 4px; font-size: 0.75rem; border: 1px solid #ddd6fe;">
                <strong>AMI</strong> (무거운 설치)
              </div>
              <i class="fas fa-plus" style="color: #94a3b8; font-size: 0.7rem;"></i>
              <div style="background: #eff6ff; color: #2563eb; padding: 0.2rem 0.5rem; border-radius: 4px; font-size: 0.75rem; border: 1px solid #bfdbfe;">
                <strong>User Data</strong> (가벼운 설정)
              </div>
            </div>
            <div style="font-size: 0.75rem; color: #64748b; margin-top: 0.3rem;">
              = <strong style="color: #475569;">Golden Image</strong> 전략 (속도와 유연성 모두 확보)
            </div>
          </div>
        </div>
        
      <span class="sub-text">
        • <strong>AMI:</strong> 설치가 오래 걸리는 OS 패치, DB 엔진 등을 미리 설치<br>
        • <strong>User Data:</strong> 자주 바뀌는 설정 파일이나 최신 코드만 부팅 시 다운로드
      </span>
    </p>
  </div>

  <div class="dd-item">
    <div class="dd-header">
      <span class="dd-title">3. AMI 삭제 시 스냅샷도 지워야 합니다.</span>
      <span class="dd-badge red">비용 주의</span>
    </div>
    <p class="dd-content">
      AMI 등록을 해제(Deregister)해도, 원본이 되는 <strong>EBS 스냅샷은 S3에 그대로 남아 과금</strong>됩니다. 완전히 삭제하려면 AMI 해제 후 스냅샷도 별도로 삭제해야 합니다.
    </p>
  </div>

</div>

<br>