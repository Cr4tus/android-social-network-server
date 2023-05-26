# Android Social Network Server

## What does the app do?
Once the server is running, it will respond to the clients' (repo: android-social-network-client) requests

## Programming Languages:
- Kotlin

## Frameworks & Libraries:
- Ktor Server & Its Plugins - https://ktor.io
    - Core (engine, etc...) - https://api.ktor.io/ktor-server/ktor-server-core/index.html
    - Authentication (for JWT) - https://ktor.io/docs/authentication.html
    - Gson (for serialization / deserialization) - https://ktor.io/docs/serialization.html
    - Websockets (for messaging) - https://ktor.io/docs/websocket.html
    - Logback (logging) - https://ktor.io/docs/logging.html

- Kmongo - https://litote.org/kmongo/
- Koin - https://insert-koin.io
- Kotlin Reflection - https://kotlinlang.org/docs/reflection.html

## Implementation:
### Authorization & Authentication:
The authentication is done using JWTs, and there is a special route where a client can make a request to validate his JWT (this is for the user to be able to use the app without entering his credentials every time). Also the http configuration can be modified to strict the access as much as possible to the server:
```kotlin
import io.ktor.application.*
import io.ktor.features.*
import io.ktor.http.*

fun Application.configureHTTP() {
    install(CORS) {
        method(HttpMethod.Post)
        method(HttpMethod.Options)
        method(HttpMethod.Put)
        method(HttpMethod.Get)
        method(HttpMethod.Delete)
        method(HttpMethod.Patch)
        header(HttpHeaders.Authorization)
        header("SocialNetwork")
//        allowCredentials = true
        anyHost() // do not do this in production!
    }

    install(DefaultHeaders) {
        header("X-Engine", "Ktor")
    }
}
```
### File Architecture
- data (entities, DTOs like requests and responses)
- domain (repos & services)
- extensions
- plugins
    - http
    - modules
    - monitoring
    - routing
    - security
    - serialization
    - sockets
- routes
- utils