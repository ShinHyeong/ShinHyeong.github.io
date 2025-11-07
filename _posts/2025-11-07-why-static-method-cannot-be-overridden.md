---
title:  "컴파일러 vs JVM: static 메소드가 오버라이딩되지 않는 이유"
categories: java
tag: [static-method, instance-method, method-overriding, method-overloading]
#redirect_from: #이전주소 입력
#search: false #만약 이 글이 검색되지 않기를 바란다면
#use_math: true #수식이 필요한 경우만 사용
---

## **1. static 메소드 vs. 인스턴스 메소드 / 메소드 오버라이딩 vs. 메소드 오버로딩**

우리 알고 있는 내용을 다시 한번 복습해보자

|  | 작업 대상 | 객체 필수? |
| --- | --- | --- |
| static 메소드 | static변수(공용데이터) 다루기 위해 태어남<br>→ 실제 객체 타입 확인할 필요없어 | 객체 생성없이 호출가능 |
| 인스턴스 메소드 | 인스턴스변수(특정객체) 다루기 위해 태어남<br>→ 실제 객체 타입을 봐야 돼 | 객체 생성 필수 |

|  | 작동시점 | 누가 | 기준 |
| --- | --- | --- | --- |
| 메소드 오버라이딩 (동적바인딩) | 런타임에 | JVM이 | **실제 객체 타입**을 기준으로 어떤 메소드를 호출할지 결정함 |
| 메소드 오버로딩 (정적바인딩) | 컴파일타임에 | 컴파일러가 | **파라미터 시그니처**를 기준으로 어떤 메소드를 호출할지 |

## 2. static 메소드 : 1단계를 거쳐서 호출됨 (컴파일 타임)

```java
public class Animal {
	
	public static void printType(){...}
	
	public static void printAddr(){...}
	public static void printAddr(String addr){...}
	
	public void sound() {...}
	public void eat() {...}
	public void eat(String food) {...} 
}

public class Dog extends Animal {

	@Override
	public static void printType(){...}
	
	@Override
	public void sound() {...} 
}
```

```java
Animal mypet = new Dog();
// 참조변수타입 : Animal (컴파일러가 보는 타입)
// 실제객체타입 : Dog (JVM이 보는 타입)

// 1.[컴파일타임] 컴파일러 : "myPet 선언타입은 Animal이구나 그걸 기준으로 동작해야겠다. Animal 클래스에 printType()은 존재하군 (문법오류여부만 확인)
// (static 키워드 발견하고는) 근데 static 메소드네. 공용데이터 처리를 위해 태어난 메소드니까 실제 객체까지 확인할 필요없겠어."
// => static 메소드 호출 과정에 따라 이러한 1단계를 걸쳤는데 오버라이딩은 불가능함을 알 수 있음
mypet.printType() 

// 1.[컴파일타임] 컴파일러 : "myPet 선언타입은 Animal이구나 그걸 기준으로 동작해야겠다. Animal 클래스에 printAddr(String)은 존재하군 (문법오류여부만 확인)
// (static 키워드 발견하고는) 근데 static 메소드네. 공용데이터 처리를 위해 태어난 메소드니까 실제 객체까지 확인할 필요없겠어."
// => static 메소드 호출 과정에 따라 이러한 1단계를 걸쳤는데 오버로딩은 가능함을 알 수 있음
mypet.printAddr("Seoul")
```

- 먼저, 컴파일 타임에 컴파일러는 `static` 키워드를 읽는 순간, 선언된 타입(`Animal`)을 기준으로 동작해야한다고 판단한다
- 그래서 `mypet.printType()` 코드를 즉시 `Animal.printType()`으로 해석해 버린다.
    
    `mypet.printAddr("Seoul")`도 마찬가지로 `Animal.printAddr("Seoul")`로 해석한다.
    
- **컴파일러는 `static` 키워드 때문에 실제 객체까지 확인할 필요가 없다고 단정지었으므로,** JVM은 런타임에 `mypet`이 `Dog`인지 확인할 기회조차 갖지 못한다
- 따라서 static 메소드는, 런타임에 실제 객체 타입을 기준으로 동작하는 메소드 오버라이딩을 하지 못한다.

## 3. 인스턴스 메소드 : 2단계를 거쳐서 호출됨 (컴파일타임 → 런타임)

```java
public class Animal {
	
	public static void printType(){
		System.out.println("이것은 [Animal 클래스]의 static 메소드입니다.");
	}

	public void sound() {...} //나중에 오버라이드 할 메소드
	public void eat() {...} //오버로딩 예시
	public void eat(String food) {...} 
}

public class Dog extends Animal {

	
	public static void printType(){
		System.out.println("이것은 [Dog 클래스]의 static 메소드입니다.");
	}
	
	@Override
	public void sound() {...} 
}
```

