+++
title = "Actuator Security"
outputs = ["Reveal"]
weight = 40
+++

{{% section %}}

## Security

---

Additional port for internal access
```yaml
management:
  server:
    port: 8081
```

```bash
http :8081/actuator/health
```
---

Add Spring Security

```xml
<dependency>
 <groupId>org.springframework.boot</groupId>
 <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

```yaml
management:
  endpoint:
    env:
      show-values: when-authorized
```

```yaml
spring:
  security:
    user:
      name: admin
      password: 123456
```

---

Stricter - Add Custom SecurityFilterChain

```java
	@Bean
	SecurityFilterChain actuatorSecurityFilterChain(HttpSecurity http) throws Exception {
		http
			.securityMatcher(EndpointRequest.toAnyEndpoint())
			.authorizeHttpRequests(requests -> requests
				.requestMatchers(EndpointRequest.to("health", "info", "metrics", "prometheus")).permitAll()
				.anyRequest().authenticated()
			)
			.httpBasic(Customizer.withDefaults())
			.csrf(csrf -> csrf.disable());
		return http.build();
	}
```

{{% /section %}}