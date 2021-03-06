# Spring REST Service OAuth

[![Build Status](https://drone.io/github.com/royclarkson/spring-rest-service-oauth/status.png)](https://drone.io/github.com/royclarkson/spring-rest-service-oauth/latest)

This is a simple REST service built using [Spring Boot](http://projects.spring.io/spring-boot/), [Spring MVC](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/mvc.html), and [Spring Security OAuth](http://projects.spring.io/spring-security-oauth/) that provides a single RESTful endpoint protected by OAuth2. The REST service is based on the [Building a RESTful Web Service](https://spring.io/guides/gs/rest-service/) getting started guide. This project is built using a preview version of Java-based configuration support, available in Spring Security [OAuth 2.0.0.RC1](https://spring.io/blog/2014/04/18/spring-security-oauth-2-0-0-rc1-available). Please log any issues or feature requests to the [Spring Security OAuth project](https://github.com/spring-projects/spring-security-oauth/issues).


## Build and Run

Use Gradle:

```sh
./gradlew clean build bootRun
```

Or use Maven:

```sh
mvn clean package && java -jar target/spring-rest-service-oauth-0.1.0.jar
```

## Usage

Test the `greeting` endpoint:

```
curl http://localhost:8080/greeting
```

You receive the following JSON response, which indicates you are not authorized to access the resource:

```
{"error":"unauthorized","error_description":"Full authentication is required to access this resource"}
```

In order to access the protected resource, you must first request an access token via the OAuth handshake. Request OAuth authorization:

```sh
curl -X POST -vu clientapp:123456 http://localhost:8080/oauth/token -H "Accept: application/json" -d "password=spring&username=roy&grant_type=password&scope=read%20write&client_secret=123456&client_id=clientapp"
```

A successful authorization results in the following JSON response:

```json
{"access_token":"ff16372e-38a7-4e29-88c2-1fb92897f558","token_type":"bearer","expires_in":43199,"scope":"read write"}
```

Use the `access_token` returned in the previous request to make the authorized request to the protected endpoint:

```sh
curl http://localhost:8080/greeting -H "Authorization: Bearer ff16372e-38a7-4e29-88c2-1fb92897f558"
```

If the request is successful, you will see the following JSON response:

```json
{"id":1,"content":"Hello, World!"}
```

## Cloud Foundry Demo

The service is deployed to Cloud Foundry and available for testing. Modify the previous commands to point to the following URL:

```
curl http://rclarkson-restoauth.cfapps.io/greeting
```
