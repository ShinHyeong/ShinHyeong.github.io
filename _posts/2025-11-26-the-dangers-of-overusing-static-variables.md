---
title:  "Static 변수를 남용하면 안되는 이유"
categories: Java
excerpt: "편리해 보이지만 위험한 Static 변수. Static 변수의 특성을 생각하며, 메모리 구조와 멀티스레드 환경에서의 치명적인 단점을 알아보자."
#redirect_from: #이전주소 입력
#search: false #만약 이 글이 검색되지 않기를 바란다면
#use_math: true #수식이 필요한 경우만 사용
---
<div class="story-box" markdown="1">

개발을 하다 보면 객체 생성 없이 바로 접근할 수 있는 `static` 변수의 편리함에 끌릴 때가 있습니다. 하지만 이 편리함 뒤에는 <strong class="highlight-text">'공유'</strong>라는 치명적인 특성이 숨어 있습니다. 

오늘은 Static 변수와 인스턴스 변수의 결정적인 차이를 알아보고, 왜 Static 변수를 상태 관리에 사용하면 안 되는지 구체적인 예시를 통해 알아보겠습니다.

</div>

## Static 변수 vs. Instance 변수

<div class="story-box" markdown="1">

가장 먼저 이해해야 할 것은 <strong class="highlight-text">'데이터가 어디에 저장되고, 누구와 공유되는가'</strong>입니다. 이 차이가 모든 문제의 시작점입니다.

</div>

