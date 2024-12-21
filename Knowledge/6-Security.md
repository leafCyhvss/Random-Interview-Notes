## jwt是用来做authentication还是authorization?
JWT（JSON Web Token）通常用于身份验证（authentication）而不是授权（authorization）。

1. **Authentication（身份验证）**：JWT 主要用于验证用户或应用程序的身份。当用户登录或进行身份验证时，通常会生成一个包含用户身份信息的 JWT。该 JWT 会随后用于验证用户的请求，确保请求是由已经验证的用户发起的。

2. **Authorization（授权）**：授权通常是在用户经过身份验证后进行的操作，它确定用户是否有权限执行特定操作或访问特定资源。JWT 通常不包含有关用户授权的信息。**授权通常需要另外的机制，如访问控制列表（Access Control List）或角色/权限系统**，来确定用户是否被授予执行特定操作的权限。

JWT 的一个常见用例是生成一个包含用户身份信息（例如用户ID或用户名）的令牌，该令牌可以在用户登录后发送给客户端。客户端随后在每个请求中包含该令牌，以便服务器可以验证用户的身份并执行相应的操作。然而，**授权信息通常不包含在 JWT 中**，而是由服务器根据用户的身份和角色进行授权检查。

总之，JWT 主要用于身份验证，验证用户或应用程序的身份，而不是用于授权，确定用户是否有权限执行特定操作。授权通常需要额外的逻辑和数据来管理。

## 授权

Apply **Authorization** to APIs
```java
@PreAuthorize("hasRole('ADMIN')")
public void deleteAccount(Long accountId) {
    // ...
}
```
Then Only the user who has admin role can call this API deleteAccount

授权前要先认证
1. 用户信息会加载入Authentication
2. 对象getAuthorities()
3. AccessDecisionManager决定这次请求能不能成功



## 认证


![[Pasted image 20230721000230.png]]

Spring Security来验证他们: 
![[Pasted image 20230721000159.png]]


是的，JSON Web Token（JWT）经常在认证过程中被使用，尤其是在构建无状态的、基于REST的微服务应用程序时。在这种情况下，JWT通常用于在客户端和服务器之间安全地传递声明（claims）和其他信息。

以下是JWT在Spring Security中的基本使用步骤：

1. **用户输入用户名和密码**：这一步与上述过程类似，用户通过提供他们的凭证（例如用户名和密码）来开始认证过程。

2. **服务器验证用户凭证**：服务器接收到这些凭证后，将使用Spring Security来验证他们。这可能涉及到查找数据库，确认用户名对应的密码是否匹配等等。

3. **服务器生成JWT**：一旦用户被验证，服务器将生成一个JWT。这个JWT将包含用户的信息（称为声明）和一个签名。这个签名是由服务器生成的，用来证明JWT是由它创建的，没有被篡改过。

4. **服务器返回JWT**：然后，服务器将这个JWT发送回客户端。客户端将保存这个JWT以供后续使用。

5. **客户端使用JWT进行请求**：当客户端想要访问一个受保护的资源时，它将将这个JWT添加到它的请求头中。通常，这个JWT被放在一个名为`Authorization`的头中，前缀为`Bearer `。

6. **服务器验证JWT**：服务器收到请求后，将从请求头中提取JWT，然后验证签名和声明。如果JWT有效，那么服务器将认为这个请求是被认证的，并以JWT中的声明作为用户的身份。

这就是使用JWT进行认证的基本过程。值得注意的是，JWT一旦被创建并签名，就不能被修改。如果需要改变用户的认证状态或权限，需要生成一个新的JWT。