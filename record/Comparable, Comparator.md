## comparable과 comparator

> 본 글은 Comparable, Comparator을 이해하지 못해서 구글링 후 블로그에서 Comparable, Comparator에 관한 글을 공부한 글입니다. 

Comparable과 Comparator 둘 다 **인터페이스(Interface)**다. 즉 Comparable과 Comparator를 사용하려면 인터페이스 내에 **선언된 메소드를 반드시 구현(재정의, override)**해야 한다. 

- Comparable - 인터페이스, **자기자신과 매개변수 객체를 비교**
  - compareTo(T o)
- Comparator - 인터페이스, **두 매개변수 객체를 비교**
  - 메소드는 많지만 우리가 실질적으로 구현해야 하는 메소드는 compare(T o1, T o2)

<details>
    <summary> 인터페이스, 클래스 팁 </summary>
    <div markdown="1">
        [Tip] 인터페이스는 어떤 사물에 대해 기본적인 필수요소들을 선언만 해놓은 것이고, 이러한 필수요소들을 구체적으로 구현하는 것은 클래스라고 보면 된다. 
        그래서 인터페이스 파일의 경우 메소드를 선언만 해놓을 뿐 함수의 내용은 구체화하지 않는다. 이를 흔히 추상 메소드라고 한다. <br/>
        보충필요...<br/>
        default, static으로 선언된 메소드가 아니면 이는 추상메소드라는 의미로 반드시 재정의를 해주어야 한다. <br/>
        - default : 재정의 가능	<br/>
        - static : 재정의 불가능	<br/>
		bool equals(Object obj) 메소드는 default나 static이 안붙어있음에도 구현이 강제되지 않는 이유는 모든 객체의 최상위 타입(객체)인 Object 클래스에서 정의되었기 때문이다. <br/>
    </div>
</details>

먼저 간단히 요약을 하고 넘어가자면,

- comparable 인터페이스를 사용하려면 compareTo(T o) 메소드를 구현
- comparator 인터페이스를 사용하려면 compare(T o1, T o2) 메소드를 구현

**comparable과 comparator 인터페이스**를 객체 정렬에 사용할 수 있는 것이지, 객체 정렬'만'을 위해서 사용하는 것은 아니다. 중요한 것은 **"객체를 비교할 수 있도록 만든다."** 이다. 

primitive type(byte, int, double...)의 비교는 부등호로 가능하다. 자바 자체에서 비교 기능을 제공해주기 때문이다. =  기본 자료형은 자바 자체에서 비교 기능을 제공해주니깐 비교하는 기준을 만들 필요가 없다. 

하지만 새로운 '클래스 객체'를 만들어서 비교한다고 가정해보자. 그럼 두 객체를 비교할 때 기준이 어떻게 되는가? 객체는 사용자가 기준을 정해주지 않는 이상 어떤 객체가 더 높은 우선순위를 갖는지 판단할 수 없다. 이러한 문제점을 해결하기 위해 Comparable과 Comparator를 사용한다. 

다시 정리하자면, Comparable은 자기 자신과 매개변수 객체를 비교하고, Comparator는 두 매개변수 객체를 비교한다. 다시 말하자면 비교하는 행위만 같을 뿐이지 **비교 대상이 다르다**는 것이다. 굳이 또 다른 차이점을 짚자면 Comparable은 lang패키지에 있으므로 import해줄 필요가 없다. Comparator는 util 패키지에 있다.



### Comparable

> 자기 자신과 매개변수를 비교, compareTo(T o)

먼저 Comparable 인터페이스의 정의를 보면 interface Comparable<T>{...}로 되어있다. (<T>에 관해서는 제너릭 참조) 다시 말하자면 **<T>는 하나의 객체 타입이 지정될 자리**라고 보면 된다. 클래스를 만들 때, 기본적으로 사용 방법은 다음과 같다.

``` java
public class ClassName implements Comparable<Type> {
    ...code...;
    //필수구현
    @Override
    public int compareTo(Type o){
        /**
        비교 구현
        */
    }
}
```

 필수 구현으로 명시된 부분인 compareTo(Type o)가 **객체를 비교할 기준을 정의**하는 부분이다. 
Comparable은 자기 자신과 매개변수 객체를 비교한다고 했으니, ClassName이 생성한 객체 자신이 되고, 매개변수 객체는 ClassName.compartTo(o);를 통해 들어온 파라미터 o가 비교할 객체가 되는 것입니다. 

예시를 들었을 때, Student클래스로 학생들을 비교하고 싶을 때를 들어보자.

``` java
class Student implements Comparable<Student>{
    //1. 일단 comparable을 implements
    //2. student끼리 비교를 하고 싶으므로 Type에 <Student>
    int abe;
    int classNumber;
    
    Student(int age, int classNumber){
        this.age = age;
        this.classNumber = classNumber;
    }
    
    @Override
    public int compareTo(Student o){
        // 또 다른 Student 객체와 비교하고싶으니깐 이 자리에 Student o
        // 비교 구현 - 나이를 비교하고 싶다면
        if(this.age > o.age){//내 자신이 더 크면
            return 1;
        }else if(this.age == o.age){
            return 0;
        }else{////내 자신이 더 작으면
            return -1;
        }
        //compareTo 함수를 보면 int값을 반환하게 되어있으므로
    }
}
```

compareTo 함수는 int 값을 반환하는데, 다시 말하자면 값을 비교해서 정수를 반환하는 구조입니다. 즉 자기자신을 기준으로 삼아 대소관계를 파악하는데, 반환하는 값은 내가 상대방보다 **얼만큼 큰지**를 반환합니다. 즉 음수이면 마이너스 만큼 '큰'거니깐, 작은거죠.  굳이 1, 0, -1이 아니라 양수, 0, 음수라고 봐도 됩니다. 
사실 조건문을 통해(<,>,==) 대소비교를 하고 그에 따라 1,0,-1을 반환하는 방식이 이해하기도 쉽고 정석입니다. 
하지만, 사실상 두 값의 차를 반환해버리면 한방에 3개의 조건식을 만족할 수 있잖아?

``` java
class Student implements Comparable<Student>{
    //1. 일단 comparable을 implements
    //2. student끼리 비교를 하고 싶으므로 Type에 <Student>
    int abe;
    int classNumber;
    
    Student(int age, int classNumber){
        this.age = age;
        this.classNumber = classNumber;
    }
    
    @Override
    public int compareTo(Student o){
        // 또 다른 Student 객체와 비교하고싶으니깐 이 자리에 Student o
        // 비교 구현 - 자신이 크면 양수, 같으면 0, 자신이 비교대상보다 작으면 음수가 반환될 것이므로
     	return this.age - o.age; 
    }
}
```

``` java
//example
public class Test {
	public static void main(String[] args)  {
 
		Student a = new Student(17, 2);	// 17살 2반
		Student b = new Student(18, 1);	// 18살 1반
		
		
		int isBig = a.compareTo(b);	// a자기자신과 b객체를 비교한다.
		
		if(isBig > 0) {
			System.out.println("a객체가 b객체보다 큽니다.");
		}
		else if(isBig == 0) {
			System.out.println("두 객체의 크기가 같습니다.");
		}
		else {
			System.out.println("a객체가 b객체보다 작습니다.");
		}
		
	}
 
}

class Student implements Comparable<Student>{
    //1. 일단 comparable을 implements
    //2. student끼리 비교를 하고 싶으므로 Type에 <Student>
    int abe;
    int classNumber;
    
    Student(int age, int classNumber){
        this.age = age;
        this.classNumber = classNumber;
    }
    
    @Override
    public int compareTo(Student o){
        // 또 다른 Student 객체와 비교하고싶으니깐 이 자리에 Student o
        // 비교 구현 - 자신이 크면 양수, 같으면 0, 자신이 비교대상보다 작으면 음수가 반환될 것이므로
     	return this.age - o.age; 
    }
}
```

여기서 this는 a객체 자신을 의미하고, o는 b객체를 의미합니다. 편하다고 생각할 수 있지만, 여기에는 **뺄셈 과정에서 자료형의 범위를 넘어버리는 경우**가 발생할 수 있습니다. 범위 밖을 넘어버리면 반대편의 값으로 넘어가버리는데, 주어진 범위의 하한선을 넘는 것이 **언더플로우**, 주어진 범위의 상한선을 넘는 것을 **오버플로우**라고 합니다. 

이러한 이유로 위의 편한 방법을 사용하려면 언더플로우, 오버플로우가 발생할 여지가 있는지 반드시 확인하고 사용해야하며, primitive값에 대해서 위와 같은 예외를 확인하기 어렵다면 <,>,==으로 대소비교를 해주는 것이 일반적으로 권장되는 방식. 

> [중간 요약]
>
> - comparable, compareTo는 자기 자신과 매개변수를 비교
> - compareTo 메소드를 반드시 구현해야 함. 
>
> - compareTo는 정수를 반환, 자기 자신을 기준으로 상대방과 차이값을 비교하여 반환




### Comparator

> 두 매개변수 객체를 비교, compare(T o1, T o2)

comparable과 비슷해서 자주 헷갈린다. 하지만 comparator의 기억할 점은, 두 매개변수 객체를 비교한다는 것입니다. 자기 자신이 아니라 파라미터(매개변수)로 들어오는 두 객체를 비교하는 것이다. 형식을 보면, Comparable과 인터페이스 형식이 유사합니다.

``` java
interface Comparator<T>{...}
```

앞서 설명한 것과 같이 <T>는 하나의 객체 타입이 지정될 자리입니다. 기본적인 사용방법은 다음과 같습니다. 

```java
import java.util.Comparator;	// import 필요
public class ClassName implements Comparator<Type> { 
 
/*
  ...
  code
  ...
 */
 
	// 필수 구현 부분
	@Override
	public int compare(Type o1, Type o2) {
		/*
		 비교 구현
		 */
	}
}
```

compare 메소드가 우리가 객체를 비교할 기준을 정의해주는 부분이 됩니다. 두 객체를 비교하기 때문에 매개변수로 두 개를 받는 것. compare 메소드를 구현해야 합니다. 위에서 언급한 CompareTo와 비슷하지만 자기자신과 비교되느냐 안되느냐의 차이입니다. 
예시를 들어보겠습니다. 

``` java
import java.util.Comparator;	// import 필요
class Student implements Comparator<Student> {
 
	int age;			// 나이
	int classNumber;	// 학급
	
	Student(int age, int classNumber) {
		this.age = age;
		this.classNumber = classNumber;
	}
	
	@Override
	public int compare(Student o1, Student o2) {
    
		// o1의 학급이 o2의 학급보다 크다면 양수
		if(o1.classNumber > o2.classNumber) {
			return 1;
		}
		// o1의 학급이 o2의 학급과 같다면 0
		else if(o1.classNumber == o2.classNumber) {
			return 0;
		}
		// o1의 학급이 o2의 학급보다 작다면 음수
		else {
			return -1;
		}
	}
}
```

이번엔 두 객체를 비교해야 하기 때문에, 파라미터로 들어오는 o1, o2의 비교기준을 비교해주는 것입니다. 





---

#### 출처

- 개념
  https://st-lab.tistory.com/243

- 마크다운에서 토글 사용하기
  https://computer-science-student.tistory.com/388