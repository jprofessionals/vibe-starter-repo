# Kotlin + Ktor Documentation

## Project Setup

### Gradle Configuration
```kotlin
// build.gradle.kts
plugins {
    kotlin("jvm") version "1.9.0"
    kotlin("plugin.serialization") version "1.9.0"
    id("io.ktor.plugin") version "2.3.0"
}

dependencies {
    implementation("io.ktor:ktor-server-core")
    implementation("io.ktor:ktor-server-netty")
    implementation("io.ktor:ktor-server-content-negotiation")
    implementation("io.ktor:ktor-serialization-kotlinx-json")
    implementation("io.ktor:ktor-server-auth")
    implementation("io.ktor:ktor-server-auth-jwt")
    
    testImplementation("io.ktor:ktor-server-tests")
    testImplementation("org.jetbrains.kotlin:kotlin-test-junit")
}
```

### Application Configuration
```kotlin
// Application.kt
fun main() {
    embeddedServer(Netty, port = 8080, host = "0.0.0.0") {
        configureSerialization()
        configureRouting()
        configureAuthentication()
    }.start(wait = true)
}
```

## Routing Patterns

### Basic Routing
```kotlin
// Routing.kt
fun Application.configureRouting() {
    routing {
        route("/api") {
            get("/health") {
                call.respond(HttpStatusCode.OK, mapOf("status" to "healthy"))
            }
            
            route("/users") {
                get {
                    // Get all users
                    val users = userService.getAllUsers()
                    call.respond(users)
                }
                
                get("/{id}") {
                    val id = call.parameters["id"]?.toIntOrNull()
                    if (id == null) {
                        call.respond(HttpStatusCode.BadRequest, "Invalid ID")
                        return@get
                    }
                    
                    val user = userService.getUserById(id)
                    if (user == null) {
                        call.respond(HttpStatusCode.NotFound, "User not found")
                        return@get
                    }
                    
                    call.respond(user)
                }
                
                post {
                    val user = call.receive<User>()
                    val createdUser = userService.createUser(user)
                    call.respond(HttpStatusCode.Created, createdUser)
                }
            }
        }
    }
}
```

### Authentication & Authorization
```kotlin
// Authentication.kt
fun Application.configureAuthentication() {
    install(Authentication) {
        jwt("auth-jwt") {
            realm = "Access to the '/api' path"
            verifier(JWTConfig.verifier)
            validate { credential ->
                val payload = credential.payload
                val email = payload.getClaim("email", String::class)
                if (email != null) {
                    UserIdPrincipal(email)
                } else {
                    null
                }
            }
        }
    }
}

// Protected routes
authenticate("auth-jwt") {
    get("/api/protected") {
        val principal = call.principal<UserIdPrincipal>()
        call.respond("Hello, ${principal?.name}!")
    }
}
```

## Dependency Injection

### Service Layer
```kotlin
// UserService.kt
interface UserService {
    suspend fun getAllUsers(): List<User>
    suspend fun getUserById(id: Int): User?
    suspend fun createUser(user: User): User
}

class UserServiceImpl : UserService {
    override suspend fun getAllUsers(): List<User> {
        // Implementation
        return emptyList()
    }
    
    override suspend fun getUserById(id: Int): User? {
        // Implementation
        return null
    }
    
    override suspend fun createUser(user: User): User {
        // Implementation
        return user
    }
}
```

### Dependency Injection Setup
```kotlin
// DIModule.kt
val diModule = module {
    single<UserService> { UserServiceImpl() }
    single<UserRepository> { UserRepositoryImpl() }
}

// Application.kt
fun Application.configureDI() {
    install(Koin) {
        modules(diModule)
    }
}
```

## Serialization

### Data Classes
```kotlin
// Models.kt
@Serializable
data class User(
    val id: Int,
    val name: String,
    val email: String,
    val createdAt: String
)

@Serializable
data class CreateUserRequest(
    val name: String,
    val email: String
)
```

### Content Negotiation
```kotlin
// Serialization.kt
fun Application.configureSerialization() {
    install(ContentNegotiation) {
        json(Json {
            prettyPrint = true
            isLenient = true
            ignoreUnknownKeys = true
        })
    }
}
```

## Testing

### Application Tests
```kotlin
// ApplicationTest.kt
class ApplicationTest {
    @Test
    fun testRoot() = testApplication {
        application {
            configureRouting()
        }
        
        client.get("/api/health").apply {
            assertEquals(HttpStatusCode.OK, status)
            assertEquals("""{"status":"healthy"}""", bodyAsText())
        }
    }
}
```

### Service Tests
```kotlin
// UserServiceTest.kt
class UserServiceTest {
    private lateinit var userService: UserService
    
    @Before
    fun setup() {
        userService = UserServiceImpl()
    }
    
    @Test
    fun `should return empty list when no users exist`() = runTest {
        val users = userService.getAllUsers()
        assertTrue(users.isEmpty())
    }
}
```

## Error Handling

### Exception Handling
```kotlin
// ExceptionHandling.kt
fun Application.configureExceptionHandling() {
    install(StatusPages) {
        exception<IllegalArgumentException> { call, cause ->
            call.respond(HttpStatusCode.BadRequest, mapOf("error" to cause.message))
        }
        
        exception<NotFoundException> { call, cause ->
            call.respond(HttpStatusCode.NotFound, mapOf("error" to cause.message))
        }
        
        exception<Exception> { call, cause ->
            call.respond(HttpStatusCode.InternalServerError, mapOf("error" to "Internal server error"))
        }
    }
}
```

## Best Practices

### Code Organization
- Keep handlers thin; push business logic to services
- Use dependency injection for better testability
- Implement proper error handling with custom exceptions
- Use suspend functions for asynchronous operations
- Validate request data before processing
- Implement proper logging without sensitive information

### Security
- Use HTTPS in production
- Implement proper authentication and authorization
- Validate and sanitize all inputs
- Use environment variables for sensitive configuration
- Implement rate limiting for public endpoints

### Performance
- Use connection pooling for database connections
- Implement caching where appropriate
- Use async/await for I/O operations
- Monitor application metrics

## References
- [Ktor Documentation](https://ktor.io/docs/)
- [Kotlin Documentation](https://kotlinlang.org/docs/home.html)
- [Ktor Testing Guide](https://ktor.io/docs/testing.html)
- [Kotlin Coroutines Guide](https://kotlinlang.org/docs/coroutines-guide.html)

