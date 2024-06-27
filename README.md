# tmp




```
@Setter
@Getter
public class User {
    private String name;
    private int age;
    private List<Address> addresses;


    public static void main(String[] args) {
        String json = "[{\"name\":\"Alice\",\"age\":28,\"addresses\":[{\"street\":\"123 Elm St\"},{\"street\":\"456 Oak St\",\"city\":\"Anywhere\"}]},{\"name\":\"Bob\",\"age\":25}]";

        ObjectMapper mapper = new ObjectMapper();
        mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);

        try {
            List<User> users = mapper.readValue(json, TypeFactory.defaultInstance().constructCollectionType(List.class, User.class));

            for (User user : users) {
                System.out.println("Name: " + user.getName());
                System.out.println("Age: " + user.getAge());
                if(user.getAddresses() == null) continue;
                for (Address address : user.getAddresses()) {
                    System.out.println("Address: " + address.getStreet() + ", " + address.getCity());
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

@Setter
@Getter
class Address {
    private String street;
    private String city;
}

```

```
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class RouteForwardingController {

    @RequestMapping(value = "/**/{path:[^.]*}")
    public String redirect() {
        // Forward to home page so that route is preserved.
        return "forward:/index.html";
    }
}
```


```
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ViewControllerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class MvcConfig implements WebMvcConfigurer {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("forward:/index.html");
    }
}
```

```
<error-page>
    <error-code>404</error-code>
    <location>/index.html</location>
 </error-page>

```
