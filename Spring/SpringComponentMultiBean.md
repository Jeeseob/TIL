## 스프링 빈을 여러개 조회해야 할 때

간단한 예시로, 사용자가 %할인과, 정액 할인 중 하나를 고를 수 있다면, 스프링 빈에 두가지 빈이 모두 등록 되어 있어야 한다.   
이때, 여러개의 빈을 등록하는 방법은 List, Map 이 있다.   


```java
class DiscountService {
  private final Map<String, DiscountPolicy> policyMap;
  private final List<DiscountPolicy> policyList;

   // 스프링 빈 여러개 등록 (List, Map 둘 다 가능)
   @Autowired
   public DiscountService(Map<String, DiscountPolicy> policyMap, List<DiscountPolicy> policyList) {
   this.policyMap = policyMap;
   this.policyList = policyList;
}
```

<br>

Map에서 해당 코드를 적용하면 다음과 같이 상황에 따라 다른 스프링 빈을 적용하여 사용할 수 있다.   

```java
public int discount(Member member, int price, String discountCode) {

  // key값(String)으로 해당 객체를 가져온다.
  DiscountPolicy discountPolicy = policyMap.get(discountCode);
  return discountPolicy.discount(member, price);
}
```

```java
int rateDiscountPrice = 
    discountService.discount(member, 20000, "rateDiscountPolicy");
```
