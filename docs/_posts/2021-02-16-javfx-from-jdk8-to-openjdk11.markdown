---
layout: post
title:  "JavaFX: from jdk8 to openjdk11!"
date:   2021-02-16 22:52:49 +0800
categories: jekyll update
---
As Oracle [published][javafx-oracle] the libs for JavaFX have been casted out the JDK to be open source to [OpenJFX][javafx-openjfx]: 
Therefore when it is time to update you working project to a newer java version, your project build needs to undergo some light adaptations.

update your Java version
{% highlight bash %}
export PATH=$JAVA11_HOME/bin;$PATH
{% endhighlight %}

update your maven compiler configuration
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

in Java 8 you may have used the plugin com.zenjava:org.openjfx, after many years of good and loyal service for Java 11+ it is superseded by [org.openjfx:javafx-maven-plugin][javafx-maven-plugin]
So you can update your maven pom.xml to
{% highlight xml %}
<plugin>
	<groupId>org.openjfx</groupId>
	<artifactId>javafx-maven-plugin</artifactId>
	<version>0.0.5</version>
	<configuration>
		<release>11</release>
		<mainClass>fr.kdefombelle.xmlcompare.gui.XmlCompareGuiMain</mainClass>
	</configuration>
</plugin>
{% endhighlight %}


Note my Java class is as follows:
![Java FX](({{ site.url }}/assets/2021-02-16-javafx-main.png){:class="img-responsive"}

{% highlight java %}
public class XmlCompareGuiMain extends Application {

    //~ ----------------------------------------------------------------------------------------------------------------
    //~ Methods 
    //~ ----------------------------------------------------------------------------------------------------------------

    public static void main(String[] args) {
        launch(XmlCompareGuiMain.class, (java.lang.String[]) null);
    }
	
{% endhighlight %}

Check out the [openjfx doc][javafx-maven] for more info on how to get the most out of maven build in Java 11+ but examplifying a working configuration is always good as the evil is in the details.

[javafx-oracle]: <https://www.oracle.com/fr/java/technologies/javase/javafx-overview.html>
[javafx-openjfx]: <https://openjfx.io/>
[javafx-maven]:   <https://openjfx.io/openjfx-docs/#maven>
[javafx-maven-plugin]: <https://github.com/openjfx/javafx-maven-plugin>
