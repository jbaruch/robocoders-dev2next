# Shelly Bulb Integration Rules
// **Activation: Model Decision - "Apply when working with Shelly bulb API integration, color control, or RGBW lighting"**

## ONLY Use This Endpoint (CRITICAL)

```
GET http://{bulb-ip}/light/0?turn=on&red={r}&green={g}&blue={b}&white={w}&gain=100
```

## White Channel Calculation (MANDATORY)

```java
// ✅ ALWAYS calculate white as minimum RGB
int white = Math.min(Math.min(red, green), blue);
```

This ensures proper color balance on RGBW bulbs.

## DO NOT USE These Endpoints

❌ `/white/0` - Not supported in this demo
❌ `/color/0` - Use `/light/0` instead
❌ Any advanced features (effects, transitions, etc.)

## Request Parameters

```yaml
turn: "on" | "off"
red: 0-255
green: 0-255
blue: 0-255
white: 0-255 (ALWAYS calculated as min(r,g,b))
gain: 100 (always use 100 for demo)
```

## Example Implementation

```java
private String buildShellyUrl(int red, int green, int blue) {
    int white = Math.min(Math.min(red, green), blue);
    return String.format(
        "http://%s/light/0?turn=on&red=%d&green=%d&blue=%d&white=%d&gain=100",
        bulbIp, red, green, blue, white
    );
}
```

## Configuration

In `application.properties`:
```properties
# Shelly Bulb Configuration
shelly.bulb.ip=192.168.1.42  # Update with actual IP
shelly.bulb.timeout=5000
```

## Troubleshooting

**Bulb not responding:**
- Verify bulb IP in application.properties
- Test with `scripts/test-shelly-api.sh`
- Check network connectivity
- Use `scripts/discover-shelly-devices.sh` to find bulb

**Wrong colors displayed:**
- Ensure white channel is calculated as min(r,g,b)
- Verify gain is set to 100
- Check that /light/0 endpoint is used (not /color/0)