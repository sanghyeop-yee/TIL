# Spring 에서 자주 사용하는 Annotation

> Thu Aug 18, 2022

---

[toc]



## Annotation 이란?

> JAVA에서 `Annotation` 이라는 기능이 있습니다. 사전상으로는 주석의 의미이지만 Java 에서는 주석 이상의 기능을 가지고 있습니다. Annotation은 자바 소스 코드에 추가하여 사용할 수 있는 메타데이터의 일종입니다. 소스코드에 추가하면 단순 주석의 기능을 하는 것이 아니라 특별한 기능을 사용할 수 있습니다.



## 2. Spring의 대표적인 Annotation과 역할



### Spring Bean 이란?

>  우리가 알던 기존의 Java Programming 에서는 Class를 생성하고 new를 입력하여 원하는 객체를 직접 생성한 후에 사용했었습니다. 하지만 Spring에서는 직접 new를 이용하여 생성한 객체가 아니라, Spring에 의하여 관리당하는 자바 객체를 사용합니다. 이렇게 **Spring에 의하여 생성되고 관리되는 자바 객체를 Bean**이라고 합니다.



```java
// HelloController.java
@Controller
public class HelloController{
  // Http GET method 의 /hello 경로로 요청이 들어올 때 처리할 Method 를 @GetMapping 어노테이션을 사용하여 Mapping 을 사용할 수 있다.
	@GetMapping("hello")
	public String hello(Model model){
		model.addAttribute("data", "This is data!");
		return "hello";
	}
}
```



### @Component



### @ComponentScan



### @Bean



### @Controller

"스프링! 이 클래스가 Controller 의 역할을 할거야"

```java
@Controller // 이 class 는 Controller 역할을 할거야!
@RequestMapping("/news") // 이 Class 는 /news 로 들어오는 요청을 모두 처리할거야!
public class NewsController{
  @RequestMapping(method=RequestMethod.GET)
  public String getNews(Model model){
    // GET method, /news 요청을 처리해줘!
  }
}
```





### @RequestHeader

"Request 의 header 값을 가져와줘"

```java
@Controller
@RequestMapping("/user")
public class MemberController{
  @RequestMapping(method = RequestMethod.GET)
  public String getUser(@RequestHeader(value="Accept-Language")String acceptLanguage){
    // GET method, /user 요청을 처리해줘!
  }
}
```





### @RequestMapping

"요청 들어온 URI 요청과 Annotation value 값이 일치하면 해당 클래스나 메소드가 실행됩니다."

```java
@Controller
@RequestMapping("/user")
public class UserController{
  @RequestMapping(method=RequestMethod.GET)
  public String getUser(Model model){
    // GET method, /user 요청을 처리
  }
  @RequestMapping(method=RequestMethod.POST)
  public String addUser(Model model){
    // POST method, /user 요청을 처리
  }
  @RequestMapping(value="/info", method=RequestMethod.GET)
  public String addUser(Model model){
    // GET method, /user 요청을 처리 \
  }
}
```



### @RequestParam

```java
@Controller                   // 이 Class는 Controller 역할을 합니다
@RequestMapping("/user")      // 이 Class는 /user로 들어오는 요청을 모두 처리합니다.
public class UserController {
    @RequestMapping(method = RequestMethod.GET)
    public String getUser(@RequestParam String nickname, @RequestParam(name="old") String age {
        // GET method, /user 요청을 처리
        // https://naver.com?nickname=dog&old=10
        String sub = nickname + "_" + age;
        ...
    }
}
```





### @RequestBody



### @ModelAttribute



### @ResponseBody

@ResponseBody은 메소드에서 리턴되는 값이 View 로 출력되지 않고 HTTP Response Body에 직접 쓰여지게 됩니다. return 시에 json, xml과 같은 데이터를 return 합니다.

```java
@Controller                   // 이 Class는 Controller 역할을 합니다
@RequestMapping("/user")      // 이 Class는 /user로 들어오는 요청을 모두 처리합니다.
public class UserController {
    @RequestMapping(method = RequestMethod.GET)
    @ResponseBody
    public String getUser(@RequestParam String nickname, @RequestParam(name="old") String age {
        // GET method, /user 요청을 처리
        // https://naver.com?nickname=dog&old=10
        User user = new User();
        user.setName(nickname);
        user.setAge(age);
        return user;
    }
}
```





### @Autowired



### @GetMapping

RequestMapping(Method=RequestMethod.GET)과 똑같은 역할

```java
@Controller                   // 이 Class는 Controller 역할을 합니다
@RequestMapping("/user")      // 이 Class는 /user로 들어오는 요청을 모두 처리합니다.
public class UserController {
    @GetMapping("/")
    public String getUser(Model model) {
        //  GET method, /user 요청을 처리
    }
    
    ////////////////////////////////////
    // 위와 아래 메소드는 동일하게 동작합니다. //
    ////////////////////////////////////

    @RequestMapping(method = RequestMethod.GET)
    public String getUser(Model model) {
        //  GET method, /user 요청을 처리
    }
}
```





### @PostMapping

RequestMapping(Method=RequestMethod.POST)과 똑같은 역할

```java
@Controller                   // 이 Class는 Controller 역할을 합니다
@RequestMapping("/user")      // 이 Class는 /user로 들어오는 요청을 모두 처리합니다.
public class UserController {
    @RequestMapping(method = RequestMethod.POST)
    public String addUser(Model model) {
        //  POST method, /user 요청을 처리
    }

    ////////////////////////////////////
    // 위와 아래 메소드는 동일하게 동작합니다. //
    ////////////////////////////////////

    @PostMapping('/')
    public String addUser(Model model) {
        //  POST method, /user 요청을 처리
    }
}
```





### @SpringBootTest



### @Test



## 3. Lombok의 대표적인 Annotation과 역할

Lombok은 코드를 크게 줄여주어 가독성을 크게 높힐 수 있는 라이브러리입니다. 대표적인 Annotation은 아래와 같습니다.

### @Setter

Class 모든 필드의 Setter method를 생성해줍니다.

### @Getter

Class 모든 필드의 Getter method를 생성해줍니다.

### @AllArgsConstructor

Class 모든 필드 값을 파라미터로 받는 생성자를 추가합니다.

### @NoArgsConstructor

Class 기본 생성자를 자동으로 추가해줍니다.

### @ToString

Class 모든 필드의 toString method를 생성한다.

