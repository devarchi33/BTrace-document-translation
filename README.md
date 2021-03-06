# BTrace-document-translation
BTrace User Guide 한국어 번역

## [BTrace 사용자 안내서 (원문링크)](https://kenai.com/projects/btrace/pages/UserGuide)

ver. 1.2(20101020)
**BTrace** 는 자바를 위한 안전하고, 동적인 추적 도구이다. BTrace 는 동작하고 있는 Java 프로그램의 클래스를 동적으로 조작 하면서 작동한다.

## BTrace 용어
__Probe Point__

  일련의 추적 구문이 실행되는 "location" 또는 "event". Probe Point 는 우리가 추적하길 원하는 구문들이 있는 "place" 또는 "event" 에 관심이 있다.

__Trace Actions or Actions__

  probe 가 "fire" 할때마다 실행되는 추적 구문들.
  
  __Action Methods__
  
  probe 가 도착할 때마다 실행되는 추적 구문들은 class 의 static method  안에 정의 되어져 있다. 이러한 메소드들을 "action"이라고 부른다.
  
## BTrace Program 구조
  하나의 BTrace 프로그램은 하나 또는 그이상의 plain java class 이다.
  ```
  public static void
  ```
  BTrace 어노테이션이 붙은 메소드들. 어노테이션들은 추적할 프로그램에서 "위치"를 특정해 준다.(probe points 라고 알려진)
 추적할 행위들은 static method 구문안에 놓인다. 이러한 메소드들이 "action" 메소드로서 참조 된다.
 
## BTrace 제약조건들
추적 프로그램의 동작이 "읽기전용" 을 보장하기 위해서(예를들면, 추적행위 는 추적 프로그램의 상태를 변경하지 않는다.) 그리고 제한되어져 있다. (예를들면, 추적행위는 제한된 시간안에 종료된다.) BTrace 프로그램은 오직 일련의 제한된 행위들만 허락된다.
  
  * 새로운 객체를 생성할 수 없다.
  * 새로운 배열을 생성할 수 없다.
  * 새로운 예외상황을 발생시킬 수 없다.
  * 임의의 인스턴스나 static 메소드를 호출할 수 없다. - 오직 com.sun.btrace.BTraceUtils 의 public static method 들 과 BTrace 프로그램안에서 선언된 메소들만 호출할 수 있다.
  * 1.2 이전 버전은 인스턴스 필드를 가질수 없다. 오직 static public void 를 return 하는 메소드들만 BTrace 프로그램안에서 허락되어진다. 그리고 모든 필드들은 static 지정어를 가져야만 한다.
  * 타겟이 되는 프로그햄의 클래스들 또는 객첵들에 static 또는 인스턴스 필드들을 할당할 수 없다.(추적의 상태는 변형될 수 있다.)
  * outer, innerm nested 또는 local class 를 가질수 없다.
  * synchronized 블록 또는 synchronized 메소드를 가질 수 없다.
  * 반복구문을 가질 수 없다.(for, while, do~while)
  * 임의의 클래스를 확장할 수 없다. (super class 는 java.lang.Object 이어야만 한다.)
  * 인터페이스를 구현할 수 없다.
  * assert 구문을 갖을 수 없다.
  * class 리터럴을 사용할 수 없다.
  
이러한 제약조건들은 "unsafe" 모드로 BTrace 를 동작하면 피할 수 있다. 추적 스크립트 와 엔진 모두 "unsafe" 모드로 설정되어야만 한다. 이러한 스크립트들은 "@BTrace(unsafe=true)" annotation 이 붙어야 한다. 그리고 엔진은 unsafe 모드로 구동 되어야 한다.

## 간단한 BTrace 프로그램 (1.2 버전)
```
// import all BTrace annotations
import com.sun.btrace.annotations.*;
// import statics from BTraceUtils class
import static com.sun.btrace.BTraceUtils.*;
 
// @BTrace annotation tells that this is a BTrace program
@BTrace
class HelloWorld {
 
  // @OnMethod annotation tells where to probe.
  // 이 예시에서 java.lang.Thread 의 start() 에 진입하는데 관심이 있다.
  @OnMethod(
    clazz="java.lang.Thread",
    method="start"
  )
  void func() {
    sharedMethod(msg);
  }
  void sharedMethod(String msg) {
    // println is defined in BTraceUtils
    println(msg);
  }
}
```

## 간단한 BTrace 프로그램 (1.2 미만 버전)
```
// import all BTrace annotations
import com.sun.btrace.annotations.*;
// import statics from BTraceUtils class
import static com.sun.btrace.BTraceUtils.*;

//@BTrace annotation tells that this is a BTrace program
@BTrace
public class HelloWorld {

  // @OnMethod annotation tellls where to probe.
  // 이 예제에서, Thread.start() 에 관심이 있다.
  @OnMethod(
    clazz="java.lang.Thread",
    method="start"
  )
  public static void func() {
    // print is defiened in BTraceUtils
    // you can only call the static methods of BTraceUtils
    println("about to start thread!");
  }
}  
```
위 BTrace 프로그램은 동작중인 Java 프로세스에 대하여 동작할 수 있다. 이 프로그램은 타겟 프로그램이 Thread.start() 에 의해서 thread 를 시작할 때만다 BTrace 클라이언트에 "about to start a thread!" 를 출력할 것이다. 다른 흥미로운 probe point들이 있다. 예를들면, 우리는 메소드 가 주는 리턴, 메소드가 던지는 익셉션, 메소드 안에있는 getter, setter, 객체/배열의 생성, 라인 넘버, 익셉션에 추적 action 을 삽입할 수 있다. 자세한 사항은 [@OnMethod and other annotations](https://kenai.com/projects/btrace/pages/UserGuide#btrace_anno)를 참조하길 부탁드린다.

## BTrace 를 동작시키는 단계

  1. 당신이 추적하길 원하는 타겟 Java 프로세스의 프로세스 아이디를 찾아라. 당신은 프로세스 아이디를 찾기 위해서 jps tool을 사용할 수 있다.
  2. BTrace 프로그램을 작성하라. - 샘플들중에서 하나를 수정해서 시작할수 있다(?)
  3. btrace tool 을 아래의 명령어로 동작 시켜라.
  ```
  btrace <pid> <btrace-script>
  ```
  
## BTrace 명령어
BTrace 는 아래에 보여지는 명령행을 사용해서 동작한다.
```
btrace [-I <include-path>] [-p <port>] [-cp <classpath>] <pid> <btrace-script> [<args>]
```

 
