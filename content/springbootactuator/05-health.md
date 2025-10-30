+++
title = "Actuator Health"
outputs = ["Reveal"]
weight = 50
+++

{{% section %}}
## Health

Show Details
```yaml
management:
  endpoint:
    health:
      show-details: always
```

---

`http :8081/actuator/health`

```json
{
    "components": {
        "diskSpace": {
            "details": {
                "exists": true,
                "free": 870792753152,
                "path": "/Users/matthew/projects/meirongdev/actuator-demo/.",
                "threshold": 10485760,
                "total": 994662584320
            },
            "status": "UP"
        },
        "ping": {
            "status": "UP"
        },
        "ssl": {
            "details": {
                "invalidChains": [],
                "validChains": []
            },
            "status": "UP"
        }
    },
    "status": "UP"
}
```

---
### More builtin

```yaml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```
Additional starters may add their own health indicators

---

output when redis is not available:
```json
{
    "components": {
        "diskSpace": {
            ...
            "status": "UP"
        },
        "ping": {
            "status": "UP"
        },
        "redis": {
            "details": {
                "error": "org.springframework.data.redis.RedisConnectionFailureException: Unable to connect to Redis"
            },
            "status": "DOWN"
        },
        "ssl": {
            "details": {
                "invalidChains": [],
                "validChains": []
            },
            "status": "UP"
        }
    },
    "status": "DOWN"
}
```

---
Customize Health Indicator
```java
@Bean(name = "depsIndicator")
    HealthIndicator depsHealthIndicator() {
        return () -> {
            Health.Builder healthBuilder = Health.up();

            // Define the services to check
            Map<String, Boolean> serviceHealthMap = Map.of(
                "serviceA", checkServiceA(),
                "serviceB", checkServiceB()
            );

            // Check each service and update health status
            serviceHealthMap.entrySet().forEach(entry -> {
                if (!entry.getValue()) {
                    healthBuilder.down().withDetail(entry.getKey(), entry.getKey() + " is down");
                } else {
                    healthBuilder.withDetail(entry.getKey(), entry.getKey() + " is healthy");
                }
            });

            boolean allServicesHealthy = serviceHealthMap.values().stream()
                .allMatch(Boolean::booleanValue);

            return allServicesHealthy ? healthBuilder.build() : healthBuilder.down().build();
        };
    }
```

--- 

output in `/actuator/health`:
```json
    "depsIndicator": {
            "details": {
                "serviceA": "serviceA is healthy",
                "serviceB": "serviceB is down"
            },
            "status": "DOWN"
        },
```
--- 
Cache

```yaml
management:
    endpoint:
        health:
            cache:
                time-to-live: 30s
```

{{% /section %}}

