# Self-Balancing Propeller Arm 
## Overview:
I built a one-dimensional self-balancing arm from scratch using a Raspberry Pi Pico 2W, MPU-6050 IMU, brushless motor/ESC and a custom 3D-printed structure. The system uses accelerometer and gyroscope measurements within a complementary filter to estimate the arm angle, which is then regulated using a modified PID controller. From a range of initial base orientations and any sudden perturbations, the arm reliably converges to within around ±1° of the target angle within 5-10 seconds. This project took about a month to complete. 

<p align="center">
  <img src="https://github.com/user-attachments/assets/5cd33ece-e322-4828-9722-45c406142014" alt="Main Front" width="500">
  <img src="https://github.com/user-attachments/assets/33a3d235-10cc-4d66-813a-8e8b13e97d36" alt="Main Back" width="441">
</p>


## Demonstration:

https://github.com/user-attachments/assets/cc19a248-7b5d-472b-b7f7-29e474a6c493

https://github.com/user-attachments/assets/626fe528-b719-49d1-bcd2-d1c44d32caf1

https://github.com/user-attachments/assets/6ee76d56-edfb-427c-8fbc-3b9a1a96ed86

https://github.com/user-attachments/assets/a21b0ab1-aff1-4e92-b919-6b75410c2ac8




## Aims and Motivation:
The motivation behind this project was to prepare me for my ultimate goal of building a quadcopter from scratch, so this project was more of a test run of a simpler system. This included learning how to design models within CAD, designing and soldering an electronic circuit consisting of a battery powered propeller that is controlled via a Raspberry Pi in conjunction with an IMU, and then creating and tuning a PID controller to control the propeller; all of which are concepts and practices I will encounter when I take on the quadcopter.

As for the self-balancing arm, my initial aim was to have it hover exactly vertically if left unperturbed, and if perturbed, for it to gracefully return to its original position within around 1-3 seconds. This however proved to be impossible for reasons I'll explain below, so eventually I settled on aiming for the arm to hover at an angle of 8 degrees away from the normal, and for it to return to its original position within 5-10 seconds. There were additional aims as well: I wanted the structure to not require plugging into some kind of external power source (so using a battery instead), and for the entire structure to not have any components dangling out the sides or with any exposed wires. I also wanted the structure to be as compact as possible, for it to be turned on and off via a switch, and to have the electronic components to be visible from the outside for aesthetic purposes. 

## Electronics:
<img width="1244" height="495" alt="image" src="https://github.com/user-attachments/assets/891671d6-dbcb-42e6-a3cb-c769a2c830ad" />

Looking left to right, the LiPo battery supplies power into two paths: one to power the propeller, and the other to power the Raspberry Pi and MPU. 

For the Raspberry Pi Pico (lower) path, the buck converter is first used before the Pico to step down the voltage from approximately 12V to 5V; 12 volts is appropriate for the motor but far too high to supply directly to the Pico and could damage it. The Pico is then powered from this 5V supply, with the MPU receiving 3.3V via its VCC-3V3 connection. The Pico communicates with the MPU over I2C and the GP5-SCL connection provides the clock to keep information transfer in sync. 

When the arm starts to tip over, the MPU registers the linear acceleration and angular velocity of the MPU (located at the top of the arm) and sends this information to the Pico via the SDA-GP4 connection. These values are used in a complementary filter within the Pico's software to estimate the angle of the arm relative to the direction of gravity, which is then compared to the target angle. Then, combined with PID software on the Pico, the Pico creates and sends a corresponding PWM signal via the GP0's connection to the ESC.

For the motor (upper) path, the ESC is powered directly from the LiPo. The ESC interprets the PWM signal from the Pico and controls the power to the motor that corresponds to this PWM signal. The propeller therefore spins, providing the torque required to return the arm to the target angle.

Finally, although not included in this schematic, a mechanical switch was added immediately after the battery on the ground wire before the splitting. 

