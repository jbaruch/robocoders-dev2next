# RGBW Control App: Webcam to Smart Bulb Color Control

## 🎯 Product Intent

Build a live demo web application that:
1. Captures webcam video in the browser
2. Extracts the dominant color from each frame
3. Sends color data to a Shelly Duo RGBW bulb over local network
4. Controls the bulb's color in real-time based on webcam input

**Use Case**: Interactive demonstration of color extraction and IoT control - wave colored objects in front of camera to change bulb color.

## 🔒 Methodology

**Follow `copilot-instructions.md` strictly - Intent Integrity Chain (IIC) methodology applies.**

You MUST work through phases: Analysis → Specification → Test Generation → Implementation, with STOP gates between each phase for user approval.

## 📚 Context7 Requirements - MANDATORY

Before ANY work, resolve and retrieve documentation for:

### Backend Libraries
- **Spring Boot** (latest) - REST endpoints, RestClient, application configuration
- **jmdns** - mDNS device discovery for local network
- **Shelly Duo** or **Shelly API** - Bulb control API documentation

### Frontend Libraries  
- **Preact** - Component library
- **htm** - JSX alternative for CDN use
- **MediaStream API** - Webcam access (if available in Context7)
- **Canvas API** - Image data extraction (if available in Context7)

### Testing Libraries
- **Spring Boot Test** - Backend testing
- **Vitest** or appropriate testing framework - Frontend testing

**If ANY Context7 lookup fails → STOP immediately and report per IIC instructions.**

## 🏗️ Technical Stack

Use the scaffolded workspace:
- **Backend**: Java (latest LTS), Spring Boot (latest 3.x)
- **Frontend**: HTML, Preact via CDN (htm/preact)
- **Build**: Maven (already configured)
- **Network**: HTTP only (no WebSockets)
- **Discovery**: mDNS via jmdns library
- **Deployment**: Local only (demo app)

## 🎨 Functional Requirements

### Backend Responsibilities

1. **Color Control Endpoint**
   - `POST /api/color`
   - Accept RGB values: `{"r": 0-255, "g": 0-255, "b": 0-255}`
   - Validate input ranges
   - Convert RGB to format required by Shelly API (consult Context7)
   - Send HTTP request to bulb using Spring RestClient
   - Return success/error status

2. **Bulb Discovery** 
   - Use jmdns library for mDNS discovery (consult Context7 for correct usage)
   - Discover Shelly Duo devices on local network
   - **CRITICAL**: Bind to actual network interface, NOT localhost (consult Context7 jmdns docs for correct binding)
   - Cache discovered bulb IP
   - Provide endpoint: `GET /api/bulb/status` returning discovery state and IP

3. **Configuration**
   - Default bulb IP in `application.yml` (fallback if discovery fails)
   - Configurable discovery timeout
   - Configurable bulb model/service type

4. **Error Handling**
   - Network errors (bulb unreachable)
   - Invalid color values
   - Discovery failures
   - Return appropriate HTTP status codes per Spring Boot best practices

### Frontend Responsibilities

1. **Webcam Capture**
   - Request camera permission via MediaStream API (consult Context7 if available)
   - Display live video feed
   - Handle permission denied gracefully

2. **Color Extraction**
   - Sample video frames using Canvas API (consult Context7 if available)
   - Extract dominant color using simple algorithm (average RGB from center region)
   - Update extraction at reasonable interval (100-200ms)

3. **UI Components**
   - Video preview element
   - Color preview (showing extracted color)
   - Bulb discovery status indicator
   - Discovered bulb IP display
   - Error messages
   - Start/Stop controls

4. **API Integration**
   - Send extracted colors to backend via `POST /api/color`
   - Throttle requests appropriately
   - Handle network errors
   - Display connection status

5. **Visual Feedback**
   - Show current extracted color
   - Show bulb connection status
   - Show discovery status (searching/found/failed)
   - Error states

## 🚫 Constraints

- **NO** databases (stateless app)
- **NO** WebSockets (HTTP only)
- **NO** Docker (runs locally)
- **NO** authentication (demo app)
- **NO** npm/webpack build for frontend (CDN only)
- **NO** complex state management
- **YES** must work on local network
- **YES** must bind to network interface for discovery (NOT localhost)

## 🧪 Key Behaviors to Specify (Phase 2)

### Backend Behaviors
- Color endpoint accepts valid RGB values
- Color endpoint rejects invalid values
- Color endpoint communicates with bulb per Shelly API
- Discovery finds Shelly device on network using correct interface binding
- Discovery caches bulb IP
- Status endpoint returns discovery state

### Frontend Behaviors
- Webcam permission requested and handled
- Color extracted from video frames
- Color sent to backend at throttled rate
- UI displays current state (color, discovery, errors)
- User can start/stop color tracking

### Integration Behaviors
- End-to-end: webcam → extraction → backend → bulb
- Discovery failure falls back to configured IP
- Network errors handled gracefully

## 🎯 Success Criteria

1. **Functional**:
   - Webcam captures and displays video
   - Dominant color extracted visibly
   - Bulb color changes in real-time reflecting webcam input
   - mDNS discovers bulb automatically
   - Discovery status visible to user
   - Fallback to configured IP works

2. **Non-Functional**:
   - Color change response < 200ms
   - Discovery completes within 10 seconds
   - Minimal CPU usage
   - Graceful error handling

3. **Code Quality**:
   - All tests pass (TDD per IIC)
   - Clean separation of concerns
   - No IIC violations
   - All third-party API usage verified via Context7

## 🚀 Phase 1: Analysis - START HERE

Begin with IIC Phase 1 per `copilot-instructions.md`.

Your analysis should:

1. **Resolve ALL Context7 dependencies** listed above
2. **Define architecture**:
   - Backend component boundaries (controller, service, client layers)
   - Frontend component structure
   - API contracts between frontend/backend
   - Network communication patterns
3. **Document technical decisions** in `docs/tech_assumptions.md`:
   - Why mDNS over hardcoded IP (user experience)
   - Why RestClient over other HTTP clients
   - Why simple color averaging (demo simplicity)
   - Interface binding strategy for discovery (Context7 jmdns guidance)
   - Shelly API interaction approach (Context7 Shelly guidance)
4. **Update all Phase 1 documentation** per IIC requirements

**Present your analysis and STOP for approval before Phase 2.**

## 💡 Known Technical Considerations

### mDNS Network Interface Binding
Per user experience: binding to localhost will NOT discover devices on LAN. Consult Context7 jmdns documentation for correct interface binding approach.

### Frontend CDN Pattern
```html
<script type="module">
  import { html, render } from 'https://unpkg.com/htm/preact/standalone.module.js';
  // App code here
</script>
```

### Color Extraction Approach
Simple averaging of center region pixels is sufficient for demo purposes. More sophisticated algorithms (clustering, histogram) are NOT required unless user requests.

## ⚠️ Critical Reminders

1. **Follow `copilot-instructions.md`** - Complete IIC process
2. **Use Context7 for ALL third-party APIs** - Shelly API, jmdns, Spring Boot, Preact
3. **STOP at every phase gate** - Do not proceed without approval
4. **Never modify test/spec/** - Per IIC protected files rules
5. **Keep it simple** - This is a demo app

## 🎬 Begin Phase 1

Resolve Context7 dependencies and perform analysis per IIC methodology.

**Do not skip to specification or implementation.**