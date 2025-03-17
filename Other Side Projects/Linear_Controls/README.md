## Mathworks Toolboxes and Add-Ons Needed
* Control System Toolbox
* Optimization Toolbox

# Project Details

## Overview
This project of set of projects is to apply linear control theory and understand/optimize the performance and stability of PID controllers that are used to control the attitude and position of a space vehicle. 

## Preview of Current Progress

#### Single Axis Attitude PD Controller Tuning for a Constant Reference Trajectory
The plot below shows a rigid body rotating about a single axis with a PD controller to control the rigid body to the desired reference angle (30 deg Z-axis), mass property of the rigid body has a moment of inertia of 85 kg*m^2. The controller was tuned using pole placement (full state feedback) to achieve the desired 90s settling time and zero maximum overshoot percentage. 

![system_response_plot](https://github.com/user-attachments/assets/a0476f74-4853-4d86-bab5-0f536522e92c)

The plot below shows a rigid body rotating about a single axis with an "optimized" PID controller to control the rigid body to the desired reference angle (30 deg Z-axis). This controller started with the the PD controller gains from the scenario above, and was optimized with integral contribution to achieve the settling times and maximum overshoot percentages. For this scenario, the desired settling time decreased to 60 seconds relative to the scenario above.

![system_response_plots](https://github.com/user-attachments/assets/c59083e9-9fc3-4fb5-ad2c-19169d7c41b3)

The optimizer can use some improvement, as it resulted in missing the desired settling time and maximum overshoot percentage. The integrator contribution was zeroed intentionally as it was found that it did not affect the controller's performance.

#### Single Axis PID Controller Tuning for an Oscillatory Reference Trajectory
The plot below shows a rigid body rotating about three axes using the same optimized PID controller in the scenario above to control the rigid body to the desired oscillatory reference attitude trajectory, with a frequency of 0.0067 Hz (1/150).

![system_response_plots_oscillatory_reference](https://github.com/user-attachments/assets/31dfdf25-f100-402d-b9f6-9cb7228bbf5a)

The closed loop system response has a phase lag relative to the reference attitude trajectory, such that the rigid body is trailing behind the reference angle. Potentially a phase lead compensator can be added to the controller to increase the controller's response to the moving reference trajectory or the PD controller gains can be increased to increase the closed loop system's response. The plot below shows the Bode plots for the closed loop system.

![closed_loop_bode_plots](https://github.com/user-attachments/assets/ab5e32e8-0e1a-4657-b89f-d937da09bc16)

There is ample gain and phase margin to increase the controller gains, thus with greatly increased proportional and derivative gains results in the improved closed loop system's response as shown below,

![system_response_plots_oscillatory_reference_increased_gains](https://github.com/user-attachments/assets/3a560a5b-9886-4aff-a845-9f5ff4b89b55)

Even with the increased proportional and derivative gains, there is still ample gain and phase margin as shown in the Bode plots below.

![closed_loop_bode_plots_increased_gains](https://github.com/user-attachments/assets/22c99da9-ec0c-41ba-86ac-43240ea26bef)

Note that the increased proportional and derivative gains shifted the magnitude curve rightwards in the magnitude plot, increasing the controller bandwidth frequency and thus increasing the closed loop system's response to higher frequencies. However in a realistic sitation, the torque required for this system is extremely high. As shown above, the required control torque peaked at 100 N-m. By introducing the control torque limiter at +/-1 N-m for a more realistic situation, this nonlinear effect affects the closed loop system's response as shown below.

![system_response_plots_oscillatory_reference_increased_gains_with_torque_limits](https://github.com/user-attachments/assets/99fdb821-661f-4e78-9311-26fa29c82d50)

With the control torque limited at 1 N-m, the closed loop system is able to maintain the desired oscillatory reference trajectory once it has converged closer to the reference trajectory. To resolve the initial overshoot and oscillatory closed loop system response, a potential solution is to introduce a guidance trajectory to smooth out the closed loop system's response during the initial minute.


---------------

#### Three Axis Attitude PID Controller Tuning for a Constant Reference Trajectory
The plot below shows a rigid body rotating about three axes with a PD controller to control the rigid body to the desired reference attitude (5 deg X-axis, -10 deg Y-axis, 15 deg Z-axis). The controller was tuned using pole placement (full state feedback) to achieve the desired 90s settling times and zero maximum overshoot percentages. 

![system_response_plots](https://github.com/user-attachments/assets/02461306-ec9b-4ab8-8059-b22ed336d479)


The plot below shows a rigid body rotating about three axes with an "optimized" PID controller to control the rigid body to the desired reference attitude (5 deg X-axis, 10 deg Y-axis, -15 deg Z-axis). This controller started with the the PD controller gains from the scenario above, and was optimized with integral contribution to achieve the settling times and maximum overshoot percentages. For this scenario, the desired settling time decreased to 60 seconds relative to the scenario above.

![system_response_plots](https://github.com/user-attachments/assets/179f81f8-3384-4b28-a689-33e23b48d5fa)

The optimizer can use some improvement, as it resulted in missing the desired settling times and maximum overshoot percentages. To meet the lower settling time, the optimizer increased the controller response and gains. It's interesting to see that the optimizer zeroed out the integral gains. This is due to the lack of perturbations and real life effects being modeled, which would cause the controller to potentially have a steady state error offset. Thus, no integral contribution is needed if there's no steady state error offset.

#### Three Axis PID Controller Tuning for an Oscillatory Reference Trajectory
The plot below shows a rigid body rotating about three axes using the same optimized PID controller in the scenario above to control the rigid body to the desired oscillatory reference attitude trajectory, with a frequency of 0.00167 Hz (1/600).

![system_response_plots_oscillatory_reference](https://github.com/user-attachments/assets/dd42487b-c1e4-4b5b-9254-ffe6b2ce2bd2)

The closed loop system response has a phase lag relative to the reference attitude trajcetory, such that the rigid body's attitude is trailing behind the reference attitude. Potentially a phase lead compensator can be added to the controller to increase the controller's response to the moving reference trajectory. 
