# Java Backend Rules
// **Activation: Glob Pattern - `**/*.java`**

## Use Modern Java 25 Features

ALWAYS use records for DTOs:

```java
// ✅ Correct
public record ColorRequest(
    @Min(0) @Max(255) int red,
    @Min(0) @Max(255) int green,
    @Min(0) @Max(255) int blue
) {}

// ❌ Wrong - don't use classes for DTOs
public class ColorRequest { ... }
```

## Constructor Injection (MANDATORY)

```java
// ✅ ALWAYS use constructor injection
@Service
public class ShellyBulbService {
    private final String bulbIp;
    
    public ShellyBulbService(@Value("${shelly.bulb.ip}") String bulbIp) {
        this.bulbIp = bulbIp;
    }
}

// ❌ NEVER use field injection
@Autowired  // DON'T DO THIS
private ShellyBulbService service;
```

## HTTP Client Usage

```java
// ✅ ALWAYS use RestClient (Spring Boot 3.x+)
RestClient restClient = RestClient.builder()
    .requestFactory(new SimpleClientHttpRequestFactory())
    .build();

// ❌ NEVER use deprecated RestTemplate
RestTemplate restTemplate = new RestTemplate();  // DEPRECATED
```

## Code Style Rules

- Method length: max 20 lines
- Variable names: meaningful and descriptive
- Comments: only for non-obvious logic
- Exception handling: simple try-catch, no custom exception hierarchies

## Error Handling Strategy

```java
// ✅ Simple try-catch in service layer
public boolean updateBulbColor(int red, int green, int blue) {
    try {
        String url = buildShellyUrl(red, green, blue);
        restClient.get().uri(url).retrieve().toBodilessEntity();
        return true;
    } catch (Exception e) {
        log.error("Failed to update bulb: {}", e.getMessage());
        return false;
    }
}

// ✅ Return boolean success, don't throw to controller
// ❌ NO custom exception hierarchies
// ❌ NO @ControllerAdvice unless absolutely necessary
```

## Deprecated API Awareness

- Check Spring Boot 3.5.6 documentation before using any API
- Use @Deprecated annotation awareness in IDE
- Prefer newer alternatives explicitly mentioned in docs

## Code Review Checklist

Before committing Java code, verify:
- No deprecated APIs used
- Records used for all DTOs
- Constructor injection used
- RestClient used (not RestTemplate)
- Methods under 20 lines
- Meaningful variable names
- No unnecessary comments
- No complex exception handling
- No forbidden dependencies