```java
Animal mypet = new Dog();
// 참조변수타입 : Animal (컴파일러가 보는 타입)
// 실제객체타입 : Dog (JVM이 보는 타입)

// 1.[컴파일타임] 컴파일러 : "mypet의 선언타입인 Animal 클래스에 따르면 해당 메소드는 존재하군(문법오류여부만 확인). 
// 근데 static 키워드가 없는걸 보니 인스턴스 메소드구나. JVM아 실제 객체를 확인해줘 (넘김)"
// 2.[런타임] JVM : "아 실제객체타입은 Dog 였네 Dog클래스의 sound()에 맞춰서 동작해야겠다"
// => 인스턴스 메소드 호출 과정에 따라 이러한 2단계를 걸쳤는데 오버라이딩은 가능함을 알 수 있음
mypet.sound() 

// 오버로딩은 가능할까? 예
// 1.[컴파일타임] 컴파일러 : "ㅇㅋ mypet의 선언타입인 Animal 클래스에 따르면 해당 메소드는 존재하군(문법오류여부만 확인)
// 근데 static 키워드가 없는걸 보니 인스턴스 메소드구나. JVM아 실제 객체를 확인해줘 (넘김)"
// 2.[런타임] JVM: "아 실제객체타입은 Dog 였네 Dog가 eat(String)에 맞춰서 동작해야겠다
 //              -> 오버라이딩 안했네? 그럼 부모인 Animal의 eat(String) 실행!"
// => 인스턴스 메소드 호출 과정에 따라 이러한 2단계를 걸쳤는데 오버로딩은 가능함을 알 수 있음
mypet.eat("chicken")
```

**`mypet.sound()` (오버라이딩이 가능할까?):**

1. **[컴파일]** `Animal`에 `sound()`가 있는지 확인 (통과)
2. **[런타임]** `mypet`의 실제 객체 `Dog`를 확인. `Dog`에 `sound()`가 오버라이딩되어 있으니 `Dog.sound()` 실행.

**`mypet.eat("chicken")` (오버로딩이 가능할까?)**

1. **[컴파일]** `Animal`에 `eat(String)`이 있는지 확인 (통과)
2. **[런타임]** `mypet`의 실제 객체 `Dog`를 확인. `Dog`에 `eat(String)`이 **없는지** 확인.
    
    없으니까 선언 타입인 `Animal`로 올라가서 `Animal.eat("String")` 실행.
    

## 4. 🤔 인스턴스 메소드는 동적바인딩이잖아. 오버로딩은 정적바인딩인데.. 왜 정적바인딩인 오버로딩이 가능해?

단순히 인스턴스 메소드는 동적 바인딩 , 메소드 오버로딩은 정적 바인딩으로 매칭 시켜서 외우면 안된다.

먼저 결론부터 이야기하자면 static은 설계목적상 정적바인딩만 실행한다. 그러나, 인스턴스 메소드는 정적바인딩을 실행한다음, 동적바인딩을 실행한다. 인스턴스 메소드는 이 2단계로 오버라이딩과 오버로딩을 모두 성공시킬뿐이다.

**인스턴스 메소드는 1) 일단 메소드 시그니처가 결정되고 나면(정적), 2) 그 구현체를 런타임에 찾는다(동적)**

- 오버로딩(정적) : 컴파일 시점에 "수많은 메소드 시그니처 중에 **어떤 시그니처를 고를지**" 결정하는 것
    - 1단계 : 컴파일러가 선언타입에 의거하여 메소드 시그니처를 고른다.
    - 2단계 : JVM이 실제 객체 타입에 의거하여 동작하려 한다. 하지만 딱히 오버라이딩을 안했음을 확인한다. 다시 아까 정한 선언타입의 객체로 구현하기로 결정한다
- 오버라이딩(동적) : 런타임 시점에 "그렇게 고른 메소드를 **어떤 객체로 구현할지**" 결정하는 것
    - 1단계 : 컴파일러가 선언타입에 의거하여 해당 메소드를 고른다.
    - 2단계 : JVM이 실제 객체 타입에 의거하여 동작하려한다. 실제 객체 타입으로 구현하기로 결정한다.

## 5. 요약

- static 메소드 : 메소드 오버로딩만 가능
- 인스턴스메소드 : 메소드 오버라이딩, 메소드 오버로딩 둘 다 가능

|  | 메소드 **오버라이딩** | 메소드 오버로딩 |
| --- | --- | --- |
| **static 메소드** | ❌ | ✅ |
| 인스턴스메소드 | ✅ | ✅ |