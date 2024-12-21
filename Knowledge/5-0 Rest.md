## 6种 API
1. REST（代表性状态转移）：REST是简单性和无处不在的拥护者，是一种主要利用HTTP方法的建筑风格。 它可以轻松地与资源互动，使其成为众多应用程序和现代API的首选模式。
2. SOAP（简单对象访问协议）：SOAP是API领域的重量级竞争者，因复杂性和力量而蓬勃发展。 它使用XML来定义结构化通信。 虽然需要SOAP客户端和服务器，但它以其强度和坚固性来补偿，就像一辆建造精良的越野车，可以应对崎岖的地形。
3. GraphQL：作为API宇宙中一颗冉冉升起的新星，GraphQL提供了灵活性和精度。 它允许客户确切地询问他们需要的东西，减少冗余并提高性能。 把它想象成一个私人购物者——你得到的只是你想要的东西，仅此而已。
4. gRPC（谷歌远程过程调用）：gRPC是API宇宙的加速器。 在HTTP/2上运行并使用二进制数据，这一切都与性能和速度有关，特别是对于微服务架构。 它就像一列高速列车，确保快速可靠的通信。
5. WebSockets：如果您需要实时和双向通信，WebSockets就是答案。 非常适合聊天应用程序、实时流媒体和实时数据交换，就像客户端和服务器之间有一条开放的电话线。
6. MQTT（消息队列遥测传输）：MQTT是一种轻量级信使，专为资源有限、带宽低和不可靠的网络环境而设计。 把它想象成一个决心送你的邮件的邮政工作人员，无论风雨无阻

## Rest
1. What are HTTP request methods?
   Get 
   Put
   Post
   Delete
   Patch
   
   POST方法用于创建资源，PUT方法用于完整替换资源，而PATCH方法用于部分更新资源。
1. What is the difference between Put and Post?
   Post is used to **submit data to be processed** by the resource. It is typically used for **creating new resources** or submitting updates to existing resources. While PUT is used to **update an entire existing resource** at the specified URI. The entire representation of the resource is sent in the request, and the server replaces the resource at that URI with the new representation.
   
   follow up: If you make multiple PUT requests to update a resource, the end result will be the same as if you made a single PUT request. If you make multiple POST requests to create a resource, it may result in the creation of multiple resources.
   
2. What are HTTP Status codes? and explain them.( 200, 201, 302, 400, 404, 500)
   **200 OK:** request is successful. commonly used for **Get**
   **201 Created**: request was successful, and a new resource was created as a result. typically used for **POST**
   **302 Found**: a redirect response. the requested resource **has been temporarily moved to a different URL**. And client should typically follow the provided redirect URL to access the resource.
   **400 Bad Request**:  the server cannot process the client's request due to a malformed **syntax or invalid parameters**. The server cannot understand the request due to client error.
   **404 Not Found**:  the requested resource could not be found on the server. The server cannot locate the requested resource or the URL is invalid.
   **500 Internal Server Error**: an unexpected error occurred on the server while processing the request. The server encountered an internal problem and was unable to fulfill the request.
   **502 Bad Gateway**
   使用了一个反向代理服务器（如 Nginx）来分发请求到后端的多个应用服务器。如果**其中一个应用服务器因为配置错误**（例如，**错误的端口或已经停止运行的服务**）而**无法正确响应反向代理的请求**，反向代理服务器会收到一个无效的响应。此时，反向代理服务器将向用户显示一个 HTTP 502 错误，表明它作为网关在处理请求时遇到问题
   **504 Gateway Timeout**
   模拟过程：
   用户尝试检查一个商品的实时库存。
   代理服务器发送请求到提供库存信息的远程服务。
   远程服务响应延迟，未在代理服务器设定的超时时间内回应。
   代理服务器等待超时后，返回 HTTP 504 错误给用户
   
