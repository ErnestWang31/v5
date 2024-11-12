---
title: "Dummy-Robotic-Arm"
date: 2024-11-10
draft: false
---

**Quick video of the robot operating: [dummy_move.MP4](https://drive.google.com/file/d/19syZx1zGBMIvCTpJkcvSTVYOZutUH3FT/view?usp=sharing)**
Every engineer who's watched Iron M. n has dreamed of having their own JARVIS. While building a full-scale robotic lab assistant wasn't realistic, I decided to create my compact desktop version that could bring that sci-fi dream to my desk.

I named it "**DUMMY**" as a nod to Tony Stark's chaotic robot DUM-E. ![[Pasted image 20241109150655.png|right|250]]
A few requirements I had before making this project:
- It must be **compact**: Fit on my desk without taking up too much space
- It must be **smooth**: Needed precise, fluid movements for reliable operation
- It must be **powerful**: Required enough strength to be useful, not just a toy
- It must **look** **sick**: If you're building a robot assistant, it better looks fire!
# Firmware/Control Software {#firmware}
- [Stepper Motor Control System](../stepper-motor-control-system)
- [6-DOF Robot Kinematics](../6-dof-robot-kinematics) 
- [Motion Control System](../motion-control-system)
- [Motor Driver and FOC](../motor-driver-and-foc)
- [Encoder Calibration System](../encoder-calibration-system)
# Mechanical Design
## Body/Joints
I arrived at this final design after 3 prototyping iterations and 144 versions.
FEA of each joint and connection was conducted to validate the structural integrity of the robot.![[Pasted image 20241109151042.png|right]]
In the end, the CNC version of the robot can hold up to 2kg of mass on its end effector, which is equivalent to exactly 7 of my favourite Chinese drinks:
I am currently finding materials and redesigning the robot for 3D printing, to make the process cheaper and more streamlined.
**![|250](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeoFnI5t7OBq2lSQOIDl1KKcjLwoUPXpiQZAnRlLHAAsPGsKcxzb_r2PHkNMQDqCeFyI66sp2BvagmOlYTzTOea00w66EeWLDT4Kq--eWl9aY58syq9jkQO3WUsnmbwT9eHOhel4fhz0nJ9hep5HONBvOub?key=zwGjVZ7EjAL1kdZw6LWOIA)**
## Actuator Design
The original goal was to 3D print custom harmonic actuators, but the tolerance is just too tight for the operation to be smooth and accurate. But here is my design anyway.
**![|300](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdqgwOZalXKUUkEMgMWDEsqQUtCl99xX8Aox1uDETC4mpsuDVYOST4uiFejUR233nwReUCjl4rHEi0jl-Q97Tw0GIu0yueAu7yPs1iq4S-U9zpqYsOH-easIxCLdM-BATBGS_Ku4xNlrdcwRy5FWskXro6u?key=zwGjVZ7EjAL1kdZw6LWOIA)****![|200](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdx5ivxbcE_e5mW1Kbw76HRaVKRMRm6ngwn7sRrU74LVnDayMo1SbHn_OMm0tk6MrQcSG54KeeZ0S7_mvAVvY3qPYoPIPHvMG5K6BUCoyR0YuCxxxuDm36qj7hNG5gs22_aLe5xaSWGCutBRvmVkFmVtbdY?key=zwGjVZ7EjAL1kdZw6LWOIA)****![|200](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf_Ned4o7-vaGOtbCLnHSVNO0jZAvauALhq62wBhR8yjUxvStVvXLTis-IAIgKvlAxJFEY5P52G9lDj3zF4s_JdU4LsRoNCmJVUOsdortIcsTBVB0ejF2Y7BH8w8vUTExMiYM03oTCVh9qW2FfnI35rwSU?key=zwGjVZ7EjAL1kdZw6LWOIA)**
A harmonic reducer (or harmonic drive) works on the principle of mechanical wave propagation using three main components:
- A wave generator (input)
- A flex spline (flexible gear)
- A circular spline (rigid outer gear)
The key to its operation is the small difference in the number of teeth between the flex spline and circular spline. The flex spline 2 fewer teeth than the circular spline. When the wave generator rotates, it creates an elliptical shape in the flex spline, causing its teeth to mesh with the circular spline at two points. This tooth difference creates a slow, precise rotation of the output.
The formula used to calculate the ratio of input vs. output is ZcZc-Zf, where Z_c and Z_f are the circular and flex spline respectively. In this case, the ratio is 50. 
Then I made and prototyped a smaller version which would fit better for my application with a torque ratio of 1:20:
 ![[Pasted image 20241109151433.png]]
After playing around with custom 3D-printed actuators, I folded and opted for an off-the-shelf harmonic reducer because of issues with accuracy and friction with PLA.
# Next Steps?
I designed Dummy with 7 degrees of![[Pasted image 20241109151526.png|right|200]] freedom to mimic a human arm, making it adaptable for various robotic applications:
- Adding a claw end effector transforms it into a functional pick-and-place robotic arm
- Mounting it on a humanoid frame (which I named Trumbo) creates a basic humanoid robot
- Integrating it with a mobile base similar to a Roomba enables movement capabilities
While currently a conceptual design, Dummy's modular nature opens up endless possibilities for future applications.
