# Project Details

## Mathworks Toolboxes and Add-Ons Used
* MATLAB Support for MinGW-w64 C/C++ Compiler
* Simscape
* Simscape Multibody
* ~~Aerospace Toolbox~~ Replaced with NASA JPL's SPICE Toolkit

## Overview
This Matlab-Simulink simulator provides a simulation environment to develop and test GN&C FSW algorithms with translation/rotational astrodynamics and GN&C actuator/sensor hardware models. 

## Preview of Current Progress
#### Sun Earth Nadir Pointing Mode Over One Orbit Period
Goal is to align the +Y body axis along the Sun vector and constrain the +Z body axis along the Earth nadir vector.

https://github.com/user-attachments/assets/aba0aee5-d5a9-4b44-8b49-2c1ac8e1a988

#### Slew to Sun Earth Nadir Pointing Mode With 4 Reaction Wheels (pyramid configuration), Simscape multibody video

https://github.com/user-attachments/assets/d3fc6f18-1a44-4a82-9971-3981435ee97a

![Simulink_Data_Inspector_Snapshot](/Plots_and_Videos/11_16_2024_SunNadir_PolarOrbit_Test/Simulink_Data_Inspector_Snapshot.png)

#### Slew to Sun Earth Nadir Pointing Mode With 4 Control Moment Gyroscopes (pyramid configuration, conventional CMG mode), Simscape multibody video
Note Control Moment Gyroscope (CMG) wheel mass properties were scaled down by factor of 10 of the reaction wheel mass properties and operational wheelspeeds set at 200 RPM, this allows us to visually see the gimbals rotating for this demonstration. Also note CMG gimbal and wheel max torque limits were scaled up by factor of 10 of the reaction wheel max torque limits, to reduce the time it takes ramp up the wheelspeeds to operational range.

https://github.com/user-attachments/assets/9e2aed14-e257-4c0b-abb4-4891a4976cd4

![Simulink_Data_Inspector_Snapshot](/Plots_and_Videos/01_22_2025_SunNadir_4CMGs_Test/AttitudeError_CMGs_Time_Profile.png)

#### Slew to Sun Earth Nadir Pointing Mode With 4 Control Moment Gyroscopes (pyramid configuration, wheel-only CMG mode), Simscape multibody video
Note the CMGs can be used as reaction wheels as well! Gimbal angles and rates are controlled to near zero. 

https://github.com/user-attachments/assets/25e63f3f-2789-49f2-ae9f-847c4ab967a6

![Simulink_Data_Inspector_Snapshot](/Plots_and_Videos/01_23_2025_SunNadir_4CMGs_WheelOnlyMode_Test/AttitudeError_CMGs_Time_Profile.png)

## Simulator Architecture
![GNC Matlab Simulator](/Plots_and_Videos/GN&C_Simulator_Architecture.png)


## Tasks

| Task                | Sub-Task     |  Status       | Notes                        |
|---------------------|--------------| --------------|------------------------------|
| Develop astrodynamics model |       |  |                              |
|| Set up simulation environment | In Progress      | task scheduler, solver settings, foundation for Simulink models/blocks/masks, implement configurable .json/.yaml files    |
|| Develop single rigid body rotational astrodynamics model with reaction wheels | Done     | utilizing simscape multibody toolbox     |
|| Develop single rigid body rotational astrodynamics model with control moment gyroscopes | [In Progress](https://github.com/TonyDTiger/My_Projects/issues/7)    | utilizing simscape multibody toolbox     |
|| Develop two-body orbit translational astrodynamics model             | Done      | Earth-satellite       |
|| Add Sun to astrodynamics model using SPICE ToolKit  | Done      | Sun-Earth-satellite (validated, see [commit](https://github.com/TonyDTiger/My_Projects/commit/e045363d198284494153262852be432f93adc091))       |
|| Add J2 perturbation to two-body astrodynamics model    | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/8)      |  |
|| Add air drag perturbation to two-body astrodynamics model  | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/8)     | atmospheric models (exponential, ...), flat plate, cannonball, panel models (rotational kinematics involved)      |
|| Add solar radiation pressure perturbation to two-body astrodynamics model   | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/8)      |  flat plate, panel models (rotational kinematics involved)      |
|| Add gravity gradient perturbation to two-body astrodynamics model   | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/8)      |  rotational kinematics involved      |
|| Add Earth magnetic field model to astrodynamics model   | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/28)      |       |
|| Add Moon perturbation to two-body astrodynamics model   | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/8)      |       |
|| Augment rotatable solar array wing(s) onto single rigid body rotational astrodynamics model | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/29)   |    |
| Develop GN&C FSW algorithms   | |      |   |    
|| Set up flight software environment |  [Not Started](https://github.com/TonyDTiger/My_Projects/issues/30)      | task scheduler (done), Object Oriented Programming architecture framework, solver settings, foundation for Simulink models/blocks/masks    ||
|| Develop reaction wheel control algorithms   | In Progress     | Nonlinear PD(done), Nonlinear PID with integrator windup limiter (verified, see [commit](https://github.com/TonyDTiger/My_Projects/commit/2af48a42e6087d56dd540e822083168cc7b3e857)), LQR. Implement torque limiter based on BCT RW4 motor torque specification, 0.25 N-m (verified, see [commit](https://github.com/TonyDTiger/My_Projects/commit/135a7ad0dfe1eefa58bf032221b1f0bfadbef94c)). Assume perfect sensors, use sim telemetry. Control law executes at a realistic frequency (20 Hz)
|| Develop control moment gyroscope control algorithms   |  [In Progress](https://github.com/TonyDTiger/My_Projects/issues/9)    | Nonlinear PD, Nonlinear PID with integrator windup limiter , LQR. Implement torque limiter based on TBD CMG motor torque specification, TBD N-m . Assume perfect sensors, use sim telemetry. Add CMG singularity estimation. Implement CMG mode selection: Wheel-only mode (done), conventional CMG mode (done), variable speed CMG mode. Control law executes at a realistic frequency (20 Hz outer kinematics loop, 100 Hz inner dynamics loop)                            |
|| Develop basic attitude guidance modes | [In Progress](https://github.com/TonyDTiger/My_Projects/issues/10)    |  Sun point, Sun-nadir point (done), Inertial point (done)                          |
|| Develop thruster-based translational control algorithms | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/11)      |  impulse delta-Vs                           |                      |
|| Develop TRIAD attitude determination algorithm | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/12)      |  Sun-Earth TRIAD                            |
|| Develop magnetic torque rod-based momentum management algorithm | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/18)     |  dependent on Earth magnetic field model development                             |
|| Develop basic attitude guidance algorithms | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/16)      |  SLERP, keep-out zone avoidance    |
|| Develop solar array Sun tracking algorithm | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/17)      |  dependent on rotatable solar array wing(s) development                             |
|| Develop QUEST attitude determination algorithm | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/13)     |  develop after star tracker sensor model has finished                            |
|| Develop Kalman filter attitude determination algorithm | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/14)      |  develop after star tracker and inertial measurement units sensor models have finished                            |
|| Develop Kalman filter orbit determination algorithm | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/15)     |  develop after star tracker sensor model has finished                            |
| Devleop GN&C hardware models       | |      |   |                              |
|| Develop analog Sun sensor model | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/19)       |   capture perturbations such as: noise, ...                          |
|| Develop digital Sun sensor model | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/20)        |                              |
|| Develop star tracker sensor model | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/21)       |  develop after celestial star catalog model has finished. capture perturbations such as: noise, ...                           |
|| Develop inertial measurement unit sensor model | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/22)      |  accelerometer and gyro, capture perturbations such as: drift bias, scale factor, noise                           |
|| Develop GPS sensor model | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/23)      |  develop after GPS satellites dynamics model has finished                            |
|| Develop reaction wheel actuator model | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/24)      |  capture perturbations such as: friction, wheel imbalance, back EMF                            |
|| Develop reaction wheel tachometer sensor model | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/25)        |  capture perturbations such as: noise, ...                           |
|| Develop magnetometer sensor model | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/26)       |  develop after Earth magnetic field dynamics model has finished                             |
|| Develop torque rod actuator model | [Not Started](https://github.com/TonyDTiger/My_Projects/issues/27)       |  develop after Earth magnetic field dynamics model has finished                            |

## Progress

- [x] Functional first-cut simulation
- [x] Define project tasks (this is iterative)
- [ ] Complete Kanban board of tasks
- [ ] Document reference frames used: inertial frame, body frame, solar system barycenter
- [ ] Develop unit tests of FSW algorithms and astrodynamics models
- [ ] Add Monte Carlo capabilities to project, and test run Monte Carlos
- [ ] ...

## Notes

**Resources**: Links to useful resources or documents
* NASA Planetary Fact Sheets
  * [Link to Planetary Fact Sheets](https://nssdc.gsfc.nasa.gov/planetary/planetfact.html)
    * [Definition of "Mean Longitude"](https://space.stackexchange.com/questions/8813/what-is-the-difference-between-mean-anomaly-and-mean-longitude)
    * [Definition of "Longitude of Perihelion"](https://astronomy.stackexchange.com/questions/11477/what-is-exactly-the-longitude-of-the-perigee)
  * [Table of Planets' Properties](https://nssdc.gsfc.nasa.gov/planetary/factsheet/)
* NASA Current State of the Art GNC Hardware
  * [Current State of the Art GNC Hardware](https://www.nasa.gov/smallsat-institute/sst-soa/guidance-navigation-and-control/#5.2)
* Resource 1 - Link 1 
  * This link ...
  
**Ideas**: Any new ideas or suggestions for the project
* Idea 1 - Link 1 
  * This idea ...
 
 **Lessons learned**: Lessons learned from this project
* Matlab
  * Simulink
    * Signals - A signal is a piece of data intended to change frequently, possibly every time step
    * Block parameters - A block parameter is an attribute of a block, and should be kept constant. Updating block parameters during simulations can slow it down, preclude the model from ever being compiled into a real-time application, and hide data dependencies. (https://blogs.mathworks.com/simulink/2011/03/08/how-do-i-change-a-block-parameter-based-on-the-output-of-another-block/)

## Coding Guidelines
### Variables
* No single letter variables. The only exceptions are ‘ii’, 'jj', 'kk', as an iteration index
* Lowercase Snake case is used for naming FSW variables (eg. SC_attitude_BN) and FSW functions (eg. quaternion_to_mrp)
* Uppercase Snake case is used for naming sim variables (eg. SC_Angular_Rates_BN)
* Pascel case is used for naming Simulink Models and Libraries (eg. FlightSoftware)
### Reference Frame Convention
* A variable expressed in one frame with respect to a second frame is represented by an underscore and two upper-case letters denoting the frame relation (eg. dcm_WB)
* A variables' components express expressed in a frame is represented by an underscore and one upper-case letter denoting the reference frame (eg. angular_rates_WB_B)
  * An inertia tensor of the hub about point C, defined in B frame components, is denoted by the following: inertia_C_B
  * A position vector from point A to point B, expressed relative to inertial frame is denoted by the following: position_BA_N


## Contact

- **Project Manager**: Tony Ly
- **Team Members**: Katarina Labuguen
