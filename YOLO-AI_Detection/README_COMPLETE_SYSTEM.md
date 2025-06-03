# 🔥 Complete ESP32-CAM Fire Detection System

A comprehensive real-time fire detection system using ESP32-CAM, YOLO AI models, and web-based monitoring dashboard.

## 🌟 System Overview

This project provides a complete end-to-end fire detection solution:

1. **ESP32-CAM Device** - Captures images and sends to AI server
2. **AI Server** - Processes images using trained YOLO fire detection model  
3. **Real-time Dashboard** - Web interface for monitoring and testing
4. **CoreIOT Integration** - Optional IoT platform connectivity
5. **Database Storage** - SQLite database for detection history

## 🚀 Quick Start

```bash
# Navigate to the project directory
cd WebApp_IOT/Yolo-AI_Detection

# Run the complete system launcher
python start_complete_system.py
```

This will automatically:
- ✅ Check all dependencies
- ✅ Install required packages
- ✅ Start AI server (port 5001)
- ✅ Start dashboard (port 8080)
- ✅ Start CoreIOT integration (optional)
- ✅ Run system tests
- ✅ Display access URLs

## 📊 Access the System

Once running, access these URLs:

- **🔥 Fire Detection Dashboard**: http://localhost:8080
- **🤖 AI Server API**: http://localhost:5001
- **📈 API Status**: http://localhost:5001/api/status

## 🔧 System Components

### 1. AI Server (`server/ai_server.py`)
- **Port**: 5001
- **Function**: Processes images using YOLO fire detection model
- **Models**: 
  - `yolov8n.pt` - General object detection
  - `fire_detection_best.pt` - Your trained fire detection model
- **API Endpoints**:
  - `POST /api/detect` - Process image for fire detection
  - `GET /api/status` - Server health status
  - `GET /api/models` - Available models
  - `GET /api/health` - Health check

### 2. Real-time Dashboard (`fire_detection_dashboard.py`)
- **Port**: 8080
- **Function**: Web-based monitoring interface
- **Features**:
  - ✅ Real-time device status monitoring
  - ✅ Live detection feed with WebSocket updates
  - ✅ Detection statistics and charts
  - ✅ Image upload testing interface
  - ✅ Bounding box visualization
  - ✅ Alert level indicators (CRITICAL, HIGH, MEDIUM, LOW)
  - ✅ Responsive modern UI with Bootstrap 5

### 3. CoreIOT Integration (`coreiot_integration.py`)
- **Function**: Connects to CoreIOT platform for enterprise integration
- **Features**:
  - ✅ Subscribe to shared attributes
  - ✅ Process images from multiple ESP32-CAM devices
  - ✅ Store detection history in SQLite database
  - ✅ Real-time alerts and notifications
  - ✅ Device status tracking

### 4. ESP32-CAM Code (`src/hybrid_ai_processing.cpp`)
- **Function**: Camera firmware for ESP32-CAM devices
- **Features**:
  - ✅ WiFi connectivity
  - ✅ Camera image capture (configurable resolution)
  - ✅ Base64 image encoding
  - ✅ HTTP POST to AI server
  - ✅ Configurable detection intervals
  - ✅ LED indicators for status
  - ✅ MQTT publishing for alerts

## 🔥 Testing Fire Detection

### Method 1: Dashboard Upload

1. **Open Dashboard**: http://localhost:8080
2. **Scroll to "Test Fire Detection"** section
3. **Upload Image**: Drag & drop or click to select
4. **View Results**: See detection results with bounding boxes

### Method 2: Real Fire Images Test

```bash
# Run test with real fire dataset images
python test_with_real_data.py
```

This downloads real fire images and tests detection accuracy.

### Method 3: ESP32-CAM Integration

1. **Flash ESP32-CAM** with `src/hybrid_ai_processing.cpp`
2. **Configure WiFi** credentials in the code
3. **Set AI Server URL** to your server IP:5001
4. **Monitor Dashboard** for real-time detections

## 📱 Dashboard Features

### Real-time Statistics
- **Total Detections (24h)**: Number of images processed
- **Fire Alerts (24h)**: Number of fire detections
- **Active Devices**: Currently connected ESP32-CAM devices
- **Alert Rate**: Percentage of detections that were fire alerts

