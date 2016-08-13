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
  * 
 
