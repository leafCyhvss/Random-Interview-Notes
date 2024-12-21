
## 怎么用actuator 监控
**actuator + micrometer + prometheus + Grafana** 

通过 Spring Actuator，可以轻松地访问应用的多种运行时指标、健康状况、信息等。然而，Spring Actuator 本身并不直接提供特定于 HTTP 状态码的统计信息
**Micrometer 为各种监控系统提供统一的接口，方便开发者不改变代码的情况下，将数据发送到不同的监控系统**
Prometheus 是一个开源的系统监控和警报工具箱，它使用时间序列数据来记录和处理来自被监控应用的指标。Prometheus 存储收集到的所有监控数据为时间序列格式，每个数据点由时间戳和键值对组成。

Grafana config and visualize the data from prometheus.




Spring Actuator 是 Spring Boot 的一个子项目，它为应用添加了多种生产级的服务。通过 Spring Actuator，可以轻松地访问应用的多种运行时指标、健康状况、信息等。然而，Spring Actuator 本身并不直接提供特定于 HTTP 状态码的统计信息。它提供了基础的度量（metrics）和健康检查（health checks）功能，但关于详细的 HTTP 状态码统计，可能需要额外的配置或工具来实现。

### 使用 Spring Actuator 收集基础 Metrics
Spring Actuator 通过 `/actuator/metrics` 端点提供基础的度量信息，如果你使用的是 Micrometer（Spring Boot 2.x 推荐的度量库），你可以配置额外的仪表板来跟踪特定指标，如 HTTP 请求的状态码。

1. **引入依赖**：
   确保在你的 `pom.xml` 或 `build.gradle` 中加入了 Spring Boot Actuator 和 Micrometer 的依赖。

   ```xml
   <!-- pom.xml -->
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-actuator</artifactId>
   </dependency>
   <dependency>
       <groupId>io.micrometer</groupId>
       <artifactId>micrometer-core</artifactId>
   </dependency>
   ```

2. **配置 Actuator**：
   在你的 `application.properties` 或 `application.yml` 中启用所需的端点：

```properties
   management.endpoints.web.exposure.include=health,info,metrics
```

   你可以通过访问 `/actuator/metrics/http.server.requests` 来看到 HTTP 请求的度量，包括状态码。

### 监控 HTTP 状态码
要更详细地监控每个 HTTP 状态码，你可以使用 Micrometer 与外部监控系统（如 Prometheus）结合，然后在 Grafana 上创建仪表板来可视化这些数据。

1. **集成 Prometheus**：
   添加 Prometheus 的 Micrometer 支持。

   ```xml
   <dependency>
       <groupId>io.micrometer</groupId>
       <artifactId>micrometer-registry-prometheus</artifactId>
   </dependency>
   ```

2. **配置 Prometheus 抓取**：
   配置 Prometheus 服务器抓取 Spring Boot 应用暴露的 `/actuator/prometheus` 端点。

3. **在 Grafana 上创建仪表板**：
   使用从 Prometheus 收集的数据在 Grafana 上创建仪表板，监控不同的 HTTP 状态码。

### 日志收集与 Splunk
关于如何在 Splunk 和 Grafana 上查看日志和监控指标：
- **日志收集**：配置应用的日志框架（如 Logback 或 Log4j2）将日志直接发送到 Splunk，或者使用 Splunk 的日志收集工具（如 Splunk Universal Forwarder）。
- **Splunk**：在 Splunk 中配置搜索和仪表板来分析和可视化日志数据。
- **Grafana**：配置 Grafana 与 Prometheus 的集成，以展示如 QPS、HTTP 状态码、CPU、内存等指标。

通过这些步骤，你可以有效地利用 Spring Actuator 和相关工具来监控和分析你的 Spring Boot 应用的运行状况。


## Prometheus
### Prometheus 是什么？
**Prometheus** 是一个开源系统监控和警报工具包，常用于收集和存储在时间序列数据库中的指标数据。它由 SoundCloud 创造，现在是一部分云原生计算基金会（CNCF）项目，和 Kubernetes 以及其他工具一起协同工作非常有效。Prometheus 特别适用于处理多维数据，如通过标签收集的指标。它的强大查询语言（PromQL）允许用户编写详细的查询来检视他们的实时数据，或者进行历史数据分析。

### Prometheus 的读音
Prometheus 的读音是 /prəˈmiːθiəs/ ，其中“Prom”读作 [prəm] 类似于 "prom" in "prominent"；“the”读作 [θi]；结尾的“us”读作 [əs]。

### 结合 Spring Actuator、Micrometer 和 Prometheus 使用
是的，你可以回答说你们使用 **Actuator**、**Micrometer** 加上 **Prometheus** 来监控应用：

1. **Spring Actuator** 提供了实时的应用监控能力，可以暴露多种Web端点来查看应用状态和指标。
2. **Micrometer** 作为度量收集的工具，充当应用和外部监控系统（如 Prometheus）之间的桥梁。它可以生成多种监控系统支持的度量标准。
3. **Prometheus** 则用于抓取这些度量数据，存储并通过其强大的查询语言提供实时监控数据的可视化支持。

这种组合提供了一套完整的监控解决方案，可以精确地观察和响应应用的运行状况，从性能指标到业务流程等都可以监控。使用 Prometheus 的好处还包括它的高可用性和扩展性，以及它可以与 Grafana 这样的工具无缝集成来创建动态的、视觉化的仪表板。这使得开发和运维团队能够实时地观察应用表现，并据此做出快速决策。