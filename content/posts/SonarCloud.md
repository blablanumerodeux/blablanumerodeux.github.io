---
title: "Secrets and properties management"
date: 2019-10-17T20:53:32-04:00
draft: false
---

# SonarCloud integration

This article as a reminder on how to integrate your project into SonarCloud.

https://sonarcloud.io/projects


## Maven

In the pom.xml add:
```xml
    <properties>        <sonar.projectKey>blablanumerodeux_kayak</sonar.projectKey>
        <sonar.organization>blablanumerodeux</sonar.organization>
        <sonar.host.url>https://sonarcloud.io</sonar.host.url>
<!--        this should not be committed-->
        <sonar.login>d0d909736d1c196a530e6f3f26e62bee1a81cd7f</sonar.login>
    </properties>

```

use this maven command only if you're the only dev in the team, otherwise you may override the scan of others:
```bash
mvnw verify sonar:sonar -DskipTests -f pom.xml
```

## CircleCi 

Otherwise use circleci and set your token within an env variable called :
SONAR_TOKEN

https://www.baeldung.com/sonar-qube

https://docs.sonarqube.org/latest/analysis/gitlab-cicd/


Also SONAR_HOST_URL can also be set as a env var 

Finally, here is an example of maven command that you can use within your circleci config file 

```
mvnw source:jar javadoc:jar install -DcreateChecksum=true -e sonar:sonar
```


Also, don't forget to include your environment variables (via the context features) with your workflows on your circleci config file like such: 

```yml
  
workflows:  
  version: 2  
  
  just-build:  
    jobs:  
      - build:  
          context: SonarCloud


```


## More doc on how the sonar maven plugin works

https://stackoverflow.com/questions/14979530/why-does-the-maven-command-mvn-sonarsonar-work-without-any-plugin-configurati

https://blog.sonarsource.com/we-had-a-dream-mvn-sonarsonar/



## IntelliJ IDEA plugins

### SonarLint

Go on your SonarCloud profile and generate a new token.
Use it to connect to SonarCloud with SonarLint so that your SonarCloud connection within SonarLint will have access to all your organisations and repo.
So that you won't need to regernate 1 token per repo.

Then "Update binding".
Done.
then Ctrl-Shift-S to analyse a specific file. Also more options are available in the SonalLint Bar (at the bottom) or in the Analyze menu.



### SonarQube

Configure your SonarCloud account on the IntelliJ IDEA params.
Then Analyze menu -> Inspect code (Ctrl-Alt-Shift-I) -> inspection profile Sonar -> ok.

Done

NB: use this plugin only if you're alone on the project.
Otherwise you may override the scan of others



## Project badge 

once you sent your project to SonarCloud, on the right bottom corner you can generate your badge and paste it on your README.md file.

example of a badge: 

```md
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=dans-la-rue_homeless&metric=alert_status)](https://sonarcloud.io/dashboard?id=dans-la-rue_homeless)
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbOTQ5NjMzOTY1LC0yMDg4MjY4XX0=
-->