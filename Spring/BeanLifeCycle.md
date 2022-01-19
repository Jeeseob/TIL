## 스프링 빈 라이프사이클 콜백

어플리케이션 실행 시, 스프링 빈의 라이프 사이클은 다음과 같다.

> 스프링 컨테이너 생성 -> 스프링 빈 생성 -> 의존관계 주입 -> 초기화 콜백 -> 스프링 빈 사용 -> 소멸 전 콜백 -> 스프링 종료   

이때, 스프링 빈 생성 혹은 의존관계 주입과정에서는 객체 생성 및 연결만을 담당하고,    
무거운 작업(데이터 베이스 연결 등)은 초기화 함수를 사용하는 것이 좋다.   

<br>

또한 종료시에도, 스프링 종료와 동시에 종료하기 보단,    
미리 빈이 소멸되기 전에 종료 메소드를 실행한 후, 스프링을 종료하는 것이 예상치 못한 오류를 막을 수 있는 방법 이다.

> 초기화 콜백: 빈이 생성되고, 빈의 의존관계 주입이 완료된 후 호출  
> 소멸전 콜백: 빈이 소멸되기 직전에 호출


<br>

스프링에서는 3가지 방법으로 생명주기 콜백을 지원한다.



<br>

* 인터페이스(InitializingBean, DisposableBean) 
>  두가지 인터페이스를 implements해서, 해당 인터페이스의 함수를 override하는 방법이다.
>  단점으로 외부 라이브러리에서 코드를 변경하지 못할 때, 사용이 불가능하고, 메소드 명을 변경할 수 없다.

```java
public class NetworkClient implements InitializingBean, DisposableBean {
  @Override // InitializingBean 초기화
  public void afterPropertiesSet() throws Exception {...}
    
  @Override // DisposableBean 종료
  public void destroy() throws Exception {...}
}
```



<br>

* 설정 정보에 초기화 메서드, 종료 메서드 지정 (@Bean)활용
> @Bean(initMethod = "init", destroyMethod = "close")
> 스프링 빈을 등록할 때, @Bean 뒤에 설정 정보를 붙이는 방법이다.
> 장점은 내부 코드를 변경하지 않고 사용할 수 있다.(외부 라이브러리에 사용가능)
> 단점은 다른 객체에 있는 메소드 명을 확인한 후 적용해야한다.

```java
@Bean(initMethod = "init", destroyMethod = "close")
public NetworkClient networkClient() {...}
```


<br>

* @PostConstruct, @PreDestroy 애노테이션 지원
> 자바에서 제공하는 어노테이션을 활용하는 방법이다.
> 장점은 해당 객체의 메소드에 어노테이션을 붙이면 간단하게 적용된다는 것이다.
> 단점은 역시 변경이 불가능한 외부 라이브러리에서 사용하기 어렵다는 것이다.

```java
public class NetworkClient {
  @PostConstruct
    public void init() {...}
    
  @PreDestroy
    public void close() {...}
}
```


<br>

* 결론적으로 대부분 @PostConstruct, @PreDestroy을 사용하고, 외부 라이브러리를 사용하는 특수한 상황에서는 설정정보를 사용한다.
* 인터페이스는 사용하지 않는 것이 좋다.