<div class="info-box" style="max-width: 800px; margin: 0 auto 2rem auto; font-family: 'Pretendard', -apple-system, sans-serif; background-color: #f8f9fa; border-radius: 12px; padding: 30px; box-shadow: 0 4px 20px rgba(0,0,0,0.05); color: #333;">
  <div style="display: flex; gap: 20px; flex-wrap: wrap;">
    <div style="flex: 1; min-width: 300px; background: #fff; border: 2px solid #e74c3c; border-radius: 12px; padding: 35px 20px 25px 20px; position: relative; overflow: hidden; box-shadow: 0 4px 15px rgba(231, 76, 60, 0.15); display: flex; flex-direction: column; align-items: center;">
      <div style="position: absolute; top: 0; left: 0; background: #e74c3c; color: #fff; padding: 5px 15px; font-size: 12px; font-weight: bold; border-bottom-right-radius: 10px;">위험 (공유)</div>
      <div style="text-align: center; color: #e74c3c; margin: 0 0 5px 0; font-size: 22px; font-weight: 800;">Static 변수</div>
      <p style="text-align: center; font-size: 14px; color: #7f8c8d; margin: 0 0 20px 0; font-weight: 500;">Method Area (클래스 로딩 시 1회 생성)</p>
      <div style="margin: 10px 0 20px 0;">
        <span style="font-size: 60px; filter: drop-shadow(0 4px 6px rgba(231, 76, 60, 0.3));">☁️</span>
      </div>
      <div style="font-weight: 800; color: #c0392b; margin-bottom: 15px; font-size: 15px;">단 하나의 공유 공간</div>
      <div style="display: flex; justify-content: center; align-items: center; gap: 15px; width: 100%;">
        <div style="display: flex; flex-direction: column; gap: 8px;">
          <div style="background: #ecf0f1; padding: 6px 10px; border-radius: 15px; font-size: 11px; display: flex; align-items: center; gap: 4px; color: #555;"><span style="font-size: 14px;">🤖</span> 객체 A</div>
          <div style="background: #ecf0f1; padding: 6px 10px; border-radius: 15px; font-size: 11px; display: flex; align-items: center; gap: 4px; color: #555;"><span style="font-size: 14px;">👾</span> 객체 B</div>
          <div style="background: #ecf0f1; padding: 6px 10px; border-radius: 15px; font-size: 11px; display: flex; align-items: center; gap: 4px; color: #555;"><span style="font-size: 14px;">🎃</span> 객체 C</div>
        </div>
        <div style="display: flex; flex-direction: column; justify-content: center; color: #e74c3c; font-size: 20px; font-weight: bold;">
             <span>↘️</span><span>➡️</span><span>↗️</span>
        </div>
        <div style="background: #fadbd8; border: 3px dotted #e74c3c; padding: 10px; border-radius: 12px; text-align: center; flex: 1; display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 80px;">
          <div style="font-size: 28px; margin-bottom: 5px;">📦</div>
          <div style="font-weight: bold; color: #c0392b; font-size: 12px; line-height: 1.2;">실제 값<br>(모두가 여기를 봄)</div>
        </div>
      </div>
    </div>
    <div style="flex: 1; min-width: 300px; background: #fff; border: 2px solid #27ae60; border-radius: 12px; padding: 35px 20px 25px 20px; position: relative; overflow: hidden; box-shadow: 0 4px 15px rgba(39, 174, 96, 0.15); display: flex; flex-direction: column; align-items: center;">
      <div style="position: absolute; top: 0; left: 0; background: #27ae60; color: #fff; padding: 5px 15px; font-size: 12px; font-weight: bold; border-bottom-right-radius: 10px;">안전 (독립)</div>
      <div style="text-align: center; color: #27ae60; margin: 0 0 5px 0; font-size: 22px; font-weight: 800;">Instance 변수</div>
      <p style="text-align: center; font-size: 14px; color: #7f8c8d; margin: 0 0 20px 0; font-weight: 500;">Heap Area (객체 생성 시 마다 생성)</p>
      <div style="margin: 10px 0 20px 0; display: flex; gap: 15px;">
        <span style="font-size: 50px; filter: drop-shadow(0 4px 6px rgba(39, 174, 96, 0.3));">📦</span>
        <span style="font-size: 50px; filter: drop-shadow(0 4px 6px rgba(39, 174, 96, 0.3)); opacity: 0.6;">📦</span>
      </div>
      <div style="font-weight: 800; color: #1e8449; margin-bottom: 15px; font-size: 15px;">객체별 독립 공간</div>
      <div style="display: flex; flex-direction: column; gap: 12px; width: 100%; margin-top: 5px;">
        <div style="display: flex; align-items: center; gap: 10px;">
          <div style="background: #ecf0f1; padding: 8px 10px; border-radius: 15px; font-size: 11px; width: 70px; display: flex; align-items: center; justify-content: center; gap: 4px; color: #555;"><span style="font-size: 14px;">🤖</span> 객체 A</div>
          <div style="color: #27ae60; font-size: 20px; font-weight: bold;">➡️</div>
          <div style="background: #d5f5e3; border: 2px solid #27ae60; padding: 8px 10px; border-radius: 12px; font-size: 12px; flex: 1; display: flex; align-items: center; gap: 8px; color: #1e8449; font-weight: bold;">
            <span style="font-size: 18px;">👜</span> A 주머니 속 값
          </div>
        </div>
        <div style="display: flex; align-items: center; gap: 10px;">
          <div style="background: #ecf0f1; padding: 8px 10px; border-radius: 15px; font-size: 11px; width: 70px; display: flex; align-items: center; justify-content: center; gap: 4px; color: #555;"><span style="font-size: 14px;">👾</span> 객체 B</div>
          <div style="color: #27ae60; font-size: 20px; font-weight: bold;">➡️</div>
          <div style="background: #d5f5e3; border: 2px solid #27ae60; padding: 8px 10px; border-radius: 12px; font-size: 12px; flex: 1; display: flex; align-items: center; gap: 8px; color: #1e8449; font-weight: bold;">
            <span style="font-size: 18px;">🎒</span> B 주머니 속 값
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<div class="story-box" markdown="1">

* **Static 변수 (공용 칠판)**
    * 프로그램이 시작될 때 **Method Area**에 딱 **하나**만 생성됩니다.
    * 모든 인스턴스(객체)가 이 <strong class="highlight-text">하나의 공간을 공유</strong>합니다.
    * 쉽게 말해, 교실 앞에 있는 **'공용 칠판'**과 같습니다. 누군가 칠판에 낙서를 하면, 반 전체 학생이 그 낙서를 보게 됩니다.

* **Instance 변수 (개인 공책)**
    * `new` 연산자로 객체를 생성할 때마다 **Heap Area**에 <strong class="highlight-text">매번 새로</strong> 생성됩니다.
    * 각 객체는 자신만의 독립적인 값을 가집니다.
    * 이는 학생 개개인이 가진 **'개인 공책'**과 같습니다. 내가 공책에 필기를 해도 짝꿍의 공책에는 아무런 영향을 주지 않습니다.