### Device Status Monitoring
- **Device List**: All connected ESP32-CAM devices
- **Last Seen**: Time since last communication
- **Status**: ACTIVE/OFFLINE indicator
- **Detection Count**: Total detections per device

### Live Detection Feed
- **Recent Detections**: Real-time list of latest detections
- **Alert Levels**: Color-coded severity (CRITICAL/HIGH/MEDIUM/LOW)
- **Confidence Scores**: AI model confidence percentages
- **Processing Times**: Performance metrics

### Detection Chart
- **24-Hour Activity**: Hourly breakdown of detections
- **Fire vs Total**: Comparison of fire alerts to total detections
- **Interactive Chart**: Hover for detailed information

### Image Testing Interface
- **Drag & Drop Upload**: Easy image testing
- **Real-time Processing**: Live processing status
- **Bounding Box Visualization**: Fire detection regions highlighted
- **Detailed Results**: Confidence scores and coordinates

## 🔧 Configuration

### AI Server Configuration
Edit `server/ai_server.py`:
```python
CONFIG = {
    "default_confidence": 0.5,        # Detection threshold
    "max_image_size": 5 * 1024 * 1024, # 5MB max
    "models_dir": "models"            # Model storage directory
}
```

### ESP32-CAM Configuration
Edit `src/hybrid_ai_processing.cpp`:
```cpp
#define WIFI_SSID "Your_WiFi_Name"
#define WIFI_PASSWORD "Your_WiFi_Password"
#define AI_SERVER_URL "http://your-server-ip:5001/api/detect"
#define CAPTURE_INTERVAL 5000         // 5 seconds
#define DETECTION_THRESHOLD 0.7       // 70% confidence
```

### Dashboard Configuration
Edit `fire_detection_dashboard.py`:
```python
class FireDetectionDashboard:
    def __init__(self):
        self.ai_server_url = "http://localhost:5001"
        self.detection_threshold = 0.5
```

### CoreIOT Configuration
Edit `coreiot_integration.py`:
```python
COREIOT_CONFIG = {
    "coreiot_token": "YOUR_COREIOT_TOKEN",
    "coreiot_base_url": "https://api.coreiot.io",
    "device_id": "ESP32CAM_FIRE_DETECTOR",
    "detection_threshold": 0.5
}
```

## 📊 Database Schema

The system uses SQLite database with these tables:

### `fire_detections`
- `id` - Auto-increment primary key
- `device_id` - ESP32-CAM device identifier
- `timestamp` - Detection timestamp
- `fire_detected` - Boolean fire detection result
- `confidence` - AI confidence score (0.0-1.0)
- `bbox` - JSON bounding box coordinates
- `image_size` - JSON image dimensions
- `processing_time_ms` - Processing time in milliseconds
- `alert_level` - Alert severity (CRITICAL/HIGH/MEDIUM/LOW/NONE)
- `image_data` - Base64 image thumbnail

### `device_status`
- `device_id` - Primary key device identifier
- `last_seen` - Last communication timestamp
- `status` - Device status (ACTIVE/OFFLINE)
- `total_detections` - Total detection count
- `fire_alerts` - Fire alert count

## 🚨 Alert System

### Alert Levels
- **CRITICAL** (>80% confidence): 🔴 Immediate action required
- **HIGH** (60-80% confidence): 🟠 High priority alert  
- **MEDIUM** (40-60% confidence): 🟡 Monitor situation
- **LOW** (<40% confidence): 🟢 Possible detection
- **NONE** (no fire): ⚪ All clear

### Alert Channels
- ✅ **Dashboard Notifications**: Real-time browser alerts
- ✅ **WebSocket Broadcasting**: Live updates to all clients
- ✅ **Database Logging**: Persistent storage
- ✅ **CoreIOT Integration**: Enterprise platform alerts
- 🔄 **Email/SMS** (configurable): External notifications

## 🔧 Hardware Setup

### ESP32-CAM Wiring
```
ESP32-CAM AI-Thinker Board:
- VCC → 5V
- GND → GND  
- U0R → Pin 0 (for programming)
- U0T → Pin 1 (for programming)
- GPIO 0 → GND (for programming mode)
```

### Camera Configuration
```cpp
config.frame_size = FRAMESIZE_VGA;    // 640x480 for good accuracy
config.jpeg_quality = 12;             // High quality (10-63)
config.fb_count = 1;                  // Frame buffer count
```

## 📈 Performance Optimization

