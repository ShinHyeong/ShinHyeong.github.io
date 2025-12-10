---
title:  "[Java] Pass by Value vs. Pass by Reference (Call by Value vs. Call by Reference)"
categories: Java
excerpt: "메소드에서 객체를 수정하면 원본도 바뀌는데, 왜 자바는 Pass by Reference가 아닐까요? 많은 개발자가 헷갈리는 '참조값의 복사' 개념에 대해 알아봅시다."
description: "자바 면접 단골 질문인 'Call by Value vs Call by Reference'의 차이를 알아봅니다. 많은 개발자들이 헷갈려하는, 객체를 메서드로 전달했을 때 원본이 변경되는 현상을 '주소값 복사(Pass by Value)' 관점에서 설명했습니다. 또, C언어의 포인터 방식(Pass by Reference)과 비교하여 자바가 원본 변수를 보호하는 원리를 메모리 관점에서 시각적으로 정리했습니다."
#redirect_from: #이전주소 입력
#search: false #만약 이 글이 검색되지 않기를 바란다면
#use_math: true #수식이 필요한 경우만 사용
---

<div class="story-box" markdown="1">

# Pass by Value vs. Pass by Reference

프로그래밍에서 메서드에 변수를 넘길 때 **값만 복사해서 주는지**, 아니면 **메모리 주소 자체를 주는지**는 매우 중요합니다. 이 전달 방식에 따라 메서드 내부의 변경 사항이 원본 변수에도 반영되는지가 결정되기 때문입니다.

이 두 가지 방식을 각각 **Pass by Value(값에 의한 전달)**와 **Pass by Reference(참조에 의한 전달)**라고 부릅니다.
그렇다면 자바는 어느 쪽일까요?

결론부터 말하자면 <strong class="highlight-text">Java는 오직 '값만 복사해서 전달하는' 방식만 사용</strong>합니다.(Pass by Value)

"어? 저는 메소드에서 객체 수정하니까 원본도 바뀌던데요?"라고 반문하실 수 있습니다. 객체를 전달할 때는, 마치 주소를 직접 전달받은 것처럼 원본 데이터가 수정되는 현상이 발생하기 때문입니다.

바로 이 지점이 Pass by Value와 Pass by Reference를 가장 헷갈리게 만드는 부분입니다. 겉보기엔 비슷해 보이지만 내부 원리는 완전히 다른 이 두 개념을 명확히 정리해 보겠습니다.

</div>

<br>

## 1. 개념 정리 <span class="title-sub-desc">: 값 복사 vs 주소 전달</span>

