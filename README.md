## OpenFaas Java8 function with custom maven repository

In this sample I tried to add a custom dependency to the functions `build.gradle`

```
dependencies {
    ...
    compile project(':model')

    // ADDED CUSTOM DEPENDENCY
    compile "io.confluent:kafka-connect-avro-converter:5.0.0"
}
```

To resolve this dependency we need to add a custom maven repository: `http://packages.confluent.io/maven/`

### working

It is working if the custom maven repo is added to the Java8 templates `build.gradle`

[build.gradle](https://raw.githubusercontent.com/CrystalMethod/openfaas-java8-template-with-custom-maven-repo/master/working/template/java8/build.gradle)

```
allprojects {
    repositories {
        jcenter() 

        // ADDED CUSTOM MAVEN REPOSITORY
        maven { url "http://packages.confluent.io/maven/" }
    }
}
```

```
> cd working
> faas-cli build -f maven-repo.yml
[0] > Building maven-repo.
...
[0] worker done.
```

### nonworking

It is working if the custom maven repo is added to the Java8 templates `build.gradle`

[build.gradle](https://raw.githubusercontent.com/CrystalMethod/openfaas-java8-template-with-custom-maven-repo/master/nonworking/maven-repo/build.gradle)

```
repositories {
    jcenter()

    // ADDED CUSTOM MAVEN REPOSITORY
    maven { url "http://packages.confluent.io/maven/" }
}
```

```
> cd nonworking
> faas-cli build -f maven-repo.yml
[0] > Building maven-repo.
...
> Task :function:compileJava
> Task :entrypoint:compileJava FAILED

FAILURE: Build failed with an exception.

* What went wrong:
Could not resolve all files for configuration ':entrypoint:compileClasspath'.
> Could not find io.confluent:kafka-connect-avro-converter:5.0.0.
  Required by:
      project :entrypoint > project :function

* Try:
Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output. Run with --scan to get full insights.

* Get more help at https://help.gradle.org

BUILD FAILED in 18s
```

