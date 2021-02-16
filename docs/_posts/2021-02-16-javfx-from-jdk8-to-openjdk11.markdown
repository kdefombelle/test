---
layout: post
title:  "JavaFX: from jdk8 to openjdk11!"
date:   2021-02-16 22:52:49 +0800
categories: javafx
---
As Oracle [published][javafx-oracle] the libs for JavaFX have been casted out of the JDK to be open sourced to [OpenJFX][javafx-openjfx]
Therefore when it is time to update you working project to a newer java version, your project build needs to undergo some light adaptations.

### Update your Java version
{% highlight bash %}
export PATH=$JAVA11_HOME/bin;$PATH
{% endhighlight %}

### Update your maven compiler configuration
{% highlight xml %}
<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-compiler-plugin</artifactId>
	<version>3.8.0</version>
	<configuration>
		<release>11</release>
	</configuration>
</plugin>
{% endhighlight %}

### Update maven javafx plugin
in Java 8 you may have used the plugin com.zenjava:org.openjfx, after many years of good and loyal service for [Java 11+][openjdk] it is superseded by [org.openjfx:javafx-maven-plugin][javafx-maven-plugin]
So you can update your maven pom.xml to
{% highlight xml %}
<plugin>
	<groupId>org.openjfx</groupId>
	<artifactId>javafx-maven-plugin</artifactId>
	<version>0.0.5</version>
	<configuration>
		<release>11</release>
		<mainClass>com.company.MyClassMain</mainClass>
	</configuration>
</plugin>
{% endhighlight %}

### Update maven dependencies
As the JavFX libs are no longer included in the JDK it should be referenced now as dependencies
```xml
<dependency>
	<groupId>org.openjfx</groupId>
	<artifactId>javafx-controls</artifactId>
	<version>15.0.1</version>
</dependency>
<!-- the below only if you use fxml -->
<dependency>
	<groupId>org.openjfx</groupId>
	<artifactId>javafx-fxml</artifactId>
	<version>15.0.1</version>
</dependency>
```

### Run in IDE (eclipse)
Add the following VM arguments
```
--module-path "path/to/openjfx-11.0.2_windows-x64_bin-sdk/javafx-sdk-11.0.2/lib" --add-modules javafx.controls,javafx.fxml
```

### Run in command line
To compile or build simply run mvn compile or mvn install, the org.openjfx:javafx-maven-plugin will add for you the necessary libraries.
To execute the Java FX app from the command line
```bash
mvn javafx:run
```

Note my Java class is as follows:
{% highlight java %}
public class MyClassMain extends Application {

    //~ ----------------------------------------------------------------------------------------------------------------
    //~ Methods 
    //~ ----------------------------------------------------------------------------------------------------------------

    public static void main(String[] args) {
        launch(MyClassMain.class, (java.lang.String[]) null);
    }
	
{% endhighlight %}

### Increase your knowledge
Check out the [openjfx doc][javafx-maven] for more info on how to get the most out of maven build in Java 11+ but examplifying a working configuration is always good as the evil is in the details.

[javafx-oracle]: <https://www.oracle.com/fr/java/technologies/javase/javafx-overview.html>
[javafx-openjfx]: <https://openjfx.io/>
[javafx-maven]:   <https://openjfx.io/openjfx-docs/#maven>
[javafx-maven-plugin]: <https://github.com/openjfx/javafx-maven-plugin>
[openjdk]: <https://openjdk.java.net/projects/jdk/11/>
