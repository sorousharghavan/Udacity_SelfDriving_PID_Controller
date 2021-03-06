# PID Controller
### Self-Driving Car Engineer Nanodegree - _Term 2, Project 4_
### By: **Soroush Arghavan**
---

_Build instructions are at the bottom of this page_

## Introduction
This project uses a PID controller to steer a simulator around a track autonomously.

To see a video of the vehicle driving a lap around the track autonomously visit the link below:
[Video link](https://youtu.be/rteyqHYAR4Q)

## Reflection
### The effect of the P, I, D component of the PID algorithm in implementation. Is it what you expected?
The P controller determines how agile the controller is to respond to deviation from the set point. The larger the P gain is, the larger the steer value will be. Ideally, a very large P gain is desired for this project. However, by increasing the P gain, the car will overshoot and begin to oscillate out of control. This is expected since the vehicle will begin to deviate further and further from the set point, increasing the steer value each time. This is where the D controller becomes handy.

Kp = 0.2, Ki = 0, Kd = 0;
![P Gain](./high_p.gif)

Kp = 1, Ki = 0, Kd = 0;
![High P Gain](./high_p_2.gif)


The D controller will dampen the amplitude of the oscillatiosn and allow for larger P gains. Again, this is aligned with the expectations since large P gains will cause larger differentials in steer values.

Kp = 0.2, Ki = 0, Kd = 2;
![D Gain](./d_gain.gif)

The I controller ensures that the vehicles remains close to the center of the track as even the slightest deviations will build up to a large steer value. However, since the simulator does not have any "friction" or dissipation of energy, even a small value of I gain will result in a slight oscillation about the set point (center of the track). This is not surprising since the I controller is usually used to overcome physical imperfections such as friction or bias.

###  How I chose the final hyperparameters (P, I, D coefficients)
The strategy I chose was to start with a proportional controller and find a sufficiently large P gain first. Then, to dampen the oscillations, I added a derivative controller and increase D gain from small values until the vehicle became stable. At the end, I added a small I gain in order to get the vehicle to stay closer to the center of the track. Below are the manual steps I took to tune the controller:

  //pid.Init(0.2,0,0.01); //Too much oscillation.
  //pid.Init(0.1,0,0.01); //incresing oscillation leading to destabilization
  //pid.Init(0.1,0,0.1); //reaction time of the controller is reduced leading to faster compensation of oscillation.
  //pid.Init(0.05,0,0.3); //car went on the shoulder but much more stable than before. need to increase kp to make controller faster.
  //pid.Init(0.07,0,0.3); //less oscillation, car on shoulder
  //pid.Init(0.055,0,0.7); //very little oscillation but car is lazy
  //pid.Init(0.06,0,0.8); // car is still lazy and on the shoulder
  //pid.Init(0.075,0,0.95); // car is still lazy and on the shoulder
  //pid.Init(0.09,0,1.2); //car is a lot more agile but still hits the shoulders on sharp turns
  //pid.Init(0.095,0,1.35); //car is a lot more agile but still hits the shoulders on sharp turns
  //pid.Init(0.1,0,1.65);//car is a lot more agile but still hits the shoulders on sharp turns
  //pid.Init(0.16,0,2); //almost perfect. add a bit of integral gain to centralize the car on the road
  //pid.Init(0.16,0.0005,2); // too much overshoot
  pid.Init(0.16,0.00025,2); // perfect!

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `./install-mac.sh` or `./install-ubuntu.sh`.

* Simulator. You can download these from the [project intro page](https://github.com/udacity/self-driving-car-sim/releases) in the classroom.

There's an experimental patch for windows in this [PR](https://github.com/udacity/CarND-PID-Control-Project/pull/3)

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

