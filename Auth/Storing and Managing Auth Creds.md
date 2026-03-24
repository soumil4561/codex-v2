
Alright first lets setup the client sided stuff. I'll be starting with the next [docs](https://nextjs.org/docs/app/guides/authentication#authentication) here, since i myself have not much clue. This will be more of a exploratory journey per se.

While i am building client side on next, i will still follow a static route majorly. Alright based on the docs, i think they are themselves suggesting to go for Auth libs. The doc there is more for educational purpose (which is fine). Just looking at the different libs is confusing. NextJSAuth recommends Better Auth. 

## The spring side
I will come to it later. First i want to establish my base backend. Now while its perfectly fine to go with webflux, i'll be sticking to webmvc since its just less of a headache to maintain/get started. 

Created a starter application using the initializr. I'll be following the docs to set this up, step-by-step... 

### Understanding Spring Security
Spring security is a deep flow state in itself. Before even implementation, i think its best if i just go through that architecture of the [flow](https://docs.spring.io/spring-security/reference/servlet/authentication/architecture.html#servlet-authentication-securitycontextholder). Alright looks like its composed of several components:

1. `SecurityContextHolder`: Holds the security context, duh. If this is filled, it means the user is authenticated. It doesn't care how its populated. Makes sense.. 
   The holder itself uses a `ThreadLocal` to store the context. Which means that when a request comes in the auth context is available to only that thread, makes sense since each request is executed in its own thread.
2. `SecurityContext`: It contains the Authentication Object and lives in the `SecurityContextHolder`.
3. `Authentication`: Its the interface representing the current authenticated user. It also is used as the input to the auth manager for authentication itself. Contains: 
   - principal: The user. Often an instance of `UserDetails`
   - credentials: Password. Ideally its cleared once auth is complete for security
   - authority: Collection of `GrantedAuthority` instance. Roles/Scopes. 
4. `Granted Authority` High level permissions granted to the user. Ex: roles and scopes. On a high level, giving someone a role which is mapped to certain scopes is easier. But it would be interesting to see how the reverse happens, ie individual scope level permission mapping. For example you could be a ROLE_USER, but have some more extended capabilities granted by a higher authority. Now creating a special "role" for this one-off would not be great, but at the same time is something the application should be able to support.
5. `AuthenticationManager`:  The API that defines how the Filters performs the authentication. This is the manager who populates the `SecurityContextHolder` with the Authentication object. _While the implementation of `AuthenticationManager` could be anything, the most common implementation is [ProviderManager](https://docs.spring.io/spring-security/reference/servlet/authentication/architecture.html#servlet-authentication-providermanager)_.



## Authentication Mechanism

### 1. Password and Username: The Basic Auth
Pretty straight forward, enter a username and password and just walk right in. Lets see what do we need for spring. 

alright, after a defining a basic "SecurityConfig", i have defined (hardocoded) a user and password and using a form login (comes with spring security), i can login into the application. The most basic, the least scalable scenario. 

Looks like first thing to do is to tear this down, almost instantly :). The current setup (ie `withDefaultPasswordEncoder`) is actually something that stores the key hardcoded in the lib itself. Lets remove the form login and instead login via a /login route. 

Defining an authentication Manager using a new Provider manager definition. Now given that we know that a Provider manager consists of list of providers, its better to define it like that from the beginning since we know this is going to support multiples anyways, sort of like this:|

```java
@Bean  
public AuthenticationManager authenticationManager(UserDetailsService userDetailsService, PasswordEncoder passwordEncoder){  
    DaoAuthenticationProvider authenticationProvider = new DaoAuthenticationProvider(userDetailsService);  
    authenticationProvider.setPasswordEncoder(passwordEncoder);  
      
    return new ProviderManager(List.of(authenticationProvider));  
}
```

Alright since we removed the form login and have now instead just the basic auth, we can get started. Now the basic auth is actually something in itself:

>The "Basic" HTTP authentication transmits credentials as user ID/password pairs, encoded using base64.
>As the user ID and password are passed over the network as clear text (it is base64 encoded, but base64 is a reversible encoding), the basic authentication scheme is not secure. HTTPS/TLS should be used with basic authentication to prevent credential interception.
><cite>MDN Docs</cite>

So while its "works", being easily reversible is probably not a good idea for it, right. Lets work on this.


> Up until now the user lives "in-memory" because we are using an `InMemoryUserDetailsManager` . Instead of this ideally there should be a user service delegated for this particular thing. I will also be building a "user-module" which would cover all of this.


## 2. Session Management via cookie
Storing a session on server end is a popular way to handle persistence. On a successful login, set client side cookie which is the SessionID. The server would store it in memory or in some db (usually a ttl based). Then, based on the policies, the server simply manages the session state both for the client cookie as well as the redis state.

