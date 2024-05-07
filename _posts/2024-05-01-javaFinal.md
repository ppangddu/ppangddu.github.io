---
layout: post
title: final 변수와 상수
subtitle:
categories: markdown
tags: [final 변수]
---
final 키워드는 이름 그대로 끝!이라는 뜻이다.

변수에 final 키워드가 붙으면 더는 값을 변경할 수 없다.

alt + insert = Generate


``` java
package final1;

public class ConstructInit {

    final int value;

    public ConstructInit(int value) {
        this.value = value;
    }
}
```
생성자를 통해서 값을 넣어주어야 한다.


``` java
package final1;

public class FieldInit {

    static final int CONST_VALUE = 10;
    final int value = 10;

    public FieldInit(int value) {
        this.value = value;
    }
}
```

이런 경우에는 초기값이 있어서 생성자를 통해서 값을 넣어주지 못한다.  
자바에는 static final 이 두 개가 붙은 변수를 상수라고 한다. 상수의 경우는 다 대문자로 적어준다.
<br>


``` java
package final1;

public class FinalFieldMain {

    public static void main(String[] args) {
        System.out.println("생성자 초기화");
        ConstructInit constructInit1 = new ConstructInit(10);
        ConstructInit constructInit2 = new ConstructInit(20);
        System.out.println(constructInit1.value);
        System.out.println(constructInit2.value);

        //final 필드 - 필드 초기화
        System.out.println("필드 초기화");
        FieldInit fieldInit1 = new FieldInit();
        FieldInit fieldInit2 = new FieldInit();
        FieldInit fieldInit3 = new FieldInit();
        System.out.println(fieldInit1.value);
        System.out.println(fieldInit2.value);
        System.out.println(fieldInit3.value);

        //상수
        System.out.println("상수");
        System.out.println(FieldInit.CONST_VALUE);
    }
}
```
FieldInit과 같이 final 필드를 필드에서 초기화 하는 경우, 모든 인스턴스가 다음 오른쪽 그림과 같이 같은 값을 가진다.  
여기서는 FieldInit 인스턴스의 모든 value 값은 10이 된다.
왜냐하면 생성자 초기화와 다르게 필드 초기화는 필드의 코드에 해당 값이 미리 정해져있기 때문이다.   
모든 인스턴스가 같은 값을 사용하기 때문에 결과적으로 메모리를 낭비하게 된다. (물론 JVM에 따라서 내부 최적화를 시도할 수 있다.)  
또, 메모리 낭비를 떠나서 같은 값이 계속 생성되는 것은 개발자가 보기에 명확한 중복이다. 이럴 때 사용하면 좋은 것이 바로 **static**이다.
<br>

- static final FieldInit.MY_VALUE는 static 영역에 존재한다. 그리고 final 키워드를 사용해서 초기화 값이 변하지 않는다. 
- static 영역은 단 하나만 존재하는 영역이다. MY_VALUE 변수는 JVM 상에서 하나만 존재하므로 앞서 설명한 중복과 메모리 비효율 문제를 모두 해결할 수 있다.

__이런 이유로 필드에 final + 필드 초기화를 사용하는 경우 static을 붙여서 사용하는 것이 효과적이다.__

상수(Constant)
상수는 변하지 않고, 항상 일정한 값을 갖는 수를 말한다.  

<h3> 자바 상수 특징 </h3>
- static final 키워드를 사용한다.
- 대문자를 사용하고 구분은 _(언더스코어)로 한다.
- 필드를 직접 접근해서 사용한다.
    - 상수는 값을 변경할 수 없다. 따라서 필드에 직접 접근해서 데이터가 변하는 문제가 발생하지 않는다.


``` java
package final1;

public class Constant {
    //수학 상수
    public static final double PI = 3.14;
    //시간 상수
    public static final int HOURS_IN_DAY = 24;
    public static final int MINUTES_IN_HOUR = 60;
    public static final int SECONDS_IN_MIMUTE = 60;
    //애플리케이션 설정 상수
    public static final int MAX_USERS = 2000;
}
```
다른 클래스에서 바로 접근이 가능하다.
<br>

``` java
package final1;

public class ConstantMain2 {

    public static void main(String[] args) {
        System.out.println("프로그램 최대 참여자 수 : " + Constant.MAX_USERS);
        int currentUserCount = 999;
        process(currentUserCount++);
        process(currentUserCount++);
        process(currentUserCount++);
    }

    private static void process(int currentUserCount) {
        System.out.println("참여자 수 : " + currentUserCount);
        if (currentUserCount > Constant.MAX_USERS) {
            System.out.println("대기자로 등록합니다.");
        } else {
        System.out.println("게임에 참여합니다.");
        }
    }
}
```
 - Constant.MAX_USERS 상수를 사용했다. 만약 프로그램 최대 참여자 수를 변경해야 하면 Constant.MAX_USER의 상수값만 변경하면 된다.
- 매직 넘버 문제를 해결했다. 숫자 1000이 아니라 사람이 인지할 수 있게 MAX_USERS라는 변수명으로 코드를 이해할 수 있다.

``` java
package final1;

public class FinalRefMain {
    public static void main(String[] args) {
        final Data data = new Data();
        //data = new Data(); //final 변경 불가 컴파일 오류

        //참조 대상의 값은 변경 가능
        data.value = 10;
        System.out.println(data.value);
        data.value = 20;
        System.out.println(data.value);
    }
}
```
참조형 변수 data에 final이 붙었다. 변수 선언 시점에 참조값을 할당했으므로 더는 참조값을 변경할 수 없다.

``` java
data.value = 10
data.value = 20
```
그런데 참조 대상의 객체의 값은 변경할 수 있다. 
- 참조형 변수 data에 final이 붙었다. 이 경우 참조형 변수에 들어있는 참조값을 다른 값으로 변경하지 못한다. 쉽게 이야기해서 이제 다른 객체를 참조할 수 없다. 참조형 변수에 들어있는 참조값만 변경하지 못한다는 뜻이다. 이 변수 이외에 다른 곳에 영향을 주는 것이 아니다.
- Data.value는 final이 아니다. 따라서 값을 변경할 수 있다.

![image](https://github.com/ppangddu/blog-comments/assets/157614269/238ddfa0-c910-4f91-b0cc-2ff0b84b1663)

정리하면 참조형 변수에 final이 붙으면 참조 대상 자체를 다른 대상으로 변경하지 못하는 것이지, 참조하는 대상의 값은 변경할 수 있다.