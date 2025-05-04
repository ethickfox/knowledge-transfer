# **Spring Security**

It provides several essential security features like [l](l), authorization, [rolebasedaccesscontrolusingsprings](rolebasedaccesscontrolusingsprings),  remembers the password, URL protection, concurrent active sessions management, etc. To enable Spring security in Java Web application, you need to configure three things -  declare a **delegating proxy filter** in web.xml, add the ContextLoaderListener in web.xml and provide actual security constraints on applicationContext-Security.xml file. Since Spring security uses a chain of filters to implement various security constraints, also known as Spring's **"security chain filter"**, it relies on web container for the initialization of delegating filter proxy.

### Salting

Salt is random data that is combined with a password before password hashing. This makes a dictionary attack more difficult. This process is known as salting. The hashed version of the password is then stored in a database along with salt.

Btw, some hashing algorithms are not suitable for password hashing, if salt is too small or predictable it's possible to recover passwords by matching random words with salt then comparing the hashed version of output with the data stored in the database.

[howtoenablesp](howtoenablesp) includes password hashing out of the box. Since version 3.1, Spring Security automatically takes care of salting too. You can use PasswordEncoder implementation to implement password hashing in Spring security. The two important implementations of the new PasswordEncoder interface are BCryptPasswordEncoder and the confusingly named StandardPasswordEncoder based on SHA-256. The BCrypt implementation is the recommended one.

### D**elegating filter proxy**

The delegating filter proxy is a generic bean that provides a link between web.xml and application-Context.xml. Spring security uses filters to implement several security related cross-cutting concerns like authentication and authorization.

Since filters need to be declared in the web.xml so that the Servlet container can invoke them before passing the request to the actual Servlet class.

The Spring Security framework uses a chain of filters to apply various security concerns like intercepting the request, detecting (absence of) authentication, redirecting to the authentication entry point, or pass the request to authorization service, and eventually let the request either hit the servlet or throw a security exception (unauthenticated or unauthorized).

![Untitled 60.png](Untitled%2060.png)

![Untitled 1 18.png](Untitled%201%2018.png)

The DelegatingFitlerProxy glues these filters together and forms the security filter chain. That's why you see the name "springSecurityFilterChain" when we declare DelegatingFilterProxy as a filter in web.xml.

Here are some of the important filters from Spring's security filter chain, in the order they occur in:

- **SecurityContextPersistenceFilter -** This filter restores Authentication from the JSESSIONID cookie.
- **UsernamePasswordAuthenticationFilter -** This filter performs authentication.
- **ExceptionTranslationFilter -** This filter catch security exceptions from FilterSecurityInterceptor.
- **FilterSecurityInterceptor -** This filter may throw authentication and authorization exceptions.

You can add or replace individual filters with your own logic in Spring's security filter chain.

## **SecurityContext**

The SecurityContext is used to store the details of the currently authenticated user, also known as a principle. So, if you have to get the username or any other user details, you need to get this SecurityContext first.

### **SecurityContextHolder**

