# Project Code Reference for Presentation

This document contains key code snippets and explanations for your PowerPoint presentation, categorized by technology and functionality.

---

## 1. API Fetch and Applications

**Concept**: The application consumes both external public APIs (client-side) and internal project APIs (server-side).

### External API (JavaScript)
*Found in: `wwwroot/js/dashboard.js`*

```javascript
// Fetch health tips from an external API (API Ninjas)
function fetchHealthTips() {
    $.ajax({
        url: 'https://api.api-ninjas.com/v1/quotes?category=health',
        type: 'GET',
        headers: { 'X-Api-Key': 'demo' },
        success: function(data) {
            if (data && data.length > 0) {
                displayHealthTip(data[0]);
            }
        }
    });
}
```

### Internal API (C#)
*Found in: `Controllers/HealthController.cs`*

```csharp
[HttpGet]
public IActionResult GetReadings()
{
    // Returns data as JSON for the frontend to consume
    var readings = _healthDataService.GetAllReadings();
    return Json(readings);
}
```

---

## 2. Bluetooth Integration

**Concept**: Uses the Web Bluetooth API to connect to physical devices securely.

*Found in: `wwwroot/js/web-bluetooth.js`*

```javascript
// Requesting a device with specific services
const device = await navigator.bluetooth.requestDevice({
    filters: [{ services: ['heart_rate'] }],
    optionalServices: ['battery_service']
});

// Connecting to the GATT Server
const server = await device.gatt.connect();
```

---

## 3. LINQ Applications (Efficient Data Processing)

**Concept**: Language Integrated Query (LINQ) is used in C# to filter, sort, and aggregate data efficiently in memory.

*Found in: `Services/HealthDataService.cs`*

```csharp
// Grouping readings by Device ID and sorting them
public Dictionary<string, List<HealthReadingDto>> GetReadingsGroupedByDevice()
{
    return _readings
        .GroupBy(r => r.DeviceId)
        .ToDictionary(g => g.Key, g => g.OrderByDescending(r => r.Timestamp).ToList());
}

// Calculating Averages using GroupBy and Average
public Dictionary<string, double> GetAveragesByType()
{
    return _readings
        .GroupBy(r => r.DeviceType)
        .Where(g => g.Any())
        .ToDictionary(g => g.Key, g => g.Average(r => r.Value));
}
```

---

## 16. Data Processing with LINQ (Presentation Slide)

**Key Features Demonstrated:**
*   **Real-time Aggregation**: Calculating statistics instantly.
*   **Filtering**: Selecting specific data (e.g., by device type).
*   **Statistical Averages**: Computing mean values for analysis.

*Found in: `Services/HealthDataService.cs`*

```csharp
// 1. Filtering by Device Type (Where)
public List<HealthReadingDto> GetReadingsByDeviceType(string deviceType)
{
    return _readings
        .Where(r => r.DeviceType.Equals(deviceType, StringComparison.OrdinalIgnoreCase))
        .OrderByDescending(r => r.Timestamp)
        .ToList();
}

// 2. Real-time Aggregation (GroupBy)
public Dictionary<string, List<HealthReadingDto>> GetReadingsGroupedByDevice()
{
    return _readings
        .GroupBy(r => r.DeviceId)
        .ToDictionary(g => g.Key, g => g.OrderByDescending(r => r.Timestamp).ToList());
}

// 3. Calculating Statistical Averages (Average)
public Dictionary<string, double> GetAveragesByType()
{
    return _readings
        .GroupBy(r => r.DeviceType)
        .Where(g => g.Any())
        .ToDictionary(g => g.Key, g => g.Average(r => r.Value));
}
```

---

## 17. Real-Time Feedback (Presentation Slide)

**Concept**: Giving the user immediate visual confirmation when data arrives using jQuery to trigger CSS animations.

*Found in: `wwwroot/css/dashboard.css` (Animation) & `wwwroot/js/web-bluetooth.js` (Trigger)*

