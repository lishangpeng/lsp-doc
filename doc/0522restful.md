restful 的等级

0级：使用http传输

1级：每个资源都有一个对应的url

2级：使用http的状态码对应不同的结果

3级：使用超媒体。在资源的表达中包含了链接信息

 

```
@PageableDefault(page = 2, size = 17, sort = "username,asc")
一个分页的注解
```



```
user类
public interface UserSimpleView {};
public interface UserDetailView extends UserSimpleView {};

get方法
@JsonView(UserSimpleView.class)

controller类
@JsonView(User.UserDetailView.class)
返回不同的视图
```



```
@RunWith(SpringRunner.class)
@SpringBootTest
public class UserControllerTest {

   @Autowired
   private WebApplicationContext wac;

   private MockMvc mockMvc;

   @Before
   public void setup() {
      mockMvc = MockMvcBuilders.webAppContextSetup(wac).build();
   }
}
TDD开发
```

