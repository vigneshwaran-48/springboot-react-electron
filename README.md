## Maven + Springboot + React + Electron (+ OpenJDK)

This Spring-React-Electron starter template is a fork of appreciated/maven-springboot-electron. It is designed for running web applications that serve React UI files within an Electron app. The project structure includes a React application located in src/main/app, a Spring Boot backend in src/main/java, and an Electron application in src/main/electron. When the -Pproduction profile is activated, the React app is built and the generated files are moved to src/main/resources/static to be served by the Spring Boot application. This setup allows for seamless integration and deployment of modern web applications with a robust backend and desktop environment.

* Instead of Gradle only Maven is being used (of course also Node but indirectly)
* When building the Electron application an OpenJDK will be included to start the Java web application

## Proof of concept
The purpose of this project was sole of personal interest to show that this concept (Electron -> shipped JDK -> Java Web Application as Jar) is possible. Before taking it into production, you should check your requirements carefully. On one hand, the project is in its current form far from ideal. Electron wastes out of the box a lot of resources on the client-side, shipping an additional JVM and an embedded web server does not make it better (Why would I need a Browser and an OpenJDK just to use a Java Web application as a desktop application?). If it needs to be Java, why not use Swing or JavaFX? But I can imagine some use cases where this could come in handy, f.e. as an in-between solution before migrating to the cloud.  
An improvement to the concept could be done by getting rid of the OpenJDK by using native images of the GraalVM. Getting rid of the browser is currently not possible but could also be improved a lot by using [tauri](https://github.com/tauri-apps/tauri) instead of Electron. If PWAs get available for all platforms and can run applications running on localhost, the browser could eventually be dropped entirely. Currently, an updater functionality is missing, also logging needs to be done manually.

## How to build
`mvn clean install -Pproduction`

The artifacts from the electron build will be put into:
* `target\electron\application-darwin-x64`
* `target\electron\application-win32-x64`
* `target\electron\application-linux-x64`

When using macOS or Linux, `wine` is required to build `windows` (check the maven build for further information).

When using Windows, admin privileges are required to build `darwin` (check the maven build for further information).

## Shipping OpenJDK
Since not all your users have the right JVM available via classpath an OpenJDK 17 will be packed into the electron builds

## Documentation
* [Configure electron Application](https://github.com/appreciated/maven-springboot-electron/tree/master/src/main/electron)
* [Configure the electron archs Part1](https://github.com/appreciated/maven-springboot-electron/blob/master/src/main/electron/package.json)
* [Configure the electron archs Part2](https://github.com/appreciated/maven-springboot-electron/blob/master/pom.xml#L236-L257)

* [Electron build Part1](https://github.com/appreciated/maven-springboot-electron/blob/master/pom.xml#L198-L259)
* [Electron build Part2](https://github.com/appreciated/maven-springboot-electron/blob/master/pom.xml#L333-L358)
* [Configure OpenJDK download & injection](https://github.com/appreciated/maven-springboot-electron/blob/master/pom.xml#L260-L332)

## Build
When executing `mvn clean install -Pproduction` by default the `windows` (x64), `darwin` (x64) and `linux` (x64) will be built.

The rest is currently not supported but adding those shouldn't be too hard but some changes will need to be made at the following files:
* [Create 'scripts' for the other electron archs](https://github.com/appreciated/maven-springboot-electron/blob/master/src/main/electron/package.json#L14-L17)
* [Add the new 'scripts' to the maven build](https://github.com/appreciated/maven-springboot-electron/blob/master/pom.xml#L236-L257)
* [Download the correct JDK](https://github.com/appreciated/maven-springboot-electron/blob/master/pom.xml#L265-L294)
* [Unpack the Downloaded JDK](https://github.com/appreciated/maven-springboot-electron/blob/master/pom.xml#L296-L332)
* [Inject downloaded JDK to the electron build](https://github.com/appreciated/maven-springboot-electron/blob/master/pom.xml#L359-L395)
* [Use correct JDK at runtime](https://github.com/appreciated/maven-springboot-electron/blob/master/src/main/electron/main.js#L108-L139)


## Pull requests are welcome
