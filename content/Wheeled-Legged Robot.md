---
title: "wheeled-legged robot"
date: 2024-01-01
draft: false
---
Ever watched Boston Dynamics' robots pulling off incredible stunts but thought "there's got to be a simpler way"? After diving into a research paper on wheel-legged robots and seeing the explosion of humanoid robots everywhere, I couldn't shake the feeling that we're overcomplicating robot mobility. So I set out to build something that combines the best of both worlds - the stability of wheels with the adaptability of legs!![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXew4-WFsULSy8zlU9z0digVlPpTL0ZACiuGoFv8NriuW8JDvFky-f3p3gICxFkK6WqptSHuuh9GcDYdRX-gRsPlHrka5EntRmvjq9Ef0XJ86Gdu9Rc70H7tidh1rEDJlYuBfSfDyR3CqfwGWEetwB6DBYOE?key=zwGjVZ7EjAL1kdZw6LWOIA)

Before jumping into CAD, I laid out some key goals:

1. Stability First: The robot needed to handle rough terrain without the constant balancing act of humanoid designs
    
2. Keep It Simple: Less components = fewer headaches (and fewer things to break!)
    
3. Efficiency Matters: Why waste energy constantly maintaining balance when you don't have to?
    
4. Useful: it had to handle real-world scenarios
    

## Leg Design![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeSyKljlyJ4-1ouLhzmDao4HhLlZe0UaO85gEHeHaPh8WM4FpaYuPe_2tKiU8uHhCFmSEo2uMfhVZRxrVci8NVkRSHj6X4FhCyki409UGP4_x9CBMiPuFrrhbWv86x3Yw1bWC9FHKYpfHHJrfsoHjUNGPjX?key=zwGjVZ7EjAL1kdZw6LWOIA)

The core innovation of this design centers on the leg mechanism, which comprises four essential components working in harmony. The hip motor drives the system. It acts as the primary actuator. Position feedback enables precise motion control.

The thigh rod connects to the motor assembly. It bears the primary structural loads. This component is crucial for the parallelogram mechanism.

The drive rod maintains geometric alignment. It keeps platform orientation stable. Vertical motion becomes decoupled from body orientation.![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcqyTRPj0r7Ss1IFxkfjjttLgarJkX0wqZ_CwkELP1vn67LvrQNu_aCuAZ0h20Ha1Hrqa6eRRlj2NSUkwE-OGjMViUqsjl1PltSl10rSWBlKoGR3s1dsX7dIvc5xHsEwEZncWIM3clXylOsPhv2dNRaU2Y?key=zwGjVZ7EjAL1kdZw6LWOIA)

The calf rod interfaces with the ground. It integrates the wheel assembly. Compliance mechanisms ensure smooth operation.

A single degree of freedom defines this system. It uses a parallelogram mechanism. This approach offers key advantages. The leg trajectory stays independent from body orientation. Control system complexity decreases significantly. Energy efficiency improves through simpler actuation.![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXedi1FlDbzOI__mvcOOB--Rdrw5rRaNwog74DBNICCuDbBPUsr_Aud5Ok0C7stFAK80p4mLMaIBujuOGMetdrc--R3_-LgSUcPkiDp4Z1ynqEQzyGkSPuIm2GKLYAxURHsCxIpU3jyy8CIaw7h7IWpVdGlC?key=zwGjVZ7EjAL1kdZw6LWOIA)

## Control System

I designed a dual-loop control architecture. The system separates body stabilization from leg motion. This separation maximizes stability performance.

Body stabilization uses LQR control. I selected four key state variables. Platform distance forms the position basis. Speed captures dynamic motion. Angle measures orientation deviation. Angular velocity completes the state feedback. Motor encoders deliver position data. An IMU provides orientation measurements.

I implemented state tracking as follows:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcuKeKEDXf-y81CwzDnIIFfAy9U-2ZLAE3mxQ6PB2-SrJ7GV0jyrMfqW9zdApTltTp-qdN8Hhfj9qWAXp_JppoltPIwDj-TcqWbhg0uOBvPdlvhZAyIS2YIizDR45kEYe43lYiBVB2KgyyCOhnMZ-52wH5r?key=zwGjVZ7EjAL1kdZw6LWOIA)

The system uses PID controllers for output generation. Each controller targets specific motion aspects. The outputs combine seamlessly. Real-time processing ensures smooth operation:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf0leDTirXxuvUSI5aE8FTTjFAq9dma6dB7-WVxsR8SKamI_VFUUcXlI34lpyYEDaV_DjK9a4xnSYhxy5Pb6YeJlYeaG3_6jinm-1FdBFrJwAknIX2_SfR4URyBNRzYfsyJ_XnLnMtWTKRldI2rXxF_s-A?key=zwGjVZ7EjAL1kdZw6LWOIA)

I chose ADRC for leg motion control. This handles terrain variations effectively. It estimates unknown surface conditions. Compensation occurs automatically. The robot maintains stability across different surfaces.

## Electronic Integration

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcZy2I-gntY09HXaG7NEWq2rUOQl2oaE2AmwDOTZQPHWB9viDcAQfZDVWtWp7OWyhvMCRqA5O6fgQf9We3fc9rFU1p1T08UdOznbeJxoPhIyrvt8ENrCWszlPM0U4_RJ6Ooi1PCUUnL9w8yP0pDjJoLxNLr?key=zwGjVZ7EjAL1kdZw6LWOIA)![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcp_vb9g7Ayr6MCBLAFtI5KSGyQAdJPCEBnA0qi_tyLEZdtPdGfeIZ8rS4g8kyNUYSD9omZVeii5l5cIyrnvaz3iqIUeRfXwVVzuggcaF113Xm9Qn6Dz8VV_CmVoG94ET41U1gRps9ChHY6uS3AjF8LuTwM?key=zwGjVZ7EjAL1kdZw6LWOIA)**