### Server Performance
- **GPU Acceleration**: Use CUDA-enabled PyTorch for faster inference
- **Model Optimization**: Use YOLOv8n for speed, YOLOv8m for accuracy
- **Image Preprocessing**: Resize images to optimal resolution
- **Caching**: Cache model loading for faster processing

### Network Optimization
- **Image Compression**: Optimize JPEG quality vs file size
- **Connection Pooling**: Reuse HTTP connections
- **Batch Processing**: Process multiple images together
- **Edge Caching**: Cache results for repeated images

### Database Optimization
- **Indexes**: Add indexes on frequently queried columns
- **Cleanup**: Implement automatic old data cleanup
- **Backup**: Regular database backups
- **Sharding**: Split data across multiple databases for scale

## 🔒 Security Considerations

### Production Deployment
- ✅ **HTTPS**: Use SSL/TLS for all communications
- ✅ **Authentication**: Implement API key authentication
- ✅ **Rate Limiting**: Prevent API abuse
- ✅ **Input Validation**: Validate all image uploads
- ✅ **CORS**: Configure proper cross-origin policies
- ✅ **Firewall**: Restrict network access
- ✅ **Monitoring**: Log all activities

## 🚀 Deployment Options

### Local Deployment
```bash
# Direct Python execution
python start_fire_detection_system.py
```

### Docker Deployment
```bash
# Build Docker image
docker build -t fire-detection-system .

# Run container
docker run -p 5001:5001 -p 8080:8080 fire-detection-system
```

### Cloud Deployment
- **AWS EC2**: Deploy on elastic compute instances
- **Google Cloud**: Use compute engine or cloud run
- **Azure**: Deploy on virtual machines
- **Heroku**: Use container deployment

## 📊 Monitoring & Logging

### System Logs
- **AI Server**: Processing times, model performance
- **Dashboard**: User interactions, system status
- **ESP32-CAM**: Connection status, capture intervals
- **CoreIOT**: Integration status, message counts

### Performance Metrics
- **Detection Accuracy**: True positive/negative rates
- **Processing Speed**: Average inference time
- **System Uptime**: Service availability
- **Device Connectivity**: Connection success rates

## 🆘 Troubleshooting

### Common Issues

#### AI Server Not Starting
```bash
# Check Python version
python --version  # Should be 3.8+

# Check model file
ls server/models/fire_detection_best.pt

# Check dependencies
pip install -r server/requirements.txt
```

#### Dashboard Not Loading
```bash
# Check port availability
netstat -an | grep 8080

# Install dashboard dependencies
pip install -r dashboard_requirements.txt

# Check AI server connection
curl http://localhost:5001/api/status
```

#### ESP32-CAM Connection Issues
- ✅ Verify WiFi credentials
- ✅ Check server IP address
- ✅ Ensure adequate power supply (5V 2A)
- ✅ Check serial monitor for error messages

#### Poor Detection Accuracy
- ✅ Improve lighting conditions
- ✅ Use higher resolution images
- ✅ Retrain model with more diverse data
- ✅ Adjust confidence threshold

## 🎯 Next Steps

### Enhancements
- [ ] **Mobile App**: iOS/Android mobile application
- [ ] **Multi-Camera**: Support for multiple camera feeds
- [ ] **Cloud Storage**: Store images in cloud storage
- [ ] **Machine Learning**: Continuous model improvement
- [ ] **Integration**: Connect with fire department systems
- [ ] **Analytics**: Advanced detection analytics
- [ ] **Edge AI**: Run inference directly on ESP32

### Production Features
- [ ] **Load Balancing**: Handle multiple devices
- [ ] **Auto-scaling**: Dynamic resource allocation
- [ ] **Monitoring**: Comprehensive system monitoring
- [ ] **Backup**: Automated backup systems
- [ ] **Security**: Enterprise security features

## 📝 License

This project is open source and available under the MIT License.

## 🙏 Acknowledgments

- **Ultralytics YOLO**: For the excellent YOLO implementation
- **ESP32 Community**: For Arduino libraries and examples
- **Bootstrap**: For the responsive UI framework
- **Chart.js**: For beautiful data visualization
- **Flask**: For the web framework
- **Your Team**: For the fire detection model training

---

🔥 **Ready to detect fires with AI!** Start with `python start_fire_detection_system.py` and monitor at http://localhost:8080 
