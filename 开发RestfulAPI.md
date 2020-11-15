##### first controller

```shell
mkdir src/main/java/com/zeaho/spring/springbootdemo/controller
 
cat > src/main/java/com/zeaho/spring/springbootdemo/controller/HelloController.java <<EOF
package com.zeaho.spring.springbootdemo.controller;
 
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
 
/**
 * @author LuZhong
 */
@RestController
@RequestMapping("/hello")
public class HelloController {
    @GetMapping("/world")
    public String world(@RequestParam("name") String name) {
        return String.format("hello world! hello %s!", name);
    }
}
EOF
mvn spring-boot:run
curl http://127.0.0.1:8080/hello/world?name=lzzz
```



##### first service

```shell
mkdir src/main/java/com/zeaho/spring/springbootdemo/service
 
cat > src/main/java/com/zeaho/spring/springbootdemo/service/HelloService.java <<EOF
package com.zeaho.spring.springbootdemo.service;
 
import org.springframework.stereotype.Service;
 
/**
 * @author LuZhong
 */
@Service
public class HelloService {
    public String sayHello(String name) {
        return String.format("hello world! hello %s!", name);
    }
}
EOF
```



##### inject service into controller

```shell
cat > src/main/java/com/zeaho/spring/springbootdemo/service/HelloService.java <<EOF
package com.zeaho.spring.springbootdemo.controller;
 
import com.zeaho.spring.springbootdemo.service.HelloService;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
 
/**
 * @author LuZhong
 */
@RestController
@RequestMapping("/hello")
public class HelloController {
    private final HelloService helloService;
 
    public HelloController(HelloService helloService) {
        this.helloService = helloService;
    }
 
    @GetMapping("/world")
    public String world(@RequestParam("name") String name) {
        return helloService.sayHello(name);
    }
}
EOF
mvn spring-boot:run
curl http://127.0.0.1:8080/hello/world?name=service
```