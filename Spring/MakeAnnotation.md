## Annotation 재정의

@Qualifier 는 뒤에 문자열이 오기 때문에, 오타를 찾기 어렵다.   
이럴때, 문자열을 넣는 대신 Annotation을 재정의 하여 오타를 방지할 수 있다.

<br>

```java
// Qualifier 대체이기 때문에, Qualifier에 있던 어노테이션 그대로 복사
@Target({ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER, ElementType.TYPE, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented

// 추가로 Qualifier 어노테이션도 선언해줌
@Qualifier
// 어노테이션 이름확인
public @interface MainDiscountPolicy {
}

```

<br>

```java
// 스프링빈에 등록될 객체 코드
@MainDiscountPolicy 
public class RateDiscountPolicy implements DiscountPolicy {...}
```

<br>


```java
// 의존관계 주입관련 생성자 코드
@Autowired
// @Qualifier 대신 만든 어노테이션 사용가능
public OrderServiceImpl(MemberRepository memberRepository, @MainDiscountPolicy DiscountPolicy discountPolicy) {
  this.memberRepository = memberRepository;
  this.discountPolicy = discountPolicy;
}
```

* 주의 사항
> 무분별한 annotation 재정의는 유지보수에 혼란을 야기할 수 있다.    
> 우선적으로 스프링에서 지원하는 어노테이션을 사용하고, 추가적으로 필요한 부분에만 사용할 수 있도록 하자.   
