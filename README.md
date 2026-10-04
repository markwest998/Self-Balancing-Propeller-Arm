# Self-Balancing Propeller Arm 
## Overview:
I built a one-dimensional self-balancing arm from scratch using a Raspberry Pi Pico 2W, MPU-6050 IMU, brushless motor/ESC and a custom 3D-printed structure. The system uses accelerometer and gyroscope measurements within a complementary filter to estimate the arm angle, which is then regulated using a modified PID controller. From a range of initial base orientations and any sudden perturbations, the arm reliably converges to within around ±1° of the target angle within 5-10 seconds. This project took about a month to complete. 

<p align="center">
  <img src="https://github.com/user-attachments/assets/5cd33ece-e322-4828-9722-45c406142014" alt="Main Front" width="500">
  <img src="https://github.com/user-attachments/assets/33a3d235-10cc-4d66-813a-8e8b13e97d36" alt="Main Back" width="441">
</p>


## Demonstration:





## Aims and Motivation:
The motivation behind this project was to prepare me for my ultimate goal of building a quadcopter from scratch, so this project was more of a test run of a simpler system. This included learning how to design models within CAD, designing and soldering an electronic circuit consisting of a battery powered propeller that is controlled via a Raspberry Pi in conjunction with an IMU, and then creating and tuning a PID controller to control the propeller; all of which are concepts and practices I will encounter when I take on the quadcopter.

As for the self-balancing arm, my initial aim was to have it hover exactly vertically if left unperturbed, and if perturbed, for it to gracefully return to its original position within around 1-3 seconds. This however proved to be impossible for reasons I'll explain below, so eventually I settled on aiming for the arm to hover at an angle of 8 degrees away from the normal, and for it to return to its original position within 5-10 seconds. There were additional aims as well: I wanted the structure to not require plugging into some kind of external power source (so using a battery instead), and for the entire structure to not have any components dangling out the sides or with any exposed wires. I also wanted the structure to be as compact as possible, for it to be turned on and off via a switch, and to have the electronic components to be visible from the outside for aesthetic purposes. 

## Electronics:


## Structural Design and Assembly:

## Code:

## PID Tuning:

## Experimental Results:

## Problems / Iterations:
