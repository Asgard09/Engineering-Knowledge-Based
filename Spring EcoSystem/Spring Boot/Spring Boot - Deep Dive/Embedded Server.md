# 1. Definitions
An embedded server is a web server like Tomcat, Jetty that run inside Spring Boot, it will eliminate the complexity of deploying application to external server.
# 2. Why it matters?
# 2.1. The world before
- Need many steps for deploying in the external server: Install Tomcat, Build war file, Restart Tomcat
- Environments in consistency
- Complex setup
## 2.2. How it solve well ?
Spring Boot package application, Tomcat, dependencies,  into a single executable JAR. When the application starts, Spring Boot automatically starts the server, and listening for the HTTP request.
