# Frontend & UI Rules
// **Activation: Glob Pattern - `**/*.html, **/*.js, **/*.css`**

## UI Design Requirements (MANDATORY)

**Minimum sizes for projection visibility:**

```css
font-size: 18px minimum
button-font-size: 24px minimum
button-height: 50px minimum
color-preview-size: 250px × 250px
```

**High contrast for visibility:**
```css
background: dark colors
text: light colors with high contrast
borders: clearly visible (2px+)
```

## Visual Design Guidelines

### Color Scheme (Dark theme for projection)
```css
--bg-primary: #1a1a1a;
--bg-secondary: #2d2d2d;
--text-primary: #ffffff;
--text-secondary: #cccccc;
--accent: #4a9eff;
--success: #4caf50;
--error: #f44336;
```

### Layout Pattern
```css
/* CSS Grid for main layout */
.container {
  display: grid;
  grid-template-columns: 1fr 2fr 1fr;
  grid-template-rows: auto 1fr auto;
  height: 100vh;
  padding: 2rem;
  gap: 2rem;
}

/* Large, touch-friendly buttons */
button {
  padding: 1.5rem 3rem;
  font-size: 1.75rem;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.3s;
}
```

## Camera Handling

```javascript
// ✅ Enumerate devices properly
navigator.mediaDevices.enumerateDevices()
  .then(devices => {
    const videoDevices = devices.filter(d => d.kind === 'videoinput');
    // Populate dropdown
  });

// ✅ Handle permissions gracefully
navigator.mediaDevices.getUserMedia({ video: true })
  .catch(err => {
    statusDiv.textContent = "Camera access denied. Please grant permissions.";
  });
```

## Mode Management

```javascript
// Manual mode: Send button enabled, no auto-sending
// Auto mode: Send button disabled, auto-send every 3 seconds

let mode = 'manual';
let autoInterval = null;

function startAutoMode() {
  autoInterval = setInterval(() => {
    const color = extractColor();
    sendColorToBulb(color.r, color.g, color.b);
  }, 3000);
}

function stopAutoMode() {
  if (autoInterval) {
    clearInterval(autoInterval);
    autoInterval = null;
  }
}
```

## Color Detection (Color Thief)

```javascript
// ✅ Use Color Thief library (loaded via CDN)
const colorThief = new ColorThief();
const video = document.getElementById('webcam');

function extractColor() {
  const canvas = document.createElement('canvas');
  canvas.width = video.videoWidth;
  canvas.height = video.videoHeight;
  const ctx = canvas.getContext('2d');
  ctx.drawImage(video, 0, 0);
  
  const [r, g, b] = colorThief.getColor(canvas);
  return { r, g, b };
}
```

## Error Handling

```javascript
// ✅ Show clear error messages
async function sendColorToBulb(r, g, b) {
  try {
    const response = await fetch('/api/color', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ red: r, green: g, blue: b })
    });
    
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`);
    }
    
    statusDiv.textContent = '✅ Color updated!';
  } catch (error) {
    statusDiv.textContent = `❌ Failed: ${error.message}`;
  }
}
```

## Performance Requirements

```yaml
camera_stream: 30fps minimum
color_extraction: < 100ms
api_response: < 1 second (including network)
auto_mode_interval: exactly 3 seconds
ui_responsiveness: no lag on button clicks