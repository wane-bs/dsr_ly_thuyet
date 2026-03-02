---
layout: default
title: IoT Integration
---

# IoT Integration - Internet of Things (Internet Vạn Vật)
## Tài liệu Giới thiệu Khái niệm và Ứng dụng

> 📍 **Vị trí trong bộ tài liệu**: 5/5 - Tương tác Thế giới Vật lý  
> ⬅️ [Chainlink](./4-Chainlink.md) | 🏠 [Kiến trúc Tổng quan](./0-Kien-truc-Tong-quan.md)

### 📎 Tài liệu Liên quan
| Liên kết đến | Mối quan hệ |
|-------------|-------------|
| [Smart Contracts (Cơ bản)](./2-Smart-Contracts-Co-ban.md) | Contract emit BookRented event → trigger IoT |
| [Chainlink](./4-Chainlink.md) | Functions hash IoT logs lên blockchain |

---

## 📚 Mục lục

1. [Giới thiệu IoT](#1-giới-thiệu-iot)
2. [Kiến trúc IoT (IoT Architecture)](#2-kiến-trúc-iot)
3. [IoT Protocols và Communication](#3-iot-protocols-và-communication)
4. [Smart Devices và Sensors](#4-smart-devices-và-sensors)
5. [IoT Platforms](#5-iot-platforms)
6. [Edge Computing vs Cloud Computing](#6-edge-computing-vs-cloud-computing)
7. [IoT Security](#7-iot-security)
8. [IoT + Blockchain Integration](#8-iot--blockchain-integration)
9. [Smart Lock Systems](#9-smart-lock-systems)
10. [Use Cases và Applications](#10-use-cases-và-applications)
11. [Phân tích Implementation trong VinaLib](#11-phân-tích-implementation-trong-vinalib)

---

## 1. Giới thiệu IoT

### 👤 Góc nhìn Người dùng

**IoT là gì?**  
IoT (Internet of Things) là mạng lưới các thiết bị vật lý ("things") được kết nối internet, có khả năng thu thập và trao đổi dữ liệu mà không cần con người can thiệp. Từ đèn thông minh, tủ lạnh, camera an ninh đến khóa cửa điện tử - tất cả đều là IoT.

**Ví dụ thực tế:**
```
Ngày thường của bạn với IoT:

06:00 → Smart alarm clock đánh thức + Tự động bật đèn phòng
06:30 → Coffee maker tự pha cà phê (đã hẹn giờ từ hôm trước)
07:00 → Smart lock tự khóa cửa khi bạn ra khỏi nhà
18:00 → Camera gửi thông báo "có người lạ tại cửa"
18:30 → Bạn mở khóa từ xa bằng smartphone cho người giao hàng
19:00 → Thermostat tự điều chỉnh nhiệt độ phòng
```

**Ưu điểm cho người dùng:**
- ✅ **Tự động hóa**: Thiết bị tự hoạt động theo lịch trình/điều kiện
- ✅ **Remote Control**: Điều khiển từ xa qua smartphone
- ✅ **Data Insights**: Thu thập dữ liệu để tối ưu hóa (tiết kiệm điện, bảo mật...)
- ✅ **Convenience**: Cuộc sống tiện lợi hơn

**Nhược điểm cần lưu ý:**
- ⚠️ **Privacy Risk**: Thiết bị luôn thu thập dữ liệu cá nhân
- ⚠️ **Security**: IoT devices thường dễ bị hack
- ⚠️ **Dependency**: Phụ thuộc internet, nếu mất mạng → mất quyền điều khiển
- ⚠️ **Compatibility**: Thiết bị từ nhà sản xuất khác nhau khó tương thích

### ⚙️ Góc nhìn Hệ thống

**IoT Ecosystem:**
```
┌─────────────────────────────────────────────────┐
│   Application Layer (User Interface)           │
│   - Mobile Apps, Web Dashboards, Voice UI      │
├─────────────────────────────────────────────────┤
│   Platform Layer (Cloud/Edge)                  │
│   - Data Processing, Analytics, ML              │
│   - Device Management, Rule Engine              │
├─────────────────────────────────────────────────┤
│   Network Layer (Connectivity)                  │
│   - WiFi, Bluetooth, Zigbee, LoRaWAN, 5G       │
├─────────────────────────────────────────────────┤
│   Perception Layer (Devices & Sensors)          │
│   - Smart Locks, Cameras, Temperature Sensors   │
│   - Actuators (motors, relays)                  │
└─────────────────────────────────────────────────┘
```

**Core Components:**
1. **Sensors**: Thu thập dữ liệu từ môi trường (nhiệt độ, ánh sáng, chuyển động)
2. **Actuators**: Thực hiện hành động (mở khóa, bật đèn, điều chỉnh nhiệt độ)
3. **Connectivity**: Kết nối internet (WiFi, cellular, Bluetooth)
4. **Data Processing**: Xử lý dữ liệu (local hoặc cloud)
5. **User Interface**: Giao diện điều khiển (app, web, voice assistant)

---

## 2. Kiến trúc IoT (IoT Architecture)

### 👤 Góc nhìn Người dùng

**4 Layers chính:**

```
Layer 4: Application (Bạn thấy được)
         → Mobile app để điều khiển đèn, khóa cửa
         
Layer 3: Data Processing (Cloud làm việc)
         → Phân tích: "Bạn thường về nhà lúc 6PM"
         → Tự động bật điều hòa lúc 5:45PM
         
Layer 2: Network (Kết nối)
         → WiFi router truyền lệnh "Bật đèn" đến bóng đèn
         
Layer 1: Physical Devices (Thiết bị vật lý)
         → Bóng đèn thông minh nhận lệnh và bật sáng
```

### ⚙️ Góc nhìn Hệ thống

**Detailed Architecture:**

```
┌──────────────────────────────────────────────────────┐
│ Layer 4: Application & Service                       │
│ ┌──────────────┐  ┌──────────────┐  ┌─────────────┐ │
│ │ Web Portal   │  │ Mobile Apps  │  │ Third-party │ │
│ │ Dashboard    │  │ (iOS/Android)│  │ Integrations│ │
│ └──────────────┘  └──────────────┘  └─────────────┘ │
└──────────────────────────────────────────────────────┘
                        ↕ API/REST/GraphQL
┌──────────────────────────────────────────────────────┐
│ Layer 3: Data Processing & Management                │
│ ┌─────────────────────────────────────────────────┐  │
│ │ IoT Platform (AWS IoT, Azure IoT, Tuya, etc.)  │  │
│ │ - Device Registry                               │  │
│ │ - Data Analytics                                │  │
│ │ - Rule Engine (if temp > 30 → turn on AC)      │  │
│ │ - Machine Learning (pattern detection)         │  │
│ └─────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────┘
                        ↕ MQTT/CoAP/HTTP
┌──────────────────────────────────────────────────────┐
│ Layer 2: Network & Connectivity                      │
│ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ │
│ │  WiFi    │ │Bluetooth │ │ Zigbee   │ │ LoRaWAN  │ │
│ │  (2.4/5G)│ │ (BLE)    │ │ (mesh)   │ │ (LPWAN)  │ │
│ └──────────┘ └──────────┘ └──────────┘ └──────────┘ │
│ Gateway/Hub: Translator giữa devices và cloud        │
└──────────────────────────────────────────────────────┘
                        ↕ Local protocols
┌──────────────────────────────────────────────────────┐
│ Layer 1: Perception (Devices & Sensors)              │
│ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ │
│ │Temperature│ │ Motion   │ │ Smart    │ │ Camera   │ │
│ │ Sensor   │ │ Detector │ │ Lock     │ │ (visual) │ │
│ └──────────┘ └──────────┘ └──────────┘ └──────────┘ │
│ Microcontrollers: ESP32, Arduino, Raspberry Pi       │
└──────────────────────────────────────────────────────┘
```
---

## 3. IoT Protocols và Communication

### 👤 Góc nhìn Người dùng

**Tại sao có nhiều protocols?**

```
Tưởng tượng bạn muốn nói chuyện với bạn bè:
- Gặp trực tiếp: Nói to, rõ ràng (WiFi - high bandwidth)
- Gọi điện thoại: Tiết kiệm pin hơn (Bluetooth)
- Gửi thư: Chậm nhưng đi xa (LoRaWAN - long range)

IoT devices cũng vậy - chọn protocol tùy nhu cầu:
- Smart TV: Cần WiFi (stream video cần băng thông cao)
- Fitness tracker: Bluetooth (gần smartphone, tiết kiệm pin)
- Cảm biến nông nghiệp: LoRaWAN (xa hàng km, pin dùng 5 năm)
```

### ⚙️ Góc nhìn Hệ thống

**Protocol Comparison:**

| Protocol | Range | Bandwidth | Power | Use Case |
|----------|-------|-----------|-------|----------|
| **WiFi** | 50-100m | High (Mbps) | High | Cameras, smart TVs, door locks |
| **Bluetooth/BLE** | 10-100m | Medium (Kbps-Mbps) | Low | Wearables, beacons, proximity sensors |
| **Zigbee** | 10-100m | Low (250 Kbps) | Very Low | Smart home (mesh network) |
| **Z-Wave** | 30m | Low (100 Kbps) | Very Low | Home automation |
| **LoRaWAN** | 2-15 km | Very Low (50 Kbps) | Ultra Low | Agriculture, smart cities |
| **NB-IoT** | Wide area | Low | Low | Utilities, parking |
| **5G** | Wide area | Ultra High | Medium | Autonomous vehicles, industry 4.0 |

**MQTT Protocol (Most Popular for IoT):**

```
MQTT = Message Queuing Telemetry Transport

Architecture:
┌─────────────┐         ┌─────────────┐         ┌─────────────┐
│  Publisher  │ ──────▶ │   Broker    │ ──────▶ │ Subscriber  │
│  (Device)   │ PUBLISH │ (Mosquitto, │ DELIVER │  (App)      │
│  "temp=25°C"│         │  AWS IoT)   │         │             │
└─────────────┘         └─────────────┘         └─────────────┘
                             ↑
                        Subscribe to
                        topic: "home/temp"

Features:
- Lightweight (header chỉ 2 bytes)
- Publish/Subscribe model
- QoS levels: 0 (at most once), 1 (at least once), 2 (exactly once)
- Support retain messages
- Last Will & Testament (nếu device disconnect đột ngột)
```

**CoAP Protocol:**

```
CoAP = Constrained Application Protocol

Similar to HTTP but for IoT:
- Uses UDP (lighter than TCP)
- Request/Response model
- Methods: GET, POST, PUT, DELETE
- Designed for low-power devices

Example:
  CoAP GET coap://device.local/temperature
  Response: 25.5°C
```

---

## 4. Smart Devices và Sensors

### 👤 Góc nhìn Người dùng

**Các loại sensors phổ biến:**

```
🌡️ Temperature Sensor (Cảm biến nhiệt độ):
   → Đo nhiệt độ phòng → Tự động bật điều hòa

💡 Light Sensor (Cảm biến ánh sáng):
   → Phát hiện trời tối → Tự động bật đèn

👤 Motion Sensor (PIR - Passive Infrared):
   → Phát hiện chuyển động → Kích hoạt camera/đèn

🚪 Door/Window Sensor (Magnetic contact):
   → Phát hiện cửa mở → Gửi cảnh báo

🔊 Sound Sensor (Microphone):
   → Phát hiện tiếng động lớn → Alert

📹 Camera (Visual sensor):
   → Ghi hình, nhận diện khuôn mặt

🔐 Smart Lock (Actuator + Sensor):
   → Nhận lệnh mở khóa + Báo trạng thái khóa
```

### ⚙️ Góc nhìn Hệ thống

**Sensor Types by Measurement:**

**1. Environmental Sensors:**
```
- Temperature: DHT22, DS18B20, BME280
- Humidity: DHT11, SHT31
- Air Quality: MQ-135 (CO2), MQ-7 (CO)
- Pressure: BMP180, BMP280
- Light: LDR, BH1750
```

**2. Position & Motion:**
```
- Accelerometer: MPU6050 (detect movement, tilt)
- Gyroscope: Measure rotation
- GPS: Location tracking
- Ultrasonic: HC-SR04 (distance measurement)
```

**3. Biometric:**
```
- Fingerprint: R307 (authentication)
- Heart Rate: MAX30102
- Face Recognition: Camera + ML model
```

**4. Smart Actuators:**
```
- Smart Locks: Solenoid lock, Motor lock
- Relays: Control high-voltage devices (lights, appliances)
- Servo Motors: Open/close mechanisms
- Speakers: Audio output/alerts
```

**Sensor Data Processing:**

```
Raw Sensor Data Flow:
┌──────────────────┐
│ Physical World   │  Temperature = 25.7°C
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Analog Sensor    │  Voltage output: 2.57V
│ (thermistor)     │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ ADC (Analog to   │  Digital value: 1023 (10-bit)
│ Digital Convert) │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Microcontroller  │  Convert: (1023/1023)*100 = 25.7
│ (ESP32/Arduino)  │  Calibration + Filtering
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Edge Processing  │  Decision: temp > 25 → Send alert
│ (local logic)    │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Network Layer    │  MQTT publish: {"temp": 25.7, "alert": true}
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Cloud Platform   │  Store in database, trigger automation
└──────────────────┘
```

---

## 5. IoT Platforms

### 👤 Góc nhìn Người dùng

**Platforms là gì?**  
IoT Platform là "trung tâm điều khiển" - nơi kết nối, quản lý tất cả thiết bị IoT của bạn. Thay vì mỗi thiết bị một app riêng, platform thống nhất tất cả.

**Các platforms phổ biến:**

```
🏠 Consumer IoT:
- Google Home (Assistant)
- Amazon Alexa
- Apple HomeKit
- Samsung SmartThings

☁️ Enterprise/Cloud Platforms:
- AWS IoT Core
- Microsoft Azure IoT Hub
- Google Cloud IoT
- IBM Watson IoT

🏭 Specialized Platforms:
- Tuya Smart (manufacturers)
- ThingSpeak (data logging)
- Particle.io (hardware + cloud)
```

### ⚙️ Góc nhìn Hệ thống

**AWS IoT Core Architecture:**

```
┌─────────────────────────────────────────────────────┐
│                   AWS Cloud                         │
│                                                     │
│  ┌──────────────────────────────────────────────┐  │
│  │ AWS IoT Core (Message Broker)                │  │
│  │ - MQTT/HTTPS endpoints                       │  │
│  │ - Device Gateway                             │  │
│  │ - Device Shadow (virtual twin)               │  │
│  └─────────────┬────────────────────────────────┘  │
│                │                                    │
│  ┌─────────────┴────────────────┐                  │
│  │                               │                  │
│  ▼                               ▼                  │
│ ┌──────────────┐         ┌──────────────┐          │
│ │ IoT Rules    │         │Device Registry│         │
│ │ Engine       │         │(Thing Index)  │         │
│ └──────┬───────┘         └──────────────┘          │
│        │                                            │
│        │ Triggers                                   │
│  ┌─────┴──────────────────────────────┐            │
│  │                                     │            │
│  ▼                                     ▼            │
│ ┌──────────┐  ┌──────────┐  ┌──────────────┐      │
│ │ Lambda   │  │DynamoDB  │  │  S3 Storage  │      │
│ │ Functions│  │(Database)│  │  (Raw data)  │      │
│ └──────────┘  └──────────┘  └──────────────┘      │
└─────────────────────────────────────────────────────┘
              ↕ Secure TLS/X.509
┌─────────────────────────────────────────────────────┐
│             IoT Devices (Edge)                      │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐          │
│  │ESP32     │  │Raspberry │  │ Arduino  │          │
│  │+ Sensors │  │Pi + Camera│  │+ Actuator│          │
│  └──────────┘  └──────────┘  └──────────┘          │
└─────────────────────────────────────────────────────┘
```

**Tuya IoT Platform (Manufacturer Platform):**

```
Tuya = Turnkey IoT solution for manufacturers

Components:
┌──────────────────────────────────────────┐
│ Tuya Cloud API                           │
│ - Device Control                         │
│ - Real-time status                       │
│ - Historical data                        │
└──────────────────────────────────────────┘
              ↕ REST API
┌──────────────────────────────────────────┐
│ Tuya App (White-label)                   │
│ - Custom branding                        │
│ - Automation rules                       │
│ - User management                        │
└──────────────────────────────────────────┘
              ↕ MQTT/LAN protocol
┌──────────────────────────────────────────┐
│ Tuya-enabled Smart Devices               │
│ - Smart Locks                            │
│ - Cameras, Sensors                       │
│ - Powered by Tuya MCU module             │
└──────────────────────────────────────────┘

Key Features:
- Pre-built firmware for common devices
- Global cloud infrastructure
- Voice assistant integration (Alexa, Google)
- Open API for custom integration
```

**Device Provisioning:**

```
How a new device joins the platform:

Step 1: Manufacturing
   ├─ Device flashed with unique ID + certificates
   └─ Registered in platform database

Step 2: User Setup
   ├─ User opens app → Scan QR code / Bluetooth pairing
   ├─ Device broadcasts: "I'm here! My ID is X"
   └─ App sends WiFi credentials to device

Step 3: Cloud Registration
   ├─ Device connects to WiFi → Contacts cloud
   ├─ Cloud verifies certificate/ID
   ├─ Device assigned to user account
   └─ Device Shadow created in cloud

Step 4: Operational
   ├─ Device sends periodic heartbeats
   ├─ Subscribes to command topics
   └─ Publishes telemetry data
```

---

## 6. Edge Computing vs Cloud Computing

### 👤 Góc nhìn Người dùng

**Cloud Computing:**
```
Camera → Upload video to cloud → Cloud phân tích → Gửi alert về
Ưu: Powerful processing
Nhược: Slow (phụ thuộc internet), privacy risk
```

**Edge Computing:**
```
Camera → Phân tích ngay tại camera → Gửi alert
Ưu: Fast (real-time), privacy (data không lên cloud)
Nhược: Limited processing power
```

### ⚙️ Góc nhìn Hệ thống

**Comparison:**

| Aspect | Cloud Computing | Edge Computing |
|--------|----------------|----------------|
| **Processing Location** | Remote datacenter | Local device/gateway |
| **Latency** | High (100-500ms) | Ultra Low (<10ms) |
| **Bandwidth** | High usage | Low (only send summaries) |
| **Reliability** | Depends on internet | Works offline |
| **Cost** | Ongoing (data transfer) | Upfront (hardware) |
| **Privacy** | Data leaves premises | Data stays local |
| **Scalability** | Easy (add servers) | Limited (device constraints) |

**Hybrid Architecture (Best of Both Worlds):**

```
┌──────────────────────────────────────────────────────┐
│                   Cloud Layer                        │
│  - Long-term storage                                 │
│  - Complex ML training                               │
│  - Cross-device analytics                            │
│  - Rule updates                                      │
└──────────────────┬───────────────────────────────────┘
                   │ Periodic sync
                   │ (summaries, alerts, updates)
                   │
┌──────────────────▼───────────────────────────────────┐
│                  Edge Layer                          │
│  ┌──────────────────────────────────────────────┐   │
│  │ Edge Gateway (Raspberry Pi / NVIDIA Jetson) │   │
│  │ - Real-time inference                        │   │
│  │ - Local data filtering                       │   │
│  │ - Immediate response                         │   │
│  └──────────────────┬───────────────────────────┘   │
└─────────────────────┼───────────────────────────────┘
                      │ Local network
        ┌─────────────┼─────────────┐
        ▼             ▼             ▼
   [Camera]      [Sensor]      [Smart Lock]
   
Example:
1. Camera detects motion (Edge AI)
2. If person detected → Record locally + Send alert
3. Only send 10-sec clip to cloud (not 24/7 stream)
4. Cloud: Facial recognition + long-term storage
```

---

## 7. IoT Security

### 👤 Góc nhìn Người dùng

**Threats (Mối đe dọa):**

```
🔓 Unauthorized Access:
   Hacker mở khóa cửa smart lock của bạn từ xa

📹 Privacy Breach:
   Camera bị hack → Live stream lên dark web

🤖 Botnet Attack:
   1 triệu thiết bị IoT bị điều khiển tấn công DDoS

💸 Ransomware:
   Smart home bị khóa → Đòi tiền chuộc
```

**Best Practices for Users:**
```
✅ Change default passwords (admin/admin → strong unique password)
✅ Keep firmware updated
✅ Use separate WiFi network for IoT (guest network)
✅ Enable 2FA if available
✅ Review privacy settings (disable unnecessary data collection)
```

### ⚙️ Góc nhìn Hệ thống

**Security Layers:**

```
Layer 7: Application Security
┌────────────────────────────────────────┐
│ - Input validation                     │
│ - API authentication (OAuth 2.0, JWT)  │
│ - Role-based access control (RBAC)     │
└────────────────────────────────────────┘

Layer 6: Data Security
┌────────────────────────────────────────┐
│ - Encryption at rest (AES-256)         │
│ - Encryption in transit (TLS 1.3)      │
│ - Data anonymization                   │
└────────────────────────────────────────┘

Layer 5: Network Security
┌────────────────────────────────────────┐
│ - VPN/Private networks                 │
│ - Firewall (block unauthorized ports)  │
│ - IDS/IPS (Intrusion Detection)        │
└────────────────────────────────────────┘

Layer 4: Platform Security
┌────────────────────────────────────────┐
│ - Device authentication (X.509 certs)  │
│ - Secure provisioning                  │
│ - Anomaly detection                    │
└────────────────────────────────────────┘

Layer 3: Communication Security
┌────────────────────────────────────────┐
│ - MQTT over TLS (port 8883)            │
│ - CoAP over DTLS                       │
│ - Message signing (HMAC)               │
└────────────────────────────────────────┘

Layer 2: Device Security
┌────────────────────────────────────────┐
│ - Secure boot (verify firmware)        │
│ - Hardware Security Module (HSM)       │
│ - Trusted Execution Environment (TEE)  │
└────────────────────────────────────────┘

Layer 1: Physical Security
┌────────────────────────────────────────┐
│ - Tamper-resistant enclosures          │
│ - Secure key storage (eFuse, TPM)      │
│ - Debug port protection                │
└────────────────────────────────────────┘
```

**Authentication & Authorization:**

```
Device Authentication (mTLS):
┌──────────────┐                  ┌──────────────┐
│   Device     │ ──── Cert ────▶  │  IoT Cloud   │
│ (Client Cert)│ ◀── Verify ─────  │ (Server Cert)│
└──────────────┘                  └──────────────┘

Both sides verify each other's certificates:
- Device proves identity to cloud
- Cloud proves identity to device
→ Prevents MITM (Man-in-the-Middle) attacks

JSON Web Tokens (JWT) for API:
Header: {"alg": "HS256", "typ": "JWT"}
Payload: {"deviceId": "lock123", "exp": 1672531199}
Signature: HMACSHA256(header + payload, secret)

→ Stateless authentication, can't be forged
```

---

## 8. IoT + Blockchain Integration

### 👤 Góc nhìn Người dùng

**Tại sao kết hợp IoT + Blockchain?**

```
Problem với IoT truyền thống:
- Dữ liệu từ sensors lưu trên server tập trung
- Server có thể sửa đổi dữ liệu lịch sử
- Bạn phải tin tưởng công ty/platform

Giải pháp Blockchain:
- Sensor data → Hash → Lưu on-chain (immutable)
- Không ai (kể cả admin) sửa được lịch sử
- Minh bạch, audit được
```

**Use cases:**

```
🚗 Supply Chain:
   Temperature sensor trong container lạnh
   → Ghi log nhiệt độ mỗi giờ lên blockchain
   → Nếu hàng hỏng, có bằng chứng không thể chối cãi

🏠 Smart Home + DeFi:
   Smart lock tạo access logs on-chain
   → Cho thuê nhà qua smart contract
   → Guest unlock = auto payment released

🌾 Agriculture:
   Soil sensors → Blockchain
   → Nông dân chứng minh organic farming
   → NFT certificate cho sản phẩm
```

### ⚙️ Góc nhìn Hệ thống

**Architecture:**

```
┌─────────────────────────────────────────────────┐
│              Blockchain Layer                    │
│  ┌────────────────────────────────────────────┐ │
│  │ Smart Contract                             │ │
│  │ - Store IoT data hashes                    │ │
│  │ - Trigger actions based on IoT events      │ │
│  │ - Access control (who can unlock device)   │ │
│  └────────────────────────────────────────────┘ │
└──────────────────┬──────────────────────────────┘
                   │ Oracle (Chainlink)
                   │
┌──────────────────▼──────────────────────────────┐
│              IoT Platform                       │
│  ┌────────────────────────────────────────────┐ │
│  │ IoT Gateway/Edge                           │ │
│  │ - Aggregate sensor data                    │ │
│  │ - Hash data                                │ │
│  │ - Send to blockchain via oracle            │ │
│  └────────────────────────────────────────────┘ │
└──────────────────┬──────────────────────────────┘
                   │
        ┌──────────┴──────────┐
        ▼                     ▼
   [Smart Lock]         [Temperature Sensor]
```

**Data Integrity Pattern:**

```solidity
// Smart Contract example

struct SensorReading {
    uint256 timestamp;
    bytes32 dataHash;        // Hash of raw data
    string dataLocation;     // IPFS CID or API endpoint
    address verifier;        // Oracle that verified
}

mapping(bytes32 => SensorReading) public readings;

function recordReading(
    bytes32 _dataHash,
    string memory _dataLocation
) external onlyOracle {
    bytes32 readingId = keccak256(
        abi.encodePacked(block.timestamp, _dataHash)
    );
    
    readings[readingId] = SensorReading({
        timestamp: block.timestamp,
        dataHash: _dataHash,
        dataLocation: _dataLocation,
        verifier: msg.sender
    });
    
    emit ReadingRecorded(readingId, _dataHash);
}

// Anyone can verify:
function verifyReading(bytes32 readingId, bytes memory rawData) 
    public view returns (bool)
{
    return keccak256(rawData) == readings[readingId].dataHash;
}
```

**Workflow:**

```
1. IoT Device collects data
   Temperature: 25.3°C at 2026-01-31 10:00:00
   
2. Gateway hashes data
   Hash: 0x3a5b9c...
   Stores raw data: IPFS (bafybei...)
   
3. Oracle submits to blockchain
   Smart contract stores: {hash, timestamp, IPFS_CID}
   
4. Later verification
   User downloads raw data from IPFS
   Computes hash locally
   Compares with on-chain hash
   → Match = Data authentic
```

---

## 9. Smart Lock Systems

### 👤 Góc nhìn Người dùng

**Smart Lock là gì?**  
Khóa cửa điện tử có thể mở bằng smartphone, mã PIN, thẻ RFID, hoặc vân tay. Không cần chìa khóa vật lý.

**Các loại Smart Locks:**

```
🔐 Deadbolt Replacement:
   - Thay thế khóa cửa cũ hoàn toàn
   - Ví dụ: August Smart Lock, Yale Assure Lock

🔑 Smart Padlock:
   - Ổ khóa thông minh (cho cổng, tủ, xe đạp)
   - Ví dụ: Master Lock Bluetooth Padlock

🚪 Smart Door Handle:
   - Tích hợp tay nắm + khóa
   - Ví dụ: Schlage Encode

📦 Smart Lockbox:
   - Hộp chứa chìa khóa, mở bằng mã
   - Ví dụ: Airbnb hosts dùng cho guests
```

**Features:**

```
✨ Remote Access: Mở khóa từ xa (ở văn phòng mở cửa cho shipper)
⏱️ Temporary Access: Tạo mã tạm thời (expired sau 2 giờ)
📝 Access Logs: Lịch sử ai mở cửa lúc nào
🔔 Notifications: Alert khi cửa mở
🤖 Auto-lock: Tự động khóa sau 30 giây
🔋 Battery Backup: Pin dự phòng (dùng 6-12 tháng)
```

### ⚙️ Góc nhìn Hệ thống

**Smart Lock Architecture:**

```
┌──────────────────────────────────────────────────┐
│         Smart Lock Device                        │
│                                                  │
│  ┌────────────────┐       ┌─────────────────┐   │
│  │ Microcontroller│◀─────▶│ Actuator        │   │
│  │ (ESP32/STM32)  │       │ (Motor/Solenoid)│   │
│  └───────┬────────┘       └─────────────────┘   │
│          │                                       │
│  ┌───────┴────────────────────────────────────┐ │
│  │ Communication Modules                      │ │
│  │ - WiFi (main connectivity)                 │ │
│  │ - Bluetooth (backup/setup)                 │ │
│  │ - Zigbee (optional, mesh network)          │ │
│  └───────┬────────────────────────────────────┘ │
│          │                                       │
│  ┌───────┴────────────────────────────────────┐ │
│  │ Input Methods                              │ │
│  │ - Keypad (PIN entry)                       │ │
│  │ - RFID reader (cards/fobs)                 │ │
│  │ - Fingerprint sensor                       │ │
│  │ - Mechanical key (backup)                  │ │
│  └────────────────────────────────────────────┘ │
│                                                  │
│  ┌────────────────────────────────────────────┐ │
│  │ Status Indicators                          │ │
│  │ - LED (locked/unlocked/error)              │ │
│  │ - Speaker (beep feedback)                  │ │
│  └────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────┘
                     ↕ WiFi/BLE
┌──────────────────────────────────────────────────┐
│         Cloud Platform (Tuya/AWS IoT)            │
│  - Device state management                       │
│  - Access control rules                          │
│  - Temporary password generation                 │
│  - Activity logging                              │
└──────────────────────────────────────────────────┘
                     ↕ HTTPS/REST
┌──────────────────────────────────────────────────┐
│         Mobile App / Web Dashboard               │
│  - Lock/Unlock commands                          │
│  - User management                               │
│  - View access logs                              │
└──────────────────────────────────────────────────┘
```

**Access Control Logic:**

```
Authorization Hierarchy:

1. Owner (Permanent access)
   - Full control
   - Can add/remove users
   - Can view all logs

2. Admin (Delegated access)
   - Can unlock
   - Can create temporary codes
   - Can view logs

3. Guest (Temporary access)
   - Can unlock (with time restrictions)
   - Limited to specific time windows
   - Auto-expire passwords

4. One-time access (Delivery/Service)
   - Single-use code
   - Expires after first use or timeout
```

---

## 11. Phân tích Implementation trong VinaLib

### 📋 Tổng quan

Hệ thống VinaLib tích hợp IoT để quản lý quyền truy cập vật lý vào sách thực tế, sử dụng **Tuya Smart Locks** làm giải pháp chính. Implementation hiện tại ở giai đoạn development với **Mock Server** giả lập Tuya API để testlocal mà không cần phần cứng thật hoặc kết nối với cloud service.

### 🔧 Components

#### 1. IoT Controller (`backend/src/modules/IoT/controller.js`)

**Vai trò trong hệ thống:**

Controller này đóng vai trò như "traffic director" - router các requests từ frontend tới đúng IoT service adapter (mock hoặc production).

**Cơ chế Environment-aware:**

Hệ thống tự động switch giữa mock và production adapter dựa trên environment variable:
- **Local development**: Sử dụng `mock-iot.adapter.js` → Calls mock server (fast, free, offline)
- **Production**: Sử dụng production adapter → Calls real Tuya Cloud API (requires API credentials, internet)

**Cách hoạt động:**

Khi code chạy, nó kiểm tra `process.env.NODE_ENV`:
- Nếu = `'local'` hoặc có flag override → Load mock adapter
- Nếu = `'production'` → Load production adapter (sẽ gọi Tuya Cloud thật)

**API Endpoint exposed:**

Backend expose một POST endpoint: `/iot/:id/unlock`

**Input required:**
- `deviceId` trong URL params (ví dụ: "lock-001" - ID của smart lock tủ sách)
- Optional: booking info để log/audit

**Output trả về:**
- **Nếu thành công**: JSON object chứa:
  - `success`: true
  - `ticket`: Object chứa ticket_id và expire_time (seconds)
  - Ticket này được user dùng để mở khóa vật lý

- **Nếu thất bại**: Error message với status 500

**Design Pattern: Adapter Pattern**

Lý do dùng pattern này:
- **Flexibility**: Dễ dàng swap implementation (mock ↔ production) chỉ bằng cách thay đổi env variable
- **Testability**: Test business logic mà không cần real hardware
- **Separation of concerns**: Controller không cần biết chi tiết IoT API nào được dùng

#### 2. Mock IoT Adapter (`backend/src/modules/IoT/adapters/mock-iot.adapter.js`)

**Chức năng chính:**

Adapter này giả lập perfect như Tuya API thật sẽ hoạt động, nhưng thực tế chỉ gọi local mock server.

**Quy trình giả lập:**

**Bước 1: Nhận request từ Controller**
- Controller gọi: `MockIoTAdapter.generatePasscode(deviceId)`
- deviceId = "lock-001" (ID của tủ sách cần mở)

**Bước 2: Construct API call**
- Tạo POST request tới mock server (port 4000)
- Endpoint path giống y hệt Tuya API thật: `/tuya/v1.0/devices/{deviceId}/door-lock/password-free/door-operate`
- Method: POST (không cần body data trong mock)

**Bước 3: Gọi mock server**
- Sử dụng axios library để HTTP call
- Target: `http://localhost:4000/tuya/v1.0/devices/lock-001/door-lock/password-free/door-operate`

**Bước 4: Parse response**
- Mock server trả về JSON với structure giống Tuya thật
- Check `response.data.success`:
  - Nếu true → Extract `response.data.result` (chứa ticket_id + expire_time)
  - Nếu false → Throw error với message

**Bước 5: Return về Controller**
- Return ticket object hoặc throw exception

**Tại sao cần Mock Adapter?**

✅ **Development speed**: Không cần đợi real API setup
✅ **Cost saving**: Tuya API có rate limits và potential costs
✅ **Offline development**: Làm việc không cần internet
✅ **Deterministic testing**: Mock luôn trả về predictable results

**Production Adapter (Future):**

Khi migrate lên production, sẽ tạo `tuya-iot.adapter.js` với:
- Real Tuya Cloud endpoint: `https://openapi.tuyaus.com`
- OAuth 2.0 authentication flow:
  1. Get access token using Client ID + Secret
  2. Include token in Authorization header
  3. Sign requests với timestamp để prevent replay attacks
- Retry logic cho network failures
- Rate limiting compliance (Tuya có giới hạn requests/second)

#### 3. Mock Server (`mock-server/index.js`)

**Vai trò:**

Mock server là Express.js server chạy độc lập (port 4000) giả lập các Tuya Cloud APIs.

**Tuya Mock Endpoints:**

**Endpoint 1: Create Temporary Password**
- Path: `POST /tuya/v1.0/devices/:id/door-lock/password-free/door-operate`
- Purpose: Generate temporary access code/ticket để unlock smart lock
- Behavior:
  - Extract deviceId từ URL params
  - Generate unique ticket_id (dùng timestamp để ensure uniqueness)
  - Set expire_time = 3600 seconds (1 hour default)
  - Return success response với ticket info

**Endpoint 2: Get Access Logs**
- Path: `GET /tuya/v1.0/devices/:id/logs`
- Purpose: Query access logs của smart lock (ai đã unlock, khi nào)
- Behavior:
  - Return mock array of access events
  - Mỗi event có: timestamp, action ("unlock"), user_info
- **Future use**: Dữ liệu này sẽ dùng cho:
  - Audit trail (tracking who accessed what)
  - Yield farming logic (lockers với nhiều unlocks earn more)
  - Analytics (peak hours, popular books)

**Mock Response Structure:**

Response format giống y hệt Tuya Cloud để compatibility:
- `success`: Boolean (true/false)
- `t`: Timestamp in milliseconds (khi response được tạo)
- `result`: Object chứa actual data:
  - `ticket_id`: String unique identifier
  - `expire_time`: Number (seconds ticket còn valid)

**Ví dụ response cụ thể:**
```
{
  success: true,
  t: 1706683200000,
  result: {
    ticket_id: "mock-ticket-1706683200000",
    expire_time: 3600
  }
}
```

**Tại sao không dùng simple JSON file thay vì server?**
- Mock server cho phép simulate: Delays, errors, rate limiting
- Có thể log requests để debugging
- Gần với production environment hơn

### 📊 Data Flow (Complete Workflow)

**Toàn bộ quy trình từ khi user book sách đến khi lấy sách vật lý:**

**Phase 1: Booking Completion (On-chain)**

1. **User completes booking trên UI**
   - Chọn sách, duration, confirm deposit
   - Frontend gửi transaction lên smart contract

2. **Smart Contract processing**
   - PolicyEngine evaluate (trust score + tier + deposit)
   - Nếu approved → Create rental record
   - Mint RentalAgreementSBT cho user
   - Update book status → RENTED
   - Emit event: `BookRented(rentalId, bookId, renter, startTime, endTime)`

**Phase 2: Event Detection (Backend Listener)**

3. **Backend event listener detecting**
   - Backend có service subscribe to blockchain events
   - Detect event `BookRented` được emit
   - Parse event data:
     - rentalId: Unique ID cho rental này
     - bookId: Sách nào được thuê
     - renter: Address của người thuê
     - Physical location info: bookId maps to lockerId (tủ số mấy)

**Phase 3: IoT Ticket Generation**

4. **Backend triggers IoT unlock flow**
   - Backend gọi internal API: `POST /iot/lock-001/unlock`
   - Where "lock-001" được map từ bookId → lockerId
   - Ví dụ: Book #42 → stored in Locker A → locker ID = "lock-001"

5. **IoT Controller receives request**
   - Extract deviceId từ params ("lock-001")
   - Check environment: isLocal = true (dev mode)
   - Load MockIoTAdapter

6. **Mock Adapter calls Mock Server**
   - Construct URL: `http://localhost:4000/tuya/v1.0/devices/lock-001/door-lock/password-free/door-operate`
   - Send POST request với axios
   - Wait for response

**Phase 4: Mock Server Processing**

7. **Mock Server generates ticket**
   - Receive request for device "lock-001"
   - Generate unique ticket_id: "mock-ticket-{timestamp}"
   - Calculate expire_time: Current time + 3600 seconds
   - Return response object

8. **Response bubbles back up**
   - Mock Server → Mock Adapter
   - Mock Adapter → Controller
   - Controller → Backend business logic
   - Backend may store ticket in database for record

**Phase 5: User Notification**

9. **Frontend displays unlock info**
   - Backend sends ticket info to frontend via WebSocket/polling
   - Frontend displays modal:
     ```
     🎉 Đặt sách thành công!
     
     📍 Vị trí: Tủ số 5, Ngăn A2
     🔑 Mã mở tủ: ABC-123-456
     ⏰ Hết hạn: 18:00 hôm nay
     
     ℹ️ Hướng dẫn:
     1. Đến thư viện, tìm tủ số 5
     2. Nhập mã trên màn hình khóa
     3. Lấy sách và đóng tủ lại
     ```

**Phase 6: Physical Access**

10. **User đến library**
    - Tìm tủ số 5
    - Smart lock có keypad/app input

11. **User nhập ticket code**
    - Input: ABC-123-456
    - Smart lock validate ticket với Tuya Cloud (trong production)
    - Trong mock: Lock chỉ cần mở (không validate thật)

12. **Lock opens**
    - Solenoid/motor unlock
    - User mở cửa tủ, lấy sách
    - Đóng cửa lại

13. **Lock logs event**
    - Record unlock event:
      - Timestamp: 2026-01-31 14:30:00
      - Ticket used: ABC-123-456
      - Action: UNLOCK_SUCCESS
    - Send log to Tuya Cloud (production)
    - Mock: Store in mock server memory

**Phase 7: Audit Trail (Future với Chainlink)**

14. **IoT log → Blockchain**
    - Tuya Cloud pushes logs to backend webhook
    - Backend aggregates logs
    - Chainlink Functions call để hash log data
    - Store hash on-chain:
      ```
      Hash(unlock_event) → 0x3a4f9b...
      Stored on smart contract
      → Immutable proof of access
      ```

15. **Trust score update**
    - Successful access recorded
    - If renter returns book on time → Trust score increase
    - If late/damaged → Score decrease

### 🎯 Use Case Details

**Scenario: User "Alice" thuê sách Harry Potter**

**End-to-end narrative:**

**T+0min: Booking**
- Alice browse sách trên UI, thấy Harry Potter (#42)
- Click "Thuê sách", chọn 7 ngày
- System tính deposit: $10 (Tier B book)
- Alice confirm, MetaMask popup → Sign transaction
- Blockchain process: Trust score = 75 → AUTO approved
- Rental record created on-chain

**T+1min: Ticket Generation**
- Backend detect `BookRented` event
- Lookup: Book #42 → Locker 5, Slot A2 → deviceId: "lock-005"
- Call IoT service: generatePasscode("lock-005")
- Mock server returns: ticket = "TK-123456", expires in 1 hour
- Frontend updates: "Bạn có 1 giờ để đến lấy sách"

**T+15min: Physical Access**
- Alice arrives at library
- Finds Locker #5
- Smart lock display: "Enter Code"
- Alice inputs: TK-123456
- Lock beeps, LED green, unlocks
- Alice opens door, takes book
- Closes door → Auto-locks

**T+15min +30s: Logging**
- Smart lock sends event to mock server (simulates Tuya Cloud)
- Log entry created:
  ```
  {
    timestamp: "2026-01-31T14:15:30Z",
    deviceId: "lock-005",
    action: "UNLOCK",
    ticketId: "TK-123456",
    success: true
  }
  ```
- Future: This log hash will go on-chain

**T+7 days: Return**
- Alice returns book
- Admin verifies book condition
- Smart contract: Calculate late fee = 0 (on time)
- Refund full deposit to Alice
- Update trust score: 75 → 78 (good behavior reward)

### 🚀 Roadmap & Next Steps

**Phase 1: ✅ Mock Integration (Completed)**

Đã hoàn thành:
- Express mock server với Tuya-compatible endpoints
- Mock adapter với generatePasscode function
- Controller routing logic
- Basic unlock flow hoạt động end-to-end

**Phase 2: 🔄 Production Integration (In Progress)**

**Task 1: Tuya Cloud API Setup**
- Register Tuya Developer Account
- Create Cloud Project
- Obtain Client ID + Secret
- Configure redirect URLs

**Task 2: OAuth 2.0 Flow**
- Implement token acquisition:
  1. POST to /v1.0/token với client credentials
  2. Receive access_token (valid 2 hours)
  3. Refresh token before expiry
- Store tokens securely (encrypted environment variables)

**Task 3: Real API Adapter**
- Create `tuya-iot.adapter.js`
- Implement actual API calls:
  - Create temporary password
  - Query device status
  - Get access logs
  - Handle API errors (rate limits, device offline)

**Task 4: Error Handling & Retry**
- Network timeouts → Retry with exponential backoff
- Device offline → Queue request, retry later
- Invalid ticket → Generate new one
- Rate limit hit → Wait + retry

**Phase 3: 📝 Advanced Features (Planned)**

**Feature 1: Dynamic Pricing based on IoT Data**

Concept: Giá thuê thay đổi dựa trên demand (tracked by unlock frequency)

Implementation:
- Query Tuya logs: Count unlocks per hour for each locker
- Identify peak hours (ví dụ: 12PM-2PM lunch break = high demand)
- Adjust rental price dynamically:
  ```
  Base price: $5/day
  
  Peak hours (12-2PM, 5-7PM):
  - Unlock count > 20/hour → Price × 1.5 = $7.50/day
  
  Off-peak (night):
  - Unlock count < 5/hour → Price × 0.8 = $4/day
  ```
- Smart contract adjustable pricing via oracle input

**Feature 2: Yield Farming for Locker Owners**

Concept: Khích lệ người cung cấp smart lockers bằng revenue sharing

Flow:
1. **Locker Owner Registration**
   - Owner stakes smart lock device (register deviceId)
   - Deposit collateral (ensure device uptime)
   - Set commission rate (owner earns % of rentals)

2. **Usage Tracking**
   - Each unlock event tracked
   - Count monthly active unlocks per locker
   - Higher usage → Higher rewards

3. **Reward Distribution**
   - Formula: `Reward = (UnlockCount × BaseReward) + (Revenue × CommissionRate)`
   - Example:
     ```
     Locker #5: 100 unlocks/month
     Revenue generated: $500
     Base reward: 0.1 token/unlock
     Commission: 10%
     
     Total reward = (100 × 0.1) + (500 × 0.1)
                  = 10 + 50
                  = 60 tokens
     ```

4. **Automated via Chainlink Automation**
   - Monthly checkUpkeep: Query IoT logs
   - Calculate rewards for all lockers
   - performUpkeep: Distribute tokens to owners

**Feature 3: Blockchain Verification of Access**

Concept: On-chain immutable proof of physical access

Architecture:
```
Physical Access → IoT Log → Hash → Chainlink Functions → Smart Contract
```

Detailed Flow:
1. **Smart lock unlocks**
   - Generate access log:
     ```json
     {
       "deviceId": "lock-005",
       "timestamp": "2026-01-31T14:15:30Z",
       "ticketId": "TK-123456",
       "renterId": "0xABC...",
       "bookId": 42
     }
     ```

2. **Tuya Cloud webhook**
   - Tuya pushes log to backend endpoint: `/webhooks/tuya/access`
   - Backend receives và validates signature

3. **Batch aggregation**
   - Backend collects logs every 10 minutes
   - Aggregate multiple access events into Merkle tree
   - Calculate Merkle root hash

4. **Chainlink Functions submission**
   - Backend triggers Chainlink Functions request
   - JS code:
     ```javascript
     // Verify signature from Tuya
     const verified = verifyTuyaSignature(logs, signature)
     if (!verified) return {error: "Invalid signature"}
     
     // Hash logs
     const merkleRoot = calculateMerkleRoot(logs)
     
     // Return to contract
     return {merkleRoot, logsCount: logs.length}
     ```

5. **On-chain storage**
   - Smart contract stores:
     ```solidity
     struct AccessBatch {
         bytes32 merkleRoot;
         uint256 timestamp;
         uint256 logsCount;
     }
     mapping(uint256 => AccessBatch) public accessBatches;
     ```

6. **Verification by anyone**
   - User can prove they accessed at specific time
   - Provide: original log + Merkle proof
   - Contract verifies proof against stored root
   - If valid → Proof confirmed on-chain

**Feature 4: Geofencing**

Concept: Prevent remote unlocks (must be near device physically)

Implementation:
- User phải enable location on app
- When request unlock:
  1. Frontend gets GPS coordinates
  2. Send to backend along with unlock request
  3. Backend checks:
     - User location within 100m radius of library?
     - Yes → Generate ticket
     - No → Reject with "You must be on-site"
- Prevent scenarios:
  - User shares ticket với friend remotely
  - Hacker unlocks from другой country

### 💡 Best Practices Currently Applied

**1. Separation of Concerns**

Architecture layers rõ ràng:
- Controller: HTTP routing, request/response handling
- Adapter: External service integration logic
- Mock Server: Simulation of third-party API

Benefit: Easy to test, maintain, và swap implementations

**2. Comprehensive Error Handling**

Mọi potential failure points đều có try-catch:
- Network failures: Catch axios errors
- Invalid responses: Validate response structure before processing
- Missing data: Check required fields existence
- User-friendly errors: Return meaningful messages, không expose internal details

**3. Environment-aware Configuration**

Code tự động adapt based on environment:
- `NODE_ENV=local` → Use mocks, no real API calls
- `NODE_ENV=production` → Use real services, production endpoints
- Benefit: Same codebase works both local dev và production

**4. Logging for Debugging**

Every major action logged:
- Incoming requests logged với params
- Adapter calls logged với timestamps
- Responses logged (success/failure)
- Easy to trace issues khi debugging

### 📈 Metrics & Monitoring (Future Implementation)

**What to track:**

**1. Unlock Success Rate**
- Target: > 99% success
- Metric: `(successful_unlocks / total_unlock_attempts) × 100`
- Alert if < 95%: Indicates device issues or network problems

**2. Average Response Time**
- Target: < 500ms (from API call to ticket generation)
- Metric: Track latency at each step:
  - Controller → Adapter: < 10ms
  - Adapter → Tuya API: < 300ms
  - Total round trip: < 500ms
- Alert if > 1000ms: User experience degraded

**3. Ticket Utilization Rate**
- Metric: `(tickets_used / tickets_generated) × 100`
- Expected: ~80-90% (some users may cancel)
- If < 50%: Investigate why users not picking up books

**4. Device Health**
- **Battery Level**: Track per device
  - Alert if < 20%: Schedule battery replacement
- **Connectivity Status**: 
  - Online/offline tracking
  - Alert if offline > 5 minutes
- **Lock Mechanism Health**:
  - Failed unlock attempts (ticket valid but lock didn't open)
  - Alert if failures > 3 consecutive: Maintenance needed

**5. Security Metrics**
- Invalid ticket attempts per device
- Spike detection: > 10 failed attempts in 1 hour = potential attack
- Geofencing violations
- Expired ticket usage attempts

**Monitoring Tools to integrate:**
- Grafana dashboards for visualization
- Prometheus for metrics collection
- PagerDuty for critical alerts (device offline, high failure rate)
- Sentry for error tracking


---

## Phụ lục: Công cụ và Tài nguyên

### Development Tools

**1. IoT Platforms:**
```
- Tuya IoT Platform: https://iot.tuya.com
- AWS IoT Core: https://aws.amazon.com/iot-core
- Azure IoT Hub: https://azure.microsoft.com/en-us/services/iot-hub
- ThingSpeak: https://thingspeak.com (free data logging)
```

**2. Hardware:**
```
- ESP32 DevKit (~$5): WiFi + Bluetooth SoC
- Raspberry Pi 4 (~$35): Full Linux computer
- Arduino Uno (~$20): Beginner-friendly microcontroller
- NVIDIA Jetson Nano (~$99): Edge AI computing
```

**3. MQTT Brokers:**
```
- Mosquitto (open-source, local)
- HiveMQ (cloud, free tier)
- AWS IoT Core (managed, pay-as-you-go)
```

**4. Testing Tools:**
```
- MQTT.fx: Desktop MQTT client for testing
- Postman: Test REST APIs
- Wireshark: Network traffic analysis
- IoT Security Scanner: Check vulnerabilities
```

### Resources

- **MQTT Specs**: https://mqtt.org
- **Tuya API Docs**: https://developer.tuya.com
- **ESP32 Documentation**: https://docs.espressif.com
- **IoT Security Guide**: OWASP IoT Top 10

---

## Tổng kết

IoT đang chuyển đổi cách chúng ta tương tác với thế giới vật lý:

**🔹 Core Concepts:**  
Từ Dumb devices → Smart connected devices  
Từ Manual control → Automation  
Từ Reactive → Predictive (AI/ML on sensor data)  
Từ Isolated systems → Integrated ecosystems

**🔹 Technology Stack:**
- ✅ **Sensors & Actuators**: Collect data + Execute actions
- ✅ **Communication Protocols**: MQTT, CoAP, HTTP
- ✅ **Platforms**: AWS IoT, Azure IoT, Tuya
- ✅ **Edge Computing**: Real-time processing at source
- ✅ **Security**: Multi-layer defense (device to cloud)

**🔹 VinaLib Integration:**
- Smart Locks for physical book access
- Temporary access codes (time-limited)
- Access logs (audit trail)
- Future: Blockchain verification + Yield farming

**🎯 Future Trends:**
- **5G + IoT**: Ultra-low latency for autonomous systems
- **Edge AI**: Intelligence at device level (no cloud needed)
- **Digital Twins**: Virtual replicas of physical devices
- **IoT + Web3**: Decentralized device ownership & data

**Lưu ý cho Developers:**
- Always implement security from day 1 (not afterthought)
- Use TLS/encryption for all communications
- Plan for offline scenarios (edge processing)
- Monitor device health (battery, connectivity)
- Keep firmware updatable (OTA - Over The Air)