<div class="info-box" style="max-width: 800px; margin: 0 auto 2rem auto; font-family: 'Pretendard', -apple-system, sans-serif; background-color: #f8f9fa; border-radius: 12px; padding: 30px; box-shadow: 0 4px 20px rgba(0,0,0,0.05); color: #333;">
  <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); gap: 20px;">
    
    <div style="background: #fff; border: 2px solid #0056b3; border-radius: 12px; padding: 35px 20px 25px 20px; position: relative; overflow: hidden; box-shadow: 0 4px 15px rgba(0, 86, 179, 0.15); display: flex; flex-direction: column;">
      
      <div style="position: absolute; top: 0; left: 0; background: #0056b3; color: #fff; padding: 5px 15px; font-size: 12px; font-weight: bold; border-bottom-right-radius: 10px;">안전 (복사)</div>
      
      <div style="text-align: center; margin-bottom: 15px;">
        <div style="margin: 0 0 5px 0; color: #0056b3; font-size: 20px; font-weight: 800;">Pass by Value (원시 타입)</div>
        <p style="margin: 0; font-size: 13px; color: #7f8c8d;">"값" 자체를 복사하여 전달</p>
        
        <div style="margin: 10px 0 20px 0;">
          <span style="font-size: 60px; filter: drop-shadow(0 4px 6px rgba(0, 86, 179, 0.3));">📄</span>
        </div>
        
        <div style="font-weight: 800; color: #0056b3; margin-bottom: 15px; font-size: 15px;">원본은 영향을 받지 않음</div>
      </div>

      <div style="background: #f0f7ff; border-radius: 8px; padding: 10px; font-family: monospace; font-size: 11px; line-height: 1.4; color: #333; border: 1px solid #cce5ff; margin-bottom: 20px;">
        <div style="color: #888; margin-bottom: 2px;">// Java (Main)</div>
        <div><span style="color: #0056b3; font-weight: bold;">int a</span> = 10;</div>
        <div>func(a);</div>
        <div style="margin: 4px 0; border-top: 1px dashed #0056b3; opacity: 0.3;"></div>
        <div style="color: #888; margin-bottom: 2px;">// Method</div>
        <div>void func(<span style="color: #e74c3c; font-weight: bold;">int b</span>) {</div>
        <div>&nbsp;&nbsp;b = 20; <span style="color: #999;">// a는 안바뀜</span></div>
        <div>}</div>
      </div>
      
      <div style="display: flex; justify-content: center; align-items: center; gap: 5px; flex: 1;">
        
        <div style="text-align: center;">
          <div style="font-size: 10px; color: #999; margin-bottom: 3px;">[Main Stack]</div>
          <div style="background: #f0f7ff; border: 2px solid #0056b3; padding: 8px 12px; border-radius: 8px;">
            <div style="font-size: 10px; color: #0056b3; font-weight: bold;">int a</div>
            <div style="font-size: 18px; font-weight: 800; color: #333;">10</div>
          </div>
          <div style="font-size: 10px; color: #0056b3; margin-top: 5px; font-weight: bold;">(원본 유지됨)</div>
        </div>

        <div style="display: flex; flex-direction: column; align-items: center; justify-content: center; width: 60px; gap: 15px;">
          
          <div style="position: relative; width: 100%; height: 12px;">
             <div style="position: absolute; top: 6px; left: 0; width: 100%; height: 2px; background: #0056b3;"></div>
             <div style="position: absolute; top: 2px; right: 0; width: 0; height: 0; border-top: 5px solid transparent; border-bottom: 5px solid transparent; border-left: 6px solid #0056b3;"></div>
             <div style="position: absolute; top: -14px; width: 100%; text-align: center; font-size: 11px; color: #0056b3; font-weight: bold;">값(10) 복사</div>
          </div>

          <div style="position: relative; width: 100%; height: 12px; opacity: 0;"></div>

        </div>

        <div style="text-align: center;">
          <div style="font-size: 10px; color: #999; margin-bottom: 3px;">[Method Stack]</div>
          <div style="background: #fff; border: 2px dashed #0056b3; padding: 8px 12px; border-radius: 8px;">
            <div style="font-size: 10px; color: #777; font-weight: bold;">int b</div>
            <div style="font-size: 12px; color: #999; text-decoration: line-through; margin-bottom: -3px;">10</div>
            <div style="font-size: 18px; font-weight: 800; color: #e74c3c;">20</div>
          </div>
          <div style="font-size: 10px; color: #e74c3c; margin-top: 5px; font-weight: bold;">(복사본만 바뀜)</div>
        </div>

      </div>
    </div>

    <div style="background: #fff; border: 2px solid #8e44ad; border-radius: 12px; padding: 35px 20px 25px 20px; position: relative; overflow: hidden; box-shadow: 0 4px 15px rgba(142, 68, 173, 0.15); display: flex; flex-direction: column;">
      
      <div style="position: absolute; top: 0; left: 0; background: #8e44ad; color: #fff; padding: 5px 15px; font-size: 12px; font-weight: bold; border-bottom-right-radius: 10px;">강력 (직접 접근)</div>

      <div style="text-align: center; margin-bottom: 15px;">
        <div style="margin: 0 0 5px 0; color: #8e44ad; font-size: 20px; font-weight: 800;">Pass by Reference (C언어)</div>
        <p style="margin: 0; font-size: 13px; color: #7f8c8d;">"메모리 주소" 자체를 전달</p>
        
        <div style="margin: 10px 0 20px 0;">
          <span style="font-size: 60px; filter: drop-shadow(0 4px 6px rgba(142, 68, 173, 0.3));">🔗</span>
        </div>
        
        <div style="font-weight: 800; color: #8e44ad; margin-bottom: 15px; font-size: 15px;">원본 변수 자체를 변경 가능</div>
      </div>

      <div style="background: #fcf4ff; border-radius: 8px; padding: 10px; font-family: monospace; font-size: 11px; line-height: 1.4; color: #333; border: 1px solid #e9d5ff; margin-bottom: 20px;">
        <div style="color: #888;">// C언어 (Main)</div>
        <div><span style="color: #8e44ad; font-weight: bold;">int a</span> = 10;</div>
        <div>func(&a); <span style="color: #999;">// 주소 전달</span></div>
        <div style="margin: 4px 0; border-top: 1px dashed #d8b4fe;"></div>
        <div style="color: #888;">// Method</div>
        <div>void func(<span style="color: #555; font-weight: bold;">int *p</span>) {</div>
        <div>&nbsp;&nbsp;<span style="color: #555; font-weight: bold;">*p</span> = 20; <span style="color: #c0392b; font-weight: bold;">// 원본 변경!</span></div>
        <div>}</div>
      </div>
      
      <div style="display: flex; justify-content: center; align-items: center; gap: 5px; flex: 1;">
        
        <div style="text-align: center;">
          <div style="font-size: 10px; color: #999; margin-bottom: 3px;">[Main Stack]</div>
          <div style="background: #f4ecf7; border: 2px solid #8e44ad; padding: 8px 12px; border-radius: 8px; position: relative;">
            <div style="font-size: 10px; color: #8e44ad; font-weight: bold;">int a</div>
            <div style="font-size: 12px; color: #999; text-decoration: line-through; margin-bottom: -3px;">10</div>
            <div style="font-size: 18px; font-weight: 800; color: #c0392b;">20</div>
            <div style="position: absolute; top: -5px; right: -5px; background: #c0392b; color: #fff; width: 15px; height: 15px; font-size: 10px; border-radius: 50%; display: flex; align-items: center; justify-content: center;">!</div>
          </div>
          <div style="font-size: 10px; color: #c0392b; margin-top: 5px; font-weight: bold;">(원본 바뀜)</div>
        </div>

        <div style="display: flex; flex-direction: column; align-items: center; justify-content: center; width: 60px; gap: 15px;">
          
          <div style="position: relative; width: 100%; height: 12px;">
             <div style="position: absolute; top: 6px; left: 0; width: 100%; height: 2px; background: #8e44ad;"></div>
             <div style="position: absolute; top: 2px; right: 0; width: 0; height: 0; border-top: 5px solid transparent; border-bottom: 5px solid transparent; border-left: 6px solid #8e44ad;"></div>
             <div style="position: absolute; top: -14px; width: 100%; text-align: center; font-size: 11px; color: #8e44ad; font-weight: bold;">&a</div>
          </div>

          <div style="position: relative; width: 100%; height: 12px;">
             <div style="position: absolute; top: 6px; left: 0; width: 100%; height: 2px; background: #c0392b;"></div>
             <div style="position: absolute; top: 2px; left: 0; width: 0; height: 0; border-top: 5px solid transparent; border-bottom: 5px solid transparent; border-right: 6px solid #c0392b;"></div>
             <div style="position: absolute; bottom: -14px; width: 100%; text-align: center; font-size: 11px; color: #c0392b; font-weight: bold;">*p=20</div>
          </div>

        </div>

        <div style="text-align: center;">
          <div style="font-size: 10px; color: #999; margin-bottom: 3px;">[Method Stack]</div>
          <div style="background: #fff; border: 2px dashed #8e44ad; padding: 8px 12px; border-radius: 8px;">
            <div style="font-size: 10px; color: #555; font-weight: bold;">int *p</div>
            <div style="font-size: 12px; color: #333; margin-top: 3px; font-family: monospace; font-weight: bold;">*p=20</div>
          </div>
          <div style="font-size: 10px; color: #555; margin-top: 5px; font-weight: bold;">(직접 접근)</div>
        </div>

      </div>
    </div>

  </div>
