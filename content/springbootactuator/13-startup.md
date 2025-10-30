+++
title = "Actuator Startup"
outputs = ["Reveal"]
weight = 130
+++

{{% section %}}

## [Startup](https://docs.spring.io/spring-boot/api/rest/actuator/startup.html)

- Provides insights into the application startup process
- Displays the time taken by each component to initialize
- Useful for identifying bottlenecks during startup

---

```java
var app = new SpringApplication(ActuatorDemoApplication.class);
app.setApplicationStartup(new BufferingApplicationStartup(4096));
app.run(args);
```

{{% /section %}}