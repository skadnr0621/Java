## Singleton

> 단 하나의 객체만을 생성하게 강제하는 디자인 패턴. 

- 싱글톤 패턴이란?
  클래스가 최소 한번만 메모리를 할당하고(static), 그 메모리에 객체의 인스턴스를 오직 1개만 생성하는 디자인 패턴. 쉽게 말해 객체를 하나만(Single) 만들 수 있게 강제하는 패턴.
  
  - 생성자는 외부에서 호출할 수 없도록 private 으로 작성
  
  - 해당 인스턴스를 얻으려면 getInstance() 메서드를 사용 -> 다른 함수에서 여러번 호출하더라도 단 하나의 동일한 인스턴스를 반환. 이 과정에서 동시성 문제가 발생할 수 있는데, 밑에서 보충. 
  
  - ```java
    // 생성 예시
    class Singleton {
        private static Singleton one; //static으로 사용하기 위함.
        private Singleton() { //private으로 생성자의 접근을 막는다.
        }
    
        public static Singleton getInstance() {
            if(one==null) {
                one = new Singleton();
            }
            return one;
            // one이 null일 경우만 객체 생성. one은 static 변수이므로 이후부터는 null이 아니게 되고, 이후부터는 이미 만들어진 싱글톤 객체인 one을 항상 리턴한다. 
        }
    }
    
    public class Sample {
        public static void main(String[] args) {
            Singleton singleton1 = Singleton.getInstance();
            Singleton singleton2 = Singleton.getInstance();
            System.out.println(singleton1 == singleton2);  // true 출력
        }
    }
    
    //여기서 만든 싱글톤은 thread safe하지 않ㄴ다. 쓰레드 환경에서 안전한 싱글톤을 만드는 방법은 일단은 생략. 
    ```
  
- 싱글톤 패턴의 구현 방법(보충 요)
  - static block
    인스턴스 사용 시점이 아닌 클래스 로딩 시점에 실행
  - lazy init
    인스턴스 사용 시점에 요청해서 생성, but 멀티 스레드 환경에서 취약 / double check
  - thread safe + lazy
    synchronized 키워드로 동시 접근 문제는 해결했으나 성능 저하 발생
  - lazy holder
    java 싱글톤 생성에서 사용되는 대표적인 방법
  
- 싱글톤 패턴을 사용하는 이유
  - 최초 한번의 new 연산자를 통해서 고정된 메모리 영역을 할당받는다. 
    -> 추후 해당 객체 접근 시 메모리 낭비를 방지할 수 있다. 
  - 전역으로 사용되기 때문에 다른 클래스 간의 데이터 공유가 쉽다. 
    -> but 여러 클래스에서 동시 접근 시 동시성 문제가 발생할 수 있으니 주의해서 설계해야한다.
  - 도메인 관점에서 인스턴스가 1개만 존재하는 것을 보증하고 싶은 경우 사용한다.
  
- 싱글톤 패턴의 문제점
  - 싱글톤 패턴을 구현하는 코드가 많이 필요하다. 
    정적 팩토리 메서드에서 객체 생성을 확인하고 생정자를 호출하는 경우 멀티 스레딩 환경에서 발생할 수 있는 동시성 문제 해결을 위해 Syncronized 키워드를 사용해야한다 -?
  - 테스트 하기 어렵다. 
    전역에서 공유하므로 매번 인스턴스를 초기화하지 않으면 테스트가 온전하게 수행되기 어렵다.
  - 클라이언트가 구체 클래스에 의존하게 된다. (=결합도가 높아진다)
    new 키워드로 직접 생성하므로 SOLID 원칙 중 DIP를 위반한다. + OCP 또한 위반 가능성이 높다. -?
  - 자식 클래스를 만들 수 없다, 내부 상태를 변경하기 어렵다 등등.. 
  
- 결론
  - 효율만큼 문제점도 많다. 
  - 안티패턴으로 불릴만큼 단독 사용할 경우 객체 지향에 위반되는 사례가 많다. 때문에 스프링 컨테이너 등의 프레임 워크의 도움을 받아 문제점을 보완하며 장점을 누리고 있다. 



이 부분은 아직 명확히 이해가 되지 않았다. 추후 보충 예정

---

#### 출처 

https://tecoble.techcourse.co.kr/post/2020-11-07-singleton/

https://elfinlas.github.io/2019/09/23/java-singleton/

https://injae-kim.github.io/dev/2020/08/06/singleton-pattern-usage.html

https://wikidocs.net/228

