This post is provided to give an indepth look at BetaFlight's PID controller.
According to Betaflight is actually using a 2DoF PID controller modelled off of a [ISA-PID
compensator](https://www.mathworks.com/help/slcontrol/ug/designing-two-degree-of-freedom-pid-controllers.html#bsg5g_b).
The equation for the standard 2DoF PID control can be found
[here](https://www.mathworks.com/help/control/ug/two-degree-of-freedom-2-dof-pid-controllers.html). According to MathWorks, this controller allows for disturbances to be rejected quickly.  

This controller combines a PD feedforward controller with the traditional
feedback controller. The PD control is tuned with a `b` weight for proportional
and `c` weight for the derivative term. Although `b=1` so the `c` weight is the
only one that is tuned. 

There is a max rate specified by a static variable maxVelocity[3] for each axis

In pid.c, where rateAccelLimit is defined by the user and dT is the pid loop
time. The max rateAccelLimit is 500 deg/s?
```
maxVelocity[FD_ROLL] = maxVelocity[FD_PITCH] = pidProfile->rateAccelLimit *
100 * dT;                                                                           
maxVelocity[FD_YAW] = pidProfile->yawRateAccelLimit * 100 * dT;    
```


SP =  deg/s


Defaults for PID terms are,
[PID_ROLL] =  { 40, 40, 30 },                                                                                                                           
[PID_PITCH] = { 58, 50, 35 },                                                                                                                           
[PID_YAW] =   { 70, 45, 20 },  
c = 60


# Program Trace

Task
The main control loop is in, 
taskMainPidLoopa in fc_core.c which has multiple subtasks
pidController

0) gyroUpdate()
1) pidController()
	When the setpoint is calculated the rc input is converted to the actual
deg/s. Here all the expo rates and sensitivities are taken into account.
 This converted values are constrained by +- 1998.0f deg/sec however its not
clear where this number is derived.  

2) subTaskMotorUpdate()
		mixTable flight/mixer.c
			This is where the control inputs are given and mixed for the current
motor configuration. 
        writeMotors flight/mixer.c
 			pwmWriteMotor       in drivers/pwm.c 
				This function pointer gets assigned to whatever ESC protocol is
				being used.

3) subTaskMainSubprocesses()

These are the calculated PID values
set axisPID_P[axis], axisPID_I[axis] axisPID_D[axis]
a tpaFactor is added to each? what is this for?
