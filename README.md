# Simple Maven WAR Project

This is a minimal Maven WAR project with a single servlet (`HelloServlet`).

## Structure
- `pom.xml`: Maven build configuration
- `src/main/java/HelloServlet.java`: Simple servlet
- `src/main/webapp/WEB-INF/web.xml`: Servlet mapping

## Build
To build the WAR file, run:

```
mvn clean package
```

## Deploy
Deploy the generated WAR file (`target/simple-war-1.0-SNAPSHOT.war`) to a servlet container like Tomcat.

## Usage
Access the servlet at: `http://localhost:8080/simple-war-1.0-SNAPSHOT/hello`
