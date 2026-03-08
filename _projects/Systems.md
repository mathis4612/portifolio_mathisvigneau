---
System project. 
---
This project aimed to analyse a chosen system and what capabilities it has, as well as its limitations.


**Topic of Interest:** Honeywell Room Fan
**Abstract:** I decided to study the Honeywell room fan. The system is an open loop with 4 settings for speed: Off, 1 speed, 2 speed, and 3 speed. The system requires user input (which is why it is an open loop). For each speed setting, I will study the settling time, steady state angular velocity, and overshoot (if present), and, based on these parameters, assess whether the design is optimal or if there’s room for improvement. I will be modeling and characterizing this system using some of the concepts I’ve learned in the systems dynamics class, such as ODEs, TFs, Block Diagrams, Parameter Estimation, Steady State analysis, and Step Response.

---
**The system Overview**
---
<img src=/assets/images/Honeywell_fan.png alt="Honeywell fan" width="300">

The TurboForce fan system is a fan produced by the company Honeywell. This fan possesses a unique feature in that it is relatively silent when turned on for any of the speed options that the fan presents. The fan has a 90-degree rotation and (LxWxH): 8.94 x 6.3 x 10.9 in. It weighs about 2.9 lbs according to the Honeywell website [1].

<img src=/assets/images/Fan_knob.png alt="Alt Text" width="300">

When the knob is turned, the fan controller mechanically switches between different electrical configurations that correspond to the different speeds wanted. Each configuration has a different impedance, so selecting a higher speed applies a higher effective voltage to the motor windings. Higher effective voltage produces a larger current in the coils, which generates a stronger magnetic field and increases the motor’s torque and therefore rotational speed.

<img src=/assets/images/Fan_motor.png alt="Alt Text" width="500">

Here, we can see the main components of the motor. For our dissection, we separated the two major parts: the stator and the rotor. The stator is the stationary part of the motor and provides a fixed magnetic field. The rotor, which is connected to the fan blades, consists of a metal core with conductive wire coils wrapped around it. When current flows through the rotor coils, it interacts with the magnetic field from the stator, generating torque that causes the rotor and the attached fan blades to rotate.

---
**ODE Modeling and Transfer Functions**
---

I decided to model this system using a first-order, open-loop ODE with respect to the angular rotation, w, in rads/s. I decided to use two states, w and i (current, units of Amps), to address both the mechanical and electrical aspects of this system. I assumed that the other relevant variables are rotational inertia ( I, units of kg/m^2), viscous damping (b, units of Nms/Rads), user-controlled motor torque (Tu, units of Nm), disturbance torque (Td, units of Nm), inductance(L, units of H), resistance (R, units of Ohm), and torque gain (Ka, units of Nm/V). Since there are four speed settings (including “off”) with increasing voltages based on the desired fan speed, I decided to utilize a step function variable that represents the user voltage u(t) that can take four different values 0, U1, U2, U3, with U1 representing the input voltage at the lowest speed setting and U3 representing the input voltage at the highest speed setting. By analyzing the following variables and their relation to the system, the following ODEs and relations were produced.

<img src=/assets/images/ODEs.png alt="Alt Text" width="500">

The fan works as an open loop system since the user provides an input to set the speed, and the system does not use feedback to adjust the input.

<img src=/assets/images/Model.png alt="Alt Text" width="500">

In this model, the disturbance torque Td ​represents external disturbance torques such as friction changes and airflow interactions; however, for a typical room fan in a controlled environment, I assume that these disturbances are negligible (Td0), so they will be omitted from the model going forward, and T will be equal to Tu, which equals Kai(t). Rearranging, we can find the time constant  for the electrical and mechanical equations: 

<img src=/assets/images/mechanical_equations.png alt="Alt Text" width="350">

The mechanical time constant is the term in front of the rotational speed derivative, which is I/b; hence,

<img src=/assets/images/Time_constant.png alt="Alt Text" width="200">

Thinking about this physically, this time constant makes sense since higher inertia means more resistance to motion and slower acceleration, and higher damping means faster settling, so smaller settling time. The electrical time constant is the term in front of the current derivative, which is L/R; hence,

<img src=/assets/images/current_derivative.png alt="Alt Text" width="200">

Thinking about this physically, this time constant makes sense since higher inductance resists changes in current, causing the current to rise more slowly when the input voltage changes, while higher resistance causes the system to stabilize faster.

To find (t), we first need to find i(t). Converting it to the frequency domain gives us: (No initial i or change in i, so initial conditions equal 0)
<img src=/assets/images/frequency_domain.png alt="Alt Text" width="350">  Solving for i(s),
<img src=/assets/images/solve_Is.png alt="Alt Text" width="250">

