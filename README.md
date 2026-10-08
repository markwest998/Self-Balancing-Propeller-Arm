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
<img width="1244" height="495" alt="image" src="https://github.com/user-attachments/assets/891671d6-dbcb-42e6-a3cb-c769a2c830ad" />

Looking left to right, the LiPo battery supplies power into two paths: one to power the propeller, and the other to power the Raspberry Pi and MPU. 

For the Raspberry Pi Pico (lower) path, the buck converter is first used before the Pico to step down the voltage from approximately 12V to 5V; 12 volts is appropriate for the motor but far too high to supply directly to the Pico and could damage it. The Pico is then powered from this 5V supply, with the MPU receiving 3.3V via its VCC-3V3 connection. The Pico communicates with the MPU over I2C and the GP5-SCL connection provides the clock to keep information transfer in sync. 

When the arm starts to tip over, the MPU registers the linear acceleration and angular velocity of the MPU (located at the top of the arm) and sends this information to the Pico via the SDA-GP4 connection. These values are used in a complementary filter within the Pico's software to estimate the angle of the arm relative to the direction of gravity, which is then compared to the target angle. Then, combined with PID software on the Pico, the Pico creates and sends a corresponding PWM signal via the GP0's connection to the ESC.

For the motor (upper) path, the ESC is powered directly from the LiPo. The ESC interprets the PWM signal from the Pico and controls the power to the motor that corresponds to this PWM signal. The propeller therefore spins, providing the torque required to return the arm to the target angle.

## Structural Design and Assembly:

## Code:

## PID Tuning:

## Experimental Results:

## Problems / Iterations:
