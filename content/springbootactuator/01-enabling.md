+++
title = "Spring Boot Actuator"
weight = 10
outputs = ["Reveal"]
+++
{{% section %}}

## [Enabling Production-ready Features](https://docs.spring.io/spring-boot/reference/actuator/enabling.html)

---

```xml
<dependencies>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-actuator</artifactId>
	</dependency>
</dependencies>
```

---

access: `/actuator` endpoint

```json
{
  "_links": {
    "self": {
      "href": "http://localhost:8080/actuator",
      "templated": false
    },
    "health-path": {
      "href": "http://localhost:8080/actuator/health/{*path}",
      "templated": true
    },
    "health": {
      "href": "http://localhost:8080/actuator/health",
      "templated": false
    }
  }
}
```
{{% /section %}}
