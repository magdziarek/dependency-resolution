# Dependency Resolution

Demostrates how under specific dependency setup both Snyk and Maven Dependency Plugin fail to report the dependencies that will be part of the final WAR file.

## Explanation

This library is a multi-module project `dependency-resolution`, composed by 2 modules:
* `resolution-error`: contains the final WAR project where the dependency is added to the final artefact but not detected by Snyk or Maven Dependency Plugin.
* `trasitive-dependency`: it's necessary to transitively include the dependency so that it's ignored but others but not the Maven WAR plugin. 

## Steps to Reproduce

1. Got to the root of the project and run:
```shell
mvn clean install
```
2. Run Snyk, it will report no vulnerability (even though a vulnerable version of `snakeyaml-1.30.jar` is included in the final Jar).
```shell
snyk test --file=resolution-error/pom.xml
```
3. Run Maven Dependency plugin to check the runtime dependencies and filter for the expected dependency, it's not present:
```shell
mvn dependency:tree -Dscope=runtime -pl resolution-error | grep snakeyaml
```
4. List the dependencies included in the final WAR and filter by the expected dependency, it's present:
```shell
ls resolution-error/target/resolution-error-1.0-SNAPSHOT/WEB-INF/lib/ | grep snakeyaml
```
