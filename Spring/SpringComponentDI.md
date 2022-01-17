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
<br>

### 생성자 주입을 선택해야하는 이유  
   
* 불변

> 대부분 의존관계 주입은 어플리케이션 종료시 까지, 변하면 안된다.   
> 수정자 주입을 사용하면 해당 수정함수를 public하게 열어놔야 하기 때문에, 누군가 실수로라도 변경할 수 있다.   
> 생성자 주입의 경우 객체를 생성시, 1번만 호출되기 때문에 불변하게 설계 가능하다.    

* 누락 없음
> 수정자로 주입하는 경우, 누락이 될 수 있고, 테스트시 누락을 주의해야함.   

* final 사용가능
> final 키워드를 사용하려면, 생성자 주입을 사용해야함.   
> final 키워드가 있어야 초기화 단계에 값이 없는 것을 컴파일 오류로 확인 가능.   

<br>

### 생성자 주입 사용 정리
<br>

* 순수 자바언어의 특징을 잘 살리는 방법이다.   
* 기본적으로 생성자 주입을 사용하고, 필수 값이 아닌 경우에만 수정자 주입으로 옵션 부여를 하면 된다.   
* 생성자 주입과 수정자 주입은 동시에 사용이 가능하다.   
* 기본적으로 생성자 주입, 옵션으로 수정자 주입을 사용화면 된다.

<br>

### 롬복과 최신 트랜드

롬북 사용 방법

* 처음 셋팅시 적용

> https://start.spring.io/ 에서 Dependencies에 lombok 추가 

<br>

* gradle 및 설정파일 수정하여 적용
> build.gradle에 아래 코드 추가   
> reference에서 plugin 들어간 후, lombk 검색 및 설치   
> 이후 annotation processing 검색 후, compiler 에서 enable annotation processing 체크   

build.gradle 수정 코드
```
//lombok 설정 추가 시작
configurations {
	compileOnly {
		extendsFrom annotationProcessor
	}
}
//lombok 설정 추가 끝

dependencies {
	//lombok 라이브러리 추가 시작
	compileOnly 'org.projectlombok:lombok'
	annotationProcessor 'org.projectlombok:lombok'

	testCompileOnly 'org.projectlombok:lombok'
	testAnnotationProcessor 'org.projectlombok:lombok'
	//lombok 라이브러리 추가 끝
}
```
enable 체크

<img width = 700, src = "https://github.com/Jeeseob/TIL/blob/main/Spring/image/Annotation_processing.png">

lombok의 기능은 getter, setter, ToString, 생성자 등을 자동으로 만들어 주는 기능을 한다.   
특히 @RequiredArgsConstructor 의 경우, final이 붙은 필드를 파라미터로 가지는 생성자를 자동으로 만들어 준다.   
``` java
@Component
@RequiredArgsConstructor // Lombok 적용
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;
    
    ... // 생성자 불필요(자동 생성)
} 
```
> 코드가 굉장히 간소화 되는 것을 확인 할 수 있다.


<br>

### 조회 빈이 2개 이상인 문제 발생시

* 일단 오류 발생  


``` 
NoUniqueBeanDefinitionException:    
No qualifying bean of type 'hello.core.discount.DiscountPolicy' available: expected single matching bean    
but found 2: fixDiscountPolicy,rateDiscountPolicy
```

<br>

* 해결방법 1 : 필드 명을 빈 이름으로 변경한다.

<img width = 700, src = "https://github.com/Jeeseob/TIL/blob/main/Spring/image/RateDiscountPolicy.png">

위의 이미지와 같이, 필드 명을 하위 빈 이름으로 설정하는 경우, 해당 이름과 동일한 객체가 주입된다.    

<br>

* 해결방법 2 : @Quilifier 사용   

@Quilifier는 추가적인 구분자라고 생각하면 된다.   

```java
@Component
@Qualifier("rateDiscountPolicy")
public class RateDiscountPolicy implements DiscountPolicy {...}
```

```java
@Autowired
public OrderServiceImpl(MemberRepository memberRepository, @Qualifier("rateDiscountPolicy")DiscountPolicy discountPolicy) {
    this.memberRepository = memberRepository;
    this.discountPolicy = discountPolicy;
}
```
위의 형태로 되면, @Quilifier의 이름을 찾아서 생성자를 주입한다.   
수정자, 필드, 직접주입 등 언제든 의존관계 주입을 할 때에 사용 가능하다.   

<br>

만약 해당 @Quilifier를 가지는 스프링빈을 못찾으면, 해당 이름을 가지는 스프링빈을 추가로 찾는다.   
> 웬만하면, 두번째 기능으로 사용되지 않도록... Quilifier를 찾는 용도로만 사용하자.   


<br>


* 해결방법 3 : @Primary 사용

@Primary는 우선순위를 정한다고 생각하면 된다.
해당 어노테이션이 있는 경우, 해당 스프링빈이 우선순위 최상위로 주입된다.   
실무에서도 주로 메인으로 사용하는 코드에 많이 사용한다고 한다.   


<br>

* 추가사항
> @Primary 와 @Quilifier 둘 다 사용한다면, @Quilifier가 우선순위가 높다.





