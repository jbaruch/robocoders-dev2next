# Testing & Scripts Rules

// **Activation: Glob Pattern - `**/*.sh, **/test/**`**

## Bash Script Patterns (MANDATORY)

All bash scripts MUST follow these patterns:

```bash
#!/bin/bash

# Use colored output
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
NC='\033[0m' # No Color

# Use emojis for visual feedback
echo "🔍 Discovering devices..."
echo "✅ Success!"
echo "❌ Failed!"

# Proper error handling
if [ $? -ne 0 ]; then
    echo -e "${RED}❌ Command failed${NC}"
    exit 1
fi
```

## Integration Testing Requirements

Test script must verify:
- All endpoints accessible
- Visual color tests (red, green, blue, white)
- Error handling with invalid inputs
- Frontend accessibility

Use `curl` for HTTP requests:
```bash
curl -X POST http://localhost:8080/api/color \
  -H "Content-Type: application/json" \
  -d '{"red":255,"green":0,"blue":0}'
```

## Shelly API Testing

Use `http` command (HTTPie) for testing:
```bash
# Test red color
http GET "http://$BULB_IP/light/0?turn=on&red=255&green=0&blue=0&white=0&gain=100"

# Test device info
http GET "http://$BULB_IP/shelly"

# Test status
http GET "http://$BULB_IP/status"
```

Test requirements:
- All color channels independently
- Brightness control
- On/off states
- Device info and status checks

## Demo Readiness Checklist

Before considering app complete, verify:

```yaml
✅ Single command startup: mvn spring-boot:run
✅ Webcam dropdown populates automatically
✅ Video stream displays without manual permission prompts
✅ Color preview updates in real-time
✅ Manual mode send button works
✅ Auto mode sends every 3 seconds
✅ Bulb actually changes color (test with real device)
✅ UI visible from 20+ feet away
✅ No crashes on camera switch
✅ Error messages are clear and helpful
✅ Code is explainable in < 5 minutes
```

## Common Issues & Troubleshooting

**Camera not working:**
- Check browser permissions
- Ensure HTTPS or localhost
- Try different camera from dropdown

**Build failures:**
- Verify Java 25: `java -version`
- Verify Maven 3.9+: `mvn -version`
- Clean build: `mvn clean install`