---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

title: Who’s Afraid of Butterflies?
layout: default
---

# Who’s Afraid of Butterflies?


This website provides some additional experimental results for the paper __Who's Afraid of Butterflies: A Close Examination of the Butterfly Attack__. Our evaluation goal is to demonstrate that (1) The Butterfly attack barely influences the existing jitter in real systems to a significant extent. (2) The jitters induced by such an attack have minimal impact on control jitters.

## Evaluation on Ardupilot Drone

**Evaluation Setup**
The experiments were conducted using an actual drone equipped with Ardupilot 4.0, and featuring a Navio2 as the computing unit. The Navio2 is a board built on top of the Raspberry Pi 3b, complete with sensors and external peripherals such as radio control. We follow the same experimental setup in Butterfly attack paper by jamming the GPS message 3 over every 4 to create jitters on task `run_nav_update`. As we were unable to identify the precise vulnerable code section within the drone's software but found it within the ground station's code in version Copter-4.0, we have chosen this version as our target software for the study.

**Evaluation Result**

- Task Release Intervals. 

Figure 5(a) in the sumbitted paper is too dense to to show the actual values. For clearer visualization, the line in the figure is downsampled by 50, and the marker is downsampled by 200.
Although there are some abnormal timings, the average interval is in the nominal conditions 2.611 and 2.543 under attack. The average variance is 0.878ms and more than 87.0% of the task release intervals are within this threshold. While it is under attack, 89.2% are within 0.953ms. Besides, as evident from the figure, the attack doesn't cause a noticeable increase in jitters compared to those under normal conditions.


<center><img src="ardupilot_results/task_jitters.png" alt="Main Loop Interval" width="600"/></center>

- Control Output Intervals

We also show that the run_nav_update task's execution jitters have a minimal effect on the control output jitters. The figure below shows the jitter of the task that relays the control command to the co-processor. Notably, it exhibits significantly less jitter compared to previous tasks. This is largely attributable to the actuation command task being the first to execute in the main control loop.


<center><img src="ardupilot_results/control_jitters.png" alt="Control Output Interval" width="600"/></center>


- Control Errors

We further present the quantitative results of control errors to demonstrate that the attack outcomes caused by the Butterfly Attack are limited. We measure control errors by calculating the discrepancy between the desired and actual velocities. In two experiment sets, the control errors consistently stayed beneath the (0.03m/s) threshold, a range deemed acceptable for drone operations. Additionally, there are no substantial disparities between the two curves representing the control errors.


<center><img src="ardupilot_results/control_error.png" alt="Control Errors" width="600"/></center>

## Evaluation on Ground Vehicle

**Evaluation Setup**
We conducted the Butterfly attack on a ground vehicle powered by [ArduRover]{https://ardupilot.org/rover/} control software, wherein all controls operate on a single System-on-Chip (SoC). ArduRover is a part of the ArduPilot project and is specifically designed to control ground-based vehicles. The computing platform is the same as the previous setup, a Raspberry Pi 3b. To emulate the message delays necessary to execute the Butterfly Attack, we intentionally jammed three out of every four GPS messages. The software version is the same as the prvious evaluation - *Ardupilot Drone*.

**Evaluation Result**

The results below are also downsampled to enhance the visulization. 

- Main Task Release Intervals

The figure reports the task release interval of the main loop task in ArduRover `loop()`.
The average task interval under normal conditions is 2.574ms, while under attack conditions, it is slightly higher at 2.594ms. However, the difference is not significant. This is primarily due to the fact that in ArduRover, the variability in workload caused by skipped GPS messages is minimal.

<center><img src="rover_results/rover_task_jitters.png" alt="Control Errors" width="600"/></center>

- Control Task Intervals

To showcase that the control jitters maintain relative stability, we conducted measurements on the release interval of the task, which is responsible for sending control output to the co-processor managing the PMW signal for actuation. From the figure, we can observe that the jitters in the task that handles control messages are much more stable. This is mainly because it is also prioritized over other tasks, and thus is less impacted by the execution of other tasks.

<center><img src="rover_results/rover_control_jitters.png" alt="Control Errors" width="600"/></center>

- Actual Control Error 

The control errors are measured by the different between target acceleration and the current acceleration in each loop. The figure below shows that, in both scenarios - with and without an attack - the error remains below 0.02 m/s².

<center><img src="rover_results/rover_control_error.png" alt="Control Errors" width="600"/></center>