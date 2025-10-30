+++
title = "Actuator Access Control"
outputs = ["Reveal"]
weight = 30
+++

{{% section %}}

## access control

---

<span style="color: orange;">Only expose what you really need!</span>

> By default, access to all endpoints except for **`shutdown`** and **`heapdump`** is unrestricted.

```yaml
management:
  endpoint:
    shutdown:
      access: unrestricted
    heapdump:
      access: unrestricted
```

{{% /section %}}
