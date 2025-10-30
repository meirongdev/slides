+++
title = "Actuator K8s Support"
outputs = ["Reveal"]
weight = 70
+++

{{% section %}}
## [K8s support](https://docs.spring.io/spring-boot/reference/actuator/endpoints.html#actuator.endpoints.kubernetes-probes)

```yaml
management:
  endpoint:
    health:
      probes:
        enabled: true # This is enabled by default in k8s environment
        add-additional-paths: true
       group:
         liveness:
           include: diskSpace, ping
         readiness:
           include: db, redis # include all the indicators you want
```

<pre>
Spring Boot auto-detects Kubernetes deployment environments by checking
the environment for "*_SERVICE_HOST" and "*_SERVICE_PORT" variables. 

You can override this detection with the `spring.main.cloud-platform`
configuration property.
</pre>



---

In the k8s side, can configure like this:

```yaml
livenessProbe:
  httpGet:
    path: "/actuator/health/liveness"
    port: <actuator-port>
  failureThreshold: ...
  periodSeconds: ...

readinessProbe:
  httpGet:
    path: "/actuator/health/readiness"
    port: <actuator-port>
  failureThreshold: ...
  periodSeconds: ...
```

For the graceful shutdown, can refer to [Spring Boot 3 Graceful Shutdown with Kubernetes](https://meirong.dev/posts/graceful-shutdown-for-springboot3-in-k8s/).

{{% /section %}}