# Spring Boot Security Documentation

## Core Concepts

### Security Configuration (Spring Security 6+)
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authz -> authz
                .requestMatchers("/public/**").permitAll()
                .requestMatchers("/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .csrf(csrf -> csrf.disable()) // For stateless APIs
            .cors(cors -> cors.configurationSource(corsConfigurationSource()))
            .sessionManagement(session -> session
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            );
        return http.build();
    }
}
```

### Authentication
- **UserDetailsService**: Custom user authentication logic
- **PasswordEncoder**: BCryptPasswordEncoder for password hashing
- **JWT Authentication**: Token-based authentication for stateless APIs
- **OAuth2**: Social login and third-party authentication

### Authorization
- **Method Security**: `@PreAuthorize`, `@PostAuthorize`, `@Secured`
- **Role-based Access**: `ROLE_ADMIN`, `ROLE_USER` prefixes
- **Custom Access Decision**: `AccessDecisionManager` for complex logic

### Best Practices
- Store secrets in environment variables
- Use HTTPS in production
- Implement proper error handling
- Add security headers (HSTS, CSP, X-Frame-Options)
- Enable CSRF for web applications, disable for APIs
- Configure CORS explicitly

## Key Annotations
- `@EnableWebSecurity`: Enable Spring Security
- `@PreAuthorize("hasRole('ADMIN')")`: Method-level authorization
- `@Secured("ROLE_USER")`: Simple role-based access
- `@CrossOrigin`: CORS configuration (use sparingly)

## Testing
```java
@Test
@WithMockUser(roles = "ADMIN")
public void testAdminEndpoint() {
    // Test with mock user
}

@TestConfiguration
public class TestSecurityConfig {
    // Test-specific security configuration
}
```

## References
- [Spring Security Reference](https://docs.spring.io/spring-security/reference/)
- [Spring Boot Security Guide](https://spring.io/guides/gs/securing-web/)
- [JWT with Spring Security](https://spring.io/guides/tutorials/spring-security-and-angular-js/)

