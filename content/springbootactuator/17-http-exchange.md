+++
title = "Actuator HTTP Exchange"
outputs = ["Reveal"]
weight = 170
+++

{{% section %}}


## [HTTP Exchange](https://docs.spring.io/spring-boot/api/rest/actuator/httpexchanges.html)

> Displays HTTP exchange information (by default, the last 100 HTTP request-response exchanges). 

> Requires an HttpExchangeRepository bean.

---

```java
  @Bean
  InMemoryHttpExchangeRepository httpExchangeRepository() {
    return new InMemoryHttpExchangeRepository();
  }
```

{{% /section %}}
