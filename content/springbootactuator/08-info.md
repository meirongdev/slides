+++
title = "Actuator Info"
outputs = ["Reveal"]
weight = 80
+++

{{% section %}}
## [Info](https://docs.spring.io/spring-boot/api/rest/actuator/info.html)

---
Builtin contributors
- build
- env
- git
- java
- os
- process
- ssl

---

Build Info

```xml
<plugin>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-maven-plugin</artifactId>
  <goals>
    <goal>build-info</goal>
  </goals>
</plugin>
<plugin>
  <groupId>io.github.git-commit-id</groupId>
  <artifactId>git-commit-id-maven-plugin</artifactId>
</plugin>
```

```json
{
    "git": {
        "branch": "main",
        "commit": {
            "id": "f08b993",
            "time": "2025-10-27T14:33:29Z"
        }
    }
}
```

---

Other Builtin Contributors

```yaml
management:
  info:
    build:
      enabled: true
    git:
      enabled: true
    os:
      enabled: true
    process:
      enabled: true
    java:
      enabled: true
    ssl:
      enabled: true
```

---
Write your own

```java
@Bean
InfoContributor barInfoContributor() {
  return new InfoContributor() {
    @Override
    public void contribute(Info.Builder builder) {
      builder.withDetail("bar", Map.of("now", Instant.now().toString()));
    }
  };
}
```

{{% /section %}}