# Spring Boot 全栈技能测评题（2026 架构师版）

**测评说明**：共30题，分3级（基础8题、进阶12题、架构级10题），贴合2026生产实际/面试高频，侧重坑点和实操，做完快速定位薄弱点。

**评分标准**：基础题每题3分，进阶题每题4分，架构级题每题5分；总分100分，80分合格，90分以上为精通。

---

## 一、基础级（侧重基础用法，必掌握，24分）

1. Spring Boot 自动配置的核心原理是什么？`@SpringBootApplication` 注解包含哪三个核心注解？（3分）

2. Spring Boot 中 `application.yml` 和 `application.properties` 配置文件的优先级关系？如何实现多环境配置切换？（3分）

3. Spring Boot 3.x 默认使用的 Web 服务器是什么？如何切换为 Tomcat / Jetty / Undertow？（3分）

4. Spring Boot 中 `@RestControllerAdvice` 的作用是什么？如何实现全局异常处理？（3分）

5. Spring Boot 默认使用的数据源是什么？如何切换为 Druid / HikariCP 并配置连接池参数？（3分）

6. Spring Boot 中如何自定义 `RedisTemplate` 的序列化方式？为什么要避免使用默认 JDK 序列化？（3分）

7. Spring Boot 项目中，`@Value` 注解和 `@ConfigurationProperties` 注解的区别及适用场景？（3分）

8. Spring Boot 如何实现热部署（开发时修改代码无需重启服务）？核心依赖是什么？（3分）

---

## 二、进阶级（侧重生产坑点，高频面试，48分）

1. Spring Boot 2.x 迁移到 3.x 的核心兼容性问题有哪些？（如 JDK 版本、包名变化、废弃 API 等）（4分）

2. Spring Boot 中 `@Async` 注解实现异步调用的核心原理？为什么异步方法不能与调用方法在同一个类中？（4分）

3. Spring Boot 项目中，全局异常处理中如何统一返回 JSON 格式？如何处理特定异常（如 `NullPointerException`、`AccessDeniedException`）？（4分）

4. Spring Boot 整合 MyBatis-Plus 时，如何实现多数据源配置？跨数据源事务如何保证一致性？（4分）

5. Spring Boot 中 `Lettuce` 连接池高并发下出现线程爆炸/连接泄露的原因？如何优化 `Lettuce` 连接池参数（`maxTotal`、`maxIdle` 等）？（4分）

6. Spring Boot 3.x 虚拟线程（Virtual Thread）的配置方式？使用虚拟线程时需要注意哪些线程安全问题（如 `ThreadLocal`）？（4分）

7. Spring Boot 中如何实现自定义 Starter 组件？核心步骤是什么？如何避免 Starter 与项目现有配置冲突？（4分）

8. Spring Boot 项目打包为 Jar 包后，如何运行？如何指定配置文件（如 `--spring.profiles.active=prod`）？如何调整 JVM 参数？（4分）

9. Spring Boot 中 `@Cacheable`、`@CachePut`、`@CacheEvict` 注解的区别及使用场景？如何实现缓存一致性（如延迟双删）？（4分）

10. Spring Boot 整合 Spring Security 6.x 时，核心配置类是什么？如何实现 JWT 认证授权？（4分）

11. Spring Boot 项目中，如何排查依赖冲突（如 `JarConflictException`）？如何排除多余依赖？（4分）

12. Spring Boot 中 `@Configuration` 注解的两种模式（`proxyBeanMethods = true/false`）的区别及适用场景？（4分）

---

## 三、架构级（侧重架构选型，企业实战，50分）

1. 设计一套 Spring Boot 高并发系统的架构方案（含 Web 层、服务层、数据层、缓存层），说明核心技术选型理由及避坑点？（5分）

2. Spring Boot 微服务架构中，如何使用 Nacos 实现服务注册发现与配置中心？配置中心动态刷新（`@RefreshScope`）的原理是什么？（5分）

3. Spring Boot 项目如何整合 Sentinel 实现限流熔断？限流规则持久化方案有哪些？如何保证限流规则在服务重启后不丢失？（5分）

4. Spring Boot 3.x 与 GraalVM 整合实现原生镜像（Native Image）打包的核心步骤？打包时需要注意哪些限制（如动态代理、反射）？（5分）

5. Spring Boot 微服务可观测性体系如何搭建？（含 Actuator 监控指标、Prometheus 采集、Grafana 可视化、链路追踪 SkyWalking）（5分）

6. Spring Boot 项目中，如何处理大文件上传/下载？如何实现分片上传、断点续传？（5分）

7. 设计 Spring Boot 自定义 Starter 组件的最佳实践？（含自动配置类、条件注解、配置属性绑定、依赖排除）（5分）

8. Spring Boot 微服务部署时，如何使用 Docker Compose 一键启动（含数据库、Redis、Nacos、服务本身）？核心配置文件如何编写？（5分）

9. Spring Boot 项目性能优化的核心方向？（含 JVM 调优、虚拟线程、缓存优化、数据库连接池优化、依赖精简）（5分）

10. Spring Boot 与云原生技术（如 Kubernetes）的整合要点？如何实现服务的滚动更新、自愈、扩容缩容？（5分）

---

## 四、薄弱点对应表（做完快速定位）

|错题集中模块|薄弱点|对应技能方向|
|---|---|---|
|基础级1-4题|自动配置、基础配置、异常处理|自动配置与定制化、Web核心配置|
|基础级5-8题|数据访问、Redis、配置注入、热部署|数据访问、Spring Boot基础|
|进阶级1-6题|版本迁移、异步、虚拟线程、多数据源|版本差异、高级特性、性能优化|
|进阶级7-12题|Starter、打包部署、缓存、安全|自定义组件、部署、安全|
|架构级1-5题|高并发架构、微服务治理、可观测性|微服务架构、云原生|
|架构级6-10题|文件处理、Starter最佳实践、部署、性能|企业级实战、性能优化|
---

## 五、测评结果解读

1. **80分以下**：基础不扎实，需重点复习自动配置、基础配置、核心注解，建议先掌握 Spring Boot 基础用法和生产常见坑点。

2. **80-90分**：具备扎实的 Spring Boot 全栈能力，需进一步提升架构设计能力和高级特性（如虚拟线程、GraalVM、微服务治理）的实战经验。

3. **90分以上**：精通 Spring Boot 全栈技术，具备架构师级别设计能力，可应对复杂企业级项目的开发、优化和架构设计挑战。
> （注：文档部分内容可能由 AI 生成）