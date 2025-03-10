## Mathworks Toolboxes and Add-Ons Needed
* Control System Toolbox
* Optimization Toolbox

# Project Details

## Overview
This project of set of projects is to apply linear control theory and understand/optimize the performance and stability of PID controllers that are used to control the attitude and position of a space vehicle. 

## Preview of Current Progress

#### Single Axis PD Controller Tuning
The plot below shows a rigid body rotating about one axis with an optimized PD controller to control the rigid body to the desired reference angle (30 deg) based on the settling time and maximum overshoot percentage.

![system_response_plots](https://github.com/user-attachments/assets/6cb42494-fe66-440f-87ee-b325d44ac607)

The plot below shows the Bode plots for the closed loop system.

![bode_plots](https://github.com/user-attachments/assets/47f5c4f3-7f5f-4f65-9932-0990d6ebf57f)

---------------

#### Three Axis PID Controller Tuning for a Constant Reference Trajectory
The plot below shows a rigid body rotating about three axes with a PD controller to control the rigid body to the desired reference attitude (5 deg X-axis, -10 deg Y-axis, 15 deg Z-axis). The controller was tuned using pole placement (full state feedback) to achieve the desired settling times and maximum overshoot percentages. 

![system_response_plots](https://github.com/user-attachments/assets/02461306-ec9b-4ab8-8059-b22ed336d479)


The plot below shows a rigid body rotating about three axes with an optimized PID controller to control the rigid body to the desired reference attitude (5 deg X-axis, 10 deg Y-axis, -15 deg Z-axis). This controller started with the the PD controller gains from the scenario above, and was optimized with integral contribution to achieve the settling times and maximum overshoot percentages. For this scenario, the desired settling time decreased to 60 seconds relative to the scenario above.

![system_response_plots](https://github.com/user-attachments/assets/179f81f8-3384-4b28-a689-33e23b48d5fa)

To meet the lower settling time, the optimizer increased the controller response and gains. It's interesting to see that the optimizer zeroed out the integral gains. This is due to the lack of perturbations and real life effects being modeled, which would cause the controller to potentially have a steady state error offset. Thus, no integral contribution is needed if there's no steady state error offset.

#### Three Axis PID Controller Tuning for an Oscillatory Reference Trajectory
The plot below shows a rigid body rotating about three axes with the optimized PID controller in the scenario above to control the rigid body to the desired oscillatory reference attitude trajectory.

![system_response_plots_oscillatory_reference](https://github.com/user-attachments/assets/dd42487b-c1e4-4b5b-9254-ffe6b2ce2bd2)

The closed loop system response has a phase lag relative to the reference attitude trajcetory, such that the rigid body's attitude is trailing behind the reference attitude. Potentially a phase lead compensator can be added to the controller to increase the controller's response to the moving reference trajectory. 
