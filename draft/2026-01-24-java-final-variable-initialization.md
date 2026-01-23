각 변수별 살아있는 영역은 다음과 같습니다.

- **클래스 멤버 변수:** 클래스가 메모리에 올라갈 때까지 (`static final`)
- **인스턴스 멤버 변수:** 객체가 생성 완료될 때까지 (`final`)
- **지역 변수:** 해당 메서드 블록이 끝나기 전까지

따라서, final 변수는 **해당 변수가 쓰이기 전**에는 반드시 값이 들어있어야 합니다.

final 변수의 초기화 시점은 해당 변수가 선언된 위치에 따라 크게 3가지로 나뉩니다. 

# final 인스턴스 변수 : 객체 생성될 때 초기화

객체마다 독립적인 상수값을 갖게 하기 위해

선언 시점에 바로 초기값을 대입하거나, 생성자로 바로 초기화가 이루어질 수 있도록 함.

```java
class Example {
	final int CONSTANT = 100; // a. 변수 선언시점에서 바로 초기화
	final int constructorValue;
	
	public Example(){
		constructorValue = 200; // b. 생성자에서 초기화 
	}
}
```

# final static 변수 : 클래스가 메모리에 로드될 때 초기화

해당 클래스의 모든 인스턴스가 공유하는 공통 상수가 되기 위해

**클래스가 메모리에 로드될 때** 가장 먼저 초기화 되어야 함.

그래서 **선언과 동시에 초기화**하거나, **static 초기화 블록**에서 값을 할당함.

객체가 생성되기 전에 사용 가능해야하므로 생성자에서 초기화 불가.

```java
class Example {
	final static int CONSTANT = 100; //a. 변수 선언시점에서 바로 초기화
	
	final static int BLOCK_INIT;
	static { ////b. static 초기화 블록에서 초기화 
		BLOCK_INIT = 200; //- static블록이 가장 먼저 실행되기 때문
	}	
}
```

# final 로컬 변수 : 해당 메소드 내부에서 사용 전에 초기화

선언과 동시에 할당할 수도 있고 조건문에 따라 분기하여 나중에 할당할 수도 있음.

```java
public void method(){
	final int localVar; //선언
	//중간로직
	localVar = 10; //사용전 초기화. 그러나 어떤 경로로든 단 한번만 할당 가능
	System.out.println(localVar);
}
```

# final 파라미터 : 호출시 이미 결정됨

```java
public void method(final int param){
	//param = 20; //ERROR : 변경 불가
	System.out.println(param);
}
```