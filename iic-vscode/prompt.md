🎯 Product Intent
Build a live demo web application that:

Captures webcam video in the browser
Extracts the dominant color from each frame
Sends color data to a Shelly Duo RGBW bulb over local network
Controls the bulb's color in real-time based on webcam input

Use Case: Interactive demonstration of color extraction and IoT control - wave colored objects in front of camera to change bulb color.
Stage 1 Scope: Hardcoded bulb IP in configuration. mDNS discovery will be added in Stage 2.
🔒 Methodology
Follow copilot-instructions.md strictly - Intent Integrity Chain (IIC) methodology applies.
You MUST work through phases: Analysis → Specification → Test Generation → Implementation, with STOP gates between each phase for user approval.
📚 Context7 Requirements - MANDATORY
Before ANY work, resolve and retrieve documentation for:
Backend Libraries

Spring Boot (latest) - REST endpoints, RestClient, application configuration
Shelly Duo or Shelly API - Bulb control API documentation

Frontend Libraries

Preact - Component library
htm - JSX alternative for CDN use
MediaStream API - Webcam access (if available in Context7)
Canvas API - Image data extraction (if available in Context7)

Testing Libraries

Spring Boot Test - Backend testing
Vitest or appropriate testing framework - Frontend testing

If ANY Context7 lookup fails → STOP immediately and report per IIC instructions.
🏗️ Technical Stack
Use the scaffolded workspace:

Backend: Java (latest LTS), Spring Boot (latest 3.x)
Frontend: HTML, Preact via CDN (htm/preact)
Build: Maven (already configured)
Network: HTTP only
Configuration: Bulb IP in application.yml (hardcoded for Stage 1)
Deployment: Local only (demo app)

🎨 Functional Requirements
Backend Responsibilities

Color Control Endpoint

POST /api/color
Accept RGB values: {"r": 0-255, "g": 0-255, "b": 0-255}
Validate input ranges
Convert RGB to format required by Shelly API (consult Context7)
Send HTTP request to bulb using Spring RestClient
Return success/error status


Configuration

Bulb IP address in application.yml as bulb.ip
Use placeholder IP (e.g., 192.168.1.100) in configuration
Make IP injectable for testing


Error Handling

Network errors (bulb unreachable)
Invalid color values (out of range, negative, null)
Return appropriate HTTP status codes per Spring Boot best practices



Frontend Responsibilities

Webcam Capture

Request camera permission via MediaStream API (consult Context7 if available)
Display live video feed
Handle permission denied gracefully


Color Extraction

Sample video frames using Canvas API (consult Context7 if available)
Extract dominant color using simple algorithm (average RGB from center region)
Update extraction at reasonable interval (100-200ms)


UI Components

Video preview element
Color preview (showing extracted color)
Connection status indicator
Error messages
Start/Stop controls


API Integration

Send extracted colors to backend via POST /api/color
Throttle requests appropriately
Handle network errors
Display connection status


Visual Feedback

Show current extracted color
Show bulb connection status
Show last sent color
Error states



🚫 Constraints

NO databases (stateless app)
NO WebSockets (HTTP only)
NO Docker (runs locally)
NO authentication (demo app)
NO npm/webpack build for frontend (CDN only)
NO complex state management
NO mDNS discovery (Stage 2 feature)
YES hardcoded IP in configuration
YES must work on local network

⚠️ CRITICAL: Expected Test Failures
Integration tests that actually call the bulb WILL FAIL until the user provides the correct bulb IP.
This is EXPECTED and CORRECT behavior:

You will use a placeholder IP (e.g., 192.168.1.100) in configuration
Tests that call the real bulb endpoint will fail with network errors
DO NOT spend time trying to fix these failures
DO NOT hallucinate or guess the correct IP
DO NOT modify tests to avoid calling the bulb

When integration tests fail with network errors:

Verify the test logic is correct (validates behavior if bulb were reachable)
Document in test or comments: "Requires actual bulb IP in application.yml"
Report: "Integration tests fail as expected - awaiting user's bulb IP configuration"
STOP and present results

Unit tests that mock the HTTP client SHOULD pass and demonstrate correct behavior.
🧪 Key Behaviors to Specify (Phase 2)
Backend Behaviors

Color endpoint accepts valid RGB values (0-255 for each channel)
Color endpoint rejects invalid values (negative, >255, null)
Color endpoint formats request per Shelly API (consult Context7)
Color endpoint sends HTTP request to configured bulb IP
Color endpoint returns appropriate status codes
Configuration loads bulb IP from application.yml

Frontend Behaviors

Webcam permission requested and handled
Video stream displayed in UI
Color extracted from video frames (center region)
Color sent to backend at throttled rate
UI displays current extracted color
UI displays connection status
User can start/stop color tracking
Errors displayed to user

Integration Behaviors

End-to-end: webcam → extraction → backend → bulb (will fail until real IP provided)
Network errors handled gracefully with user feedback
Invalid color values rejected with appropriate error messages

🎯 Success Criteria

Functional:

Webcam captures and displays video
Dominant color extracted and displayed
Color sent to backend with correct format
Backend validates and formats per Shelly API
Backend sends HTTP request to configured IP
When real bulb IP provided: Bulb color changes in real-time
All error cases handled gracefully


Non-Functional:

Color extraction responsive (100-200ms intervals)
Request throttling prevents spam
Minimal CPU usage
Clear error messages


Code Quality:

All unit tests pass
Integration tests demonstrate correct behavior (but fail on network until real IP)
Clean separation of concerns
No IIC violations
All third-party API usage verified via Context7