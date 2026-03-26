# Synthetic Flight Data - Simulation 5

This directory contains synthetic flight test data generated from the Simulation 5 parameters. This data is designed to test the flight computer's detection algorithms with realistic sensor readings.

## 📊 Generated Files

### Data Files
- **`simulation_5_flight_data.csv`** - Main flight data file (9,500 samples @ 100Hz)
  - 37 columns including all sensor streams
  - Ground truth values for validation
  - Flight state labels

### Visualization Files
- **`simulation_5_validation.png`** - Matches original Simulation 5 graph
- **`sensor_validation.png`** - Multi-sensor comparison plots

### Scripts
- **`generate_flight_data.py`** - Data generator with physics model
- **`visualize_data.py`** - Creates validation plots
- **`test_algorithm.py`** - Template for testing detection algorithms

## 🚀 Flight Profile

Based on Simulation 5:
- **Peak Altitude**: 700 m
- **Time to Apogee**: ~12 seconds (from launch)
- **Motor Burnout**: 2.5s @ 245m, 196 m/s
- **Initial Velocity**: ~125 m/s
- **Total Flight Time**: ~55 seconds
- **Sample Rate**: 100 Hz (10ms intervals)

## 🔧 Hardware Simulation

### Dual BMP390 Barometers
- **Noise**: ±0.5m RMS per sensor
- **Sensor Offset**: 2.0m calibration difference between sensors
- **Outputs**: Altitude AGL, Altitude MSL, Temperature

### Dual MPU6050 IMUs
- **Accel Noise**: ±0.02g
- **Gyro Noise**: ±0.5 deg/s
- **Vibration**: 0.3g during powered flight (150Hz)
- **Outputs**: 3-axis accel (g), 3-axis gyro (deg/s)

### KX134 High-G Accelerometer
- **Range**: ±16g
- **Noise**: ±0.05g (higher than MPU6050)
- **Outputs**: 3-axis accel (g)

## 📈 Flight States

The data includes realistic state transitions:

| State | Duration | Description |
|-------|----------|-------------|
| PREFLIGHT | 5.00s | System initialization |
| CALIBRATION | 30.00s | 30-second calibration period |
| ON_PAD | 5.00s | Armed and ready |
| POWERED | 2.51s | Motor burn phase |
| COAST | 9.50s | Coast to apogee |
| DESCENT | 42.99s | Parachute descent |

## 🧪 Usage

### Regenerate Data
```bash
./venv/bin/python3 generate_flight_data.py
```

### Visualize Data
```bash
./venv/bin/python3 visualize_data.py
```

### Test Your Algorithm
```bash
./venv/bin/python3 test_algorithm.py
```

## 📋 CSV Column Reference

### Time & State
- `timestamp_ms` - Milliseconds since start
- `state` - Numeric flight state (0-7)
- `state_name` - Human-readable state name

### Fused Data (What the flight computer sees)
- `altitude_agl_m` - Fused altitude above ground level
- `velocity_ms` - Fused vertical velocity
- `acc_x_g`, `acc_y_g`, `acc_z_g` - Fused accelerations
- `acc_vertical_g` - Fused vertical acceleration
- `vibration_g` - Motor vibration magnitude
- `rotation_rate_dps` - Total rotation rate

### Raw Sensor Data
- `bmp_a_alt`, `bmp_a_msl` - BMP390 A readings
- `bmp_b_alt`, `bmp_b_msl` - BMP390 B readings
- `mpu_a_acc_x/y/z`, `mpu_a_gyro_x/y/z` - MPU6050 A
- `mpu_b_acc_x/y/z`, `mpu_b_gyro_x/y/z` - MPU6050 B
- `kx134_acc_x/y/z` - KX134 accelerometer

### Ground Truth (for validation only)
- `true_altitude` - Actual altitude
- `true_velocity` - Actual velocity
- `true_acceleration` - Actual acceleration (g)

### Metadata
- `temperature_c` - Temperature reading
- `deployed` - Boolean deployment flag
- `deploy_reason` - Deployment trigger reason
- `sensor_flags` - Bitmask of sensor health

## ✅ Validation Results

```
Barometer Noise:
  BMP_A RMS Error: 0.498 m
  BMP_B RMS Error: 2.072 m
  BMP_A-BMP_B Offset: -2.007 m

IMU Acceleration Statistics:
  MPU_A Z-axis Mean: 0.700 g
  MPU_B Z-axis Mean: 0.720 g
  KX134 Z-axis Mean: 0.710 g

Flight Event Detection:
  Motor Burnout: T+2.50s @ 245.2m, 196.2m/s
  Apogee: T+5.08s @ 700.0m
  Descent Start: T+12.01s
```

## 🎯 Testing Recommendations

Use this data to validate:

1. **Launch Detection** - Check for spike in vertical acceleration
2. **Burnout Detection** - Transition from high-G to zero-G
3. **Apogee Detection** - Velocity crossing zero, altitude peak
4. **Sensor Fusion** - Compare dual sensor averaging
5. **State Machine** - Verify transitions match expected timing
6. **Noise Handling** - Test filtering and smoothing algorithms

## 🔍 Known Characteristics

- All sensors are marked as healthy (`sensor_flags = 0b111111`)
- No sensor failures simulated
- Noise is Gaussian distributed
- Physics model uses simplified drag model
- Parachute deployment occurs at apogee (velocity ~0)
