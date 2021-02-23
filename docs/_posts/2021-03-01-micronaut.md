---
layout: post
title:  "Micronaut"
date:   2021-02-16 22:52:49 +0800
categories: micronaut framework java maven
---
As we all know [Spring][spring] there is now a lightweigth and claimed more efficient framework called Micronaut

### Cli Installation
- install micronaut-cli using a package manager as per Micronaut documentation [here](https://micronaut-projects.github.io/micronaut-starter/latest/guide/index.html#installChocolatey). More about [Chocolatey][chocolatey] for Windows users.
- add `$MICRONAUT_CLI_HOME`/bin to PATH
- check instalation *mn --version*

### Create a Sample Application
> mn create-app hello-world --build maven --features openapi

The project is scaffolded and the main class is generated
```java
public class Application {

  public static void main(String[] args) {
    Micronaut.run(Application.class, args);
  }
}
```

You can start adding your Controller for instance to spawn a server side application
```java
@Controller("/service")
public class ServiceController {

  @Post("execute")
  @Status(HttpStatus.CREATED)
  public Single<Result> execute(@Body Request request) {
    Result result = new Result();
    result.setValue(request.getValue() * 2);
    return Single.just(result);
  }

}
```

### Activate Swagger
Reference documentation is [Micronaut Open API][micronaut-openapi]

#### Configure Maven Dependencies

In in your maven pom.xml 
- add dependencies
```xml        
<dependency>
	<groupId>javax.annotation</groupId>
	<artifactId>javax.annotation-api</artifactId>
	<scope>compile</scope>
</dependency>
<dependency>
	<groupId>io.swagger.core.v3</groupId>
	<artifactId>swagger-annotations</artifactId>
	<scope>compile</scope>
</dependency>
```

- configure annotation processor
```
<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-compiler-plugin</artifactId>
	<configuration>
		<annotationProcessorPaths combine.children="append">
			<path>
				<groupId>io.micronaut.openapi</groupId>
				<artifactId>micronaut-openapi</artifactId>
				<version>${micronaut.openapi.version}</version>
			</path>
		</annotationProcessorPaths>
		<compilerArgs>
			<arg>-Amicronaut.processing.group=fr.kdefombelle.app</arg>
			<arg>-Amicronaut.processing.module=app-service</arg>
		</compilerArgs>
	</configuration>
</plugin>
```

#### openapi.properties
Create an *openapi.properties* file in at the root of you project
```
swagger-ui.enabled=true
```

Swagger
To access swagger you can activate its 


### Run in Command Line
To compile or build simply run `mvn compile` or `mvn install`, the org.openjfx:javafx-maven-plugin will add for you the necessary libraries.
To execute the Java FX app from the command line
```bash
mvnw mn:run
```

Note my Java class is as follows:
```java
public class MyClassMain extends Application {

    public static void main(String[] args) {
        launch(MyClassMain.class, (java.lang.String[]) null);
    }
```

### Run in IDE
Add the following VM arguments
```
--module-path "path/to/openjfx-11.0.2_windows-x64_bin-sdk/javafx-sdk-11.0.2/lib" --add-modules javafx.controls,javafx.fxml
```
![eclipse configuration](/assets/images/2021-02-16-javafx-eclipse.png)


![idea configuration](/assets/images/2021-02-16-javafx-idea.png)


[spring]: <https://spring.io/>
[chocolatey]: <https://chocolatey.org/>
[micronaut-openapi]: <https://micronaut-projects.github.io/micronaut-openapi/latest/guide/index.html/>