</div>

<br>

## Static 남용 시 발생하는 문제점

<div class="story-box" markdown="1">

"공유한다"는 것은 효율적으로 보일 수 있지만, 값이 수시로 변하는 상황(<strong class="highlight-text">가변 상태</strong>)에서는 재앙이 될 수 있습니다. 대표적인 두 가지 문제 상황을 시뮬레이션 해보겠습니다.

</div>

<div class="info-box" style="max-width: 1200px; margin: 0 auto 2rem auto; font-family: 'Pretendard', -apple-system, sans-serif; display: flex; gap: 20px; flex-wrap: wrap; align-items: stretch;">
    <div style="flex: 1; min-width: 320px; background: #fff; border: 1px solid #e0e0e0; border-top: 5px solid #e67e22; border-radius: 8px; box-shadow: 0 4px 15px rgba(0,0,0,0.05); overflow: hidden; display: flex; flex-direction: column;">
        <div style="padding: 15px 20px; border-bottom: 1px solid #f0f0f0; background: #fffcf5;">
        <div style="margin: 0; color: #d35400; font-size: 18px; display: flex; align-items: center; font-weight: 700;">
            <span style="font-size: 22px; margin-right: 8px;">1️⃣</span> 데이터 덮어쓰기 (Overwritting)
        </div>
        <p style="margin: 5px 0 0 0; font-size: 13px; color: #888;">"나는 철수를 저장했는데, 왜 영희가 나오지?"</p>
        </div>
        <div style="padding: 25px 15px; display: flex; flex-direction: column; gap: 20px; flex: 1; justify-content: center;">
        <div style="display: flex; align-items: center; justify-content: center; gap: 8px; flex-wrap: nowrap;"> <div style="flex: 1; min-width: 90px; text-align: center; position: relative;">
            <div style="position: absolute; top: -10px; left: 50%; transform: translateX(-50%); background: #e67e22; color: #fff; font-size: 9px; font-weight: bold; padding: 2px 6px; border-radius: 10px;">STEP 1</div>
            <div style="font-size: 24px; margin-bottom: 4px;">🤖</div> <div style="font-size: 11px; font-weight: bold; color: #555;">객체 A</div>
            <div style="background: #fdf2e9; border: 1px solid #f5cba7; color: #d35400; font-size: 10px; padding: 4px; border-radius: 6px; margin-top: 4px; white-space: nowrap;"> name = <strong>철수</strong> ➡️ 
            </div>
            </div>
            <div style="flex: 1.2; min-width: 120px; background: #fff; border: 2px dashed #d35400; border-radius: 10px; padding: 12px 8px; text-align: center; position: relative; z-index: 1;"> <div style="position: absolute; top: -8px; left: 50%; transform: translateX(-50%); background: #fff; border: 1px solid #d35400; color: #d35400; font-size: 9px; font-weight: bold; padding: 1px 6px; border-radius: 4px; white-space: nowrap;">공유된 Static 변수</div>
            <div style="margin-top: 8px; display: flex; flex-direction: column; align-items: center; gap: 3px;">
                <div style="color: #ccc; text-decoration: line-through; font-size: 12px;">name = "철수"</div>
                <div style="color: #d35400; font-size: 14px;">⬇️</div>
                <div style="color: #d35400; font-size: 14px; font-weight: 800; background: #fffcf5; padding: 3px 5px;border-radius: 4px;">name = "영희"</div>
            </div>
            </div>
            <div style="flex: 1; min-width: 90px; text-align: center; position: relative;">
            <div style="position: absolute; top: -10px; left: 50%; transform: translateX(-50%); background: #e67e22; color: #fff; font-size: 9px; font-weight: bold; padding: 2px 6px; border-radius: 10px; opacity: 0.8;">STEP 2</div>
            <div style="font-size: 24px; margin-bottom: 4px;">👾</div> <div style="font-size: 11px; font-weight: bold; color: #555;">객체 B</div>
            <div style="background: #fdf2e9; border: 1px solid #f5cba7; color: #d35400; font-size: 10px; padding: 4px; border-radius: 6px; margin-top: 4px; white-space: nowrap;">⬅️ name = <strong>영희</strong>
            </div>
            </div>
        </div>
        </div>
        <div style="background: #fbeee6; padding: 15px; text-align: center; border-top: 1px solid #f0e0d0; margin-top: auto;">
        <div style="display: inline-block; text-align: left;">
            <span style="font-size: 14px;">🤯 <strong>결과:</strong> 나중에 <strong>객체 A</strong>가 확인하면?</span><br>
            <span style="font-size: 13px; color: #a04000; margin-left: 24px;">→ "철수"는 사라지고 <strong>"영희"</strong>만 남음.</span>
        </div>
        </div>
    </div>

    <div style="flex: 1; min-width: 340px; background: #fff; border: 1px solid #e0e0e0; border-top: 5px solid #c0392b; border-radius: 8px; box-shadow: 0 4px 15px rgba(0,0,0,0.05); overflow: hidden; display: flex; flex-direction: column;">
        <div style="padding: 15px 20px; border-bottom: 1px solid #f0f0f0; background: #fff5f5;">
        <div style="margin: 0; color: #c0392b; font-size: 18px; display: flex; align-items: center; font-weight: 700;">
            <span style="font-size: 22px; margin-right: 8px;">2️⃣</span> 동시성 문제 (Race Condition)
        </div>
        <p style="margin: 5px 0 0 0; font-size: 13px; color: #888;">동시에 접근해서 계산이 누락되는 문제</p>
        </div>
        <div style="padding: 30px 20px; background: #fff; position: relative; flex: 1;">
        <div style="position: absolute; left: 50%; top: 20px; bottom: 20px; width: 2px; background: #f0f0f0; transform: translateX(-1px); z-index: 0;"></div>
        <div style="position: absolute; left: 50%; bottom: 10px; transform: translateX(-50%); font-size: 10px; color: #ccc; background: #fff; padding: 2px;">시간 흐름 ▼</div>
        <div style="text-align: center; margin-bottom: 25px; position: relative; z-index: 1;">
            <div style="display: inline-block; background: #333; color: #fff; padding: 5px 15px; border-radius: 20px; font-size: 12px; font-weight: bold;">
            공유 변수: 0
            </div>
        </div>
        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; position: relative; z-index: 1;">
            <div style="flex: 1; text-align: right; padding-right: 10px;">
            <div style="font-size: 11px; font-weight: bold; color: #c0392b;">⚡️ 스레드 A</div>
            <div style="display: inline-block; border: 1px solid #ccc; padding: 6px; border-radius: 8px; background: #fff; font-size: 11px; color: #555;">
                "현재 값 <strong>0</strong> 읽음"
            </div>
            </div>
            <div style="background: #efefef; color: #555; font-size: 10px; font-weight: bold; padding: 3px 6px; border-radius: 10px; border: 1px solid #ccc; white-space: nowrap;">STEP 1</div>
            <div style="flex: 1;"></div>
        </div>
        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; position: relative; z-index: 1;">
            <div style="flex: 1;"></div> 
            <div style="background: #c0392b; color: #fff; font-size: 10px; font-weight: bold; padding: 3px 6px; border-radius: 10px; white-space: nowrap;">STEP 2</div>
            <div style="flex: 1; text-align: left; padding-left: 10px;">
            <div style="font-size: 11px; font-weight: bold; color: #c0392b;">⚡️ 스레드 B</div>
            <div style="display: inline-block; border: 2px solid #c0392b; padding: 6px; border-radius: 8px; background: #fff5f5; font-size: 11px; color: #c0392b;">
                "나도 <strong>0</strong> 읽음!"<br>
                <span style="font-size: 9px; background: #c0392b; color: #fff; padding: 1px 4px; border-radius: 2px;">⚠️ A 저장 전!</span>
            </div>
            </div>
        </div>
        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; position: relative; z-index: 1;">
            <div style="flex: 1; text-align: right; padding-right: 10px;">
            <div style="display: inline-block; border: 1px solid #ccc; padding: 6px; border-radius: 8px; background: #f9f9f9; font-size: 11px; color: #999;">
                "0+1 = <strong>1</strong> 저장"
            </div>
            </div>
            <div style="background: #efefef; color: #555; font-size: 10px; font-weight: bold; padding: 3px 6px; border-radius: 10px; border: 1px solid #ccc; white-space: nowrap;">STEP 3</div>
            <div style="flex: 1;"></div> 
        </div>
        <div style="display: flex; justify-content: space-between; align-items: center; position: relative; z-index: 1;">
            <div style="flex: 1;"></div> 
            <div style="background: #efefef; color: #555; font-size: 10px; font-weight: bold; padding: 3px 6px; border-radius: 10px; border: 1px solid #ccc; white-space: nowrap;">STEP 4</div>
            <div style="flex: 1; text-align: left; padding-left: 10px;">
            <div style="display: inline-block; border: 1px solid #ccc; padding: 6px; border-radius: 8px; background: #f9f9f9; font-size: 11px; color: #999;">
                "0+1 = <strong>1</strong> 저장" (덮어씀)
            </div>
            </div>
        </div>
        </div>
        <div style="background: #fceaea; padding: 15px; text-align: center; border-top: 1px solid #f0e0d0; margin-top: auto;">
        <div style="display: inline-block; text-align: left;">
            <span style="font-size: 14px;">🤯 <strong>결과:</strong></span> 
            <span style="font-size: 13px; color: #922b21; line-height: 1.5;">
            A가 값을 바꾸기도 전(Step 3)에 <br>B가 <strong>옛날 값(Step 2)을 읽어서</strong> 둘 다 1을 저장함.
            </span>
        </div>
        </div>
    </div>