```css
/* 1. Define the Pulse Animation (CSS) */
@keyframes pulse {
    0% { transform: scale(1); box-shadow: 0 5px 20px rgba(0,0,0,0.2); }
    50% { transform: scale(1.05); box-shadow: 0 10px 40px rgba(0,0,0,0.3); }
    100% { transform: scale(1); box-shadow: 0 5px 20px rgba(0,0,0,0.2); }
}

.pulse-animation {
    animation: pulse 0.5s ease-in-out;
}
```

```javascript
// 2. Trigger it with jQuery when data arrives (JS)
function handleHeartRateData(value) {
    // Update text
    $('#heartRateValue').text(value);
    
    // Trigger Animation
    $('#heartRateCard').addClass('pulse-animation');
    
    // Remove class after animation finishes so it can be triggered again
    setTimeout(() => $('#heartRateCard').removeClass('pulse-animation'), 500);
}
```

---

## 18. Responsive Design (Presentation Slide)

**Concept**: Using Bootstrap's grid system and CSS media queries to adapt to mobile screens.

*Found in: `Views/Health/Index.cshtml` (Grid) & `wwwroot/css/dashboard.css` (Media Queries)*

```html
<!-- 1. Bootstrap Grid System (HTML) -->
<!-- Uses 12 columns on mobile (col-12) and 3 columns on desktop (col-md-3) -->
<div class="row mb-4">
    <div class="col-12 col-md-3">
        <div class="card shadow stat-card">
            <!-- Content -->
        </div>
    </div>
</div>
```

```css
/* 2. Custom Mobile Adjustments (CSS) */
@media (max-width: 768px) {
    .stat-card .display-3 {
        font-size: 2.5rem; /* Smaller font for mobile */
    }
    
    .btn-group .btn {
        display: block;    /* Stack buttons vertically */
        width: 100%;
        margin-bottom: 10px;
    }
}
```

---

## 19. Data Exchange (Presentation Slide)

**Concept**: The ecosystem of data formats and protocols used to transport and process information.

### 1. PHP (Supplementary Processing)
*Found in: `wwwroot/php/health-processor.php`*
```php
// Calculating BMI and Heart Rate Zones on the server side
function classifyHeartRate($heartRate, $age = 30) {
    $maxHR = 220 - $age;
    $percentage = ($heartRate / $maxHR) * 100;
    
    if ($percentage < 50) return "Resting";
    if ($percentage < 60) return "Very Light";
    return "Moderate";
}

function calculateBMI($weight, $height) {
    return $weight / ($height * $height);
}
```

### 2. JSON (Primary Data Interchange)
*Found in: `Controllers/HealthController.cs`*
```csharp
// Returning data as JSON for API communication
[HttpGet]
public IActionResult ExportJson()
{
    var readings = _healthDataService.GetAllReadings();
    return Json(readings);
}
```

### 3. XML (LINQ-to-XML Export)
*Found in: `Controllers/HealthController.cs`*
```csharp
// Generating XML files for user export using LINQ
var xml = new XElement("HealthReadings",
    readings.Select(r => new XElement("Reading",
        new XElement("Type", r.DeviceType),
        new XElement("Value", r.Value),
        new XElement("Unit", r.Unit)
    ))
);
```

### 4. AJAX (Async Data Updates)
*Found in: `wwwroot/js/dashboard.js`*
```javascript
// Updating data without page reloads
$.ajax({
    url: '/Health/AddReading',
    type: 'POST',
    contentType: 'application/json',
    success: function(response) {
        refreshReadingsTable(); // Update UI immediately
    }
});
```

### 5. External APIs (Integrations)
*Found in: `wwwroot/js/dashboard.js`*
```javascript
// Fetching global COVID-19 stats (disease.sh)
$.ajax({
    url: 'https://disease.sh/v3/covid-19/all',
    type: 'GET',
    success: function(data) {
        displayCovidWidget(data);
    }
});
```

---

## 4. DTO & View Models (DTOVM)

**Concept**: Separating internal data models from what is displayed to the user.

*Found in: `Models/HealthDashboardViewModel.cs` & `Models/HealthReadingDto.cs`*

