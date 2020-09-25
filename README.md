# Notes for Java Web Development
These are notes for specifically Java Web Development.

# Recommendations

## Quickstart 
- [Spring](https://spring.io/quickstart)

## Recommended Editors (in order)
1. IntelliJ IDEA Community Edition (Industry standard.)
2. VSCode (Free and actually really good. Probably not the best for everything. Definitely follow the helpful guide to VSCode Java development)
   - [VSCode Java Getting Started](https://code.visualstudio.com/docs/java/java-tutorial)
3. IntelliJ IDEA Ultimate (Costs moola but really good.)
4. Notepad ;)
5. Eclipse

## Recommended JDK versions 
1. JDK 11 (recommended for all new projects)
2. JDK 8 (only for legacy support)

## Recommended Frameworks
1. Spring Framework / Spring Boot
  - [Spring Framework Getting Started](https://spring.io/quickstart)
#### Others I have not tried 
2. Regular Maven
2. Quarkus
3. MicroProfile

## Recommended Deployment Methods
- helpful links
  - [Various Deployment Methods tutorial](https://docs.spring.io/spring-boot/docs/current/reference/html/deployment.html)
  - [2014 Deployment Tutorial/Getting Started](https://spring.io/blog/2014/03/07/deploying-spring-boot-applications)

1. Container (Docker)
   - Generally you should always know how to containerize your application so that you can have the best flexibility when it comes to deployment. 
2. PaaS (Heroku, Kubernetes, or Cloud Foundry)
3. Traditional Cloud Providers (AWS, Digital Ocean, Azure)

## Learning
- [cloudacademy](https://cloudacademy.com/)
- [codecademy](https://www.codecademy.com/)


# Development Getting Started
- unless otherwise noted here I will be using spring framework for the web framework.

## RESTfull service - GET
- [tutorial](https://spring.io/guides/gs/rest-service/)
## RESTfull service 2.0 - CRUD
- [tutorial](https://spring.io/guides/tutorials/bookmarks/)

## Running
- Gradle
```sh
#If you use Gradle, you can run the application by using 
./gradlew bootRun
```
```sh
#Alternatively, you can build the JAR file by using 
./gradlew build 
# and then run the JAR file, as follows:
java -jar build/libs/gs-rest-service-0.1.0.jar
```
- Maven
``` sh
# If you use Maven, you can run the application by using 
./mvnw spring-boot:run
```
``` sh
# Alternatively, you can build the JAR file with 
./mvnw clean package
# and then run the JAR file, as follows:
java -jar target/gs-rest-service-0.1.0.jar
```

## Testing
- [Apache Benchmark](http://httpd.apache.org/docs/2.0/programs/ab.html)
- [Sample Apache Benchmark](https://stackoverflow.com/questions/16519609/curl-how-to-run-a-single-curl-command-100-times?rq=1)
``` sh
# -n number of requests
# -c number of concurrent requests
# -T content type text/plain or multipart/form-data or application/json
# -p POST-file file for post request data
# -k keep alive (default is no keep alive)
# -C cookie-name=value (field is repeatable)
# -H custom header "Accept-Encoding: zip/zop;8bit" (Field is repeatable)

# make 100 test requests
ab  -T text/plain -n 100 -c 1 http://localhost:8080/greeting
# make 1000 test requests with 5 threads and adding two header lines and one cookie
ab  -T text/plain -n 1000 -c 5 -H "a:1" -H "b:2" -C "value=1" http://localhost:8080/greeting

```

## Model

``` java
import lombok.Builder;

@Builder // allows us to use the .builder()... .build() generated methods
public class Greeting {
    public final long id;
    public final String contents;
}
```
``` java
    private static final String TEMPLATE = "Hello %s!";
    private final AtomicLong counter = new AtomicLong();

    @GetMapping("/greeting")
    public Greeting greeting(@RequestParam(defaultValue = "World") final String name) {
        System.out.println(String.format("input was: %s", name));
        return Greeting.builder().id(counter.getAndIncrement()).contents(String.format(TEMPLATE, name)).build();
    }
```

## Headers
```java
    @GetMapping("/get-headers")
    public String getHeaders(@RequestHeader HttpHeaders headers) {
        headers.map().forEach((k, v) -> {
            System.out.println(String.format("Header '%s' = %s", k, v));
        });
        return "";
    }
```

## Cookies
``` java

    @GetMapping("/change-username")
    public String setCookie(HttpServletResponse response) {
        // create a cookie
        Cookie cookie = new Cookie("username", "Jovan");
        // add cookie to response
        response.addCookie(cookie);
        return "Username is changed!";

        // // create a cookie with expiration
        // cookie.setMaxAge(7 * 24 * 60 * 60); // expires in 7 days

        // // create a secure cookie (Can only be sent over a secure https connection)
        // cookie.setSecure(true);

        // // create an http only cookie (tells the browser that this cookie should only
        // be accessed by the server)
        // cookie.setHttpOnly(true);

        // // create a cookie with a specified scope (global cookie accessible every
        // where)
        // cookie.setPath("/"); // global cookie accessible every where

        // // unset/remove a cookie and set value to null (must have exact same params
        // used during set)
        // Cookie cookie = new Cookie("username", null);
        // cookie.setMaxAge(0);

        // // create a session cookie
        // cookie.setMaxAge(-1);

    }

    @GetMapping("/all-cookies")
    public String readAllCookies(HttpServletRequest request) {
        Cookie[] cookies = request.getCookies();
        if (cookies != null) {
            return Arrays.stream(cookies).map(c -> c.getName() + "=" + c.getValue()).collect(Collectors.joining(", "));
        }
        return "No cookies";
    }

    @GetMapping("/")
    public String readCookie(@CookieValue(value = "username", defaultValue = "Atta") String username) {
        return "Hey! My username is " + username;
    }
}

```

## Logging


## Metrics


## Containerizing


## Persistance