</div>

<div class="story-box" markdown="1">

- **Pass by Value (값에 의한 전달)**
    - 메서드를 호출할 때 매개변수의 <strong class="highlight-text">'값(Value)'을 복사</strong>하여 전달합니다.
    - 복사본을 전달하기 때문에 메서드 내부에서 매개변수를 변경하더라도 **원본 변수에는 영향을 주지 않습니다.**
    - 쉽게 말해, 친구에게 내 노트를 빌려주는 대신 <strong class="highlight-text">복사본을 만들어서 주는 것</strong>과 같습니다. 친구가 복사본에 낙서를 해도 내 원본 노트는 깨끗하죠.

- **Pass by Reference (참조에 의한 전달)**
    - 메서드를 호출할 때 매개변수의 **'메모리 주소(Reference)'** 자체를 전달합니다.
    - 메서드 내부에서 이 주소를 통해 원본 메모리에 접근할 수 있으므로, <strong class="highlight-text">원본 변수의 값을 직접 변경</strong>할 수 있습니다.
    - 이는 친구에게 **내 노트 원본**을 잠시 맡기는 것과 같습니다. 친구가 낙서를 하면 내 노트도 더러워집니다.

</div>

<br>

## 2. Java에서의 전달 방식 <span class="title-sub-desc">: 항상 값(Value)만 전달</span>

<div class="story-box" markdown="1">

자바는 매개변수 타입과 무관하게 항상 Pass by Value 방식을 따릅니다. 하지만 매개변수 타입에 따라 '실제로 복사되는 값'이 달라집니다. 원시 타입은 **'실제 값의 복사본'**이, 객체 타입은 **'주소 값의 복사본'**이 전달된다는 차이가 있습니다.