</div>

<div class="story-box" markdown="1">

### 1) 객체 지향의 파괴 (캡슐화 위반)

객체 지향 프로그래밍(OOP)의 핵심은 객체가 자신의 상태를 스스로 관리하고 보호하는 것입니다. 하지만 `static`으로 변수를 선언하면, 모든 객체가 하나의 변수를 공유하게 됩니다. 

위의 예시처럼 **객체 A**는 자신의 이름을 "철수"라고 저장했지만, **객체 B**가 생성되면서 값을 "영희"로 바꾸면, **객체 A**의 이름도 강제로 "영희"가 되어버립니다. 이는 객체 간의 독립성을 해치는 결과를 초래합니다.

### 2) 멀티스레드 환경에서의 Race Condition

웹 애플리케이션(Spring 등)은 기본적으로 멀티스레드 환경입니다. 만약 사용자 정보를 저장하는 변수를 `static`으로 선언한다면 어떤 일이 벌어질까요?

사용자 A가 로그인하는 도중에 사용자 B가 접속하면, <strong class="highlight-text">사용자 A의 화면에 사용자 B의 정보가 노출</strong>되는 사고가 발생할 수 있습니다. 스레드들이 공유 자원에 동시에 접근하면서 발생하는 이 **경쟁 상태(Race Condition)**는 디버깅하기도 매우 어렵습니다.

