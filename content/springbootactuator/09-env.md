+++
title = "Actuator Env"
outputs = ["Reveal"]
weight = 90
+++

{{% section %}}

## [Env](https://docs.spring.io/spring-boot/api/rest/actuator/env.html)

[Externalized Configuration](https://docs.spring.io/spring-boot/reference/features/external-config.html)

---

```yaml
management:
  endpoint:
    env:
      show-values: always # never, when-authorized, always
```

---

```java
  @Bean
  SanitizingFunction sanitizeEnv() {
    return (SanitizableData data) -> {
        if (data.getLowerCaseKey().equals("spring.application.pid")) {
            return data.withValue(999);
        }
        if (data.getLowerCaseKey().equals("spring.security.user.password")) {
            return data.withSanitizedValue();
        }
        return data;
    };
  }
```

{{% /section %}}