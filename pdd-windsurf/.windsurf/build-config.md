# Build & Configuration Guide
// **Activation: Manual (reference with @build-config)**

## Maven Configuration

```xml
<!-- ✅ ALWAYS use latest Spring Boot parent -->
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>3.5.6</version>
</parent>

<!-- ✅ Minimal dependencies only -->
<dependencies>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
  </dependency>
</dependencies>
```

## NO Additional Dependencies

NEVER ADD:
- Lombok (use records instead)
- MapStruct (manual mapping is fine)
- Apache Commons (use Java standard library)
- Guava (use Java standard library)

## Application Properties

```properties
# Server Configuration
server.port=8080

# Shelly Bulb Configuration
shelly.bulb.ip=192.168.1.42  # Update with actual IP
shelly.bulb.timeout=5000

# CORS Configuration
cors.allowed.origins=http://localhost:8080
```

## File Generation Order

When creating project from scratch:

1. `pom.xml` - Dependencies first
2. `application.properties` - Configuration
3. Model classes (`ColorRequest.java`, `ColorResponse.java`)
4. `ShellyBulbService.java` - Core logic
5. `ColorController.java` - REST endpoint
6. `WebConfig.java` - CORS configuration
7. `VibeCodingApplication.java` - Main class
8. `index.html` - UI structure
9. `app.js` - Client logic
10. `styles.css` - Styling
11. Test scripts
12. `README.md` - Documentation

## Environment Management

- Prefer `application.properties` over environment variables
- Only use env vars if explicitly needed for deployment
- Document all configuration in README

## Security Considerations

```yaml
CORS: Configured for localhost only
Input_Validation: Use @Min/@Max annotations
Network: Local network only (no internet exposure)
API_Keys: None required (local device control)
```

**Note:** This is a demo app for local network use only. Do not expose to internet.