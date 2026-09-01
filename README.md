# Nova Mapper Utils

Object-to-object mapping without an annotation processor and without a
framework. Where MapStruct generates code at compile time, this maps at
runtime through a cached reflection layer — the trade is a little speed
for zero build-time machinery.

## What's inside

| Package | Type | Does |
|---|---|---|
| — | `MapperEngine` | The entry point: `map`, `mapList`, `mapSet` |
| `config` | `MappingConfig`, `FieldMapping` | Renames, exclusions, nested configs |
| `converter` | `TypeConverter`, `DefaultConverters` | Conversion between field types |
| `reflect` | `ReflectionCache`, `ObjectInstantiator`, `FieldInfo` | The cached reflection layer |
| `result` | `MappingResult` | What mapped, what did not |
| — | `NullStrategy` | What to do with a null source field |

## Install

Published to GitHub Packages, so the repository needs to be declared and
authenticated with a token that has `read:packages`.

```kotlin
repositories {
    maven {
        url = uri("https://maven.pkg.github.com/ahincho/nova-java-mapper-utils")
        credentials {
            username = providers.gradleProperty("gpr.user").orNull ?: System.getenv("GITHUB_ACTOR")
            password = providers.gradleProperty("gpr.key").orNull ?: System.getenv("GITHUB_TOKEN")
        }
    }
}

dependencies {
    implementation("pe.edu.nova.java.libs:nova-mapper-utils:0.1.0-SNAPSHOT")
}
```

## Use

The common case needs no configuration:

```java
import pe.edu.nova.java.libs.mapper.utils.MapperEngine;

CustomerDto dto = MapperEngine.map(customer, CustomerDto.class);
List<CustomerDto> dtos = MapperEngine.mapList(customers, CustomerDto.class);
```

When names differ or a field should not travel, describe it once:

```java
MappingConfig config = MappingConfig.builder()
    .field("createdAt", "created")
    .exclude("passwordHash")
    .nullStrategy(NullStrategy.SKIP)
    .build();

CustomerDto dto = MapperEngine.map(customer, CustomerDto.class, config);
```

`config.reverse()` gives you the mapping back the other way without
declaring it twice.

## Errors

`FieldNotFoundException`, `TypeConversionException`, `NullFieldException`,
`ObjectInstantiationException` and `MappingConfigException`, all under
`MappingException`.

## Requirements

Java 25.

## License

Eclipse Public License 2.0 — see [LICENSE](LICENSE).

Copyright © 2026 Angel Hincho.
