---
title:  "Java의 main 메소드는 왜 static 메소드일까?"
categories: java
#redirect_from: #이전주소 입력
#search: false #만약 이 글이 검색되지 않기를 바란다면
#use_math: true #수식이 필요한 경우만 사용
---

# JVM←객체를 생성하지 않고도 프로그램 시작할 수 있어야

---

## 1. 프로그램 실행 과정으로 이해해보자

1. 객체가 있어야 메소드 호출이 가능한데, 프로그램 시작 시점에는 *어떤 객체도 없는 상태*. 
2. JVM이 **객체 생성 없이도 메서드를 호출**할 수 있어야 함
3. 따라서 **static**으로 선언하여 클래스 로딩 시점에 메모리에 로드되도록 함

---

## 2. 코드로 이해해보자

### **1) 프로그램 시작 시점의 상황**

어떤 객체도 생성되어 있지 않은 상태

```java
public class MyProgram {
    public static void main(String[] args) {
    
    }
}
```

### 2) **만약 main()이 static 메소드가 아니라면,**

main 메소드를 호출하기 위해서는 **MyProgram 객체가 필요함**

그런데 **누가 이 main 객체를 생성해야 하나?**

JVM? 그러면 JVM이 생성자도 알아야 함 

또, 만약 생성자에 파라미터가 필요하다면?

```java
public class MyProgram {
    public void main(String[] args) {
    
    }
}
```

### **3) 따라서 static 메소드로 선언한다**

main 메소드가 static 메소드가 되면

1. **객체 생성 없이 호출** 가능하고,
2. **클래스 로딩 시점**에 메모리에 로드된다.
3. 또, JVM이 **바로 호출**할 수 있다.

```java
public class MyProgram {
    public static void main(String[] args) {
        // 이제 여기서부터 필요한 객체들을 생성할 수 있음
        MyProgram program = new MyProgram();
    }
}
```