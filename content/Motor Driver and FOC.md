This document explains the implementation of a motor driver system using Field-Oriented Control (FOC) with sine wave lookup tables for efficient operation.

## 1. Sine Wave Lookup Table

### Overview
```cpp
#define sin_pi_m2_dpix      1024
#define sin_pi_m2_dpiybit   12

const int16_t sin_pi_m2[sin_pi_m2_dpix + 1] = {
    0, 25, 50, 75, 101, ... // Full sine wave table
};
```

Key features:
- 1024-point resolution for high precision
- Fixed-point arithmetic for efficiency
- Symmetrical positive and negative values
- 12-bit precision for DAC output

Benefits:
- Faster than real-time calculation
- Consistent timing
- Reduced CPU load
- Suitable for embedded systems

## 2. Driver Base Class

### Interface Definition
```cpp
class DriverBase {
public:
    virtual void Init() = 0;
    virtual void SetFocCurrentVector(uint32_t _directionInCount, int32_t _current_mA) = 0;
    virtual void Sleep() = 0;
    virtual void Brake() = 0;
protected:
    virtual void SetTwoCoilsCurrent(uint16_t _currentA_mA, uint16_t _currentB_mA) = 0;
    // Fast sine lookup structures
    FastSinToDac_t phaseB{};
    FastSinToDac_t phaseA{};
};
```

Features:
- Pure virtual interface
- Common FOC functionality
- Phase current control
- Power management states

## 3. TB67H450 Driver Implementation

### FOC Vector Generation
```cpp
void TB67H450Base::SetFocCurrentVector(uint32_t _directionInCount, int32_t _current_mA)
{
    // Calculate phase angles (90 degrees offset)
    phaseB.sinMapPtr = (_directionInCount) & (0x000003FF);
    phaseA.sinMapPtr = (phaseB.sinMapPtr + (256)) & (0x000003FF);

    // Lookup sine values
    phaseA.sinMapData = sin_pi_m2[phaseA.sinMapPtr];
    phaseB.sinMapData = sin_pi_m2[phaseB.sinMapPtr];
    
    // Calculate DAC values
    uint32_t dac_reg = abs(_current_mA);
    dac_reg = (uint32_t)(dac_reg * 5083) >> 12;
    // Apply sine modulation
    phaseA.dacValue12Bits = 
        (uint32_t)(dac_reg * abs(phaseA.sinMapData)) >> sin_pi_m2_dpiybit;
    phaseB.dacValue12Bits = 
        (uint32_t)(dac_reg * abs(phaseB.sinMapData)) >> sin_pi_m2_dpiybit;
}
```

Implementation details:
1. Phase angle calculation
   - 90-degree offset between phases
   - 10-bit angle resolution (1024 points)
   - Circular buffer wraparound

2. Current scaling
   - 12-bit DAC resolution
   - Current limiting
   - Fixed-point multiplication

3. Direction control
   - H-bridge direction setting
   - Zero-crossing handling
   - Efficient bit operations

## 4. Power Management

### Sleep Mode
```cpp
void TB67H450Base::Sleep()
{
    phaseA.dacValue12Bits = 0;
    phaseB.dacValue12Bits = 0;
    SetTwoCoilsCurrent(phaseA.dacValue12Bits, phaseB.dacValue12Bits);
    SetInputA(false, false);
    SetInputB(false, false);
}
```

Features:
- Zero current output
- Power saving mode
- Safe state configuration

### Brake Mode
```cpp
void TB67H450Base::Brake()
{
    phaseA.dacValue12Bits = 0;
    phaseB.dacValue12Bits = 0;
    SetTwoCoilsCurrent(phaseA.dacValue12Bits, phaseB.dacValue12Bits);
    SetInputA(true, true);
    SetInputB(true, true);
}
```

Features:
- Active motor braking
- Short circuit windings
- Rapid deceleration

## 5. Optimization Techniques

### Fast Sine Calculation
```cpp
typedef struct {
    uint16_t sinMapPtr;     // Lookup table index
    int16_t sinMapData;     // Sine value
    uint16_t dacValue12Bits; // DAC output value
} FastSinToDac_t;
```

Benefits:
- No floating-point math
- Deterministic timing
- Efficient memory usage
- Fast table lookup

### Current Scaling
```cpp
uint32_t dac_reg = (uint32_t)(dac_reg * 5083) >> 12;
```

Features:
- Fixed-point multiplication
- Efficient scaling
- 12-bit DAC resolution
- Current limiting

## 6. Key System Benefits

1. Performance
   - Fast execution
   - Deterministic timing
   - Efficient memory usage
   - No floating-point operations

2. Precision
   - 1024-point sine resolution
   - 12-bit DAC output
   - Accurate phase control
   - Smooth sinusoidal output

3. Flexibility
   - Adaptable base class
   - Multiple driver support
   - Configurable parameters
   - Power management modes

4. Reliability
   - Protected operations
   - Safe state handling
   - Current limiting
   - Robust implementation