📘 Distributed Vision-Control System (Face-Locked Servo)
Overview

This project implements a distributed vision-control system that detects and tracks faces in real-time and controls a servo motor accordingly. The system is fully modular and follows a distributed architecture:

Vision Node (PC): Captures camera frames, detects faces, and publishes movement instructions via MQTT.

ESP8266 (Edge Controller): Subscribes to MQTT messages and moves the servo motor.

Backend API (PC/Machine): Relays MQTT messages to a web dashboard using WebSocket.

Web Dashboard (Browser): Displays real-time tracking status.

This system supports topic isolation, allowing multiple teams to operate on the same broker without interference.

🔧 Architecture Diagram
[ PC - Vision Node ]
        |
        | MQTT (vision/<team_id>/movement)
        v
[ PC - Backend (WebSocket relay) ]
        |
        | WebSocket (ws://localhost:9002)
        v
[ Browser Dashboard ]

AND

[ ESP8266 Edge Controller ]
        |
        | MQTT (vision/<team_id>/movement)
        v
[ Servo Motor ]


Golden Rule:

Vision computes. Devices speak MQTT. Browsers speak WebSocket. Backend relays in real-time.

📂 Repository Structure
distributed-vision-control/
│
├── vision-node/
│   └── vision_node.py           # PC face detection & MQTT publisher
│
├── backend/
│   └── backend.py               # MQTT → WebSocket relay
│
├── esp8266/
│   └── main.py                  # MicroPython servo controller
│
├── dashboard/
│   └── index.html               # WebSocket dashboard
│
└── README.md

⚙️ Setup Instructions (Local-Only Mode)
1. Install dependencies

Python (PC Vision Node + Backend)

pip install opencv-python paho-mqtt websockets asyncio


ESP8266

Flash MicroPython using Thonny or ampy.

Mosquitto Broker (Local MQTT broker)

Windows:
Download from Mosquitto
 and run mosquitto.exe -v in a terminal.

Linux:

sudo apt update
sudo apt install mosquitto mosquitto-clients
sudo systemctl start mosquitto

2. Configure MQTT topics

Each team must use a unique team ID:

TEAM_ID = "team01"
MQTT_TOPIC = f"vision/{TEAM_ID}/movement"


PC Vision Node: Publishes movement messages.

ESP8266: Subscribes and moves servo.

Backend: Subscribes and pushes updates to dashboard.

Important: Do not use wildcard topics or other teams’ topics.

3. Run the system locally

Start MQTT broker (mosquitto -v on Windows or sudo systemctl start mosquitto on Linux).

Start Backend WebSocket relay:

cd backend
python backend.py


Run Vision Node (PC):

cd vision-node
python vision_node.py


Open Dashboard:

Open dashboard/index.html in a browser.

Ensure WebSocket URL matches backend:

const ws = new WebSocket("ws://localhost:9002");


Flash ESP8266:

Update broker IP to your PC’s local IP.

Connect servo to GPIO5 (D1).

Run main.py in MicroPython.

4. Optional: Heartbeat messages

You can publish node status to:

vision/<team_id>/heartbeat


Example payload:

{
  "node": "pc",
  "status": "ONLINE",
  "timestamp": 1730000000
}

💡 Best Practices

Use dead-zone thresholds to prevent servo jitter.

Limit message rate to 10 Hz to avoid flooding.

Smooth servo movement with small increments (2–5 degrees).

Always test locally before moving to mechanical closed-loop tracking.

📝 Dependencies

Python 3.10+

OpenCV (opencv-python)

Paho-MQTT (paho-mqtt)

Websockets (websockets)

MicroPython on ESP8266

Mosquitto MQTT Broker

🎯 Features

Real-time face tracking and servo control

Full distributed architecture

Topic isolation for multi-team environments

Web dashboard with live updates

Local-only mode (no VPS required)

Ready for open-loop (phase 1) or closed-loop (phase 2)

🏁 Running Notes

PC camera detects face → publishes MQTT → ESP moves servo → Backend updates dashboard.

Phase 1: Camera fixed.

Phase 2: Camera mounted on servo for closed-loop tracking.

Avoid direct connections between PC ↔ ESP or Dashboard ↔ MQTT.

🔗 References

Gabriel Baziramwabo ResearchGate

BenaxMedia YouTube Channel