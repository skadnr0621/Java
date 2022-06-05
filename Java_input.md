## Java 입력

본 글은 블로그에서 자바 입력에 관한 글을 공부한 글입니다. 



#### 사전지식 : 자바의 인코딩 방식

...



#### Scanner, InputStream, BufferedReader

보통 입력을 배우기 시작하면 다음과 같은 내용을 먼저 배운다.

Scanner 객체명 = new Scanner(System.in); 입력을 하기 위해 생성한 객체.
Scanner를 사용하기 위해선 Scanner를  import해야한다.
객체명.nextLine() 문자열 
객체명.nextInt() 정수형
객체명.nextDouble() 실수형을 입력받습니다.

먼저 Stream에 대한 간단한 이해가 필요합니다.

- **Stream**이란?
  스트림은 물의 흐름에 비유된다. 프로그래밍에서는 다음과 같은 것들을 말한다.

  - 파일 데이터 ( 파일의 시작과 끝이 있는 데이터의 스트림.)
  - HTTP 응답 데이터 ( 브라우저가 요청하고 서버가 응답하는 HTTP 응답 데이터도 스트림.)
  - 키보드 입력 ( 사용자가 키보드로 입력하는 문자열은 스트림이다. )
  - 즉, **데이터의 흐름**을 물의 흐름으로 이해하는 것이다. 스트림은 단방향이기 때문에 입력과 출력이 동시에 발생할 수 없으며, 그렇기 때문에 입력, 출력으로 스트림은 나뉜다.
  - 자바에서 가장 기본이 되는 스트림은 다음과 같다. 
    - 입력 InputStream 
    - 출력 OutputStream

- **Scanner**
  Scanner(System.in)은 입력 바이트 스트림인 InputStream을 통해 표준 입력을 받습니다.

- **InputStream**
  바이트 스트림인 InputStream. 바이트 단위로 데이터를 처리합니다. 
  System.in의 type도 InputStream이다.

- **InputStreamReader** 
  문자 단위로 데이터를 처리할 수 있도록 돕는다.  InputStream의 데이터를 문자로 변환하는 중개 역할을 한다.

- **BufferedReader**
  BufferedReader br =  new BufferedReader(new InputStreamReader(System.in));
  의 방식으로 사용합니다.
  버퍼를 통해 입력받은 문자를 쌓아둔 뒤 한 번에 문자열처럼 보내버립니다. 버퍼드리더를 사용할 때 우리는 입력 메소드로 readLine()을 사용하는데, 한 줄 전체를 읽기 때문에 char 배열을 하나하나 생성할 필요없이 String으로 리턴하여 바로 받을 수 있습니다.

  String 그대로 읽어들이기 때문에 별다른 정규식을 검사하지 않고, 그렇기 때문에 스캐너에 비해서 성능이 좋습니다. 알고리즘의 풀이에서도 빠른 속도 때문에 선호됩니다.

  즉, 바이트 단위로 문자를 입력받아 문자로 처리한 뒤 버퍼에 담아두었다가 일정 조건이 충족되면 버퍼를 비우면서 데이터를 보내는 것입니다. 

---

#### 출처

https://st-lab.tistory.com/41?category=830901

https://st-lab.tistory.com/92?category=830901