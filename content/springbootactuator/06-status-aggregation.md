+++
title = "Actuator Status Aggregation"
outputs = ["Reveal"]
weight = 60
+++

{{% section %}}
Status Aggregation
```yaml
management:
  endpoint:
    health:
      status:
        order: down, out-of-service, unknown, up
        http-mapping:
          down: 503
          out-of-service: 503
          unknown: 500
          up: 200
```
Add custom http-mapping will remove default mapping.

{{% /section %}}
