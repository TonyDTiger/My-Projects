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
The goal of this scenario is to align the +Y body axis along the Sun vector and constrain the +Z body axis along the Earth nadir vector. A polar orbit during the spring equinox is simulated so that the Sun vector can be near perpendicular to the Earth nadir vector relative to the spacecraft, this allows us to easily confirm that the Sun Earth Nadir Pointing mode functions correctly.

https://github.com/user-attachments/assets/aba0aee5-d5a9-4b44-8b49-2c1ac8e1a988

#### Slew to Sun Earth Nadir Pointing Mode With 4 Reaction Wheels (pyramid configuration), Simscape multibody video

The animation below shows the same scenario above using the Simscape Multibody visualization utility. A rigid body space vehicle is modeled using a PID control law with four reaction wheels to achieve the Sun Earth Nadir Pointing mode. The PID controller gains were selected using pole placement and manual tuning, as shown in the Linear_Controls project folder ("Three Axis PID Controller Tuning for a Constant Reference Trajectory" case).

https://github.com/user-attachments/assets/bae9cc37-9021-4564-9c98-de7bfb2b1879

![Simulink_Data_Inspector_Snapshot](https://github.com/user-attachments/assets/e3c9b95a-5b88-4972-9128-f7dad3abb5c6)

Note the control law demands very high and unrealistic reaction wheel torques. This can be resolved by using a torque limiter (nonlinear effect), attitude guidance law, or reducing the PID controller's gains/responsiveness. Taking the latter approach, the system response is shown below with more realistic reaction wheel torques but a longer settling time.

![Sun+YNadir+Z_03212024Ephemeris_Sim_Results](https://github.com/user-attachments/assets/48a4c422-3673-4b73-87d5-0ee304f9817f)

As a simulation validation check, the exchange of angular momentum between the space vehicle and reaction wheels is shown to be conserved by confirming that the total system angular momentum is nearly zero at all times given there are no external torques acting on the system.

#### Slew to Sun Earth Nadir Pointing Mode With 4 Control Moment Gyroscopes (pyramid configuration, conventional CMG mode), Simscape multibody video
For this simulation, the four reaction wheels above are replaced with four Control Moment Gyroscopes (CMGs). The CMG wheel mass properties were scaled down by factor of 10 of the reaction wheel mass properties and operational wheelspeeds set at 200 RPM, this allows us to visually see the gimbals rotating for this demonstration. This also highlights one of the main benefits of CMGs: given a smaller set of wheels, they can provide the same or potentially higher torque capability compared to the reaction wheels mentioned above. Also note CMG gimbal and wheel max torque limits were scaled up by factor of 10 of the reaction wheel max torque limits, this was done to reduce the time it takes ramp up the wheelspeeds to operational range for demonstration purposes.

https://github.com/user-attachments/assets/9e2aed14-e257-4c0b-abb4-4891a4976cd4

![Simulink_Data_Inspector_Snapshot](/Plots_and_Videos/01_22_2025_SunNadir_4CMGs_Test/AttitudeError_CMGs_Time_Profile.png)

#### Slew to Sun Earth Nadir Pointing Mode With 4 Control Moment Gyroscopes (pyramid configuration, wheel-only CMG mode), Simscape multibody video
Note the CMGs can be used as reaction wheels as well! Gimbal angles and rates are controlled to near zero. 

https://github.com/user-attachments/assets/25e63f3f-2789-49f2-ae9f-847c4ab967a6

![Simulink_Data_Inspector_Snapshot](/Plots_and_Videos/01_23_2025_SunNadir_4CMGs_WheelOnlyMode_Test/AttitudeError_CMGs_Time_Profile.png)

## Simulator Architecture
![GNC Matlab Simulator](/Plots_and_Videos/GN&C_Simulator_Architecture.png)

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
* A variables' components expressed in a reference frame is represented by an underscore and one upper-case letter denoting the frame (eg. angular_rates_WB_B)
  * An inertia tensor of the hub about point C, defined in B frame components, is denoted by the following: inertia_C_B
  * A position vector from point A to point B, expressed relative to inertial frame is denoted by the following: position_BA_N


## Contact

- **Project Manager**: Tony Ly
- **Team Members**: Katarina Labuguen
