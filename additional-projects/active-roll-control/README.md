<sub>[Pierre Arbaji](../../README.md) / [Additional projects](../README.md) / Active roll control</sub>

# Active Roll Control for a Model Rocket

**Undergraduate research · McGill Rocket Team, Active Flight Controls · May – August 2025**<br>
Co-authored with Majd Boulos · Funded by an $11,500 McGill Engineering Undergraduate Society grant, which I initiated and secured

**Full report:** [Design, Modeling, and Testing of an Active Roll Control System for a Model Rocket Using Fin-Mounted Control Surfaces](Active-Roll-Control-System-Report.pdf) (32 pages, MATLAB source included)

<p align="center">
  <img src="figures/test-rocket.png" width="80%" alt="CAD model of the roll-control test rocket, with control surfaces on the trailing edges of the fins">
</p>

## Problem

The McGill Rocket Team's long-term plan includes active flight control. This project was the first step: closed-loop control of roll rate using servo-driven control surfaces. Launch Canada's rules frame the design. Control is limited to roll, pitch and yaw, with guidance prohibited. The vehicle must stay stable with the system inactive, failsafes are required, and the system must remain dormant during the thrust phase.

## Design

- **Architecture trade study.** Fin-mounted trailing-edge surfaces were compared with canards on stability, responsiveness, mechanical and electrical complexity, servo torque and drag. Fin-mounted surfaces were selected because they preserve passive stability and are simpler to build, which suits a first roll-only iteration. Canards were noted as the better option for full three-axis control.
- **Testbed.** Single-stage solid-motor rocket (Aerotech I300T; 36.2 in long, 3.5 in diameter; 1.06 calibre static margin; about 3,300 ft predicted apogee). It was deliberately not optimised for mass or drag, so that the control hardware could be iterated quickly.
- **Fin can.** Three clipped-delta fins, each carrying a servo-driven control surface. Each fin is a detachable module with its servo and surface, so final assembly only requires connecting wiring to the avionics bay.
- **Actuation and electronics.** Hinge torque was estimated at about 0.09 N·m at maximum velocity. With a factor of safety of 2, an EMAX ES08MA II servo (2.0 kgf·cm at 6 V) was selected. The flight computer is an Arduino Mega with a 10-DOF IMU; the IMU code was written and calibrated in C++.

<p align="center">
  <img src="figures/fincan-assembly.png" width="65%" alt="Fin can assembly with servo-driven control surfaces, shown from two angles">
</p>

## Modelling and control

- **Vehicle model.** An OpenRocket model of the testbed provided the velocity and altitude profiles used by the controller simulation.
- **Roll dynamics.** A MATLAB model integrates the roll equation of motion at 100 Hz. Control torque scales with dynamic pressure (ISA atmosphere), aerodynamic damping opposes roll, and a fixed 1.5° fin cant applies a persistent disturbance torque.
- **Controller.** Torque-domain PID with a filtered derivative and a feed-forward term that cancels the cant torque.
- **Actuator model.** ±45° surface travel, 0.5° hinge deadband and a 20 ms first-order servo lag.
- **Safety logic.** Torque saturation, back-calculation anti-windup, and a low-dynamic-pressure guard that bleeds the integrator and parks the surfaces when there is no control authority.

## Results

In simulation over the flight profile, the controller:

- regulated roll rate to zero against the cant disturbance;
- tracked a 0 → 2 → 0 rad/s roll-rate command, with a brief saturation followed by an exponential approach, minimal chatter and no integrator windup;
- with a cascaded outer angle loop, performed two sequential +90° roll manoeuvres within an 11 s window while respecting the deflection and servo-lag limits.

<p align="center">
  <img src="figures/rate-tracking-response.png" width="90%" alt="Simulated roll-rate tracking response: roll rate against reference, control torque, surface deflection and dynamic pressure over 11 seconds">
  <br>
  <sub>Roll-rate tracking (0 → 2 → 0 rad/s): roll rate against reference, control torque, surface deflection and dynamic pressure.</sub>
</p>

## Status and next steps

Validation so far is simulation-based. The next steps defined in the report are:

1. A full-scale flight test to measure real actuator response, aerodynamic effects and latency.
2. Re-tuning to reduce overshoot.
3. Accounting for structural vibration and aeroelastic effects.
4. Extending the system to pitch and yaw for full three-axis control.

## My role

I initiated the project, secured its funding and set its scope. I derived the roll-axis dynamics, developed the closed-loop controller, and designed the test rocket and its actuation hardware.
