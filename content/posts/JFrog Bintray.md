---
title: "Maven deploy to OSS JFrog to Maven Central"
date: 2019-11-10T20:53:32-04:00
draft: false
---
# Maven deploy to OSS JFrog to Maven Central

## JFrog BinTray


Begin by creating an organisation and a package on [JFrog BinTray](https://bintray.com/dakarinternational).   
Edit your package and request that your package be [added to JCenter](https://www.jfrog.com/confluence/display/RTF/Deploying+Snapshots+to+oss.jfrog.org)  
Don't forgot to tick the option : *Host my snapshot build artifacts on the OSS Artifactory*
Wait for your request to be granted.


Then, Edit your profile and find your API key.  

## OSS JFrog

Use that key as your password to connect in [OSS-JFrog](https://oss.jfrog.org/artifactory/webapp/#/home).  
Use your BinTray profile name as your username to connect in OSS-JFrog.


## Maven

Then in your pom.xml add this directly under `<project>`: 
```xml
	<!--this to push directly a release into bintray, does not work for snapshot though-->
<!--	<distributionManagement>
		<repository>
			<id>bintray-blablanumerodeux-dakar</id>
			<name>blablanumerodeux-dakar</name>
			<url>https://api.bintray.com/maven/dakarinternational/dakar/dakar/;publish=1</url>
		</repository>
	</distributionManagement>-->

	<!--this to push to OSS -->
	<distributionManagement>
		<repository>
			<id>central</id>
			<name>oss-jfrog-artifactory-releases</name>
			<url>https://oss.jfrog.org/artifactory/oss-release-local</url>
		</repository>
		<snapshotRepository>
			<id>snapshots</id>
			<name>oss-jfrog-artifactory-snapshots</name>
			<url>https://oss.jfrog.org/artifactory/oss-snapshot-local</url>
		</snapshotRepository>
	</distributionManagement>
```
  
please note the `<id>central</id>` and `<id>snapshots</id>`, we'll use these below.

also be sure to configure correctly this part :  
```xml
	<groupId>dans-la-rue</groupId>
	<artifactId>homeless</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>
```


Now that your project is configured, go in your settings.xml file which should be somewhere here: `~/.m2/settings.xml`   
And make sure to **add** the servers config like that:  
 ```xml
<?xml version='1.0' encoding='UTF-8'?>
<settings xsi:schemaLocation='http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd'
          xmlns='http://maven.apache.org/SETTINGS/1.0.0' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>
    <servers>
        <server>
            <id>bintray-blablanumerodeux-dakar</id>
            <username>blablanumerodeux</username>
            <password>${env.BINTRAY_API_KEY}</password>
        </server>
        <server>
            <id>central</id>
            <username>blablanumerodeux</username>
            <password>${env.JFROG_API_KEY}</password>
        </server>
        <server>
            <id>snapshots</id>
            <username>blablanumerodeux</username>
            <password>${env.JFROG_API_KEY}</password>
        </server>
    </servers>
</settings>
```

Note again the `<id>central</id>` and  `<id>snapshots</id>` that match the one above.


Then you can deploy your SNAPSHOT or RELEASE with maven: `mvn clean source:jar javadoc:jar deploy -DskipTests -DcreateChecksum=true`


If you want to publish your package into the Maven Central repo, then you need to make sure that your java source files are inside your JAR file. 
To do that you can configure your maven build with the `<resources>` tag like that: 

```xml
	<build>
		<resources>
			<resource>
				<directory>src/main/resources</directory>
			</resource>
			<resource>
				<directory>src/main/java</directory>
				<includes>
					<include>**/*.java</include>
				</includes>
			</resource>
		</resources>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-javadoc-plugin</artifactId>
				<version>3.0.1</version>
				<configuration>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-jar-plugin</artifactId>
				<version>2.3.2</version>
			</plugin>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-source-plugin</artifactId>
				<version>2.1.2</version>
			</plugin>
		</plugins>
	</build>

```

## Glossary

 - OJO: Oss.Jfrog.Org
 - OSS: 
 - BinTray: Website used to manage users, organisations, repos,packages,versions,and promot versions to JCenter and Maven Central
 -  JCenter: One of the public repo where you can host freely your open source software
 - Maven Central: The most famous public repo to host freely your open source software
 - Artifactory: 

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4MjY2NzcwOTksMjkxMDY2NjNdfQ==
-->