## Mathworks Toolboxes and Add-Ons Needed
* Control System Toolbox
* Optimization Toolbox

# Project Details

## Overview
This project of set of projects is to apply linear control theory and understand/optimize the performance and stability of PID controllers that are used to control the attitude and position of a space vehicle. 

## Preview of Current Progress

#### Single Axis PD Controller Tuning Using Pole Placement and Matlab's Optimizer
The plot below shows a rigid body rotating about a single axis with a PD controller to control the rigid body to the desired reference angle (30 deg Z-axis), mass property of the rigid body has a moment of inertia of 85 kg*m^2. As an intial start, the controller was tuned using pole placement (full state feedback), which is equivalent to a PD controller for this case, to achieve the desired 90s settling time and zero maximum overshoot percentage. 

![system_response_plot](https://github.com/user-attachments/assets/af1f06d1-a98a-4207-b360-4599ac1c3195)

The plot below shows a rigid body rotating about a single axis with an "optimized" PID controller to control the rigid body to the desired reference angle (30 deg Z-axis). This controller started with the the PD controller gains from the scenario above, and was optimized with integral contribution to achieve the settling times and maximum overshoot percentages. For this scenario, the desired settling time decreased to 60 seconds relative to the scenario above.

![system_response_plots](https://github.com/user-attachments/assets/80cb79d1-a9e3-4e09-8989-ed8139e35535)

The optimizer can use some improvement, as it resulted in missing the desired settling time and maximum overshoot percentage. The integrator contribution was zeroed intentionally as it was found that it did not affect the controller's performance. The next section will evaluate the controller's performance with an oscillatory reference trajectory.

#### Single Axis PID Controller Manual Tuning for an Oscillatory Reference Trajectory
The plot below shows a rigid body rotating about three axes using the same PD controller in the scenario above to control the rigid body to the desired oscillatory reference attitude trajectory, with a frequency of 0.0067 Hz (1/150). Performance metrics such as settling time and max overshoot percentage are not evaluated, instead the attitude error time profile will be evaluated.

![system_response_plots_cosine_reference](https://github.com/user-attachments/assets/ff1fb3d4-88d4-46bb-ab57-1801bd0da617)

The closed loop system response has a phase lag relative to the reference attitude trajectory, such that the rigid body is trailing behind the reference angle. Potentially a phase lead compensator can be added to the controller to increase the controller's response to the moving reference trajectory or the PD controller gains can be increased to increase the closed loop system's response. The plot below shows the Bode plots for the closed loop system.

![closed_loop_bode_plots](https://github.com/user-attachments/assets/2441c4ad-8cda-4597-8ba3-ae38ac303c91)

There is ample gain and phase margin to increase the controller gains, thus with greatly increased proportional and derivative gains results in the improved closed loop system's response as shown below,

![system_response_plots_cosine_reference_increased_gains](https://github.com/user-attachments/assets/148640cc-135d-4a29-ac63-14add2392a8b)

Even with the increased proportional and derivative gains, there is still ample gain and phase margin,

![closed_loop_bode_plots_increased_gains](https://github.com/user-attachments/assets/f2221388-2b73-4db6-8337-7290ab72ae57)

Note that the increased proportional and derivative gains shifted the magnitude curve rightwards in the magnitude plot with a slight increase in magnitude for lower frequencies, increasing the 3 dB controller bandwidth frequency from ~0.02 Hz to ~0.4 Hz and thus increasing the closed loop system's response to higher frequencies. However in a realistic sitation, the torque required for this system is extremely high. As shown above, the required control torque peaked at ~100 N-m. By introducing the control torque limiter at +/-1 N-m for a more realistic situation, this nonlinear effect affects the closed loop system's response as shown below.

![system_response_plots_cosine_reference_increased_gains_with_torque_limits](https://github.com/user-attachments/assets/3d51f4da-5f20-4310-b8f8-34be41ae7915)

With the control torque limited at 1 N-m, the closed loop system is able to maintain the desired oscllatory reference trajectory once it has converged closer to the reference trajectory. To resolve the initial overshoot and oscillatory closed loop system response, a potential solution is to introduce a guidance trajectory that smoothens out the closed loop system's response during the initial minute. Thus rather than a oscillatory reference trajectory with a sudden jump (cosine), an oscillatory reference trajectory with a gradual build from zero is used instead (sine) as shown below,

![system_response_plots_sine_reference_increased_gains_with_torque_limits](https://github.com/user-attachments/assets/45e49e3b-c22d-4db4-93bc-1b65b61298b3)

To reduce the bounded steady state attitude error further, the integrator contribution can be re-added as shown below,

![system_response_plots_sine_reference_increased_gains_with_torque_limits_with_2degintegrator](https://github.com/user-attachments/assets/53e646ab-70b7-4969-80a1-2201b3279f58)

To reduce the the bounded steady state attitude error even further, potentially feed forward compensation can be applied or the control torque limit can be increased to enable a faster closed loop system response. Otherwise for slower or lower frequency oscillatory reference trajectory as shown below with a frequency of 0.0033 Hz (1/300), this controller is able to track the reference trajectory better.

![system_response_plots_sine_reference_increased_gains_with_torque_limits_with_2degintegrator_300s_cycle](https://github.com/user-attachments/assets/1763f1d4-2cb0-4470-aefb-9f71d76ba612)

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
