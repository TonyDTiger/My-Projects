## Python Libraries Needed
* numpy
* scipy
* matplotlib
* tkinter

## Overview
This project analyzes a linear time invariant single mass spring damper system with an open-loop input force (either sinusoidal or unit step) that's continuously applied on the single mass. Using linear algebra and linear controls theory, we can assess the long term behavior, responsiveness, and stability of this open-loop system. There are different mathematical definitions for what stability means, but overall we're looking to check if the system's states are bounded and do not grow infinitely over time. The system response (position of the single mass) is evaluated in the time domain and in the frequency domain. As a fun experiment, a GUI was created to evaluate the change in the system's response relative to variations in the system's parameters and input force.

## Single Mass Spring Damper System
The image below shows the common single mass spring damper system,

![spring_mass_damper_system_diagram](https://github.com/user-attachments/assets/1be6b45f-47eb-461a-b2e1-2c0c7afa18ba)

## Mathematical Walkthrough
A classic single mass spring damper system consists of:
- A mass \( m \)
- A spring with stiffness \( k \)
- A damper with damping coefficient \( c \)
- An input force function \( F(t) \)

Using Newton’s second law ($F = ma$) and a free body diagram, one can obtain the following 2nd order linear differential equation that represents the equation of motion for this sysytem:

$$
m \ddot{x}(t) + c \dot{x}(t) + k x(t) = F(t)
$$

Where:
- $x (t)$: Position of the mass, relative to the equilibrium point (when all forces and motion are zero)
- $\dot{x}(t)$ : Velocity
- $\ddot{x}(t)$: Acceleration

### Linear Algebra Time Domain Evaluation: State Space Matrices

Something cool we can do is convert the 2nd order linear differential equation above into a 1st order linear state space representation that follows this format:

$$
\dot{\mathbf{x}} = A\mathbf{x} + B u(t)
$$
$$
y = C \mathbf{x} + D u(t)
$$

By obtaining this 1st order state space representation, we can apply linear algebra concepts to evaluate the long term behavior of this system when it is perturbed. To make this conversion, we'll need to define the state variables as such:

- $x_1 = x$ (position)
- $x_2 = \dot{x}$ (velocity)

Resulting in the following state vector that we can use for the state space representation:

$$
\mathbf{x} = \begin{bmatrix}
x_1 \\
x_2
\end{bmatrix}
,\quad
\dot{\mathbf{x}} = \begin{bmatrix}
\dot{x}_1 \\
\dot{x}_2
\end{bmatrix}
$$

Using the 2nd order linear differential equation, we can formulate the time derivatives for the state variables:

- $\dot{x}_1 = x_2$
- $\dot{x}_2 = \ddot{x} = \frac{1}{m}(F(t) - c x_2 - k x_1)$

Resulting in the following state space matrices:

$$
A = \begin{bmatrix}
0 & 1 \\
-\frac{k}{m} & -\frac{c}{m}
\end{bmatrix},\quad
B = \begin{bmatrix}
0 \\
\frac{1}{m}
\end{bmatrix},\quad
C = \begin{bmatrix}
1 & 1
\end{bmatrix},\quad
D = \begin{bmatrix}
0
\end{bmatrix}
$$

Where the input is the input force and the output (aka observed state(s)) are:
- Input: $\ u(t) = F(t) \$
- Output: $\ y(t) = x(t)\$, we'll assume we have a perfect understanding of the state variables

We have now properly defined a 1st order state space representation of this system. Next by applying the linear algebra concept of eigenvectors and eigenvalues, we can evaluate the system's long term behavior and stability by looking at the eigenvectors and eigevenvalues of the A matrix, also known as the state transition matrix (a matrix that calculates the states' change to transition to the next time step). An eigenvector is a unique and rare vector that only has its magnitude scaled (the eigenvalue tells us how much it's scaled) after being multplied by the A matrix, and thus its direction is fixed at all times. What this means for us is that we can utilize these eigenvectors and eigenvalues as a compass to understand how our states (no matter what values they start with) will change long term over time, since we can represent all possible state vectors through the linear combination of a set of eigenvectors. The caveat to do this is that we need the same number of unique eigenvectors and eigenvalues as our number of states. Moving forward it is assumed the reader understands how the eigenvalues and eigenvectors of a square matrix are calculated.

Once we find a set of unique eigenvectors ($e_i$) and eigenvalues ($\lambda_i$), we can define a function that describes how our state evolve over time using the eigenvectors and eigenvalues as a compass, 

$$
x(t) = c_{1}e^{\lambda_1 t}e_1 + c_{2}e^{\lambda_2 t}$e_2
$$

Note the eigenvalues can be a complex number: a + ib. To assess whether the system is stable, we assess the real component of each eigenvalue and check to see if they're all less than zero. If they are not, then the exponentials with positive eigenvalues will exponentially grow over time resulting in exponential growth (unstable) in one or more of our states.

### Linear Controls Theory Frequency Domain Evaluation: Transfer Function

Now when the system is forced by an input force, we can evaluate the system's responsiveness and stability by using linear controls theory concepts. To bypass the need of running multiple time domain simulations and help simplify some math, we can utilize a transfer function which is a ratio of the system's output (position) relative to its input (input force), expressed as a ratio of polynomials in the complex frequency domain (Laplace). Thus for a given sinusoidal input force at some predefined frequency and growth/decay rate, we can predict how the system will respond by using this transfer function. To get the transfer function of this system, we can take the Laplace Transform of the 2nd order linear differential equation above, which results in:

$$
m(s^2 X(s) + s X(0) + \dot{X}(0)) + c(s X(s) - X(0)) + k X(s) = F(s)
$$

where complex variable ‘s’ ($s = σ + iω$) represents a sinusoid at some angular frequency ω multiplied by an exponential at some decay/growth rate.

Note the transfer function is strictly defined under the assumption of zero initial conditions; that is, $X(0) = \dot{X}(0) = 0$. The transfer function $G(s)$ is the ratio of the Laplace-transformed system's output $X(s)$ to the system's input $F(s)$:

$$
G(s) = \frac{X(s)}{F(s)} = \frac{1}{m s^2 + c s + k}
$$

We can utilize this transfer function to understand the system's zero state response for a given complex sinusoidal input force. Note that for a linear time invariant system as we have here, the system's output is proportional to the input. That is, the system's output will also be a complex sinusoidal waveform at the same frequency as the input but at a different amplitude and phase which is characterized as the amplification factor and phase shift. Think of amplification factor as the download/upload speed and phase shift as latency/ping on your computer, higher amplification factor (download/upload speed) and lower phase shift (latency/ping) means a more responsive system (computer) for the applied input force (software application). Moving forward, it is assumed that the input force is pure sinusoidal and with unit amplitude. Thus the complex variable ‘s’ becomes $s = 0 + iω$, this is equivalent to using the Fourier transform instead of the Laplace transform. We will revisit this Laplace domain transfer function further down.

## Results
A GUI was created to evaluate the change in the system's response relative to variations in the system's parameters and input forces. An example of a stable and unstable system are shown below.

The GUI will show two things: 
  1. Time domain (top) plot: the system's response relative to the input force and the system parameters chosen.
  2. Bode (middle and bottom) plots: the system's amplification factor and phase shift for each frequency that the sinusoidal input force is fed in with.

### Reality Check 
To confirm that everything is set up correctly, let's check that the plots are mathematically correct with a stable system. First, let's input a sinusoidal force with a frequency of 0.2 Hz into this system. 

![GUI_screenshot_m=1_k=4pt5_c=1_inputfreq=0pt2](https://github.com/user-attachments/assets/edf28e8f-77cf-4714-8dc8-77cb6b3718d1)

Looking at the Bode plots, this results in an amplification factor of ~-10 dB (equivalent to ~0.3) and a phase shift of ~25 deg. Then looking at the time domain plot, the position response has a sinusoidal signal with an amplitude of ~0.3 (0.3 amplitude gain of the input force's unit amplitude is 0.3) and its peaks/valleys lags slightly behind the input force's peaks/valleys. Thus all checks out! If we run the time domain simulation with an input force frequency of 0.6 Hz, using the Bode plots, this should result in a lower amplitude and more delayed system response.

![GUI_screenshot_m=1_k=4pt5_c=1_inputfreq=0pt6](https://github.com/user-attachments/assets/8adc90fc-4e94-4143-9ff3-f80593f39bcd)

A frequency of 0.6 Hz for the input force's frequency results in an amplification factor of ~-20 dB (equivalent to 0.1) and a phase shift of ~160 deg. Here the system's response is more attenuated and more delayed, compared to the previous system response above with the input force at 0.3 Hz frequency.

### Linear Algebra Stability Evaluation
As noted above in the Mathematical Walkthrough section, we can check the stability of a linear time invariant system by assessing the sign of the state transition matrix's eigenvalues. An example of a stable system with negative eigenvalues is shown below with a unit step input force,

![stable_unit_step_m=2pt5_k=2_c=4_inputfreq=0pt1](https://github.com/user-attachments/assets/96aa8865-0867-4115-91d0-8104244038fb)

The system's response settles on a steady state and thus remains bounded over time. Now we'll look at the same system with a unit amplitude sinusoidal input force shown below,

![stable_sinusoidal_m=2pt5_k=2_c=4_inputfreq=0pt1](https://github.com/user-attachments/assets/480c9a63-6b7b-43d2-bb11-29fd22bb1c32)

Here the system's response does not settle on a steady state, due to a constantly changing input force, but does remain bounded over time and thus stable. After tinkering around with the GUI, it's not possible to obtain a spring mass damper unstable system with positive eigenvalues since the spring and/or damper always naturally drives the system to a steady state. It would take a negative spring stiffness and/or damping coefficient to create this unstable system, which is not naturally common. In the next section, we're going to look at an interesting scenario that causes the system to become unstable through resonance.

### Linear Controls Theory Frequency Domain Stability Evaluation

Let's now evaluate a system with zero damping and a unit step input force as shown below,

![stable_unit_step_m=2pt5_k=2_c=0_inputfreq=0pt1](https://github.com/user-attachments/assets/5a2e237f-f254-4a26-8383-c1e16c4c9864)

Compared to the previous system with damping, this system response does not settle on a steady state but does remain bounded over time and thus stable. Note the state transition matrix's eigenvalue real components are zero, confirming that the system response does not decay nor grow over time. A system like this with zero eigenvalue real components is declared marginally stable. Now we'll look at the same system with a unit amplitude sinusoidal input force with a special frequency of 0.16 Hz as shown below,

![unstable_sinusoidal_m=2pt5_k=2_c=0_inputfreq=0pt16](https://github.com/user-attachments/assets/7d162984-dbc4-4d39-b2d0-1638b54d8455)

The system response becomes unstable, since it grows unboundedly over time! This happened because the input force resonated with the system, similar to pushing someone on a swing at the right time to move them higher and faster. Looking at the Bode plots, the frequency of 0.16 Hz aligns closely with the peak of the magnitude plot and also the minimum (-180 deg) of the phase plot. This can be a bad setup for this system, because an input force at this frequency will result in greatly amplifying the system response and turns negative feedback into positive feedback, which results in the unstable infinitely growing system response shown above.

## What is the purpose of all of this?
Mentioned above, the purpose for all of this is to understand a spring mass damper system's response (amplification factor and phase shift) for an open-loop input force (either sinusoidal or unit step). If the system is a linear time invariant system, we can quickly assess the stability of the system through an evaluation of the state transition matrix's eigenvalues and if the real components are negative. From a different perspective, we can identify a couple key characteristics of the system's response to an input by running a sweep of sinusoidal input forces at different frequencies: 
  1. The frequency for when the system response starts to decay in amplification factor. This tells us the system's cross-over frequency (open-loop bandwidth is defined at -3dB) or how responsive our system is. 
  2. If any, the frequency for when the system response has reversed polarity (ie. phase shift is 180 deg) and if the associated amplification factor is high enough to be a concern. This tells us how far the system is from instability. That is if we try to control this system at this critical frequency, this is equivalent to having the controls suddenly reversed which will inevitably result in a unstable system with positive feedback, similar to steering a car out of control when you're trying to steer left but the car turns right instead.

