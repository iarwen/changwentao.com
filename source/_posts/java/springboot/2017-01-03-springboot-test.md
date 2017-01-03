title: SpringBoot的单元测试
date: 2017/01/03 15:05:25
categories:
- SpringBoot
tags: [SpringBoot,单元测试]
---
项目正式使用SpringBoot,之前也只是自己边玩边学习，封装了一些单元测试的底层类以及一些常用的技巧
# 引入依赖
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <scope>test</scope>
</dependency>
```
# Service层测试的基类
```
@RunWith(SpringJUnit4ClassRunner.class)
@SpringBootTest(classes = Application.class)
@Transactional
@Rollback
public class BasicServiceTest {
    public Logger logger = Logger.getLogger(this.getClass());

    public BasicServiceTest(){

    }
}
```
注解介绍：
@RunWith：单元测试的执行类，主要提供Spring运行时的环境
@SpringBootTest:执行SpringBoot的入口类
@Transactional：配置事务，在单元测试用控制事务
@Rollback：回滚方式执行单元测试


# Controller层单元测试基类
```
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT, classes = Application.class)
@Transactional
@Rollback
abstract public class BasicWebTest   {
	protected MockMvc mvc;

	protected List<Object> controllers = new ArrayList<>();

	@Autowired
	protected TestRestTemplate restTemplate;

	public void setUp() {
		controllers.add(new Context());
		setMvc(MockMvcBuilders.standaloneSetup(controllers).build());
	}

	public MockMvc getMvc() {
		return mvc;
	}

	public void setMvc(MockMvc mvc) {
		this.mvc = mvc;
	}

	public TestRestTemplate getRestTemplate() {
		return restTemplate;
	}

	public void setRestTemplate(TestRestTemplate restTemplate) {
		this.restTemplate = restTemplate;
	}

}
```

子类继承BasicWebTest并在@Before的方法中给controllers添加需要测试的controller,然后调用super.setUp()方法
例：
```

public class ExampleWebTest extends BasicWebTest {

	@Before
	public void setUp() {
		// 添加需要测试的Controller
		controllers.add(new MainController());
		super.setUp();
	}

	@Test
	public void exampleTest() throws Exception {
		// 1.get请求URL参数
		/*
		 * @RequestMapping("/api/v1/context/{id}")
		 *
		 * @ResponseBody Map<String, Object> context(@RequestParam String
		 * param,@PathVariable String id) { Map<String, Object> map = new
		 * HashMap<>(); map.put("context", "/api/v1/context"); map.put("param",
		 * param); return map; }
		 */
		String uri = "/api/v1/context/{id}?param=1234";

		Map<String, String> param = new HashMap<String, String>();
		param.put("id", "123");

		ResponseEntity<String> exchange = restTemplate.getForEntity(uri, String.class, param);
		Assert.assertEquals(exchange.getStatusCode(), HttpStatus.NOT_FOUND);
		System.out.println(exchange.getBody());

		// 2.post提交对象
		/*
		 * @RequestMapping("/api/v1/context1")
		 *
		 * @ResponseBody Map<String, Object> context1(@RequestBody User user) {
		 * Map<String, Object> map = new HashMap<>(); map.put("context",
		 * "/api/v1/context"); return map; }
		 */
		String uri_post = "/api/v1/context1";
		HttpHeaders headers = new HttpHeaders();
		MediaType type = MediaType.parseMediaType("application/json; charset=UTF-8");
		headers.setContentType(type);
		headers.add("Accept", MediaType.APPLICATION_JSON.toString());

		Map<String, String> params = new HashMap<>();
		params.put("id", "1");
		params.put("name", "username");

		JSONObject jsonObj = new JSONObject(params);

		HttpEntity<String> formEntity = new HttpEntity<String>(jsonObj.toString(), headers);

		String result = restTemplate.postForObject(uri_post, formEntity, String.class);
		System.out.println(result);
	}
}
```
