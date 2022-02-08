## 스프링빈 Scope 설정(Prototype과 Singleton 동시 사용)

#### Prototype을 Singelton과 함게 사용하는 방법에는 크게 2가지 방법이 있다.

<br>

1안은 Java의 표준라이브러리인 JSR-330을 사용하는 것이다.   
> 이는 spring에 종속적이지 않다는 장점이 있지만, 별도의 설정(gradle에 라이브러리 추가)를 해야한다는 불편함이 있다.   

<br>

2안은 Springframework의 ObjectProvider를 사용하는 것이다.   
> 이는 추가적인 편의기능이 있고, 별도의 의존관계 추가가 필요없어 편리하다.   
> 하지만 스프링에 종속적이기 때문에, 언젠가 다른 컨테이너에서 사용해야한다면 불펺하다.   

<br>

1안의 Provider는 *import javax.inject.Provider;* 를 통해 import하였다.

```java
@Scope("singleton")
static class ClientBean {

    @Autowired
    // 1안 : java표준 사용 Provider를 통해 get한다면, 사용할 때마다, 새로운 빈을 가져올 수 있다.
    private Provider<PrototypeBean> prototypeBeanProvider;
    
    // 2안 : 스프링 내부 ObjectProvider사용
    private ObjectProvider<PrototypeBean> prototypeProvider;

    public int logic() {
        // 1안  
        PrototypeBean prototypeBean = prototypeBeanProvider.get();
        
        // 2안
        PrototypeBean prototypeBean = prototypeBeanProvider.getObject();
        
        prototypeBean.addCount();
        int count = prototypeBean.getCount();

        return count;
    }
}
```

<br>

> 대부분의 문제는 Singleton으로 해결이 가능하다.    
> 일단 알고 있으면, 다양한 문제를 해결할 수 있을 것이고, 존재여부만 기억하더라도 나중에 찾아서 사용할 수 있을 것 같다.