3. Could you tell any endpoints you developed?
   GET /orders
   GET /orders/{orderId}
   DELETE /orders/{orderId}
   POST /orders/{orderId}/renew
   PUT /quotes/{quoteId}
4. Design one set of APIs for managing the customers history orders
   GET Endpoint: /customers/{customerId}/orders
   GET Endpoint: /customers/{customerId}/orders/{orderId}
   POST Endpoint: /customers/{customerId}/orders  //Creates a new order
   PUT Endpoint: /customers/{customerId}/orders/{orderId} // Updates the details of a specific order identified by {orderId} and {customerId}
   DELETE Endpoint: /customers/{customerId}/orders/{orderId}
   GET Endpoint: /customers/{customerId}/orders/search
   GET Endpoint: /customers/{customerId}/orders/search?keyword={keyword}
   GET Endpoint: /customers/{customerId}/orders?start={startDate}&end={endDate}


## soap和http的区别和优缺点
soap:
**基于 XML，SOAP 消息通常比较冗长，导致解析处理时间长**，增加了带宽的消耗，也更**安全**。支持复杂的交易处理模式，如事务协调。

http: HTTP 是无状态的协议，这意味着每次请求之间都是独立的，这有助于服务器的扩展。 **作为一个无状态协议，HTTP 在每次请求时都需要重新建立连接，这可能导致额外的延迟和开销。**


SOAP（Simple Object Access Protocol）和HTTP（Hypertext Transfer Protocol）是两种在网络通信中常用的技术，但它们各有不同的用途和特性。理解它们之间的区别以及各自的优缺点有助于在合适的场景中选择适当的技术。


### SOAP

**SOAP** 是一种**基于 XML** 的协议，用于在网络上交换**结构化**信息。它主要用于实现网络服务，支持复杂的事务处理。

**优点**：
1. **标准化**：SOAP 是一个标准化的协议，有 W3C（World Wide Web Consortium）进行维护和规范。
2. **安全性**：SOAP 支持较高的安全性，提供了完整的安全特性，如WS-Security，支持消息的加密和认证。
3. **扩展性**：支持多种传输协议，虽然常见的是 HTTP，但也可以通过 SMTP、FTP 等进行传输。
4. **事务管理**：支持复杂的交易处理模式，如事务协调。

**缺点**：
1. **复杂性**：SOAP 信息包括大量的标准和结构，使得其较为复杂，不易理解和实现。
2. **性能**：由于基于 XML，SOAP 消息通常比较冗长，导致解析处理时间长，增加了带宽的消耗。
3. **灵活性较低**：虽然SOAP是高度标准化的，但这也限制了其在非标准场景下的灵活性。

### HTTP

**HTTP** 是一种数据通信的标准协议，主要用于分布式、协作式、超媒体信息系统中的文档传递。

**优点**：
1. **简单性**：HTTP 协议简单，易于理解和实现，开发者可以轻松构建基于 HTTP 的应用。
2. **无状态性**：HTTP 是无状态的协议，这意味着每次请求之间都是独立的，这有助于服务器的扩展。
3. **灵活性**：HTTP 允许传输任何类型的数据对象，只要双方知道如何处理这些数据即可。

**缺点**：
1. **安全性**：HTTP 本身不提供加密机制，数据在传输过程中可能被截取或篡改。虽然 HTTPS 提供了加密支持，但它需要额外的配置和处理。
2. **性能问题**：**作为一个无状态协议，HTTP 在每次请求时都需要重新建立连接，这可能导致额外的延迟和开销。**

### 结论

选择 SOAP 还是 HTTP 取决于你的具体需求。如果你需要一个高度标准化、安全性较高的解决方案，并且涉及到复杂的业务逻辑，SOAP 可能是一个更好的选择。相反，如果你追求简单、灵活，并且对性能有较高要求，使用 HTTP 可能更合适。在实际应用中，SOAP 通常被用于企业内部或需要严格事务管理的系统中，而 HTTP 则是构建开放的、标准的 Web 应用的首选技术。