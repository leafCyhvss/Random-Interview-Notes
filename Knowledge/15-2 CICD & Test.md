## CI/CD

### Concept

**Continuous integration**: The automated building and testing of your application on every new commit.

**Continuous deployment:** The automation of building, testing, and deploying the application. If all tests pass, every new commit will push new code through the entire development pipeline to production with no manual intervention.
   
### Grafana - monitor your application? 
Like QPS, network, CPU usage, Memory Usage, APIs error rates (https://grafana.com/)

Grafana is an open-source platform for analytics and monitoring. You can visualize metrics from several data sources like Prometheus, InfluxDB, Elasticsearch, etc., into real-time graphs, gauges and alerts. You could use Grafana to plot system-level metrics like CPU usage, memory usage as well as application-specific metrics like API error rates or QPS.

### cicd中的环境

每个公司都不一样！
#### 开发环境 DEV
用途：开发人员进行代码编写、单元测试和初步的功能测试。
测试：单元测试（Unit Tests），可能的话，一些基本的集成测试。
负责人：开发人员。
#### 测试环境 Test
也被称作 QA (Quality Assurance) 环境或者 Test 环境。

用途：专门用于测试团队执行更详尽的测试，包括功能测试、集成测试、性能测试和安全性测试。
测试：集成测试（Integration Tests），功能测试（Functional Tests），可能包括自动化的端到端测试。
负责人：QA工程师或者自动化测试工程师。
#### 预生产环境 Staging/stage
也被称作 Stage 或 Pre-Prod 环境。

用途：一个与生产环境几乎相同的设置，用来进行最后的预发布测试。
测试：预生产测试，验收测试（UAT，User Acceptance Testing），性能测试和安全性测试。也可以functional test
负责人：QA工程师，安全工程师，以及产品所有者（进行UAT）。
#### 生产环境 Prod
用途：运行实际应用程序供最终用户使用的环境。
测试：**生产监控，性能监控，实时错误跟踪**。**不进行传统意义上的测试**，但可能会进行**A/B测试或金丝雀发布（Canary Releases）**。(canary 重读在第二个音节)
负责人：运维工程师，SRE（Site Reliability Engineers），产品经理。

## Test
### Tools
#### Sonarqube -  code quality
SonarQube: Our team uses SonarQube extensively for continuous inspection of our code quality. It helps us detect bugs, vulnerabilities, and code smells in our Java code. We have integrated SonarQube with our CI/CD pipeline so it's part of every build and provides instant feedback on the quality of our code.

JUnit and Mockito: For unit testing, we use the JUnit framework along with Mockito for mocking dependencies. Unit tests help us catch issues early and provide a safety net for future changes.

#### Splunk
Splunk is a software platform that is used for **collecting** **analyzing** and **visualizing** machine-generated data like log files.
**Machine Data (log) → Splunk(Translator) → Structured/Searchable Data**。
可以使用splunk从分布式系统中获取并汇总log

#### distributed logs：
不同services产生的日志，例如：user下一个order，Order service写日志，payment service记录日志，shiping service 记录日志，这些日志被叫做分布式日志。可以用log4j写日志,然后用logstash collect logs, 然后可以用aws Elasticsearch: Log and event data analysis, Data exploration and analytics and Application performance monitoring

#### tracing Analysis: 
因为不同微服务是不同团队或者人开发的，追踪这个order,payment 还有shipping的日志是不是一个人的，避免出现下了订单，给了钱，但是没发货的情况，可以设置一个coRelationId,每个服务共享，用来查日志，每一个独立的订单可以共享一个traceId用于tracing服务



### Test 种类
功能测试、单元测试和集成测试是软件测试过程中不同级别的测试类型，它们各自关注软件不同的方面和层次。下面是它们的主要区别：

#### 单元测试 (Unit Testing)
- **目的**：验证代码的最小可测试部分（通常是方法或函数）是否按预期工作。
- **粒度**：最小粒度，关注单个组件。
- **隔离性**：单元测试通常在隔离环境中进行，依赖项可以被模拟或存根。
- **工具**：JUnit, NUnit, TestNG, xUnit, Mockito, Moq 等。

#### 集成测试 (Integration Testing)
- **目的**：确保多个组件或系统的不同部分能够一起正常工作。
- **粒度**：中等粒度，测试的是组件或系统间的交互。
- **隔离性**：较少隔离，因为需要实际的组件交互。
- **工具**：Spring Test, TestNG, Postman（对于API集成），JUnit with Spring Boot for Integration Testing, RestAssured 等。

#### 功能测试 (Functional Testing)
- **目的**：检查软件的特定功能是否符合要求规格。
- **粒度**：可以是任何级别，但通常是高级别的，测试完整的功能或应用程序。
- **隔离性**：通常不在隔离环境中进行，因为它涉及到应用程序的完整功能。
- **工具**：**Selenium： 模拟用户网页行为**, QTP, JBehave, Cucumber（对于BDD），**Postman（对于API**），SoapUI（对于Web服务）等。

#### 使用工具的区别
- **单元测试工具** 如 JUnit 和 Mockito，主要用于小规模的测试，可以模拟依赖项，快速运行，并且通常由开发人员在编码过程中使用。
- **集成测试工具** 如 Spring Test，有时与单元测试工具结合使用，但更侧重于组件间的接口和交互。它们可能需要数据库或其他服务的实际实例。
- **功能测试工具** 如 Selenium 或 Cucumber，用于模拟用户操作，检查用户界面和工作流，它们通常由专门的QA团队在开发的后期阶段使用，并且可以执行在生产环境下的真实场景。

在现代软件开发实践中，这些测试类型通常是连续集成和持续部署（CI/CD）流水线的一部分，以确保软件质量和快速迭代。


## Postman 测试

### 发生位置
可以在dev环境/test环境/stage
感觉可以说test环境。属于functional test 毕竟是测试API


### 创建 Postman 测试

1. **新建请求**:
   - 打开 Postman。
   - 创建一个新的请求或打开一个已有的请求。
   - 设置正确的 HTTP 方法和 URL。

2. **添加请求头部和参数**:
   - 如果需要，添加请求头部（Headers）和请求体（Body）。

3. **编写测试脚本**:
在请求的 **Tests** 标签中，编写 **JavaScript** 代码以断言响应的各个方面。例如，检查状态码、响应时间或响应体中的特定值。

在 Postman 测试脚本中，`pm` 是一个全局对象，提供了一组丰富的 API 和方法，用于访问请求、响应、环境变量、数据变量、全局变量和其他多种Postman测试框架的功能。


- `pm.response`: 这个属性提供了对HTTP响应的访问，允许你检查状态代码、数据、头部、时间等。
- `pm.expect()`: 这是一个断言库的调用，它基于Chai断言库，使你能够对比期望和实际结果。
- `pm.environment.set()`, `pm.environment.get()`: 这些方法允许你设置和获取环境变量的值，非常有用于存储和重用在多个请求中需要的数据，如认证令牌等。
- `pm.response.text()).to.include`

   
![[Pasted image 20231105142317.png]]

   ```javascript
   pm.test("Status code is 200", function () {
       pm.response.to.have.status(200);
       // 创建一个测试，用来检查HTTP响应的状态码是否为200。
   });

   pm.test("Response time is less than 500ms", function () {
       pm.expect(pm.response.responseTime).to.be.below(500);
       // 使用expect断言来验证响应时间是否少于500毫秒。
   });

   pm.test("Body matches string", function () {
       pm.expect(pm.response.text()).to.include("expected string");
       // 检查响应体中是否包含特定的字符串。
   });
   ```

4. **运行测试**:
   - 发送请求并查看测试结果。

### 获取 JWT

当需要验证时，通常会有一个认证端点，你可以向这个端点发送登录凭证（如用户名和密码），以获取 JWT。例如：

1. **创建认证请求**:
   - 创建一个新请求，指向认证端点。
   - 选择 POST 方法。
   - 在请求体中，添加你的登录凭证。

2. **发送请求获取 JWT**:
   - 发送请求。
   - 如果凭证正确，响应体中通常会包含 JWT。

### 设置 Token 过期时间

Token 的过期时间通常在服务器端生成 Token 时设置。例如，在使用 JWT 时，可以在 Token 的 `exp` (expire) 声明中指定过期时间。如果你负责服务器端代码，你可以在创建 JWT 时设置这个值。

如果你使用的是 Postman，一旦你获得了 JWT，你可以将其保存到环境变量中，并在后续请求的头部（Authorization）中使用它：

1. **保存 JWT 到环境变量**:
   - 获取到 JWT 后，在 "Tests" 脚本中添加代码将其保存为环境变量：

   ```javascript
   var jsonData = pm.response.json();
   pm.environment.set("jwt_token", jsonData.token);
   ```

2. **使用 JWT 在请求中**:
   - 在后续请求的 Headers 中添加一个 `Authorization` 键，并将值设置为 `Bearer {{jwt_token}}`，这样 Postman 会使用环境变量中的 Token。

3. **设置 Token 过期时间**:
   - 这通常是服务器端的职责。作为一个测试者，你通常不会设置 Token 过期时间，但你需要确保你的测试可以处理 Token 过期的情况。

### Token过期怎么办
如果 Token 在测试过程中过期，你可能需要编写一个**预请求脚本（Pre-request Script）** 来检查 Token 是否过期，并在需要时重新获取它。这可以通过编码实现，例如：

```javascript
if (!pm.environment.get('jwt_token') || pm.environment.get('token_expiry') < Date.now()) {
    // Logic to refresh the token
    // You would typically send a request to the auth endpoint again and update the variables
}
```

确保服务器端和客户端（在这种情况下是 Postman）的系统时间同步，以避免由于时间偏差导致的验证问题。

