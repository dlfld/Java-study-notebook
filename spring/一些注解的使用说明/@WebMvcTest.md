# @WebMvcTest

  MockMvc mockMvcl;

```text
对模块进行集成测试时，希望能够通过输入URL对Controller进行测试，如果通过启动服务器，建立http client进行测试，这样会使得测试变得很麻烦，比如，启动速度慢，测试验证不方便，依赖网络环境等，所以为了可以对Controller进行测试，我们引入了MockMVC。
MockMvc实现了对Http请求的模拟，能够直接使用网络的形式，转换到Controller的调用，这样可以使得测试速度快、不依赖网络环境，而且提供了一套验证的工具，这样可以使得请求的验证统一而且很方便。
```

```java
//在Springboot的test内
@RunWith(SpringRunner.class)
@WebMvcTest(TestController.class)
class MvcMockTestApplicationTests {
    @Autowired
    MockMvc mockMvcl;

    @Test
    void contextLoads() throws Exception {
        mockMvcl.perform(
                //构造一个post请求
                MockMvcRequestBuilders.get("/test").param("value","这是value")
        ).andDo(print());
    }

}


```

controller

```java
@GetMapping("/test")
public String test(@RequestParam  String value){
    System.out.println(value+"进来了controller");
    return value;
}
```