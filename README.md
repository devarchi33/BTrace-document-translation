# BTrace-document-translation
BTrace User Guide 한국어 번역

## BTrace 사용자 안내서

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
 