```csharp
// DTO: Defines the shape of a single data point
public class HealthReadingDto
{
    public string DeviceId { get; set; } = string.Empty;
    public double Value { get; set; }
    public DateTime Timestamp { get; set; }
}

// View Model: Aggregates multiple data sources for a specific View
public class HealthDashboardViewModel
{
    public List<HealthReadingDto> RecentReadings { get; set; }
    public Dictionary<string, double> AveragesByType { get; set; }
    public int TotalReadings { get; set; }
}
```

---

## 5. HTML and CSS Snippets

### HTML5 (Razor Syntax)
*Found in: `Views/Health/Index.cshtml`*

```html
<!-- Displaying a statistic card using Model data -->
<div class="card shadow stat-card">
    <div class="card-body text-center">
        <h5><i class="fas fa-database"></i> Total Records</h5>
        <h2 class="display-4">@Model.TotalReadings</h2>
    </div>
</div>
```

### CSS3 (Gradients & Animations)
*Found in: `wwwroot/css/dashboard.css`*

```css
/* Custom Gradient Background */
.stat-card {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
}

/* Keyframe Animation for Pulse Effect */
@keyframes pulse {
    0% { transform: scale(1); }
    50% { transform: scale(1.05); }
    100% { transform: scale(1); }
}

.pulse-animation {
    animation: pulse 0.5s ease-in-out;
}
```

---

## 6. GATT Server & Subscriptions

**Concept**: Interacting with the Bluetooth Generic Attribute Profile (GATT) to receive real-time data.

*Found in: `wwwroot/js/web-bluetooth.js`*

```javascript
// 1. Get the Primary Service
const service = await server.getPrimaryService('heart_rate'); // Standard UUID: 0x180D

// 2. Get the Characteristic
const characteristic = await service.getCharacteristic('heart_rate_measurement'); // UUID: 0x2A37

// 3. Subscribe to Notifications (Real-time updates)
await characteristic.startNotifications();
characteristic.addEventListener('characteristicvaluechanged', (event) => {
    // Handle the incoming binary data
    const value = this.parseHeartRate(event.target.value);
    this.handleHeartRateData(device.id, device.name, value);
});
```

---

## 7. AJAX

**Concept**: Asynchronous JavaScript and XML (now mostly JSON) to update the page without reloading.

*Found in: `wwwroot/js/dashboard.js`*

```javascript
// Sending data to the server without a page reload
$.ajax({
    url: '/Health/AddReading',
    type: 'POST',
    contentType: 'application/json',
    data: JSON.stringify(reading),
    success: function(response) {
        console.log('Reading saved:', response);
        refreshReadingsTable(); // Update UI
    },
    error: function(xhr, status, error) {
        console.error('Error saving reading:', error);
    }
});
```

---

## 8. jQuery and Animations

**Concept**: Simplifying DOM manipulation and adding visual feedback.

*Found in: `wwwroot/js/dashboard.js`*

```javascript
// Fade out the table, update content, then fade back in
tbody.fadeOut(300, function() {
    tbody.empty();
    readings.forEach(function(reading) {
        // ... build rows ...
    });
    tbody.fadeIn(300);
});

// Animate a button on click
$(this).animate({ 
    opacity: 0.5 
}, 200, function() {
    $(this).animate({ opacity: 1 }, 200);
});
```

---

## 9. PHP, JSON, and XML Handling

**Concept**: The project includes a PHP script (`health-processor.php`) to demonstrate multi-language integration and data processing.

### PHP Handling
*Found in: `wwwroot/php/health-processor.php`*

```php
// Receiving JSON input
$input = file_get_contents('php://input');
$data = json_decode($input, true);

// Processing logic
function analyzeHealthData($readings) {
    // ... calculate stats ...
    return $analysis;
}

// Returning JSON response
echo json_encode($response, JSON_PRETTY_PRINT);
```

### JSON & XML Export (C#)
*Found in: `Controllers/HealthController.cs`*

```csharp
[HttpGet]
public IActionResult ExportXml()
{
    var readings = _healthDataService.GetAllReadings();
    
    // Creating XML using LINQ to XML
    var xml = new XElement("HealthReadings",
        readings.Select(r => new XElement("Reading",
            new XElement("Value", r.Value),
            new XElement("Unit", r.Unit)
        ))
    );

    return Content(xml.ToString(), "application/xml");
}
```

