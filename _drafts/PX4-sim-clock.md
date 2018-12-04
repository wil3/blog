As test cases increase to full coverage, time to run the tests while also
increase. For SITL testing it is impractical to run simulations in real-time.
Instead we should be running the simulator as fast as the hardware allows. This
post is about how we can achieve this.

The problem: At the moment the MAVLink message `SYSTEM_TIME`  in PX4 is
implemented to return  
https://github.com/PX4/Firmware/blob/master/src/modules/mavlink/mavlink_messages.cpp#L1132-L1133

If the Gazebo plugins can get us the simulation time, an option is to broadcast
the system time from there.

In the Gazebo MAVLink plugin, the plugin communicates with the FC through the
local MAVLink instance using the port defined in, 
```
models/iris/iris.sdf:424:      <mavlink_udp_port>14560</mavlink_udp_port>
```
When you create a Gazebo Plugin you may subscribe to the
`ConnectWorldUpdateBegin` Event. This event is broadcast on every simulation
iteration. Therefore we can control how often this event gets called by
modifying the `real_time_update_rate` parameter in the world file specifying the
frequency in Hertz the physics updates will occur. 


The gazebo plugin opens listens 14560? The MAVLink connections are setup in
`src/modules/mavlink/mavlink_main.cpp  for the different modes (i.e., normal,
onboard, etc..). However the MAVLink connection for the simulator is missing
from here. 

I ended up finding it here here, `src/modules/simulator/simulator_mavlink.cpp`.

What is the update rate of our clock? In other words, when is OnUpdate in the Plugin called?

The plan is to,
* OnUpdate in Gazebo plugin, read the sim time
* send a SYSTEM_TIME MAVLink mesage
* Implement a receiver in PX4 to update the time_boot_ms


This seems like a big issue and is being discussed
[here](https://github.com/PX4/Firmware/pull/4817)
