+++ 
draft = false
date = 2022-04-24T02:29:15+08:00
title = "0. Introduction and Setup Environment"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
katex  = true
+++

{{<bilibili bv1HA4y1X7s1>}}
# 0. Introduction and Setup Environment

Weihang Guo

This series of blogs are based on [F1tenth](https://f1tenth.org/learn.html)

> F1TENTH is a fun, fast-paced, and flexible course that teaches the foundations of **Autonomy**:
>
> - Perception
> - Planning
> - Control
> - Analytic skills to recognize and reason about situations with moral content in the design of autonomous systems
> - This course is an integration of the concepts above and is not intended to be a beginner course in any of the above subjects. As of right now, the contents on this website are best suited for the graduate-level if not at the very least an undergraduate senior-level course.

## ROS

You need to get familiar with [ROS](http://wiki.ros.org/ROS/Tutorials) before starting. I used **ROS melodic with Ubuntu 18.04** in the rest of the blog. My suggestion is read though 1-10 and 12. I realize all the algorithms in **python**. 

![image-20220424025349261](https://raw.githubusercontent.com/baboonSTW/Blog-img/main/202204240253334.png)

---

## F1tenth Simulator

### Install and run f1tenth_simulator on host computer

* Go to https://github.com/f1tenth/f1tenth_simulator and follow README.md for installation instructions;

* Summary of installation steps: 

  1. Update ROS dependencies: 

  `$sudo apt-get install ros-melodic-tf2-geometry-msgs ros-melodic-ackermann-msgs ros-melodic-joy ros-melodicmap-server` 

  2. Create a catkin workspace with the name you want (``f1tenth_ws` for example) and under the workspace, create a `src` folder
  3.  Git clone the `f1tenth_simulator.git` into the `xxx_ws/src/` subfolder of the workspace
  4. Run `catkin_make` to build the simulator
  5. Run `roslaunch` to launch the simulator 
  6. 6. In the rviz window, add `/map` topic and `/scan` topic

---

### Run the simulator

Use command`roslaunch f1tenth_simulator simulator.launch ` to launch the simulator. 

![image-20220424030946150](https://raw.githubusercontent.com/baboonSTW/Blog-img/main/202204240309268.png)

![	](https://raw.githubusercontent.com/baboonSTW/Blog-img/main/202204240307328.png)

Press keys in the main simulator terminal which displays the status.
Manually toggle driver modes

* B - AEB enable/disable
* J - joystick on/off
* K - keyboard drive on/off
* R - random walk on/off
* N - Navigation on/off
  $\mathrm{K}$ or $\mathrm{J}$ can be combined with other modes
  Keyboard drive:
* W - forward
* S - backward
* A - left turn
* D - right turn