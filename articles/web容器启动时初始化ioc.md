-- date: 2019年8月26日	
-- tag: spring

# web容器启动时加载ioc

在普通java程序中只需要	
`ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");`

在动态web项目中，如果通过servlet/html/jsp...访问，每次都要实例化一次		
`new ClassPathXmlApplicationContext("applicationContext.xml");`

十分消耗性能。以此可以通过在web容器启动时，实例化，就可以多处使用。
## 方法一
当服务启动时（tomcat），通过监听器将SpringIOC容器初始化一次（该监听器 spring-web.jar已经提供）

所需的jar包
![](https://github.com/Chester-Chen/imgStroage/blob/master/images/web%E5%AE%B9%E5%99%A8%E5%90%AF%E5%8A%A8%E6%97%B6%E5%88%9D%E5%A7%8B%E5%8C%96ioc/%E6%89%80%E9%9C%80jar%E5%8C%85.png?raw=true)


在web.xml中
```
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:applicationContext.xml</param-value>  <!-- 直接写类路径下的ioc容器名即可 -->
	</context-param>

	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
```

## 方法二

通过约定将applicationContext.xml放在WEB-INF下，则无需配置web.xml


