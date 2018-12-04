After analyzing some quadcopter attitude step responses I noticed some werid
behavior illustrated in the image below, 

This images shows the angular velocity to be zero, however the control signals
are not uniform. The resulting behavior should have some pitch in the attitude
because motor 3 and 4 are not equal. 


Looking more closely and debugging in the Gazebo iris quadcopter demo the issue
appears to stem from forces being applied to the rotor joints at each update.
The rotor velocity is updated by the joints PID controller. The current P gain
used (0.01) is to low to achieve the desired set point. This might be
intentional otherwise to much P gain would cause oscillation and this would
have to be compensated with non-zero I and D gains.  

As a result at a lower than expected rotor RPM is reached and is limited by the
P gain. The desired velocity is never reached but the error ends up becoming
constant.  In turn the force applied to the rotor is contant. What this means
is there is a lower bound of the contorl signal in which any value above this
will max out. 

The P gain can be increased ( as well as the command max) to increase the
resolution. 

To make this more accurate, need to find the update latency from the FC control
signal to when the desired. Once we know this we can use it to define the rise
time in the step response in the Gazebo PID joint controller.

To tune the PID controller begin by increasing P, then I which will provide the
step to increase gradually. Finally addd D if any oscillation exists
