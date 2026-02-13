� Smart Face Lock System - Distributed Vision Control
Overview

This project implements a sophisticated smart face lock system that detects and tracks faces in real-time, controlling servo motors for access control. The system features a modern web dashboard and follows a distributed architecture with enhanced security features:

🎯 Vision Node (PC): Advanced face detection with confidence scoring and real-time recognition

🔧 ESP8266 (Edge Controller): Secure servo motor control with MQTT communication

🌐 Backend API (PC/Machine): Real-time WebSocket relay with enhanced dashboard integration

💻 Modern Web Dashboard: Beautiful responsive interface with live tracking status and visual feedback

This system supports topic isolation for multi-team environments and includes enhanced security features for face recognition and access control.

🏗️ System Architecture
[ PC - Vision Node ]
        |
        | MQTT (vision/team02/movement)
        v
[ PC - Backend (WebSocket relay) ]
        |
        | WebSocket (ws://localhost:9002)
        v
[ Modern Browser Dashboard ]

AND

[ ESP8266 Edge Controller ]
        |
        | MQTT (vision/team02/movement)
        v
[ Servo Motor - Lock Mechanism ]

🚀 Golden Rule:

Vision detects and recognizes. Devices communicate via MQTT. Browsers connect via WebSocket. Backend provides real-time relay with modern UI.

� Project Structure
face-lock-mqtt/
│
├── vision-node/
│   └── vision_node.py           # Advanced face detection & MQTT publisher
│
├── backend/
│   └── backend.py               # Enhanced MQTT → WebSocket relay
│
├── esp8266/
│   └── main.py                  # MicroPython servo controller
│
├── dashboard/
│   └── index.html               # Modern responsive dashboard
│
└── README.md

⚙️ Setup Instructions (Local Deployment)
1. Install Dependencies

🐍 Python Requirements (PC Vision Node + Backend)

```bash
pip install opencv-python paho-mqtt websockets asyncio numpy
```

🔌 ESP8266 Setup

Flash MicroPython using Thonny IDE or ampy tool.

📡 MQTT Broker (Local Communication)

Windows:
Download Mosquitto from official site and run:
```bash
mosquitto.exe -v
```

Linux:
```bash
sudo apt update
sudo apt install mosquitto mosquitto-clients
sudo systemctl start mosquitto
```

2. Configure System

Each team uses a unique team ID (currently set to team02):

```python
TEAM_ID = "team02"
MQTT_TOPIC = f"vision/{TEAM_ID}/movement"
```

📋 Component Roles:
- **Vision Node**: Publishes face detection and movement messages
- **ESP8266**: Subscribes and controls servo motor for lock mechanism
- **Backend**: Subscribes and pushes real-time updates to dashboard
- **Dashboard**: Displays live status with modern UI

🔒 Security Note: Always use team-specific topics to prevent cross-team interference.

3. Launch System

🚀 Start MQTT Broker:
```bash
# Windows
mosquitto.exe -v

# Linux
sudo systemctl start mosquitto
```

🌐 Start Backend WebSocket Relay:
```bash
cd backend
python backend.py
```

👁️ Run Vision Node:
```bash
cd vision-node
python vision_node.py
```

💻 Open Dashboard:
Open `dashboard/index.html` in your web browser

Ensure WebSocket connection:
```javascript
const ws = new WebSocket("ws://localhost:9002");
```

🔌 Flash ESP8266:
- Update broker IP to your PC's local network IP
- Connect servo to GPIO5 (D1) for lock mechanism
- Upload and run main.py in MicroPython

4. Advanced Features

💓 Heartbeat Monitoring:
Monitor system health via:
```
vision/team02/heartbeat
```

Example payload:
```json
{
  "node": "pc",
  "status": "ONLINE",
  "timestamp": 1730000000,
  "confidence": 0.95
}
```

💡 Best Practices & Tips

🔧 Technical Optimization:
- Use dead-zone thresholds to prevent servo jitter
- Limit message rate to 10 Hz to avoid network flooding
- Implement smooth servo movement (2-5 degree increments)
- Always test locally before mechanical deployment

🔒 Security Considerations:
- Use encrypted MQTT connections in production
- Implement proper authentication for team access
- Regularly update face recognition models
- Monitor system logs for unauthorized access attempts

🎨 UI/UX Tips:
- Dashboard automatically adapts to different screen sizes
- Color-coded status indicators for quick recognition
- Real-time confidence scoring display
- Smooth animations and transitions

📦 System Requirements

🐍 Python 3.10+
- OpenCV (opencv-python)
- Paho-MQTT (paho-mqtt)
- Websockets (websockets)
- NumPy (numpy)

🔌 Hardware Requirements
- ESP8266 Microcontroller
- Servo Motor (SG90 or similar)
- USB Camera or Webcam
- MQTT Broker (Mosquitto recommended)

💻 Software Requirements
- MicroPython on ESP8266
- Mosquitto MQTT Broker
- Modern Web Browser (Chrome, Firefox, Safari)

🎯 Key Features

🚀 Advanced Face Recognition
- Real-time face detection with confidence scoring
- Multi-person tracking capabilities
- Unknown person detection and logging
- Enhanced security with strict recognition thresholds

🏗️ Distributed Architecture
- Modular component design
- Topic isolation for multi-team environments
- Scalable and maintainable codebase
- Real-time communication via MQTT/WebSocket

🌐 Modern Dashboard
- Responsive design with glass-morphism UI
- Real-time status updates with animations
- Color-coded connection states
- Interactive hover effects and transitions

🔒 Security Features
- Team-based access control
- Encrypted communication options
- Unknown person alerting
- Comprehensive logging system

⚙️ Operational Modes
- Local-only deployment (no external dependencies)
- Ready for open-loop testing (Phase 1)
- Prepared for closed-loop tracking (Phase 2)

🏁 Operational Workflow

📹 **Phase 1 - Open Loop Testing:**
PC camera detects face → publishes MQTT → ESP controls servo → Backend updates dashboard

🔄 **Phase 2 - Closed Loop Tracking:**
Camera mounted on servo → Real-time tracking feedback → Automatic face following

⚡ **Real-time Flow:**
Face Detection → Recognition → Decision Making → Servo Control → Dashboard Update

🔗 **Important Notes:**
- Avoid direct PC ↔ ESP connections
- Prevent Dashboard ↔ MQTT direct access
- Always route through Backend for security
- Monitor system health via heartbeat messages

🔗 Additional Resources

📚 Documentation & Research
- Gabriel Baziramwabo ResearchGate Profile
- BenaxMedia YouTube Channel (Tutorials & Demos)

🛠️ Technical Support
- MQTT Protocol Documentation
- ESP8266 MicroPython Guide
- OpenCV Face Recognition Documentation
- WebSocket API Reference

🌟 Community & Updates
- GitHub Repository for latest updates
- Issue tracking and feature requests
- Community forums and discussions
