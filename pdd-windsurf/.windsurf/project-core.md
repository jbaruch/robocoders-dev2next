# Vibecoding RGBW - Core Project Rules
// **Activation: Always On**

## Project Overview
This is a **demo-focused** Spring Boot + Vanilla JS web app that controls Shelly RGBW smart bulbs using webcam color detection. 
**Simplicity is paramount** - designed for a 1-hour live conference demo.

## Critical Design Principles

### Radical Simplicity - NO Complex Infrastructure

NEVER ADD:
- Docker containers or docker-compose
- Databases (PostgreSQL, H2, MongoDB, etc.)
- Redis or any caching layer
- Message queues (Kafka, RabbitMQ, etc.)
- WebSocket connections
- Circuit breakers (Resilience4j)
- Prometheus metrics or Actuator endpoints
- Authentication/Authorization
- Frontend frameworks (React, Vue, Angular)
- Build tools (Webpack, Vite, etc.)
- CSS preprocessors (Sass, Less)

### Demo-First Mentality

ALWAYS:
- Must run with single command: `mvn spring-boot:run`
- Zero configuration for end user
- Large, projection-friendly UI (minimum 18px fonts, 24px+ buttons)
- High contrast colors for visibility from distance
- No crashes allowed - graceful degradation everywhere

## Technology Stack (STRICT)

**Backend:**
- Java version: 25 (LTS, released September 2025)
- Spring Boot: 3.5.6 (latest stable)
- Build tool: Maven 3.9+
- HTTP client: RestClient (NOT RestTemplate - deprecated)
- Validation: spring-boot-starter-validation

**Frontend:**
- HTML: HTML5 (no templating engines)
- JavaScript: Vanilla ES2024 (no frameworks)
- CSS: Modern CSS with Grid/Flexbox (no preprocessors)
- Color detection: Color Thief 2.4.0 via CDN

## Mandatory Project Structure

```
vibecoding-demo/
├── pom.xml
├── src/
│   ├── main/
│   │   ├── java/com/demo/vibecoding/
│   │   │   ├── VibeCodingApplication.java
│   │   │   ├── controller/ColorController.java
│   │   │   ├── service/ShellyBulbService.java
│   │   │   ├── model/ColorRequest.java
│   │   │   ├── model/ColorResponse.java
│   │   │   └── config/WebConfig.java
│   │   └── resources/
│   │       ├── application.properties
│   │       └── static/
│   │           ├── index.html
│   │           ├── app.js
│   │           └── styles.css
│   └── test/java/com/demo/vibecoding/
├── scripts/
│   ├── discover-shelly-devices.sh
│   ├── test-shelly-api.sh
│   └── test-integration.sh
└── README.md
```

## Explicit Exclusions

**Backend - FORBIDDEN:**
- WebSocket connections
- Circuit breakers
- Connection pooling configuration
- Actuator endpoints
- Prometheus metrics
- Redis caching
- Any database
- Docker
- MQTT protocol
- Authentication/Authorization
- Rate limiting
- Complex logging frameworks (use SLF4J default only)

**Frontend - FORBIDDEN:**
- WebSocket client
- K-means clustering for color
- OffscreenCanvas
- Web Workers
- HSL/HSV conversions
- Kelvin temperature calculations
- Complex color science
- MediaStream constraints configuration beyond basics

## Common Pitfalls to Avoid

NEVER:
- Use @EnableWebMvc (breaks Spring Boot auto-configuration)
- Use RestTemplate (deprecated, use RestClient)
- Use field injection with @Autowired (use constructor injection)
- Add "helpful" features beyond requirements
- Forget white channel calculation: min(r,g,b)
- Use /color/0 endpoint (use /light/0 instead)
- Overthink camera selection (basic dropdown is fine)
- Add premature optimizations (code clarity > performance)