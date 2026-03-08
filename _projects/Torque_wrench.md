---
layout: project
title: Torque wrench CAD Rendering
description: Advanced CAD Project
technologies: [Autodesk Fusion and ANSYS]
---
<img src=/assets/images/Torque_wrench.png alt=Torque_wrench width="650" height="650">

This project aims to design a beam that would meet the constraints demanded. It had to withstand a torque of 600 in-lbf, have a Yield/Brittle FoS constraint of ≥ 4, a crack growth FoS constraint of ≥ 2,
a Fatigue FoS constraint of ≥ 1.5, and an output voltage ≥ 1 mV/V.

<img src=/assets/images/Driver.png alt=Torque_wrench width="500" height="575">,<img src=/assets/images/Sketch.png alt=Torque_wrench width="500" height="575">
<img src=/assets/images/Mesurments.png alt=Torque_wrench width="750" height="750">


For my design, I selected Ti-6Al-4V because its high yield strength allows the wrench to avoid permanent deformation and failure under peak torque. However, its elastic modulus is relatively low, which enables it to be slightly flexible and allows for higher-sensitivity strain-gauge measurement. The material also offers strong fatigue resistance, supporting multiple loading cycles, and high fracture toughness, which slows crack growth from small flaws. 

![Beam condition](/assets/images/Beam_condition.png)

For the FEM model, I first apply the displacement condition (Yellow) on the drive, telling the software that it does not move. I set the displacement to (0, 0, 0). Then I applied the force at the edge of the beam (red face) with a force of 37.5 lbf in the y direction, which allowed the software to determine the bending of the beam.

![Shaded rendering of earlier version](/assets/images/Material.png)

Next, I defined to the software what kind of material I was using by giving it the Young’s modulus and the Poisson's ratio. Giving the software the necessary components to calculate the displacement, stress, and strain of the beam.

<img src=/assets/images/Normal_strain_contours.png alt=Torque_wrench width="500" height="600">,<img src=/assets/images/Contour_plot_of_maximum_principal_stress.png alt=Torque_wrench width="500" height="600">
(Used max stress under clamped boundary line for the maximum principal stress since indicated max is likely artificial)

![Shaded rendering of earlier version](/assets/images/Static_Structural.png)

I can see at the base of the drive that the maximum and minimum normal stress occur (indicated max/min stresses at the clamp-handle intersection are assumed to be artificial), with the largest magnitude value being 31,727 psi in tension. Using this value gives a safety factor against yielding of 3.8, which is slightly lower than the required safety factor against yielding of 4. The hand calculations predicted that the wrench would have a significantly larger factor of safety with the selected material and dimensions based on beam theory.

![Shaded rendering of earlier version](/assets/images/Displacement.png)
The load point deflection from the FEM is 0.40865in, which is only slightly higher than the hand-calculated displacement of 0.3448 in.

![Shaded rendering of earlier version](/assets/images/Strain_gauge.png)

The strain at the location of the strain gauge has a magnitude of 1136.6 microstrain or 0.0011366 strain. The hand-calculated strain gauge strain was 1212 microstrain, which is very close to and also above the output voltage ≥ 1 mV/V. So, based on the FEM strain gauge strain of 0.0011366 and the hand calculations, the torque wrench sensitivity is 0.0011366 mV/V, which meets the minimum sensitivity requirement of 0.001 mV/V. From the DwyerOmega catalog, I found that gauge “SGD-3/120-LY11” works best with our design. The type is described as “3 mm Grid Length, 1.5 mm Grid Width, 120 Resistance, ST STC Number”. Since I want a gauge on both sides of the wrench where tension and compression are happening, we need the gauge length to be less than the wrench length and the gauge width to be less than the wrench thickness. The carrier length of 7.8mm is significantly smaller than the length of the wrench, and the carrier width of 3.8mm is smaller than the wrench thickness (0.5 in or 12.7 mm); therefore, this gauge fits well within the wrench. The thickness of the gauges (3.15 mm) is also much smaller than the width of the wrench (0.6 in or 15.24 mm), so the gauges will definitely have enough space.

All in all, I can see that my design of the beam meets almost all the criteria, and this is mainly due to the hand calculation having implicite assumptions to simplify the problem which when comparing to the real world just doesn't yeild the same results which is why we can see a difference in my results and also a difference in the behavior of the beam.
