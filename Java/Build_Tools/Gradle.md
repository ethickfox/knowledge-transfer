Gradle allows a bom to be used to manage a project’s versions by declaring it as a `platform` or `enforcedPlatform` dependency. A `platform` dependency treats the versions in the bom as recommendations and other versions and constraints in the dependency graph may cause a version of a dependency other than that declared in the bom to be used. An `enforcedPlatform` dependency treats the versions in the bom as requirements and they will override any other version found in the dependency graph.

### **Dependency Management**

The DSL allows you to declare dependency management using a `:` separated string to configure the coordinates of the managed dependency, as shown in the following example:

```Groovy
dependencyManagement {
    dependencies {
        dependency 'org.springframework:spring-core:6.0.10'
    }
}
```

With either syntax, this configuration will cause all dependencies (direct or transitive) on `spring-core` to have the version `6.0.10`. When dependency management is in place, you can declare a dependency without a version, as shown in the following example:

```Groovy
dependencies {
    implementation 'org.springframework:spring-core'
}
```

To set specific version that differs from the bom:

```Java
// use the latest undertow version which is not affected by CVE-2022-1319 and not the version from the Spring Boot BOM
ext['undertow.version'] = "${springUndertowVersion}"
```