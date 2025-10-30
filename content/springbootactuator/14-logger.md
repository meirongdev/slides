+++
title = "Actuator Logger"
outputs = ["Reveal"]
weight = 140
+++

{{% section %}}

## [Logger](https://docs.spring.io/spring-boot/api/rest/actuator/loggers.html)

- Allows you to view and modify the logging levels of your application at runtime

```bash
$ curl 'http://localhost:8080/actuator/loggers/com.example' -i -X POST \
    -H 'Content-Type: application/json' \
    -d '{"configuredLevel":"debug"}'
```

{{% /section %}}
