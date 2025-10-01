RGBW Control App - Stage 2: Add mDNS Discovery
🎯 Stage 2 Intent
Enhance the existing RGBW Control App with automatic bulb discovery using mDNS, eliminating the need for manual IP configuration.
What's New:

Automatic discovery of Shelly Duo bulbs on the local network
Visual feedback showing discovery status and discovered bulb IP
Fallback to configured IP if discovery fails

What Stays: All existing functionality from Stage 1 (webcam capture, color extraction, color control).
🔒 Methodology
Follow copilot-instructions.md strictly - Intent Integrity Chain (IIC) methodology applies.
📚 Context7 Requirements - MANDATORY
Before ANY work, resolve and retrieve documentation for:
New Library

jmdns - mDNS service discovery and network interface binding

Refresh Documentation

Shelly API or Shelly Duo - Verify mDNS service type, hostname patterns, and discovery specifics

If ANY Context7 lookup fails → STOP immediately and report per IIC instructions.
🎨 New Functional Requirements
Backend: Discovery Service

mDNS Discovery Implementation

Use jmdns library for mDNS service discovery (consult Context7)
Discover Shelly Duo devices on local network
CRITICAL: Bind to actual network interface, NOT localhost

Per previous experience: localhost binding will NOT discover devices on LAN
Consult Context7 jmdns documentation for correct network interface binding approach


Identify correct mDNS service type for Shelly devices (consult Context7 Shelly docs)
Look for devices with "shelly" in hostname or appropriate service name
Extract IP address from discovered service


Discovery Lifecycle

Start discovery on application startup (or lazy on first request)
Handle discovery timeout (configurable, default 10 seconds)
Cache discovered bulb IP
Provide ability to re-trigger discovery if needed


Fallback Strategy

If discovery finds bulb: use discovered IP
If discovery fails or times out: fall back to configured IP from application.yml
Never fail startup due to discovery failure


New Endpoint

GET /api/bulb/status
Return JSON with:

Discovery state: "searching" | "found" | "failed" | "not_started"
Discovered IP (if found)
Whether using discovered or configured IP
Last discovery attempt time




Configuration Updates

Add discovery timeout to application.yml
Keep existing bulb.ip as fallback
Add optional service type configuration for mDNS



Frontend: Discovery Status UI

Status Display

Show discovery status prominently (searching/found/failed)
Display discovered bulb IP when found
Indicate whether using discovered or fallback IP
Visual distinction between discovery states (colors, icons, text)


User Feedback

"Searching for bulb..." during discovery
"Bulb found at 192.168.1.x" on success
"Using configured IP" on discovery failure
Show last discovery timestamp


Optional: Manual Re-discovery

Button to trigger discovery again
Only enable when not currently discovering



🚫 Constraints

NO changes to core color control logic (don't break Stage 1)
NO databases
NO complex state management
YES discovery must work on local network (proper interface binding)
YES fallback to configured IP must be reliable
YES discovery failure must not break application

⚠️ CRITICAL: Network Interface Binding
Based on previous user feedback:
java// ❌ WRONG - Won't discover on LAN
JmDNS jmdns = JmDNS.create(InetAddress.getLocalHost());

// ✅ CORRECT - Consult Context7 jmdns docs for actual implementation
The exact correct approach depends on the environment and should come from Context7 jmdns documentation. The key point: DO NOT bind to localhost.
🧪 Key Behaviors to Specify (Phase 2)
Backend Discovery Behaviors

Discovery service binds to correct network interface (not localhost)
Discovery searches for Shelly devices using correct service type
Discovery extracts IP from found service
Discovery caches result
Discovery times out appropriately
Discovery failure falls back to configured IP
Status endpoint returns accurate discovery state
Color control works with both discovered and configured IPs

Frontend Discovery Behaviors

UI polls or retrieves discovery status
UI displays current discovery state
UI shows discovered IP when available
UI indicates which IP is being used (discovered vs configured)
Discovery status updates without breaking color control functionality

Integration Behaviors

End-to-end with discovered IP works same as Stage 1
Fallback to configured IP works when discovery fails
Application starts successfully even if discovery fails
Discovery doesn't interfere with ongoing color control

🎯 Success Criteria

Functional:

Discovery finds Shelly bulb on local network
Discovered IP used for color control
Discovery status visible to user
Fallback to configured IP works reliably
All Stage 1 functionality continues to work


Non-Functional:

Discovery completes within configured timeout
Discovery failure doesn't delay startup
No degradation to color control performance


Code Quality:

All tests pass (including new discovery tests)
Clean integration with existing code
No IIC violations
All jmdns API usage verified via Context7