# Web Bluetooth Medical/Fitness Dashboard

A comprehensive web application that connects directly to physical hardware (heart rate monitors, thermometers, ESP32 devices) using the **Web Bluetooth API**. This project demonstrates modern web technologies including ASP.NET Core MVC, jQuery, Web Bluetooth API, and data processing with LINQ.

🌐 **Live Demo:** [https://health-dashboard-1ccg.onrender.com/](https://health-dashboard-1ccg.onrender.com/)

---

## 🎓 Academic Project - CSI418L Web Programming Lab Expo

This is a final project for **CSI418L - Web Programming Lab Expo** demonstrating:
- HTML5 & CSS3
- jQuery & jQuery Animate
- ASP.NET Core MVC (C#) - .NET 10.0
- DTOVM (Data Transfer Object View Model) pattern
- LINQ (Language Integrated Query) with GroupBy and advanced queries
- PHP for additional data processing
- JSON & XML data handling
- Web Bluetooth API for hardware connectivity

### 📄 Project Documents
| Document | Description |
|----------|-------------|
| [📑 Project Report](Web_Report.pdf) | Detailed project documentation and analysis |
| [📊 Presentation](WebBluetoothPresentation.pdf) | Project presentation slides |
| [🖼️ Poster](WebBluePoster.png) | Project expo poster |

---

## 🌟 Features

### Web Bluetooth Integration
- **Heart Rate Monitor Support**: Connect to standard Bluetooth LE heart rate monitors
- **Thermometer Support**: Connect to health thermometer devices
- **ESP32 Device Support**: Connect to custom ESP32-based sensors
- **Real-time Data Streaming**: Live data updates with low-latency visualization
- **Binary Data Handling**: Proper GATT characteristic parsing

### Data Management
- **Real-time Visualization**: Live charts and animated displays
- **Historical Data**: Store and retrieve past readings
- **LINQ Queries**: Advanced data aggregation and grouping (GroupBy, Average, Filtering, Sorting, Statistical analysis)
- **Export Options**: JSON export and XML export (using LINQ to XML)
- **PHP Processing**: Additional server-side data analysis

### Beautiful UI
- **Modern Gradient Design**: Eye-catching color schemes
- **Smooth Animations**: jQuery animate effects
- **Responsive Layout**: Works on desktop and mobile
- **Real-time Updates**: Pulse animations for new data
- **Interactive Charts**: Visual data representation

### External API Integration
- **Health Tips**: Fetch health quotes from external APIs
- **COVID-19 Data**: Global statistics display
- **Air Quality**: Environmental health data

---

## 🛠️ Technologies Used

| Category | Technologies |
|----------|-------------|
| **Backend** | ASP.NET Core MVC (.NET 10.0), C#, LINQ, PHP |
| **Frontend** | HTML5, CSS3, JavaScript (ES6+), jQuery, jQuery Animate, Web Bluetooth API, Bootstrap 5, Font Awesome |
| **Data Formats** | JSON, XML (LINQ to XML) |
| **Deployment** | Docker, Render |

---

## 🚀 Getting Started

### Prerequisites
- **.NET SDK 10.0** (or .NET 6.0+): [Download here](https://dotnet.microsoft.com/download)
- **Git**: For cloning the repository
- **Modern Browser**: Chrome 56+, Edge 79+, or Opera 43+ (for Web Bluetooth support)

### Quick Start

```bash
# Clone the repository
git clone https://github.com/eliaghazal/Web-Final.git
cd Web-Final/HealthDashboard

# Restore dependencies
dotnet restore

# Build the project
dotnet build

# Run the application
dotnet run
```

The application will start and display:
```
Now listening on: http://localhost:5000
Now listening on: https://localhost:5001
```

### Access the Dashboard
- **HTTPS** (recommended): https://localhost:5001
- **HTTP**: http://localhost:5000

### Development with Hot Reload
```bash
dotnet watch run
```

### Production Build
```bash
dotnet publish -c Release -o ./publish
```

---

## 🐳 Docker Deployment

### Build and Run Locally

```bash
# Build the Docker image
docker build -t health-dashboard .

# Run the container
docker run -p 8080:8080 health-dashboard
```

Access the application at `http://localhost:8080`

### Deploy to Render.com

1. **Push Code to GitHub** (if not already done)

2. **Create Web Service on Render**:
   - Go to [Render Dashboard](https://dashboard.render.com/)
   - Click **"New +"** → **"Web Service"**
   - Connect your GitHub account and select the repository
   - Configure:
     - **Name**: `health-dashboard`
     - **Runtime**: Select **Docker**
     - **Instance Type**: Free

3. **Wait for Deployment**: Render will build and deploy automatically (3-5 minutes)

4. **Access Your App**: Once live, visit your Render URL

> **Note**: Free tier on Render spins down after 15 minutes of inactivity. First request after inactivity may take ~50 seconds.

---

## 📱 Using Web Bluetooth

### Important Notes
⚠️ **Web Bluetooth requires HTTPS or localhost**  
⚠️ **User interaction required** - Connections must be initiated by button clicks  
⚠️ **Browser support** - Use Chrome 56+, Edge 79+, or Opera 43+

### Connecting Devices

#### Heart Rate Monitor
1. Turn on your Bluetooth LE heart rate monitor
2. Click "Connect Heart Rate Monitor" button
3. Select your device from the popup
4. View real-time BPM readings on the dashboard

**Compatible Devices**: Polar H10, Wahoo TICKR, Garmin heart rate straps, any Bluetooth LE heart rate monitor

#### Thermometer
1. Turn on your health thermometer
2. Click "Connect Thermometer" button
3. Select your device from the popup
4. Monitor temperature readings

#### ESP32 Device
1. Power on your ESP32 device
2. Click "Connect ESP32 Device" button
3. Select your device from the popup
4. View sensor data

> **Note**: ESP32 includes simulated data for demonstration if custom services aren't available.

### Without Physical Devices
The application includes simulated data for ESP32 devices, allowing you to test all features without physical hardware.

---

## 📖 Technical Implementation

### Project Structure

```
Web-Final/
├── README.md
├── Dockerfile
├── render.yaml
└── HealthDashboard/
    ├── Controllers/
    │   ├── HomeController.cs        # Dashboard controller
    │   └── HealthController.cs      # API controller with LINQ
    ├── Models/
    │   ├── HealthReadingDto.cs      # Data transfer object
    │   ├── HealthDashboardViewModel.cs  # View model
    │   └── DeviceConnectionDto.cs   # Connection DTO
    ├── Services/
    │   └── HealthDataService.cs     # LINQ operations
    ├── Views/
    │   ├── Home/Index.cshtml        # Main dashboard (HTML5)
    │   ├── Health/Index.cshtml      # Health data page
    │   └── Shared/_Layout.cshtml    # Layout template
    ├── wwwroot/
    │   ├── css/
    │   │   ├── dashboard.css        # Custom CSS3 styles
    │   │   └── site.css             # Base styles
    │   ├── js/
    │   │   ├── web-bluetooth.js     # Web Bluetooth API
    │   │   ├── dashboard.js         # jQuery & AJAX
    │   │   └── health-data.js       # Page-specific logic
    │   └── php/
    │       └── health-processor.php # PHP data processing
    ├── Program.cs                   # Application startup
    └── HealthDashboard.csproj       # Project configuration
```

### LINQ Operations Demonstrated

```csharp
// GroupBy - Group readings by device ID
var readingsByDevice = _readings
    .GroupBy(r => r.DeviceId)
    .ToDictionary(g => g.Key, g => g.OrderByDescending(r => r.Timestamp).ToList());

// GroupBy with Average - Calculate averages by device type
var averages = _readings
    .GroupBy(r => r.DeviceType)
    .Where(g => g.Any())
    .ToDictionary(g => g.Key, g => g.Average(r => r.Value));

// Where clause - Filter readings by device type
var filtered = _readings
    .Where(r => r.DeviceType.Equals(deviceType, StringComparison.OrdinalIgnoreCase))
    .OrderByDescending(r => r.Timestamp)
    .ToList();

// Complex aggregations in Analytics endpoint
var analytics = allReadings
    .GroupBy(r => r.Timestamp.Hour)
    .Select(g => new { Hour = g.Key, Count = g.Count() })
    .OrderBy(x => x.Hour)
    .ToList();
```

### LINQ to XML Export

```csharp
var xml = new XElement("HealthReadings",
    readings.Select(r => new XElement("Reading",
        new XElement("Id", r.Id),
        new XElement("DeviceId", r.DeviceId),
        new XElement("DeviceType", r.DeviceType),
        new XElement("Value", r.Value),
        new XElement("Unit", r.Unit),
        new XElement("Timestamp", r.Timestamp.ToString("o")),
        new XElement("Notes", r.Notes ?? "")
    ))
);
```

### Web Bluetooth API Implementation

```javascript
// Heart Rate Monitor Connection
const device = await navigator.bluetooth.requestDevice({
    filters: [{ services: ['heart_rate'] }],
    optionalServices: ['battery_service']
});

// Binary Data Parsing (according to Bluetooth spec)
parseHeartRate(value) {
    const flags = value.getUint8(0);
    const rate16Bits = flags & 0x1;
    return rate16Bits ? value.getUint16(1, true) : value.getUint8(1);
}
```

### jQuery & jQuery Animate

```javascript
// DOM manipulation with animation
$('#heartRateValue').text(value).hide().fadeIn(500);

// AJAX calls
$.ajax({
    url: '/Health/AddReading',
    type: 'POST',
    contentType: 'application/json',
    data: JSON.stringify(reading),
    success: function(response) { ... }
});

// Smooth animations
$('.stat-card').animate({ opacity: 1 }, {
    duration: 500,
    step: function(now) {
        $(this).css('transform', `translateY(${20 - (now * 20)}px)`);
    }
});
```

### PHP Data Processing

```php
// BMI calculation
function calculateBMI($weight, $height) {
    return $weight / ($height * $height);
}

// Heart rate classification
function classifyHeartRate($heartRate, $age = 30) {
    $maxHR = 220 - $age;
    $percentage = ($heartRate / $maxHR) * 100;
    // Returns: Resting, Very Light, Light, Moderate, Hard, Maximum
}

// Temperature classification
function classifyTemperature($temp) {
    // Returns: Low, Normal, Slightly Elevated, Fever, High Fever
}
```

---

## 🔌 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/Health/AddReading` | Add a new health reading (JSON) |
| `GET` | `/Health/GetReadings` | Get all readings as JSON array |
| `GET` | `/Health/GetReadingsByType?type={deviceType}` | Filter readings by device type |
| `GET` | `/Health/GetStatistics` | Get aggregated statistics (LINQ queries) |
| `GET` | `/Health/Analytics` | Get complex analytics with grouping |
| `GET` | `/Health/ExportJson` | Download all data as JSON file |
| `GET` | `/Health/ExportXml` | Download all data as XML file (LINQ to XML) |
| `POST` | `/Health/ImportJson` | Import JSON array of readings |
| `DELETE` | `/Health/ClearAllReadings` | Clear all stored data |

### API Usage Examples

```bash
# Add a reading
curl -X POST https://localhost:5001/Health/AddReading \
  -H "Content-Type: application/json" \
  -d '{"deviceId": "test-device", "deviceType": "heart_rate", "value": 75, "unit": "BPM"}'

# Get all readings
curl https://localhost:5001/Health/GetReadings

# Get statistics
curl https://localhost:5001/Health/GetStatistics

# Get analytics
curl https://localhost:5001/Health/Analytics
```

---

## 🔧 Troubleshooting

### Web Bluetooth Not Available
**Problem**: "Web Bluetooth API is not available" message  
**Solution**: Use Chrome, Edge, or Opera browser on HTTPS or localhost

### Cannot Connect to Device
**Problem**: Device doesn't appear in selection popup  
**Solution**:
- Ensure device is powered on and in pairing mode
- Check device battery
- Move closer to the device
- Restart Bluetooth on your computer
- Refresh the page

### Port Already in Use
**Problem**: "Address already in use" error  
**Solution**:
```bash
# Find process using port
lsof -i :5000  # Linux/Mac
netstat -ano | findstr :5000  # Windows

# Kill the process and try again
```

### Build Errors
**Problem**: Build fails with missing dependencies  
**Solution**:
```bash
dotnet clean
dotnet restore
dotnet build
```

---

## 🌐 Browser Compatibility

| Browser | Support |
|---------|---------|
| Google Chrome 56+ | ✅ Fully Supported |
| Microsoft Edge 79+ | ✅ Fully Supported |
| Opera 43+ | ✅ Fully Supported |
| Firefox | ⚠️ Web Bluetooth behind flag |
| Safari | ⚠️ Limited Web Bluetooth support |
| Internet Explorer | ❌ Not Supported |

---

## 📊 Technical Challenges Addressed

1. **Binary Data Streams**: Proper parsing of GATT characteristics according to Bluetooth specifications
2. **Low-Latency Visualization**: Real-time updates with minimal delay using efficient jQuery animations
3. **CORS Handling**: Cross-origin requests for external APIs with proper headers
4. **Data Persistence**: In-memory storage with comprehensive LINQ queries
5. **Responsive UI**: Smooth animations without performance degradation

---

## ✅ Course Requirements Checklist

- [x] HTML5 semantic markup
- [x] CSS3 with custom styling (gradients, animations, responsive)
- [x] jQuery for DOM manipulation
- [x] jQuery Animate for transitions
- [x] ASP.NET Core MVC (.NET 10.0)
- [x] C# with DTOVM pattern
- [x] LINQ queries (GroupBy, Where, OrderBy, Average, Select, etc.)
- [x] PHP for additional processing
- [x] JSON data handling
- [x] XML data handling (LINQ to XML)
- [x] Web Bluetooth API integration
- [x] External API integration

---

## 👨‍💻 Authors

**Elia Ghazal, William Ishak, George Khayat, and Bassam Farhat**  
CSI418L - Web Programming Lab Expo Final Project  
January 2026

---

## 📄 License

This project is for educational purposes as part of a Web Programming course.

---

**Note**: This is a demonstration project for academic purposes. In-memory data storage is used, so data will be lost when the server restarts.