### 원시 타입 (Primitive Type)

`int`, `double`, `boolean` 같은 원시 타입은 변수 안에 **실제 값**이 들어있습니다.
- **무엇을 넘기나?** : 변수가 가지고 있는 <strong class="highlight-text">'실제 값의 복사본'</strong>을 넘깁니다.
- **결과** : 메서드 안에서 이 복사된 값을 바꿔도, **원본 변수**는 **그대로 유지 (전혀 영향 X)**

### 객체 타입 (Reference Type)

`Class`, `Array`, `String` 같은 객체 타입은 변수 안에 **객체가 위치한 메모리 주소값(참조값)**이 들어있습니다.
- **무엇을 넘기나?** : 객체 자체가 아니라, <strong class="highlight-text">'주소값의 복사본'</strong>을 넘깁니다.
- **결과** : 주소값을 복사했기 때문에 **사용 방식에 따라 결과가 나뉩니다.**
    1. **객체 내부 수정 (`p.set` ..):** 복사된 주소를 통해 힙 영역에 접근하여 값을 바꾸면, **원본 객체의 내용도 함께 변경**됩니다. (같은 힙 메모리 공유)
    2. **새로운 객체 할당 (`p = new` ..):** 매개변수에 아예 새로운 객체(주소)를 할당하면, **원본 변수에는 아무런 영향이 없습니다.** (연결 끊김)

</div>