</div>

<br>

## 그럼 언제 Static을 써야 할까? <span class="title-sub-desc">: 올바른 사용법</span>

<div class="story-box" markdown="1">

`static`은 무조건 나쁜 것이 아닙니다. <strong class="highlight-text">"공유해도 안전한 데이터"</strong>에 사용하면 메모리 효율을 높이고 코드를 간결하게 만들 수 있습니다.

</div>

<div class="info-box" style="max-width: 800px; margin: 0 auto 2rem auto; font-family: 'Pretendard', -apple-system, sans-serif;">
  <div style="display: flex; gap: 20px; flex-wrap: wrap; align-items: stretch;">
    <div style="flex: 1; min-width: 300px; background: #fff; border: 1px solid #e0e0e0; border-top: 5px solid #2980b9; border-radius: 12px; box-shadow: 0 4px 15px rgba(41, 128, 185, 0.1); overflow: hidden; display: flex; flex-direction: column;">
      <div style="padding: 20px; border-bottom: 1px solid #f0f0f0; background: #f4f9fd;">
        <div style="margin: 0; color: #2980b9; font-size: 18px; display: flex; align-items: center; font-weight: bold;">
          <span style="font-size: 22px; margin-right: 8px;">💎</span> 불변의 상수 (Constant)
        </div>
        <p style="margin: 5px 0 0 0; font-size: 13px; color: #666;">변하지 않는 공통 값은 <strong>Static</strong>으로!</p>
      </div>
      <div style="padding: 25px; flex: 1; display: flex; flex-direction: column; align-items: center;">
        <div style="margin-bottom: 20px; text-align: center;">
          <div style="font-size: 40px; margin-bottom: 10px;">🏫</div>
          <div style="font-size: 14px; font-weight: bold; color: #333;">학교 이름 (School Name)</div>
          <div style="font-size: 12px; color: #888; margin-top: 5px;">
            "학생이 100명이든 1000명이든<br>학교 이름은 하나로 똑같습니다."
          </div>
        </div>
        <div style="background: #2c3e50; color: #ecf0f1; padding: 15px; border-radius: 8px; width: 100%; box-sizing: border-box; font-family: monospace; font-size: 12px; line-height: 1.5;">
          <span style="color: #e67e22;">public static final</span> String SCHOOL = <span style="color: #f1c40f;">"JAVA HIGH"</span>;
        </div>
        <div style="margin-top: 15px; text-align: center;">
          <span style="background: #d6eaf8; color: #2980b9; padding: 4px 10px; border-radius: 12px; font-size: 11px; font-weight: bold;">안전한 공유 (Read Only)</span>
        </div>
      </div>
    </div>
    <div style="flex: 1; min-width: 300px; background: #fff; border: 1px solid #e0e0e0; border-top: 5px solid #27ae60; border-radius: 12px; box-shadow: 0 4px 15px rgba(39, 174, 96, 0.1); overflow: hidden; display: flex; flex-direction: column;">
      <div style="padding: 20px; border-bottom: 1px solid #f0f0f0; background: #f0fdf4;">
        <div style="margin: 0; color: #27ae60; font-size: 18px; display: flex; align-items: center; font-weight: bold;">
          <span style="font-size: 22px; margin-right: 8px;">👤</span> 객체 고유 상태 (State)
        </div>
        <p style="margin: 5px 0 0 0; font-size: 13px; color: #666;">개별적으로 다른 값은 <strong>Instance</strong>로!</p>
      </div>
      <div style="padding: 25px; flex: 1; display: flex; flex-direction: column; align-items: center;">
        <div style="margin-bottom: 20px; display: flex; gap: 20px;">
          <div style="text-align: center;">
            <div style="font-size: 30px;">👦</div>
            <div style="background: #27ae60; color: #fff; font-size: 10px; padding: 2px 6px; border-radius: 4px; margin-top: 5px;">철수</div>
          </div>
          <div style="text-align: center;">
            <div style="font-size: 30px;">👧</div>
            <div style="background: #27ae60; color: #fff; font-size: 10px; padding: 2px 6px; border-radius: 4px; margin-top: 5px;">영희</div>
          </div>
        </div>
        <div style="font-size: 12px; color: #888; text-align: center; margin-bottom: 20px;">
          "학생마다 이름, 나이, 성적은<br>모두 다릅니다."
        </div>
        <div style="background: #2c3e50; color: #ecf0f1; padding: 15px; border-radius: 8px; width: 100%; box-sizing: border-box; font-family: monospace; font-size: 12px; line-height: 1.5;">
          <span style="color: #e67e22;">private</span> String studentName;<br>
          <span style="color: #e67e22;">private</span> int grade;
        </div>
        <div style="margin-top: 15px; text-align: center;">
          <span style="background: #d5f5e3; color: #27ae60; padding: 4px 10px; border-radius: 12px; font-size: 11px; font-weight: bold;">철저한 독립 (Capsulation)</span>
        </div>
      </div>
    </div>
  </div>
</div>

<div class="story-box" markdown="1">

- **상수(Constant):** 모든 객체가 공통적으로 사용하며 값이 변하지 않는 데이터는 <strong class="highlight-text">static final</strong>로 선언하여 메모리를 절약하고 안전하게 공유합니다. (예: `Math.PI`, `Integer.MAX_VALUE`)
- **유틸리티 메소드:** 객체의 상태(인스턴스 변수)를 사용하지 않고 입력받은 값으로만 처리하는 메소드는 `static` 메소드로 만드는 것이 효율적입니다. (예: `Math.random()`, `StringUtils.isEmpty()`)
- **상태(State):** 객체마다 달라져야 하는 데이터(이름, 나이, 잔고 등)는 반드시 <strong class="highlight-text">인스턴스 변수</strong>로 선언하여 캡슐화를 지켜야 합니다.

</div>