---

## 10. DOM Manipulation

**Concept**: Dynamically creating and inserting HTML elements based on data.

*Found in: `wwwroot/js/dashboard.js`*

```javascript
function displayCovidWidget(data) {
    // Creating a complex HTML structure using template literals
    const widget = $(`
        <div class="covid-widget card shadow mb-4">
            <div class="card-header bg-warning text-dark">
                <h5><i class="fas fa-virus"></i> Global COVID-19 Stats</h5>
            </div>
            <div class="card-body">
                <h4 class="text-primary">${(data.cases / 1000000).toFixed(1)}M Cases</h4>
            </div>
        </div>
    `);
    
    // Appending to the container with an animation
    $('.container-fluid').prepend(widget);
    widget.slideDown(500);
}
```

---

## 11. MVC Architecture

**Model**: `HealthReadingDto.cs` (Data Structure)
**View**: `Index.cshtml` (User Interface)
**Controller**: `HealthController.cs` (Logic connecting Model and View)

```csharp
// Controller Action
public IActionResult Index()
{
    // 1. Get Data (Model)
    var dashboardData = _healthDataService.GetDashboardData();
    // 2. Return View with Data
    return View(dashboardData);
}
```

---

## 12. JavaScript Logic

*Found in: `wwwroot/js/web-bluetooth.js`*

```javascript
// Class-based structure for managing device connections
class BluetoothHealthDevice {
    constructor() {
        this.devices = new Map();
    }

    async connectESP32Device() {
        // Logic to connecting to a generic device
    }
}

// Exporting the instance for global use
window.bluetoothDevice = new BluetoothHealthDevice();
```

---

## 13. Bluetooth Workflow Code (Presentation Slide)

**Workflow Steps:**
1. **User Gesture**: Must be triggered by a click (security requirement).
2. **Request Device**: Browser asks user to select a device.
3. **Connect GATT**: Handshake with the device's server.
4. **Subscribe**: Listen for data updates (e.g., Heart Rate).

*Found in: `wwwroot/js/web-bluetooth.js`*

```javascript
// A. User Gesture (Click Event)
$('#connectHeartRate').on('click', async function() {
    
    // B. Request Device
    const device = await navigator.bluetooth.requestDevice({
        filters: [{ services: ['heart_rate'] }] // Standard Heart Rate Service
    });

    // C. Connect to GATT Server
    const server = await device.gatt.connect();
    
    // D. Subscribe to Characteristics
    const service = await server.getPrimaryService('heart_rate');
    const characteristic = await service.getCharacteristic('heart_rate_measurement'); // UUID: 0x2A37
    
    await characteristic.startNotifications();
    characteristic.addEventListener('characteristicvaluechanged', handleNotifications);
});
```

---

## 14. Testing and Verifications

**Concept**: Ensuring the system works reliably and performs well.

*Note: While comprehensive unit test files are not present in this snippets collection, the following verification strategies are implemented in the code:*

### Latency Management
*Found in: `wwwroot/js/web-bluetooth.js`*
- **Mechanism**: Event-driven listeners (`characteristicvaluechanged`) are used instead of polling.
- **Benefit**: Reduces latency for real-time heart rate updates compared to asking the device for data every second.
- **Verification**: The code logs data immediately upon receipt: `console.log('Heart Rate: ${value} BPM');`.

### Feasibility & Reliability
*Found in: `wwwroot/js/dashboard.js`*
- **Error Handling**: Every fetch and promise includes `.catch()` or `error:` callbacks to handle connection failures gracefully.
- **Feasibility Check**: The code explicitly checks for browser support:
  ```javascript
  this.isBluetoothAvailable = 'bluetooth' in navigator;
  if (!this.isBluetoothAvailable) {
      alert('Web Bluetooth API is not available...');
  }
  ```

### Data Validation
*Found in: `Controllers/HealthController.cs`*
- **Input Validation**: `ModelState.IsValid` ensures only correctly formatted data (e.g., non-null values) is processed.
  ```csharp
  if (ModelState.IsValid) {
      _healthDataService.AddReading(reading);
  }
  ```