<div class="info-box" style="max-width: 800px; margin: 0 auto 2rem auto; font-family: 'Pretendard', -apple-system, sans-serif; background-color: #f9fafb; border-radius: 12px; padding: 30px; box-shadow: 0 4px 20px rgba(0,0,0,0.03); color: #333;">
  
  <div style="text-align: center; margin-bottom: 30px;">
    <div style="margin: 0; font-size: 22px; color: #374151; font-weight: 800;">Pass by Value: 객체 전달의 2가지 결과</div>
    <p style="margin-top: 5px; font-size: 14px; color: #6b7280;">(모두 주소값 복사이지만, 사용 방식에 따라 결과가 다름)</p>
  </div>

  <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); gap: 20px;">
    
    <div style="background: #fff; border: 2px solid #4a90e2; border-radius: 12px; padding: 25px; box-shadow: 0 4px 15px rgba(74, 144, 226, 0.1); display: flex; flex-direction: column;">
      
      <div style="text-align: center; margin-bottom: 15px;">
        <div style="display: inline-block; background: #4a90e2; color: #fff; padding: 4px 12px; font-size: 11px; font-weight: bold; border-radius: 12px; margin-bottom: 5px;">Case 1 (수정)</div>
        <div style="margin: 0; color: #4a90e2; font-size: 18px; font-weight: 800;">객체 내용 변경 (p.Set)</div>
        <p style="margin: 5px 0 0 0; font-size: 12px; color: #6b7280;">"같은 주소를 보고 내용을 바꿈"</p>
      </div>

      <div style="background: #f0f7ff; border-radius: 8px; padding: 10px; font-family: monospace; font-size: 11px; line-height: 1.4; color: #374151; border: 1px solid #d4e9f7; margin-bottom: 20px;">
        <div style="color: #9ca3af;">// Main</div>
        <div>Member m = new Member("A");</div>
        <div>func(m); <span style="color: #9ca3af;">// 주소(0x100) 복사</span></div>
        <div style="margin: 4px 0; border-top: 1px dashed #4a90e2; opacity: 0.3;"></div>
        <div style="color: #9ca3af;">// Method</div>
        <div>void func(Member p) {</div>
        <div>&nbsp;&nbsp;<span style="color: #4a90e2; font-weight: bold;">p.setName("B");</span> <span style="color: #9ca3af;">// 내용 수정</span></div>
        <div>}</div>
      </div>
      
      <div style="display: flex; justify-content: center; align-items: center; gap: 10px; flex: 1;">
        
        <div style="display: flex; flex-direction: column; gap: 20px;">
          <div style="text-align: right;">
            <div style="font-size: 10px; color: #9ca3af;">[Main Stack] m</div> <div style="background: #4a90e2; color: #fff; padding: 6px 10px; border-radius: 6px; font-size: 12px; font-family: monospace; font-weight: bold;">
              0x100
            </div>
          </div>
          <div style="text-align: right;">
            <div style="font-size: 10px; color: #9ca3af;">[Method Stack] p</div> <div style="background: #fff; border: 2px solid #4a90e2; color: #4a90e2; padding: 4px 8px; border-radius: 6px; font-size: 12px; font-family: monospace; font-weight: bold;">
              0x100
            </div>
          </div>
        </div>

        <div style="display: flex; flex-direction: column; align-items: center; justify-content: center; width: 30px; gap:20px;">
           <div style="font-size: 20px; color: #4a90e2; line-height: 0.8;">↘️</div>
           <div style="font-size: 20px; color: #4a90e2; line-height: 0.8;">↗️</div>
        </div>

        <div style="text-align: center;">
          <div style="border: 1px dashed #d1d5db; padding: 2px 6px; border-radius: 10px; font-size: 9px; color: #9ca3af; margin-bottom: 5px;">Heap Area</div>
          <div style="background: #fff; border: 3px solid #4a90e2; border-radius: 12px; padding: 15px; width: 80px; text-align: center; box-shadow: 0 4px 10px rgba(74, 144, 226, 0.15);">
            <div style="font-size: 10px; color: #4a90e2; margin-bottom: 5px; font-weight: bold;">0x100</div>
            <div style="font-size: 24px; filter: grayscale(20%) opacity(90%);">📦</div>
            <div style="background: #eaf4fc; color: #2c5282; font-size: 10px; font-weight: bold; padding: 2px 4px; border-radius: 4px; margin-top: 5px;">
              Name: "B"
            </div>
          </div>
          <div style="font-size: 10px; color: #4a90e2; margin-top: 5px; font-weight: bold;">(원본 바뀜!)</div>
        </div>

      </div>
    </div>

    <div style="background: #fff; border: 2px solid #5bc0de; border-radius: 12px; padding: 25px; box-shadow: 0 4px 15px rgba(91, 192, 222, 0.1); display: flex; flex-direction: column;">
      
      <div style="text-align: center; margin-bottom: 15px;">
        <div style="display: inline-block; background: #5bc0de; color: #fff; padding: 4px 12px; font-size: 11px; font-weight: bold; border-radius: 12px; margin-bottom: 5px;">Case 2 (새 할당)</div>
        <div style="margin: 0; color: #5bc0de; font-size: 18px; font-weight: 800;">새 객체 할당 (p = New)</div>
        <p style="margin: 5px 0 0 0; font-size: 12px; color: #6b7280;">"주소를 새로 바꿈 -> 연결 끊김"</p>
      </div>

      <div style="background: #f0faff; border-radius: 8px; padding: 10px; font-family: monospace; font-size: 11px; line-height: 1.4; color: #374151; border: 1px solid #d0f0f8; margin-bottom: 20px;">
        <div style="color: #9ca3af;">// Main</div>
        <div>Member m = new Member("A");</div>
        <div>func(m); <span style="color: #9ca3af;">// 주소(0x100) 복사</span></div>
        <div style="margin: 4px 0; border-top: 1px dashed #5bc0de; opacity: 0.3;"></div>
        <div style="color: #9ca3af;">// Method</div>
        <div>void func(Member p) {</div>
        <div>&nbsp;&nbsp;<span style="color: #5bc0de; font-weight: bold;">p = new Member("C");</span> <span style="color: #9ca3af;">// 재할당</span></div>
        <div>}</div>
      </div>
      
      <div style="display: flex; justify-content: center; align-items: center; gap: 5px; flex: 1;">
        
        <div style="display: flex; flex-direction: column; gap: 20px;">
          <div style="text-align: right;">
            <div style="font-size: 10px; color: #9ca3af;">[Main Stack] m</div> <div style="background: #5bc0de; color: #fff; padding: 6px 10px; border-radius: 6px; font-size: 12px; font-family: monospace; font-weight: bold;">
              0x100
            </div>
          </div>
          <div style="text-align: right;">
            <div style="font-size: 10px; color: #9ca3af;">[Method Stack] p</div> <div style="background: #fff; border: 2px solid #5bc0de; color: #5bc0de; padding: 4px 10px; border-radius: 6px; font-size: 12px; font-family: monospace; font-weight: bold; position: relative;">
              0x200
              <div style="position: absolute; top: -5px; right: -8px; background: #f0ad4e; color: #fff; font-size: 8px; padding: 2px 4px; border-radius: 4px; font-weight: bold;">New</div>
            </div>
          </div>
        </div>

        <div style="display: flex; flex-direction: column; justify-content: space-around; height: 100px; width: 30px; align-items: center;">
           <div style="font-size: 16px; color: #5bc0de;">➡️</div> 
           <div style="font-size: 16px; color: #5bc0de;">➡️</div> 
        </div>

        <div style="text-align: center; display: flex; flex-direction: column; gap: 10px;">
          
          <div style="background: #eefbfd; border: 1px solid #5bc0de; border-radius: 8px; padding: 5px; width: 70px; opacity: 0.6;">
            <div style="font-size: 9px; color: #5bc0de; font-weight: bold;">0x100</div>
            <div style="font-size: 16px; filter: grayscale(20%) opacity(80%);">📦</div>
            <div style="font-size: 9px; color: #6b7280;">"A" (유지)</div>
          </div>

          <div style="background: #fff; border: 2px solid #5bc0de; border-radius: 8px; padding: 5px; width: 70px; box-shadow: 0 2px 8px rgba(91, 192, 222, 0.15);">
            <div style="font-size: 9px; color: #5bc0de; font-weight: bold;">0x200</div>
            <div style="font-size: 16px; filter: grayscale(20%);">✨</div>
            <div style="font-size: 9px; color: #5bc0de; font-weight: bold;">"C" (독립)</div>
          </div>

        </div>

      </div>
    </div>

  </div>
