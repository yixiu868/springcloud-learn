# Config配置中心

## 启动问题

* [Spring Cloud配置中心遇到No spring.config.import set问题](https://blog.csdn.net/lwd18175239125/article/details/115611470)

  在client端新建一个bootstrap.properties(bootstrap.yml)文件

  ```properties
  server.port=8004
  spring.profiles.active=dev
  spring.application.name=config-client
  
  spring.cloud.config.discovery.enabled=true
  spring.cloud.config.discovery.service-id=config-server
  spring.config.import=optional:configserver:http://localhost:8003
  spring.cloud.config.name=${spring.application.name}
  spring.cloud.config.profile=${spring.profiles.active}
  ```

  评论

  ```properties
  **用户ToDoMyBest6534**:我也遇到这个问题，我看了官网，原来springcloud2020 版本 把Bootstrap被默认禁用，
  同时spring.config.import加入了对解密的支持。对于Config Client、Consul、Vault和Zookeeper的配置导入，
  如果你需要使用原来的配置引导功能，
  那么需要将org.springframework.cloud:spring-cloud-starter-bootstrap依赖引入到工程中
  ```

  

