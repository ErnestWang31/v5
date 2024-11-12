---
title: "Dummy Robot Control System"
date: 2024-01-01
draft: false
---
# Dummy Robot Control System Documentation

This document details the implementation of a 6-DOF robot control system, including movement control, command handling, and system management.

## 1. Robot Configuration and Initialization

### Robot Constructor
```cpp
DummyRobot::DummyRobot(CAN_HandleTypeDef* _hcan)
{
    motorJ[ALL] = new CtrlStepMotor(_hcan, 0, false, 1, -180, 180);
    motorJ[1] = new CtrlStepMotor(_hcan, 1, true, 30, -170, 170);
    // Additional joint configurations...
}
```

Key configurations for each joint:
- CAN bus communication
- Motor IDs and directions
- Gear ratios
- Joint limits
- Kinematic parameters

## 2. Movement Control

### Joint Movement (MoveJ)
```cpp
bool DummyRobot::MoveJ(float _j1, float _j2, float _j3, float _j4, float _j5, float _j6)
{
    // Validate joint limits
    // Calculate movement times
    // Set joint speeds
    // Execute movement
}
```

Features:
- Joint limit validation
- Synchronized movement
- Dynamic speed calculation
- Safety checks

### Linear Movement (MoveL)
```cpp
bool DummyRobot::MoveL(float _x, float _y, float _z, float _a, float _b, float _c)
{
    // Solve inverse kinematics
    // Validate solutions
    // Choose optimal solution
    // Execute movement
}
```

Implementation details:
1. Cartesian coordinate input
2. Multiple solution handling
3. Path optimization
4. Joint limit checking

## 3. Safety Features

### Emergency Stop
```cpp
void DummyRobot::CommandHandler::EmergencyStop()
{
    context->MoveJ(context->currentJoints.a[0], ...);
    context->isEnabled = false;
    ClearFifo();
}
```

Safety measures:
- Immediate movement stop
- Motor power control
- Command queue clearing
- State management

### Joint Limits
```cpp
void DummyRobot::SetJointSpeed(float _speed)
{
    if (_speed < 0)_speed = 0;
    else if (_speed > 100) _speed = 100;
    jointSpeed = _speed * jointSpeedRatio;
}
```

Protection features:
- Speed limits
- Acceleration control
- Current limiting
- Position boundaries

## 4. Command System

### Command Handling
```cpp
uint32_t DummyRobot::CommandHandler::ParseCommand(const std::string &_cmd)
{
    switch (context->commandMode)
    {
        case COMMAND_TARGET_POINT_SEQUENTIAL:
        case COMMAND_CONTINUES_TRAJECTORY:
        // Command parsing and execution
    }
}
```

Command types:
- Joint movement (>)
- Linear movement (@)
- Speed control
- System control

### Command Modes
```cpp
void DummyRobot::SetCommandMode(uint32_t _mode)
{
    commandMode = static_cast<CommandMode>(_mode);
    // Configure mode-specific parameters
}
```

Available modes:
1. Sequential point-to-point
2. Continuous trajectory
3. Interruptible movement
4. Motor tuning

## 5. System Calibration

### Home Calibration
```cpp
void DummyRobot::CalibrateHomeOffset()
{
    // Calibration sequence
    // Position setting
    // Offset application
}
```

Calibration process:
1. Motor enable control
2. Reference position setting
3. Home offset application
4. System reboot

## 6. Status Monitoring

### Position Updates
```cpp
void DummyRobot::UpdateJointAngles()
void DummyRobot::UpdateJointPose6D()
```

Monitoring features:
- Joint angle tracking
- Cartesian position calculation
- Movement state monitoring
- System status updates

## 7. Additional Features

### Hand Control
```cpp
void DummyHand::SetAngle(float _angle)
void DummyHand::SetMaxCurrent(float _val)
```

Gripper features:
- Angle control
- Current limiting
- Safety bounds
- Enable/disable control

### Motor Tuning
```cpp
void DummyRobot::TuningHelper::Tick(uint32_t _timeMillis)
{
    // Calculate sinusoidal movement
    // Apply to selected joints
}
```

Tuning capabilities:
- Frequency control
- Amplitude adjustment
- Joint selection
- Real-time adjustment

## Key System Benefits

1. Safety
   - Comprehensive limit checking
   - Emergency stop capability
   - Current protection
   - Movement validation

2. Precision
   - Accurate position control
   - Synchronized joint movement
   - Path optimization
   - Calibration support

3. Flexibility
   - Multiple movement modes
   - Command queueing
   - Dynamic speed control
   - Tuning capabilities

4. Reliability
   - Robust error handling
   - State monitoring
   - Communication management
   - System protection