</div>

<div class="story-box" markdown="1">

위 그림은 객체를 전달했을 때 발생할 수 있는 두 가지 상황을 보여줍니다.

**1. Case 1: 객체 내용 변경 (`p.setName("B")`)**
- "리모컨(주소값)을 복사해 줬더니, 그 리모컨으로 채널을 돌려버린 상황"입니다.
- 복사된 참조값(`p`)도 원본과 **같은 0x100 번지의 객체**를 가리키고 있습니다. 따라서 `p`를 통해 객체의 내부 데이터를 수정하면 원본 객체도 영향을 받습니다.

**2. Case 2: 새 객체 할당 (`p = new Member("C")`)**
- "복사해 준 리모컨(주소값)을 버리고, 새로 산 TV의 리모컨을 쥔 상황"입니다.
- 매개변수 `p`에 새로운 객체(`0x200`)의 주소를 덮어씌웠습니다. 이제 `p`와 원본 `m`은 서로 다른 객체를 가리킵니다. 따라서 `p`가 무슨 짓을 해도 원본 `m`(`0x100`)은 안전합니다.

</div>

<br>

## 3. 원본이 바뀌었는데 왜 Pass by Reference가 아닐까?<br><span class="title-sub-desc">: 변수가 가리키는 곳의 차이</span>

<div class="story-box" markdown="1">

"어쨌든 결과적으로, 복사본이 아니라 **원본 데이터**의 내용이 바뀌었으니 Pass by Reference 아닌가요?"라고 생각할 수 있습니다. 하지만 <strong class="highlight-text">메모리 관점</strong>에서 보면 명확한 차이가 있습니다. 아래 그림에서 **"메모리 상에서 화살표가 어디를 향하는지"**에 주목해 주세요.

</div>

