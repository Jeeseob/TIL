## Spring Component 의존관계 자동주입
<br>

### 다양한 의존관계 주입 방법
<br>
의존 관계 주입 방법 4개지
* 생성자 주입
* 수정자 주입(setter)
* 필드 주입
* 일반 메서드 주입

#### 생성자 주입

생성자 호출시 @Autoㅈired를 사용하는 것.    
> 생성자가 1개만 있다면, AutoWired를 생략해도 된다.
> setter에도 @Autowired 사용 가능은 하다
<br>

생성자 주입 시 특징
> 생성자 호출시점에 딱 1번만 호출되는 것이 보장된다. (singleton)     
> '불변, 필수' 의존관계에서 사용한다. -> 강제로 수정하는 함수를 만들지 않는 이상 불변이다.     
>> setter 만들 때, 조심하자. 여러 개발자와 협력할 땐, 조심해야 한다.
<br>

스프링은 크게 2가지 life cycle로 구성되어 있다.   
* @Compnonet : 스프링 빈 등록  (생성자 주입은 빈 등록 시 일어난다.)   
* @Autowired : 의존관계 주입 이다. (이때, 의존관계는 setter에 해당한다.)

<br>

> getter, setter를 사용하는 것을 자바 빈 프로퍼티 규약이라고 한다.

<br>

#### 필드 주입   

<br>

아래 이미지처럼 필드자체에서 주입하는 방식이다.   
<img width = 700, src = "https://github.com/Jeeseob/TIL/blob/main/Spring/image/Filed_Dependency.png">

<br>

하지만 권장되지 않는다...    
> 외부에서 변경이 불가능하다   
> 스프링이 없는 상태에서 테스트를 위해 다른 필드를 사용하기 어렵다.   
> setter를 열면 변경할 수 는 있지만, 불변이 아니게 된다...   
>> 어플리케이션과 전혀 관련 없는 테스트 코드에서는 사용하기도 한다. (@SpringBootTest)   
>> 스프링 설정을 목적으로 하는 @Configuration 에서 특별한 용도로 사용되기도 한다.   

<br>

#### 일반 메소드 주입   
<br>

아무런 method 위에 @Autowired를 추가해서 의존관계를 주입하는 것.   
일반적으로 수정자, 생성자 주입선에서 구현가능하다.    
때문에 보통 쓰이진 않는다.   

<br>

### 의존관계 자동주입 옵션  
<br>

@Autowired를 사용할 때, 테스트시 스프링 빈이 주입되지 않아야 하는 경우가 생길 수 있다.    
<br>

@Autowired(required=false) : 자동주입할 대상이 없을 때, 수정자 메소드 자체가 호출되지 않는다.   
```java
@Autowired(required = false)   
public void setNoBean1(Member noBean1) {   
  System.out.println("noBean1 = " + noBean1);   
}   
```

@Nulable : 대상이 없으면 null이 입력된다.   
```java
@Autowired   
public void setNoBean2(@Nullable Member noBean2) {   
  System.out.println("noBean2 = " + noBean2);   
}   
```

Optional\<T\> : 값이 있을 수도 있고, 없을 수도 있다. 자동 주입할 대상이 없으면 Optional.empty가 입력된다   
 
```java
@Autowired   
public void setNoBean3(Optional<Member> noBean3) {   
  System.out.println("noBean3 = " + noBean3);   
}
```



                       