Now, we have an equation that can be used to find the equation for i(t) using the MAE 3260 Laplace table [2].  This expression best matches #12 in the table, where as(s+a) in the frequency domain equals 1-e-at in the time domain. To be able to use the table, we must first transform the equation to be similar to #12.

<img src=/assets/images/transform_equation.png alt="Alt Text" width="400">

In this form, we see that the equation is now in the form of #12, where a is R/L and Un/R is the gain term. Hence, we can now solve for (t), and we get:

<img src=/assets/images/wt_eq.png alt="Alt Text" width="500">

This equation describes the current over time, which ultimately leads to a second-order system. However, since electric dynamics happen extremely quickly, the system effectively behaves as a first-order system, and we can make the assumption:

<img src=/assets/images/first_order_equations.png alt="Alt Text" width="150">

Now, we can repeat this process to find w(t): 

<img src=/assets/images/repeat_process_wt.png alt="Alt Text" width="500">

Plug in i(t) assuming i(t)=Un/R: 

<img src=/assets/images/UnR.png alt="Alt Text" width="300">

Transform to the frequency domain with zero initial conditions: 

<img src=/assets/images/frequency_domain_initial_zero.png alt="Alt Text" width="300">

Find w(s):

<img src=/assets/images/find_w(s).png alt="Alt Text" width="200">

Also similar to #12, so convert: 

<img src=/assets/images/convert.png alt="Alt Text" width="250">

Convert to the time domain using the table: 

<img src=/assets/images/time_domain_using_table.png alt="Alt Text" width="250">


And now we have an equation that describes the rotational speed in terms of the defined parameters. From this equation, we can see that the steady state solution is (Ka*Un)/(b*R), which makes sense according to the parameters included and the fact that the rotational inertia is not included since it physically should not affect the final rotational speed value, just the time to reach the steady state. Graphing this system analytically gives us an idea of how the system behaves.

<img src=/assets/images/Graph.png alt="Alt Text" width="500">
As expected, rotational speed increases rapidly in the beginning from zero to the steady state value and then remains steady. Depending on the values of b and I, the system exhibits different settling times, with b>I showing faster settling times and I>b showing slower settling times. This shows that as engineers, we want to have large damping and small rotational inertia to optimize reaching the ideal rpm the fastest and carefully choose Ka, Un, b, R  to allow the system to produce different steady state rotational speeds that a user might want.


---
Step Response and Parameter Estimation
---

The Honeywell turbofan adjusts its speed by modulating the motor current rather than using PWM or direct voltage control. Therefore, the appropriate input for the step response is the commanded speed signal, since this is the control variable that causes a sudden change in motor current. The step response is obtained by applying a step change in the speed command and recording the resulting fan speed over time.

Unfortunately, we could not measure the rotating speed of the Honeywell fan nor read the input that the system is trying to achieve, since the speeds are only described as “1, 2, 3” and we were not able to find information on the input voltages for these settings, nor discern them from the dissection. Nevertheless, we were able to use video recordings to find the settling times by comparing the time from turning the knob to achieving stable fan rotation.

For speed 1: ts = 3.0 seconds

For speed 2: ts = 2.4 seconds

For speed 3: ts = 1.7 seconds

---
System Assessment + Improvements
---

Overall, the Honeywell Room Fan is designed very effectively in terms of system design. Having the system open loop is the right decision, as the goal of the fan is very simple and does not need to have a PID/feedback loop for its system. The rise time is very short as it reaches its steady state in a very short amount of time. This is because the fan is designed to have low rotational inertia and sufficent enough damping, as shown in the ODE and Transfer Functions section,  so the amount of torque necessary to accelerate it is low, and thus each setting has enough voltage to make it move comfortably. Other great decision choices include low weight, which allows the user to place the fan anywhere they want and even on the wall, a user-friendly design, as it is very simple to use with a single indicative knob to control the fan, and high rotatability, allowing the fan to rotate up to 90 degrees vertically to meet user needs. 

Some improvements that could be made would be to add a feature to make it a closed loop by adding feedback control. If the system could sense the temperature of the room, then it could modify its speed or turn off and on without the user's intervention. To make this possible, it would need to possess high-accuracy sensors that could well assess the environment from the location of the fan. Other practical suggestions include making the fan more accessible for cleaning/lubrication, since dust build up inside the system proves to be a common issue. 

---
References
---
[Honeywell turbofan](https://www.honeywellstore.com/store/products/honeywell-turboforce-air-circulator-fan-ht-900.htm#:~:text=In%20the%20winter%20months%2C%20the,pair%20with%20a%20heating%20system.&text=Unique%20blade%20design%20creates%20less,away) [1]

MAE 3260 laplace table [2] 

<img src=/assets/images/laplace_table.png alt="Alt Text" width="350">

