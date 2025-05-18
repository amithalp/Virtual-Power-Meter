# Virtual-Power-Meter
A virtual power meter to present power meter on hubitat dashboard

# 🔌 Virtual Power Meter Device

### A visual segmented bar meter for power monitoring in Hubitat dashboards

---

## ⚙️ Overview

The **Virtual Power Meter Device** is a virtual driver for Hubitat that visualizes power usage as a **segmented, color-coded horizontal bar**. It dynamically reflects real-time power input (in kW), showing usage intensity through a gradient from green (low) to red (high).

Ideal for dashboards using `attribute` tiles.

---

## 🎯 Features

- Displays power in **kW** (converted from W)
- Configurable:
  - **Min/Max** power range (e.g., 0 to 40000 W)
  - **Segment count** (e.g., 8–20 bars)
- Dynamic HSL-based color shift (green → red)
- Attribute `charCount` shows HTML size for dashboard safety
- Manual test command for visual debugging
- Optimized to stay under **1024-character** dashboard limit (works with Hub Mesh)

---

## 📦 Installation

1. Go to **Hubitat → Drivers Code**
2. Click **+ New Driver**
3. Paste the full driver code
4. Save it as: `Virtual Power Meter Device`
5. Go to **Devices → + Add Virtual Device**
6. Choose **Type**: `Virtual Power Meter Device`
7. Click **Save Device**

---

## 🔧 Device Preferences

| Field             | Description                                      |
|------------------|--------------------------------------------------|
| `powerMin`        | Power value (W) representing 0% (default: 0)     |
| `powerMax`        | Power value (W) representing 100% (default: 40000)|
| `powerUnit`       | Display unit (`kW`) — auto-converted from W     |
| `segmentCount`    | Number of meter segments (recommend ≤ 12)       |

---

## 🚀 Manual Testing

On the device page:

1. Enter a power value (in watts) into the `testPower` command
2. Click **Set**
3. The bar and `charCount` will update accordingly

---

## 🔁 Rule Machine Integration

You can automatically update the meter using **Rule Machine**:

### 🛠 Example Setup

#### 1. **Trigger:**
- Power change on your **real power meter device**

#### 2. **Action:**
- Custom Action → `setPowerAndMeter`
  - Target: your **Virtual Power Meter Device**
  - Parameter: `Number`
  - Value: `%value%` (current power in W)

> This will update both the numeric and visual meter in one call.

---

## 🧱 Dashboard Display

1. Add the device to your Hubitat Dashboard
2. Choose **tile template**: `attribute`
3. Select attribute: `powerMeter`

### Optional CSS (Dashboard → Advanced CSS):
```css
#tile-105 {
  background-color: transparent !important;
  padding: 0 !important;
}
#tile-105 .tile-title {
  display: none;
}
#tile-105 .tile-primary {
  border-radius: 10px;
  overflow: hidden;
  position: absolute;
  top: 3px;
  bottom: 3px;
  left: 5px;
  right: 5px;
}
```

## 📊 Attributes

| Attribute     | Type   | Description                                                   |
|---------------|--------|---------------------------------------------------------------|
| `power`       | Number | Raw input power in watts (W)                                  |
| `powerMeter`  | String | Rendered HTML string that displays the segmented meter        |
| `charCount`   | Number | Number of characters in the `powerMeter` attribute (max 1024) |

---

## 🧑‍💻 Commands

| Command              | Parameters | Description                                               |
|----------------------|------------|-----------------------------------------------------------|
| `setPower(value)`    | Number     | Sets the `power` attribute (W)                            |
| `setPowerMeter(value)` | Number   | Generates and sets the HTML meter for the given power     |
| `setPowerAndMeter(value)` | Number | Updates both `power` and `powerMeter` in one call         |
| `testPower(value)`   | Number     | Manually test power value and update meter (device page)  |

---

## 🛑 Character Limit Note

Hubitat Dashboards and Hub Mesh truncate string attributes that exceed **1024 characters**.

To avoid issues:
- Keep `segmentCount` low (≤ 8–12 recommended)
- Use the `charCount` attribute to monitor current length
- Consider minimizing margins, padding, and label text if needed

If `charCount` exceeds 1024:
- The meter will **not render** on the dashboard
- The attribute will be **excluded** from Hub Mesh

---

## 📘 Example Display (Dashboard Attribute Tile)

This is what the `powerMeter` attribute renders as on a Hubitat dashboard using an **attribute tile**:

Power 3.52 kW
█ █ █ █ ░ ░ ░ ░
(8 segments, color-coded from green to red)


- Each `█` represents a filled segment
- Each `░` represents an unfilled segment
- Segment color shifts from **green (low)** to **red (high)** based on power level
- Value is displayed in **kW**, rounded to two decimal places

You can fully customize the appearance using Dashboard CSS or by editing the HTML generation in the driver.

---

## 🧷 Credits

**Driver Author**: Amit Halperin  
**Namespace**: `amithalp`  
**Project**: Virtual Power Meter Device for Hubitat

This driver is released for personal and community use. Contributions, forks, and adaptations are welcome!


