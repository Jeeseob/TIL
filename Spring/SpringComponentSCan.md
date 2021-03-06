## Spring Component Scan
<br>

### 컴포넌트 스캔과 의존관계 자동 주입
<br>
스프링 빈 설정파일인 config 클래스에서   
@ComponentScan 를 사용하면, 
@Component어노테이션이 붙어있는 모든 클래스를 자동으로 스프링 빈에 등록하게 된다.   

> 이때, 의존관계 주입은 @Autowired를 생성자 위에 붙여 사용하면 된다.      
> 이렇게 되면, 스프링은 해당 타입에 맞는 클래스를 자동으로 스프링 빈으로 등록하고, 의존관계를 주입해서 적용 해준다. 
>> --> 마치 ac.getBean(class) 같은 느낌.   

기존에는 @Bean으로 설정해서, 스프링 빈을 등록했지만,위의 기능을 사용하면 자동으로 처리한다.    
때문에 클래스 내부에서 의존관계에 대한 정보를 넣어야 한다.   
<br>
### 컴포넌트 탐색위치와 기본 스캔 대상
<br>
@ComponentScan에서 여러 설정값을 통해, 스프링 빈에 등록되는 클래스, 어노테이션 등을 변경할 수 있다.

```
basePackages = "패키지.패키지2"
```
베이스 패키지를 설정해서 해당 패키지의 하위 패키지에 해당하는 클래스만 찾도록 할 수 있다.

> 기본적으로 모든 JAVA 파일을 Scan하면, 너무 오래걸리고, 많은 파일을 읽어야 한다.
> "," 로 구분해서 여러개를 지정할 수 있다.  

```
basePackagesClasses = 클래스 이름.class
```
해당 클래스가 있는 패키지를 시작점으로 둔다.  

기본값으로 설정을 지정하지 않는경우, 현재 클래스 부터 하위 패키지를 스캔한다.   
> basePackagesClasses = 현재.class 라고 생각하면 됨.   
>>보통 프로젝트 시작 루트에 AppConfig와 같은 메인 설정을 두고, @ComponentScan 어노테이션을 붙인 후 기본값으로 사용한다.   
<br>

> 스프링 부트를 사용하면, @SpringBootApplication을 시작 루트위에 두는 것이 관례이다. (해당 어노테이션 안에 @ComponentScan이 있음.)   
> 해당 어노테이션을 포함하는 객체를 제외한 나머지 객체들만 스프링빈에 등록하도록 설정   

```
@ComponentScan(
    basePackages = "hello.core.member" ,"hello.core.order",
    basePackagesClasses = AutoAppConfig.class, 
    
    exculdeFilters = @ComponentScan.Filiter(type = Filiter.ANNOTATION, classes = Configuration.class)   
)
```

아래 사진처럼 보통 SpringBootApplication에서 처음에 만들어지는 class에 @SpringBootApplication 어노테이션이 있는 이유이다.
<img width = 800 src= "https://github.com/Jeeseob/TIL/blob/main/Spring/image/CoreApplication_class.png">


### 컴포넌트 스캔 대상

@ComponentScan은 @Component를 포함한 여러 어노테이션을 스캔하게 된다.

> @Component : 컴포넌트 스캔에 사용    
> @Controller : 스프링 MVC 컨트롤러에서 사용   
> @Service : 스프링 비즈니스 로직에서 사용   
> @Repository : 스프링 데이터 접근 계층에서 사용   
> @Configuration : 스프링 설정정보에서 사용   
<br>

> 아래 이미지 처럼 어노테이션 설정에 존재한다.
<img width = 800 src= "https://github.com/Jeeseob/TIL/blob/main/Spring/image/Service_annotation.png">
<br>

> 어노테이션은 상속관계라는 것이 없다. 단순히 어노테이션이 특정 어노테이션이 있는 것을 인식하는 것이다.
> 또한, 이러한 기능은 자바언어에서 지원하는 것이 아니라 스프링 프레임워크에서 지원하는 기능이다.
>> 물론 어노테이션 자체는 자바에서 지원하는 기능이 맞다. (@interface = )

어노테이션은 메타정보이기 때문에, 어노테이션 만으로 추가적인 부가기능도 있다.   
<br>

> @Controller : 스프링 MVC 컨트롤러로 인식   
> @Repository : 스프링 데이터 접근 계층으로 인식하고, 데이터 계층의 예외를 스프링 예외로 변환해준다. -> DB접근 예외를 추상화  
> @Configuration : 앞서 보았듯이 스프링 설정 정보로 인식하고, 스프링 빈이 "싱글톤"을 유지하도록 추가 처리를 한다.  
> @Service : 특별한 부가 기능은 없지만, 개발자들이 비즈니스 로직임을 인식하는데 도움이 된다.
<br>

### 컴포넌트 필터
<br>

> includeFilters : 컴포넌트 스캔 대상을 추가로 지정한다.   
> excludeFilters : 컴포넌트 스캔에서 제외할 대상을 지정한다.

<br>

```
@ComponentScan(
    includeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = MyIncludeComponent.class), 
    excludeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = MyExcludeComponent.class)
)
```
> 위와 같은 형태로, 포함할 필터, 제외할 필터 등을 설정할 수 있다.   
<br>
FilterType은 5가지 옵션이 있다.   
<br><br>
ANNOTATION: 기본값, 애노테이션을 인식해서 동작한다.   
<br><br>

> ex)  org.example.SomeAnnotation  ASSIGNABLE_TYPE: 지정한 타입과 자식 타입을 인식해서 동작한다.   
> ex)  org.example.SomeClass  ASPECTJ: AspectJ 패턴 사용   
> ex)  org.example..*Service+  REGEX: 정규 표현식   
> ex)  org\.example\.Default.*  CUSTOM:  TypeFilter 이라는 인터페이스를 구현해서 처리   
> ex)  org.example.MyTypeFilter   

<br>

예를 들어, BeanA에 @MyIncludeComponent가 있지만, BeanA를 제외하고 싶다면 아래와 같이 하면 된다.   

```
@ComponentScan(   
    includeFilters = {         
        @Filter(type = FilterType.ANNOTATION, classes = MyIncludeComponent.class),   
    },   
    excludeFilters = {         
        @Filter(type = FilterType.ANNOTATION, classes = MyExcludeComponent.class),
        @Filter(type = FilterType.ASSIGNABLE_TYPE, classes = BeanA.class)  
    }
)    
```

<br>

참고:   
> @Component 면 충분하기 때문에,   
> includeFilters 를 사용할 일은 거의 없다.  
> excludeFilters는 여러가지 이유로 간혹 사용할 때가 있지만 많지는 않다.      
> 특히 최근 스프링 부트는 컴포넌트 스캔을 기본으로 제공하는데,   
> 옵션을 변경하면서 사용하기 보다는 스프링의 기본 설정에 최대한 맞추어 사용하는 것을 권장한다.

<br>
<br>

### 중복 등록과 충돌

컴포넌트 스캔에서 같은 빈 이름이 등록된다면??

> 자동 빈 등록 VS 자동 빈 등록
>> 'ConflictingBeanDefinitionException' 발생

> 수동 빈 등록 VS 자동 빈 등록
>> 오류 없이, 수동 등록 빈이 우선권을 가져, 오버라이딩 한다.
>> log로 확인 가능하다. 'Overriding bean definition for bean 'springBeanName' with a different definition: replacing'

<br>

하지만 실제로는 개발자의 의도대로 되는 것 보단, 실수로 꼬여서 발생하는 경우가 많다.
> 최근 스프링 부트에서는 에러가 발생한다. 
> (@SpringBootApplication이 있는 메인 클래스를 실행하는 경우 오류 발생)
>> log에서 확인 가능 Consider renaming one of the beans or enabling overriding by setting spring.main.allow-bean-definition-overriding=true
<br>
하지만, 설정을 변경하면 가능하다.    
application.properties에 'spring.main.allow-bean-definition-overriding=true'를 추가하여 설정을 변경할 수 있다.   
기본값은 false이다.   









