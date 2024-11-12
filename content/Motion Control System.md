This document details the implementation of a  motion control system that includes motion planning and motor control for precise movement control.

## 1. Motion Planner

### Overview
The Motion Planner implements various tracking mechanisms for smooth motion control:
- Current tracking
- Velocity tracking
- Position tracking
- Position interpolation
- Trajectory tracking
### Tracker Components
#### Current Tracker
```cpp
void MotionPlanner::CurrentTracker::CalcSoftGoal(int32_t _goalCurrent)
{
    int32_t deltaCurrent = _goalCurrent - trackCurrent;
    // Current ramping logic
}
```
Features:
- Smooth current transitions
- Current limiting
- Bi-directional control
#### Velocity Tracker
```cpp
void MotionPlanner::VelocityTracker::CalcSoftGoal(int32_t _goalVelocity)
{
    int32_t deltaVelocity = _goalVelocity - trackVelocity;
    // Velocity ramping logic
}
```
Implements:
- Velocity ramping
- Acceleration control
- Zero-crossing handling
#### Position Tracker
```cpp
void MotionPlanner::PositionTracker::CalcSoftGoal(int32_t _goalPosition)
{
    int32_t deltaPosition = _goalPosition - trackPosition;
    // Position and velocity profile generation
}
```
Features:
- Position profiling
- Velocity limiting
- Smooth acceleration/deceleration

## 2. Motor Control

### Core Control Loop
```cpp
void Motor::CloseLoopControlTick()
{
    // 1. Update encoder data
    // 2. Estimate velocity and position
    // 3. Calculate control output
    // 4. Apply motor commands
}
```

### Control Modes

#### Current Control
```cpp
void Motor::Controller::CalcCurrentToOutput(int32_t current)
{
    focCurrent = current;
    // Calculate FOC position and apply current vector
}
```
Features:
- Field-Oriented Control (FOC)
- Current limiting
- Phase advance compensation

#### Velocity Control (PID)
```cpp
void Motor::Controller::CalcPidToOutput(int32_t _speed)
{
    // PID control implementation for velocity
    config->pid.vError = _speed - estVelocity;
    // Calculate and apply control output
}
```
Implements:
- PID control
- Anti-windup
- Output limiting

#### Position Control (DCE)
```cpp
void Motor::Controller::CalcDceToOutput(int32_t _location, int32_t _speed)
{
    // Position and velocity error calculation
    // DCE (Dynamic Control Enhancement) implementation
}
```
Features:
- Position tracking
- Velocity feedforward
- Dynamic error compensation

## 3. Safety Features

### Stall Protection
```cpp
// Stall detection logic
if (current == config.motionParams.ratedCurrent &&
    abs(controller->estVelocity) < MOTOR_ONE_CIRCLE_SUBDIVIDE_STEPS / 5)
{
    controller->stalledTime += motionPlanner.CONTROL_PERIOD;
    // Handle stall condition
}
```

### Overload Protection
```cpp
if (current == config.motionParams.ratedCurrent)
{
    controller->overloadTime += motionPlanner.CONTROL_PERIOD;
    // Handle overload condition
}
```

## 4. State Management

### Motor States
- `STATE_NO_CALIB`: Uncalibrated
- `STATE_STOP`: Stopped
- `STATE_STALL`: Stalled
- `STATE_OVERLOAD`: Overloaded
- `STATE_FINISH`: Target reached
- `STATE_RUNNING`: In motion

### Operating Modes
- `MODE_STEP_DIR`: Step/Direction input
- `MODE_COMMAND_POSITION`: Position control
- `MODE_COMMAND_VELOCITY`: Velocity control
- `MODE_COMMAND_CURRENT`: Current control
- `MODE_COMMAND_TRAJECTORY`: Trajectory following

## 5. Performance Features

### Position Estimation
```cpp
controller->estVelocityIntegral += (
    (controller->realPosition - controller->realPositionLast) * motionPlanner.CONTROL_FREQUENCY
    + ((controller->estVelocity << 5) - controller->estVelocity)
);
```
Implements:
- Velocity estimation
- Position prediction
- Lead angle compensation

### Motion Optimization
```cpp
float pMax = (float) context->config.motionParams.ratedVelocityAcc * _time * _time / 4;
if ((float) deltaPos > pMax)
{
    // Optimize trajectory for time/distance constraints
}
```
Features:
- Time-optimal trajectories
- Velocity/acceleration limiting
- Path optimization

## 6. System Integration

### Configuration Management
```cpp
void Motor::Controller::AttachConfig(Motor::Controller::Config_t* _config)
{
    config = _config;
    // Initialize control parameters
}
```

### Hardware Interfaces
```cpp
void Motor::AttachEncoder(EncoderBase* _encoder)
void Motor::AttachDriver(DriverBase* _driver)
```
Supports:
- Multiple encoder types
- Various motor drivers
- Flexible configuration
