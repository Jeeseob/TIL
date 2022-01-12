Spring Bean이란


컴포넌트 스캔과 의존관계 자동 주입

스프링 빈 설정파일인 config 클래스에서
@ComponentScan 를 사용하면, @Component어노테이션이 붙어있는 모든 클래스를 자동으로 스프링 빈에 등록하게 된다.   
이때, 의존관계 주입은 스프링빈에 등록할 클래스에서 @Component와 함꼐 @Autowired를 생성자 위에 붙여 사용하면 된다.   
이렇게 되면, 스프링은 해당 타입에 맞는 클래스를 자동으로 스프링 빈으로 등록하고, 의존관계를 주입해서 적용 해준다. --> 마치 ac.getBean(class) 같은 느낌.

기존에는 @Bean으로 설정해서, 스프링 빈을 등록했지만, 위의 기능을 사용하면 자동으로 처리한다. 때문에 클래스 내부에서 의존관계에 대한 정보를 넣어야 한다.

컴포넌트 탐색위치와 기본 스캔 대상

@ComponentScan에서 여러 설정값을 통해, 스프링 빈에 등록되는 클래스, 어노테이션 등을 변경할 수 있다.

@ComponentScan(
    // 베이스 패키지를 설정해서 해당 패키지의 하위 패키지에 해당하는 클래스만 찾도록 할 수 있음
    // 기본적으로 모든 JAVA 파일을 Scan하면, 너무 오래걸리고, 많은 파일을 읽어야 한다.
    // ,로 구분해서 여러개를 지정할 수 있다.
    basePackages = "hello.core.member" ,"hello.core.order",

    // 해당 클래스가 있는 패키지를 시작점으로 둔다.
    basePackagesClasses = AutoAppConfig.class,

    // 지정하지 않는경우, 현재 클래스 부터 하위 패키지를 스캔한다.
    // basePackagesClasses = 현재.class 라고 생각하면 됨.
    // 보통 프로젝트 시작 루트에 AppConfig와 같은 메인 설정을 두고, @ComponentScan 어노테이션을 붙인 후 기본값으로 사용한다.

    // 스프링 부트를 사용하면, @SpringBootApplication을 시작 루트위에 두는 것이 관례이다. (해당 어노테이션 안에 @ComponentScan이 있음.)

    <img src= "/image/CoreApplication_class.png">

    // 해당 어노테이션을 포함하는 객체를 제외한 나머지 객체들만 스프링빈에 등록하도록 설정
    exculdeFilters = @ComponentScan.Filiter(type = Filiter.ANNOTATION, classes = Configuration.class)
)