## Structural Design and Assembly:
By far the most time consuming part of this project was designing the base and arm. I started first with a LEGO prototype to get a rough idea for the design; it included a basic straight arm with an axel on the bottom to connect to the base and allow for the arm to freely spin. For the base, I added high-rising walls on either side of the arm to stop the arms tilting at a given point so that it couldn't fall over completely and damage anything. I had also planned for the arm to cause the base to tilt over when the arm hits the limits of the base, and so added some extra plates on either end of the base to increase its length in the direction of the arm's motion. I was originally going to use this for my final design (wasn't taking the project very seriously at the time) had it not been for the fact that LEGO isn't particularly sturdy and the arm and base would rattle at the axel. Moving to 3D printing + CAD was therefore the next obvious step.

<img src="https://github.com/user-attachments/assets/4ccd1635-0377-46ea-9c2d-4ecabac37eff" alt="Main Front" width="270">
<img src="https://github.com/user-attachments/assets/d66232ba-132c-482f-9ac1-ac523c72d363" alt="Main Front" width="729">

Above shows the evolution of the designs I came up with before finalizing on version 4 on the right. My first model was very basic, more or less an equivalent to the LEGO model I'd made to test for sturdiness. The arm I'd put more thought into however, there was a cutout for the Pico to slot into such that the pins would be visible from the other side, another for the MPU to fit in snugly at the top, another for the wires of the motor to travel down the arm, two larger ones for the ESC and buck converter to fit into the arm, and one more for the USB cable to be connected whilst the Pico is fixed onto the arm. I also had holes for the motor and Pico to be fixed onto the arm, originally intending to make my own 3D printed compressible pins that would compress and then snap back after going through the hole to hold the Pico and motor in place. This idea would later be abandoned as I decided to simply super glue the Pico to the arm on the final version and, once I realized the motor must be embedded into the arm to avoid complications with the location of the center of gravity, thus shortening the distance between motor and outside to the length of a screw, I used the given screws for the motor instead. I also wanted to remove as much slack as possible between the arm and base, and so printed the arm to be exactly 0.4mm slimmer in width compared to the width of the inside arm of the base. I found this clearance to work surprisingly well and so didn't change it for all future versions. 

It was also around this time where I had decided that I wanted all components, including the battery, to be stored somewhere within the design and so, moving to version two, I had added some low-rising walls around the perimeter of the plates to house the cables and battery. I played around with various ideas about how to store the battery effectively and where exactly the subsequent wires would travel from the battery up to the electrical components on the arm, and eventually landed on the idea of storing the battery on its shortest edge such that the battery wires would run directly along the ground and not start halfway up the battery as I planned for the wires to travel under the axel of the arm, loop around the other walled plate on the other side, and then up the arm in the middle. To store the battery effectively, I added little bits of plastic into my design that would stick out from the walls to create a region that would exactly match the dimensions of the battery so that it wouldn't slide around. I had also planned to add a mechanical switch into my design of the base, and so had cut out a portion of the base to fit one next to the axel on the left (there was no space on the right with the battery taking up all the room). In doing so however, it would block way for the wires to travel back up the arm after they'd looped round, and so added an opening about a couple centimeters up so that the wires would travel above the switch instead. This did not work nearly as well as intended though, as the wires did not like being lifted up prematurely and so caused lots unevenness in tension on the arm as it tilted. Speaking of the arm, I had added the additional cut out for the motor to slot into the arm to keep the bulk of the mass located at the center of the arm. I'd also increased the depth of a lot of the cut outs mentioned earlier as the wires and components took up far more space than expected. There was however a surprise problem I hadn't anticipated in that the wires of the buck converter would sprout out the sides of the chip rather than the top (was assembling and soldering the electronic circuit alongside the designing of the base + arm and so hadn't factored in the additional thin wires between components into the design at the time; probably should have entirely completed the circuit and soldering first in retrospect), and so additional cut outs were scheduled for the following version as the buck converter didn't currently lie flat.  

Version 3 was more or less the final design. To fix the wiring problem relating to the switch, I decided to raise the height of the axel by about half a centimeter to allow just enough room for the switch to fit on the right side with the battery. This allowed me to bring the opening for the wires to travel back the arm to be lowered to its original height which worked far better in the testing that followed (more on this later). I also decided to round the corners of the walls of the plates to make it look a little nicer. I also had thoughts about potentially making a lid that would clip on top of the walls of the plates to hide the wires that ran underneath in a loop, and even did some little 3D printed tests of various designs for how the lid would clip on. The tests were either too stiff to not be clipped on at all, or not strong enough to withstand any deforming in order to clip onto the base reliably, so I abandoned the idea. For the arm, I added small cutouts for the wires of the buck converter to fit into the arm. I also found that plugging in jumper wires into the Pico when its attached to the arm to be difficult, as the depth of the current cutouts weren't large enough to push down the wires any further on the pins, so I had cut out another section to allow for that. 

I ran into a rather large problem though when it came to actually permanently installing the switch into the circuit. The switch sat about a centimeter above the ground where the wires from the battery ran, and so I would have had to try and curl the wires up and solder them onto the switch. This proved to be almost impossible as there isn't much space to fit your fingers or any tool to keep the wires up whilst you solder at the same time. Eventually, before committing to any soldering, I printed version 4 of the base, which now included a cut out on the bottom so I could push the wires from below. I also lowered the height of the switch to be level with the wires from the battery by removing some of the plastic stick-outs that held the battery in place (thankfully the switch was just the right size to replace this plastic bit anyway). Version 3 of the arm printed very badly for reasons I'm still unsure about, and so for the reprint, I added supports to the design such that the arm was printed vertically rather than lying flat. 

The following pictures are my final designs for the base and arm, with added diagrams for the dimensions:

<p align="center">
  <img src="https://github.com/user-attachments/assets/dc05e420-056d-4a3c-bebb-e457861a2637" alt="Main Front" width="500">
  <img src="https://github.com/user-attachments/assets/10d28d17-75ad-4287-b20c-f1025ee152b5" alt="Main Front" width="500">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/54107afe-b979-44c5-8b15-4614c3b3f54c" alt="Main Front" width="500">
  <img src="https://github.com/user-attachments/assets/0582b7e7-84e0-44d8-8530-00c484e75965" alt="Main Front" width="500">
</p>

I hadn't done much soldering before the project, so when I had to shorten or combine wires together, my first few attempts of soldering did not go very well at all and I had to cut sections off that failed badly and try again. Eventually I figured out that fraying both ends and then interlocking them and adding solder with heat-shrink over the top, then wrapping all wires together with electrical tape for added strength worked very well. The basic thin jumper wires from breadboard circuits were very fragile when trying to solder the buck converter to the Pico, and so I bought and used thicker wire (18 AWG) instead. There were a couple times where I tested the circuit to see if it would work, in which I hadn't covered the open wires where they were soldered on the components, and also hadn't noticed that my metal axel was lying on the table connecting two of them together. Thankfully nothing was permanently broken but the loud bang I received when turning on the switch made me far more careful and wrap every open wire in electrical tape after that. I was also very careful about the propeller and it potentially cutting me if I wasn't careful, although there was a time where I had removed the propeller, but the motor was lying sideways on the table when I tested the circuit, causing it suddenly and very quickly roll off the table, taking all the electrical components with it. Definitely didn't do any more tests without bolting down the motor first after that. 

## Code:



## PID Tuning:



## Experimental Results:



