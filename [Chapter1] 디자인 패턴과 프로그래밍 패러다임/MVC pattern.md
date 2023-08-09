## MVC 패턴

<img src="https://junhyunny.github.io/images/mvc-pattern-1.JPG" alt="img" style="zoom:33%;" /> <img src="https://junhyunny.github.io/images/mvc-pattern-2.JPG" alt="img" style="zoom: 25%;" />

MVC 패턴은 **모델(Model), 뷰(View), 컨트롤러(Controller)**로 이루어진 디자인 패턴이다.

MVC 패턴을 통해 **비즈니스 처리 로직과 사용자 인터페인스 화면을 분리함**으로써 유지보수, 재사용, 확장이 용이해지고 **개발의 효율성이 증가**한다.

예시로는 React, Google의 Angular JS, Python의 Django가 존재한다.



### 구성 요소

**1. 모델(Model)**

모델은 데이터베이스, 상수, 변수 등과 같은 **애플리케이션 데이터와 비즈니스 처리 로직을 의미**한다.

모델은 데이터 추출, 저장, 삭제, 업데이트 등의 역할을 수행한다. 즉, 데이터와 비즈니스 로직을 관리하는 역할을 한다.

**2. 뷰(View)**

뷰는 inputbox, checkbox, textarea 등 **사용자 인터페이스 요소**를 나타낸다.

**사용자와 상호작용**하며 컨트롤러로부터 받은 모델의 결과값을 사용자에게 **화면으로 출력**하는 역할을 한다.

**3. 컨트롤러(Controller)**

컨트롤러는 **하나 이상의 모델과 하나 이상의 뷰를 잇는 다리 역할**을 하며 이벤트 등 메인 로직을 담당한다.

컨트롤러는 모델과 뷰의 생명주기를 관리하고 모델이나 뷰의 변경 통지를 받으면 이를 해석하여 각각의 구성 요소에 해당 내용을 알려준다.



### 동작 과정

1. 사용자의 action이 컨트롤러에 들어온다.
2. 컨트롤러는 사용자의 action을 확인하고 model을 업데이트한다.
3. 컨트롤러는 model을 나타내줄 view 선택한다.
4. view는 model을 이용하여 화면을 나타낸다.



### MVC 패턴을 지키기 위한 규칙

1. 모델은 컨트롤러와 뷰에 의존하지 않아야 한다.

   (= 모델 내부에 컨트롤러와 뷰에 관련된 코드가 있으면 안 된다.)

2. 뷰는 모델에만 의존해야 하고, 컨트롤러에는 의존하면 안 된다.

   (= 뷰 내부에는 모델의 코드만 있을 수 있고 컨트롤러의 코드가 있으면 안된다.)

   ```java
   public class OutputView{
     public void printProfile(Student student){
       System.out.println("이름은 "+student.getName()+"입니다.");
     }
   }
   ```

3. View가 Model로부터 데이터를 받을 때는, 사용자마다 다르게 보여주어야 하는 데이터에 대해서만 받아야 한다.

   - 뷰 = 사용자한테 보이는 UI + 모델로부터 받은 데이터

4. 컨트롤러는 모델과 뷰에 의존해도 된다.

   (= 컨트롤러 내부에는 모델과 뷰의 코드가 있을 수 있다.)

   - 컨트롤러는 모델과 뷰의 중개자 역할을 하면서 전체 로직을 구성하기 때문이다.

   ```java
   public class Controller{
     public static void main(String[] args)
     {
       Student student = new Student("기철", 25);
       OutputView.printProfile(student);
     }
   }
   ```

5. 뷰가 모델로부터 데이터를 받을 때, 반드시 컨트롤러에서 받아야 한다.



### MVC 패턴의 단점

MVC 패턴에서 컨트롤러와 뷰는 1:N 관계를 가지며 모델과 뷰는 컨트롤러를 통해 소통을 이루기 때문에 의존성이 완전히 분리될 수 없다. 

결과적으로, 애플리케이션이 복잡해질수록 모델과 뷰의 관계가 복잡해지고 컨트롤러가 불필요하게 커지는 현상이 발생하게 된다.

이를 보완하기 위해 MVP, MVVM, Flux, Redux 등 다양한 패턴이 생겼다.

<img src="https://junhyunny.github.io/images/mvc-pattern-3.JPG" alt="mvc-pattern-3" style="zoom:30%;" />



---

### References

https://cocoon1787.tistory.com/733

https://junhyunny.github.io/information/design-pattern/mvc-pattern/

https://velog.io/@seongwon97/MVC-%ED%8C%A8%ED%84%B4%EC%9D%B4%EB%9E%80

https://velog.io/@peaceminusone/TIL-%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-MVC-MVP-MVVM-%ED%8C%A8%ED%84%B4

https://hyeon9mak.github.io/5-rules-for-MVC-pattern/

https://www.youtube.com/watch?v=ogaXW6KPc8I&t=107s