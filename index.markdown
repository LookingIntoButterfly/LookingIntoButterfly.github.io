---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

title: Who’s Afraid of Butterflies?
layout: default
---

# Who’s Afraid of Butterflies?


This website provides some additional experimental results for the paper __Who's Afraid of Butterflies: A Close Examination of the Butterfly Attack__. Our evaluation goal is to demonstrate that (1) The Butterfly attack barely influences the existing jitter in real systems to a significant extent. (2) The jitters induced by such an attack have minimal impact on control jitters.


## Evaluation on Ground Vehicle

**Evaluation Setup**
We conducted the Butterfly attack on a ground vehicle powered by [ArduRover](https://ardupilot.org/rover/) 4.0 control software, wherein all controls operate on a single System-on-Chip (SoC). ArduRover is a part of the ArduPilot project and is specifically designed to control ground-based vehicles. The computing platform is Navio2, a board built on top of the Raspberry Pi 3b, complete with sensors and external peripherals such as radio control. To emulate the message delays necessary to execute the Butterfly Attack, we intentionally jammed three out of every four GPS messages.
**Evaluation Result**

The line in the figure is downsampled by 50, and the marker is downsampled by 200 to enhance the visualization. 

- Main Task Release Intervals


The subsequent figure displays the task release intervals for the main control loop operating at 50Hz and influenced by GPS jamming. Although the anticipated interval is 20ms, the observed average stands at 19.99ms. The variances under attack and non-attack conditions are 0.0019ms and 0.0011ms, respectively. The negligible difference can be attributed to the insignificant workload triggered by the GPS signal.

<center><img src="rover_results/rover_task_jitters.png" alt="Control Errors" width="600"/></center>

- Control Output Intervals

Another task manages the control output at 400Hz. To assess its sensitivity to jitters originating from the main loop task, we manually injected 1ms of jitters into the main loop task and studied its influence on the control output task. The results reveal average intervals of 2.49ms during the attack and 2.48ms without, indicating negligible differences.

<center><img src="rover_results/rover_control_jitters.png" alt="Control Errors" width="600"/></center>

- Actual Control Error 

we injected an additional 2ms of jitter into the control output to assess its potential negative impact on control performance. Our findings show that the consequences are negligible: the average control error in lateral acceleration, both under attack and normal conditions, is 0.003 m/s², with only a 0.4% increase in control errors.

<center><img src="rover_results/rover_control_error.png" alt="Control Errors" width="600"/></center>


## Evaluation on Ardupilot Drone

**Evaluation Setup**
The experiments were conducted using an actual drone equipped with Ardupilot 4.0, and featuring a Navio2 as the computing unit. We follow the same experimental setup in Butterfly attack paper by jamming the GPS message 3 over every 4 to create jitters on task `run_nav_update`. As we were unable to identify the precise vulnerable code section within the drone's software but found it within the ground station's code in version Copter-4.0, we have chosen this version as our target software for the study.

**Evaluation Result**

- Task Release Intervals. 

For better visualization, the plotted lines in figures below are downsampled by 100, and the marker is downsampled by 400.


The following plots the task release time for `update_GPS`. Under nominal conditions, the average interval is 2.507ms and 99.9% of the intervals lie within one variance 0.43ms. Under attack conditions, the average interval is 2.508ms and 99.9% of the intervals are also within one variance 0.43ms. This indicates no discernible difference between the attack and normal conditions.


<center><img src="ardupilot_results/drone_task_jitters.png" alt="Main Loop Interval" width="600"/></center>

- Control Output Intervals

The intervals of outputting control commands are depicted as follows. Under normal conditions, the average interval is 2.505ms with a variance of 0.45ms, and 99.95% of the intervals fall within one variance. In the attack scenario, the average interval is 2.51ms with a variance of 0.47ms, and 99.95% of the intervals also fall within one variance. There is no discernible difference in the jitters between the attack and non-attack scenarios either.


<center><img src="ardupilot_results/drone_control_jitters.png" alt="Control Output Interval" width="600"/></center>


- Control Errors

Increasing the jitters to a level that leads to significant degradation is challenging, as the control software is designed to be resilient to timing variations to a certain extent.  To explore this, we manually injected a 2ms jitter into the control output and recorded the control error in the velocity controller to gauge control performance. In both attack and non-attack scenarios, the control errors remain below the threshold of 0.03m/s, which is considered acceptable for drones compared to errors under normal conditions.


<center><img src="ardupilot_results/drone_control_error.png" alt="Control Errors" width="600"/></center>
