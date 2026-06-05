+++
title = "How ArchUnit works"
weight = 60
outputs = ["Reveal"]
+++

{{% section %}}

## ⚙️ How ArchUnit works (1/2)

A **bytecode analysis engine + a fluent assertion DSL**. Pure static
analysis on compiled `.class` files — no Spring context, no running app.

```text
.class files
   ↓  1. Import   — ClassFileImporter builds a graph of
                    JavaClass / JavaMethod / JavaField
   ↓  2. Evaluate — fluent DSL walks that object graph
   ↓  3. Condition — each match checked by an ArchCondition
   ↓  4. Report   — failures listed with FQN + line + reason
```

Because it reads bytecode directly, it can see annotations, inheritance,
field types, and even **method-call relationships** without executing code.

---

## ⚙️ How ArchUnit works (2/2)

The three-part DSL — `what().that(predicate).should(condition)`:

```java
@AnalyzeClasses(packages = "com.example",
                importOptions = ImportOption.DoNotIncludeTests.class)
class ArchitectureTest {

  @ArchTest
  static final ArchRule noFieldInjection =
      noFields().should().beAnnotatedWith(Autowired.class)
                .because("use constructor injection");
}
```

| ✅ Can check | ❌ Cannot check |
|---|---|
| annotations, inheritance, interfaces | runtime behavior / return values |
| method-call & package dependencies | dynamic-proxy behavior |
| field types, method signatures | `application.yml` config |
| custom bytecode patterns | reflection targets |

---

## ArchUnit — conventions as rules

Common "ArchUnit red flags" — CI fails if present:

```java
noFields().should().beAnnotatedWith(Autowired.class);    // no field injection
noClasses().should().callConstructor(ObjectMapper.class);// reuse shared bean
noClasses().should().callConstructor(RestTemplate.class);// use RestClient
noClasses().should().accessClassesThat()
           .haveFullyQualifiedName("java.lang.System");   // no System.out
classes().that().areAnnotatedWith(RestController.class)
         .should().haveSimpleNameEndingWith("Controller");
```

> The team agreement and the build check are the **same artifact**.
> Custom `ArchCondition<T>` / `DescribedPredicate<T>` handle anything bespoke.

{{% /section %}}