The SecurityContextHolder is a helper class, which provides access to the security context. By default, it uses a [ThreadLocal](http://javarevisited.blogspot.com/2012/05/how-to-use-threadlocal-in-java-benefits.html#axzz54hCQprEv) object to store security context, which means that the security context is always available to methods in the same thread of execution, even if you don't pass the SecurityContext object around. Don't worry about the [ThreadLocal memory leak](http://javarevisited.blogspot.sg/2013/01/threadlocal-memory-leak-in-java-web.html#axzz57W6MT09l) in web application though, Spring Security takes care of cleaning ThreadLocal.

```Java
Object principal = SecurityContextHolder.getContext()
                                        .getAuthentication()
                                        .getPrincipal();

if (principal instanceof UserDetails) {
  String username = ((UserDetails)principal).getUsername();
} else {
  String username = principal.toString();
}
```

![Untitled 2 13.png](../../_img/Untitled%202%2013.png)

If you ever need to know current logged-in user details like, in Spring MVC controller, I suggest you declare a dependency and let Spring provide you the Principal object, rather you querying for them and create a tightly coupled system.

```Java
@Controller
public class MVCController {

  @RequestMapping(value = "/username", method = RequestMethod.GET)
  @ResponseBody
  public String currentUserName(Principal principal) {
     return principal.getName();
  }

}

// OR

@Controller
public class SpringMVCController {

  @RequestMapping(value = "/username", method = RequestMethod.GET)
  @ResponseBody
  public String currentUserName(Authentication authentication) {
     return authentication.getName();
  }
}
```

### Limiting number of concurrent sessions

You will need to include the following xml snippet in your _Spring Security Configuration file_ mostly named as applicaContext-security.xml. You can name the file whatever you want but just make sure you use the same name in all relevant places

```XML
<session-management invalid-session-url="/logout.html">
    <concurrency-control max-sessions="1" error-if-maximum-exceeded="true" />
</session-management>
```

## Authentication

Проверка подлинности путем проверки введенных имени и пароля с сохраненными в базе

## Authorization

Проверка разрешений на доступ к тому или иному ресурсу

![Untitled 3 13.png](../../_img/Untitled%203%2013.png)

![Untitled 4 10.png](Untitled%204%2010.png)

  

## **Method Security**

Simply put, Spring Security supports authorization semantics at the method level.

Typically, we could secure our service layer by, for example, restricting which roles are able to execute a particular method — and test it using dedicated method-level security test support.

- **By default, Spring AOP proxying is used to apply method security.** If a secured method A is called by another method within the same class, security in A is ignored altogether. This means method A will execute without any security checking. The same applies to private methods.
- **Spring** _**SecurityContext**_ **is thread-bound.** By default, the security context isn't propagated to child threads.

```XML
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-config</artifactId>
</dependency>
```

- The _prePostEnabled_ property enables Spring Security pre/post annotations.
- The _securedEnabled_ property determines if the _@Secured_ annotation should be enabled.
- The _jsr250Enabled_ property allows us to use the _@RoleAllowed_ annotation.

```Java
@Configuration
@EnableGlobalMethodSecurity(
  prePostEnabled = true, 
  securedEnabled = true, 
  jsr250Enabled = true)
public class MethodSecurityConfig 
  extends GlobalMethodSecurityConfiguration {
}
```

### @Secured

Annotation is used to specify a list of roles on a method. So, a user only can access that method if she has at least one of the specified roles.

```Java
@Secured({ "ROLE_VIEWER", "ROLE_EDITOR" })
public String getUsername() {
    SecurityContext securityContext = SecurityContextHolder.getContext();
    return securityContext.getAuthentication().getName();
}
```

The _@Secured_ annotation doesn't support Spring Expression Language (SpEL).

### @RolesAllowed

The _@RolesAllowed_ annotation is the JSR-250’s equivalent annotation of the _@Secured_ annotation.

```Java
@RolesAllowed("ROLE_VIEWER")
public String getUsername2() {
    //...
}
    
@RolesAllowed({ "ROLE_VIEWER", "ROLE_EDITOR" })
public boolean isValidUsername2(String username) {
    //...
}
```

### @PreAuthorize/@PostAuthorize

Both _@PreAuthorize_ and _@PostAuthorize_ annotations provide expression-based access control.

The _@PreAuthorize_ annotation checks the given expression before entering the method, whereas the _@PostAuthorize_ annotation verifies it after the execution of the method and could alter the result.

```Java
@PreAuthorize("hasRole('ROLE_VIEWER')")
public String getUsernameInUpperCase() {
    return getUsername().toUpperCase();
}
```

```Java
@Service
@PreAuthorize("hasRole('ROLE_ADMIN')")
public class SystemService {

    public String getSystemYear(){
        //...
    }
 
    public String getSystemDate(){
        //...
    }
}
```

The _@PreAuthorize(“hasRole(‘ROLE_VIEWER')”)_ has the same meaning as _@Secured(“ROLE_VIEWER”)_, which we used in the previous section.

Moreover, we can actually use the method argument as part of the expression:

```Java
@PreAuthorize("\#username == authentication.principal.username")
public String getMyRoles(String username) {
    //...
}
```

_@PostAuthorize_ annotation provides the ability to access the method result:

```Java
@PostAuthorize("returnObject.username == authentication.principal.nickName")
public CustomUser loadUserDetail(String username) {
    return userRoleRepository.loadUserByUserName(username);
}
```

**Method Security Meta-Annotation**

```Java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@PreAuthorize("hasRole('VIEWER')")
public @interface IsViewer {
}

//usage
@IsViewer
public String getUsername4() {
    //...
}
```

Security expressions:

- **_hasRole_****,** **_hasAnyRole_**
- **_hasAuthority_****,** **_hasAnyAuthority_**
    
    Roles and authorities are similar in Spring.
    
    The main difference is that roles have special semantics. Starting with Spring Security 4, the ‘_ROLE__‘ prefix is automatically added (if it's not already there) by any role-related method.
    
    So _hasAuthority(‘ROLE_ADMIN')_ is similar to _hasRole(‘ADMIN')_ because the ‘_ROLE__‘ prefix gets added automatically.
    
- **_permitAll_****,** **_denyAll_**
- **_isAnonymous_****,** **_isAuthenticated,_**
    -  **_isRememberMe  
          
        _**_Through the use of cookies, Spring enables remember-me capabilities, so there's no need to log into the system each time._
    -  **_isFullyAuthenticated  
          
        _**_If some parts of our services require the user to be authenticated again, even if the user is already logged in._
- **_principal_****,** **_authentication_**
    
    These expressions allow us to access the _principal_ object representing the current authorized (or anonymous) user and the current _Authentication_ object from the _SecurityContext_, respectively.
    
    We can, for example, use _principal_ to load a user's email, avatar, or any other data that's accessible from the logged-in user.
    
- **_hasPermission  
      
    _**This expression is intended to be a bridge between the expression system and Spring Security’s ACL system, allowing us to specify authorization constraints on individual domain objects based on abstract permissions.
    
    ```Java
    @PreAuthorize("hasPermission(\#article, 'isEditor')")
    public void acceptArticle(Article article) {
       …
    }
    ```
    
    Only the authorized user can call this method, and they need to have _isEditor_ permission in the service.
    

### _**@PreFilter/@PostFilter**_

## **Basic authentication**

RESTful web services can be authenticated in many ways, but the most basic one is basic authentication. For basic authentication, we send a username and password using the HTTP [Authorization] header to enable us to access the resource. Usernames and passwords are encoded using base64 encoding (not encryption) in Basic Authentication. The encoding is not secure since it can be easily decoded.

```Java
		@Bean
    public UserDetailsService userDetails(){
        UserDetails userDetails = User.withDefaultPasswordEncoder()
                .username("user")
                .password("qwe")
                .roles(Role.USER.name())
                .build();
        UserDetails adminDetails = User.withDefaultPasswordEncoder()
                .username("admin")
                .password("qwe")
                .roles(Role.ADMIN.name())
                .build();
        return new InMemoryUserDetailsManager(userDetails, adminDetails);
    }

@Bean
    public UserDetailsService userDetails(DataSource dataSource) {
        return new JdbcUserDetailsManager(dataSource);
    }
```

Configuration

```Java
@Configuration
@EnableWebSecurity
public class WebSecurityConfiguration {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
                .authorizeHttpRequests((auth) -> auth
                        .requestMatchers("/","/home").permitAll()
                        .requestMatchers("/flats").hasRole("USER")
                        .requestMatchers("/users").hasRole("ADMIN")
                        .anyRequest().authenticated()
                )
                .httpBasic(withDefaults());
        return http.build();
    }
```

password encoder, default - bcrypt

```Java
@Bean
  public PasswordEncoder passwordEncoder(){
      return NoOpPasswordEncoder.getInstance();
  }
```

![Untitled 5 10.png](../../_img/Untitled%205%2010.png)

## Form based auth

![Untitled 6 10.png](../../_img/Untitled%206%2010.png)

![Untitled 7 8.png](Untitled%207%208.png)

## **Digest authentication**

  

## **Session management**

As far as security is concerned, session management relates to securing and managing multiple users' sessions against their request. It facilitates secure interactions between a user and a service/application and pertains to a sequence of requests and responses associated with a particular user. Session Management is one of the most critical aspects of Spring security as if sessions are not managed properly, the security of data will suffer. To control HTTP sessions, Spring security uses the following options:

- SessionManagementFilter.
- SessionAuthneticationStrategy
- With these two, spring-security can manage the following security session options:
- Session timeouts (amount of time a user can remain inactive on a website before the site ends the session.)
- Concurrent sessions (the number of sessions that an authenticated user can have open at once).
- Session-fixation (an attack that permits an attacker to hijack a valid user session).

6. Explain SecurityContext and SecurityContext Holder in Spring sec

## OAth2

[OAuth](https://oauth.net/) is an open standard that describes a process of authorization. It can be used to authorize user access to an API. For example, a REST API can restrict access to only registered users with a proper role.

An OAuth authorization server is responsible for authenticating the users and issuing access tokens containing the user data and proper access policies.

```YAML
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-oauth2-authorization-server</artifactId>
    <version>0.2.0</version>
</dependency>
```

![Untitled 8 8.png](../../_img/Untitled%208%208.png)

![Untitled 9 8.png](../../_img/Untitled%209%208.png)

![Untitled 10 8.png](../../_img/Untitled%2010%208.png)

![Untitled 11 8.png](Untitled%2011%208.png)

### Security Server

```Java
@Configuration
@Import(OAuth2AuthorizationServerConfiguration.class)
public class AuthorizationServerConfig {
    @Bean
    public RegisteredClientRepository registeredClientRepository() {
        RegisteredClient registeredClient = RegisteredClient.withId(UUID.randomUUID().toString())
          .clientId("articles-client")
          .clientSecret("{noop}secret")
          .clientAuthenticationMethod(ClientAuthenticationMethod.CLIENT_SECRET_BASIC)
          .authorizationGrantType(AuthorizationGrantType.AUTHORIZATION_CODE)
          .authorizationGrantType(AuthorizationGrantType.REFRESH_TOKEN)
          .redirectUri("http://127.0.0.1:8080/login/oauth2/code/articles-client-oidc")
          .redirectUri("http://127.0.0.1:8080/authorized")
          .scope(OidcScopes.OPENID)
          .scope("articles.read")
          .build();
        return new InMemoryRegisteredClientRepository(registeredClient);
    }
}
```

The properties we're configuring are:

- Client ID – Spring will use it to identify which client is trying to access the resource
- Client secret code – a secret known to the client and server that provides trust between the two
- Authentication method – in our case, we'll use basic authentication, which is just a username and password
- Authorization grant type – we want to allow the client to generate both an authorization code and a refresh token
- Redirect URI – the client will use it in a redirect-based flow
- Scope – this parameter defines authorizations that the client may have. In our case, we'll have the required _OidcScopes.OPENID_ and our custom one, articles. read

Configure a bean to apply the default OAuth security and generate a default form login page:

```Java
@Bean
@Order(Ordered.HIGHEST_PRECEDENCE)
public SecurityFilterChain authServerSecurityFilterChain(HttpSecurity http) throws Exception {
    OAuth2AuthorizationServerConfiguration.applyDefaultSecurity(http);
    return http.formLogin(Customizer.withDefaults()).build();
}

@EnableWebSecurity
public class DefaultSecurityConfig {

    @Bean
    SecurityFilterChain defaultSecurityFilterChain(HttpSecurity http) throws Exception {
        http.authorizeRequests(authorizeRequests ->
          authorizeRequests.anyRequest().authenticated()
        )
          .formLogin(withDefaults());
        return http.build();
    }

    // ...
}
```

Each authorization server needs its signing key for tokens to keep a proper boundary between security domains.

```Java
@Bean
public JWKSource<SecurityContext> jwkSource() {
    RSAKey rsaKey = generateRsa();
    JWKSet jwkSet = new JWKSet(rsaKey);
    return (jwkSelector, securityContext) -> jwkSelector.select(jwkSet);
}

private static RSAKey generateRsa() {
    KeyPair keyPair = generateRsaKey();
    RSAPublicKey publicKey = (RSAPublicKey) keyPair.getPublic();
    RSAPrivateKey privateKey = (RSAPrivateKey) keyPair.getPrivate();
    return new RSAKey.Builder(publicKey)
      .privateKey(privateKey)
      .keyID(UUID.randomUUID().toString())
      .build();
}

private static KeyPair generateRsaKey() {
    KeyPairGenerator keyPairGenerator = KeyPairGenerator.getInstance("RSA");
    keyPairGenerator.initialize(2048);
    return keyPairGenerator.generateKeyPair();
}

@Bean
public ProviderSettings providerSettings() {
    return ProviderSettings.builder()
      .issuer("http://auth-server:9000")
      .build();
}
```

![Untitled 12 8.png](Untitled%2012%208.png)

  

![Untitled 13 8.png](../../_img/Untitled%2013%208.png)

**@EnableWebSecurity**

конфигурация(содержит @Configuration) для Spring Security

## **Secured object**

Объект, который имеет какие-то ограничения, по правам доступа, которые могут к нему обратиться

## **Authentication manager**

основной интерфейс стратегии для аутентификации. Он отвечает за проверку аутентификации.

## **SecurityContext**

содержит объект Authentication и в случае необходимости информацию системы безопасности, связанную с запросом от пользователя.

### **WebSecurityConfigureradapter**

класс для переопределения настроек Security. Переопрепределяется configure

### **UserBuilder**

класс для создания и настройки пользователей

### **HttpSecurity**

настройки доступа для http запросов

- antMatchers - путь
- hasRole - доступ для ролей
- jdbcAuthentication - данные о пользователях находятся в бд

# Authentication

Authentication is a process to verify that the user is the one who he claims to be. It is generally implemented using a username and password. If a user enters the correct username and password then authentication is successful, otherwise, authentication failed.

![Untitled 14 8.png](../../_img/Untitled%2014%208.png)

# Authorisation

Authorisation provides access control. For example, only the admin can see some pages in a web application. To implement that, the admin must have some admin-related permissions or roles.

![Untitled 15 7.png](../../_img/Untitled%2015%207.png)

![Untitled 16 7.png](../../_img/Untitled%2016%207.png)