<div class="info-box" style="max-width: 800px; margin: 0 auto 2rem auto; font-family: 'Pretendard', -apple-system, sans-serif; background-color: #f8f9fa; border-radius: 12px; padding: 30px; box-shadow: 0 4px 20px rgba(0,0,0,0.05); color: #333;">
  
  <div style="text-align: center; margin-bottom: 30px;">
    <div style="margin: 0; font-size: 22px; color: #2c3e50; font-weight: 800;">결정적 차이: 화살표의 목적지</div>
    <p style="margin-top: 5px; font-size: 14px; color: #7f8c8d;">변수가 가리키는 곳(Heap)을 건드리느냐, 변수 그 자체(Stack)를 건드리느냐</p>
  </div>

  <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); gap: 20px;">
    
    <div style="background: #fff; border: 2px solid #0056b3; border-radius: 12px; padding: 25px; box-shadow: 0 4px 15px rgba(0, 86, 179, 0.15); display: flex; flex-direction: column;">
      
      <div style="text-align: center; margin-bottom: 15px;">
        <div style="display: inline-block; background: #0056b3; color: #fff; padding: 4px 12px; font-size: 11px; font-weight: bold; border-radius: 12px; margin-bottom: 5px;">Java</div>
        <div style="margin: 0; color: #0056b3; font-size: 18px; font-weight: 800;">Pass by Value (객체)</div>
        <p style="margin: 5px 0 0 0; font-size: 12px; color: #7f8c8d;">"객체의 주소(0x100)만 복사"</p>
      </div>

      <div style="background: #f0f7ff; border-radius: 8px; padding: 10px; font-family: monospace; font-size: 11px; line-height: 1.4; color: #333; border: 1px solid #cce5ff; margin-bottom: 20px;">
        <div style="color: #888;">// Java</div>
        <div>Member m = new Member("A");</div>
        <div>func(m); <span style="color: #999;">// 0x100 복사</span></div>
        <div style="margin: 4px 0; border-top: 1px dashed #0056b3; opacity: 0.3;"></div>
        <div style="color: #888;">// Method</div>
        <div>void func(Member p) {</div>
        <div>&nbsp;&nbsp;p.setName("B"); <span style="color: #0056b3; font-weight: bold;">// Heap 접근</span></div>
        <div>}</div>
      </div>
      
      <div style="display: flex; justify-content: center; align-items: center; gap: 5px; flex: 1;">
        
        <div style="display: flex; flex-direction: column; gap: 20px;">
          <div style="text-align: right;">
            <div style="font-size: 10px; color: #999;">[Main Stack]</div>
            <div style="background: #0056b3; color: #fff; padding: 6px 10px; border-radius: 6px; font-size: 12px; font-family: monospace; font-weight: bold;">
              m: 0x100
            </div>
          </div>
          <div style="text-align: right;">
            <div style="font-size: 10px; color: #999;">[Method Stack]</div>
            <div style="background: #fff; border: 2px solid #0056b3; color: #0056b3; padding: 4px 8px; border-radius: 6px; font-size: 12px; font-family: monospace; font-weight: bold;">
              p: 0x100
            </div>
          </div>
        </div>

        <div style="display: flex; flex-direction: column; align-items: center; justify-content: center; width: 30px; gap: 20px;">
           <div style="font-size: 20px; color: #0056b3; line-height: 0.8;">↘️</div>
           <div style="font-size: 20px; color: #0056b3; line-height: 0.8;">↗️</div>
        </div>

        <div style="text-align: center;">
          <div style="border: 1px dashed #ccc; padding: 2px 6px; border-radius: 10px; font-size: 9px; color: #999; margin-bottom: 5px;">Heap Area</div>
          <div style="background: #fff; border: 3px solid #0056b3; border-radius: 12px; padding: 15px; width: 80px; text-align: center; box-shadow: 0 4px 10px rgba(0, 86, 179, 0.2);">
            <div style="font-size: 10px; color: #0056b3; margin-bottom: 5px; font-weight: bold;">0x100</div>
            <div style="font-size: 24px;">📦</div>
            <div style="background: #eaf4fc; color: #004085; font-size: 10px; font-weight: bold; padding: 2px 4px; border-radius: 4px; margin-top: 5px;">
              Name: "B"
            </div>
          </div>
          <div style="font-size: 10px; color: #0056b3; margin-top: 5px; font-weight: bold;">(내용 변경)</div>
        </div>

      </div>
    </div>

    <div style="background: #fff; border: 2px solid #8e44ad; border-radius: 12px; padding: 25px; box-shadow: 0 4px 15px rgba(142, 68, 173, 0.15); display: flex; flex-direction: column;">
      
      <div style="text-align: center; margin-bottom: 15px;">
        <div style="display: inline-block; background: #8e44ad; color: #fff; padding: 4px 12px; font-size: 11px; font-weight: bold; border-radius: 12px; margin-bottom: 5px;">C언어</div>
        <div style="margin: 0; color: #8e44ad; font-size: 18px; font-weight: 800;">Pass by Reference</div>
        <p style="margin: 5px 0 0 0; font-size: 12px; color: #7f8c8d;">"변수 자체의 주소(&m)를 전달"</p>
      </div>

      <div style="background: #fcf4ff; border-radius: 8px; padding: 10px; font-family: monospace; font-size: 11px; line-height: 1.4; color: #333; border: 1px solid #e9d5ff; margin-bottom: 20px;">
        <div style="color: #888;">// C언어</div>
        <div>struct Member m = {"A"};</div>
        <div>func(<span style="color: #8e44ad; font-weight: bold;">&m</span>); <span style="color: #999;">// 주소 전달</span></div>
        <div style="margin: 4px 0; border-top: 1px dashed #d8b4fe;"></div>
        <div style="color: #888;">// Method</div>
        <div>void func(Member *p) {</div>
        <div>&nbsp;&nbsp;<span style="color: #c0392b; font-weight: bold;">p->name = "B";</span> <span style="color: #999;">// 원본 직접 변경</span></div>
        <div>}</div>
      </div>
      
      <div style="display: flex; justify-content: center; align-items: center; gap: 5px; flex: 1;">
        
        <div style="text-align: center;">
          <div style="font-size: 10px; color: #999; margin-bottom: 3px;">[Main Stack]</div>
          <div style="background: #f4ecf7; border: 2px solid #8e44ad; border-radius: 12px; padding: 15px; width: 80px; text-align: center; position: relative;">
            <div style="font-size: 10px; color: #8e44ad; margin-bottom: 5px; font-weight: bold;">struct m</div>
            <div style="font-size: 24px;">📦</div>
            <div style="background: #e8daef; color: #4a235a; font-size: 10px; font-weight: bold; padding: 2px 4px; border-radius: 4px; margin-top: 5px;">
              Name: "B"
            </div>
            <div style="position: absolute; top: -5px; right: -5px; background: #c0392b; color: #fff; width: 15px; height: 15px; font-size: 10px; border-radius: 50%; display: flex; align-items: center; justify-content: center;">!</div>
          </div>
          <div style="font-size: 10px; color: #c0392b; margin-top: 5px; font-weight: bold;">(여기가 바뀜)</div>
        </div>

        <div style="display: flex; flex-direction: column; align-items: center; justify-content: center; width: 80px; gap: 10px;"> <div style="position: relative; width: 100%; height: 14px;">
             <div style="position: absolute; top: 7px; left: 0; width: 100%; height: 2px; background: #8e44ad;"></div>
             <div style="position: absolute; top: 3px; right: 0; width: 0; height: 0; border-top: 5px solid transparent; border-bottom: 5px solid transparent; border-left: 6px solid #8e44ad;"></div>
             <div style="position: absolute; top: -8px; width: 100%; text-align: center; font-size: 10px; color: #8e44ad; font-weight: bold;">&m</div>
          </div>

          <div style="position: relative; width: 100%; height: 14px;">
             <div style="position: absolute; top: 7px; left: 0; width: 100%; height: 2px; background: #c0392b;"></div>
             <div style="position: absolute; top: 3px; left: 0; width: 0; height: 0; border-top: 5px solid transparent; border-bottom: 5px solid transparent; border-right: 6px solid #c0392b;"></div>
             <div style="position: absolute; bottom: -8px; width: 100%; text-align: center; font-size: 10px; color: #c0392b; font-weight: bold; white-space: nowrap;">p->name="B"</div>
          </div>

        </div>

        <div style="text-align: center;">
          <div style="font-size: 10px; color: #999; margin-bottom: 3px;">[Method Stack]</div>
          <div style="background: #fff; border: 2px dashed #8e44ad; padding: 10px 5px; border-radius: 8px; width: 80px;">
            <div style="font-size: 10px; color: #555; font-weight: bold;">struct *p</div>
            <div style="font-size: 11px; color: #333; margin-top: 3px; font-family: monospace; font-weight: bold;">&m</div>
            <div style="font-size: 9px; color: #999;">(Main 주소)</div>
          </div>
          <div style="font-size: 10px; color: #555; margin-top: 5px; font-weight: bold;">(직접 접근)</div>
        </div>

      </div>
    </div>

  </div>
</div>

<div class="story-box" markdown="1">

이 비교의 핵심은 <strong class="highlight-text">"메소드가 Main 스택의 변수 `m`을 건드릴 수 있는가?"</strong> 입니다.

- **Java** : 메소드는 변수 `m`이 어디 있는지 모릅니다. 
  - 단지 `m`이 가리키는 **'힙 영역의 객체 주소(0x100)'**만 복사 받았습니다.
  - 따라서 힙에 있는 객체 데이터는 수정할 수 있지만, <strong class="highlight-text">Main 스택에 있는 변수 `m` 자체(0x100이라는 값)</strong>는 절대 건드릴 수 없습니다. (원본 변수 보호)

- **C언어** : 메소드는 변수 `m`의 위치를 압니다.
  - 메소드는 변수 `m`의 **'스택 메모리 주소(&m)'**를 직접 전달받아, `m`의 **위치(&m)**를 알게 되었습니다.
  - 직접 접근 (* 연산자): 그런데 C언어에는 **"그 변수으로 들어가라(*)"**는 특수 명령어가 있습니다.
  - 따라서 포인터를 통해 **Main 스택에 있는 `m`에 직접 접근(*)**해서, `m`이 가리키는 대상을 아예 다른 것으로 바꿔버릴 수 있습니다. (원본 변수 변경 가능)

요약하자면, 자바는 <strong class="highlight-text">원본 변수를 보호</strong>하기 위해 <strong class="highlight-text">항상 값을 복사</strong>해서 전달합니다. 객체의 경우 그 '값'이 주소값일 뿐입니다. **참조값 자체가 바뀐 것이 아니라**, 참조값의 복사본을 통해 가리키는 객체의 ‘내용’이 바뀐 것입니다.

</div>