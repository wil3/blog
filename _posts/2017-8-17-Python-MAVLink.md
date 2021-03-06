---
title: MAVLink with Python
---
I'm in the process of migrating my old [blog](https://wil3.blogspot.com/) to
Github so bare with me. Also due to research in a new area (drone flight development) I'll be
updating this blog  frequently as a digital journal in an attempt to prevent an
all to familiar habit of not documenting anything and then forgetting how I did
something months or years later.  


Anyway, when it comes to software development with PX4 and other MAVLink enabled
vehicles, it appears Python using [DroneKit](https://github.com/dronekit/dronekit-python/) and MAVROS are popular methods. As my language of choice as a researcher is Python for numerous reasons I'm not
going to get into I'll be going down that path first to try and be independent
of ROS as long as I can.  


I've found most of the documentation for DroneKit to be high level (in fairness
DroneKits goal is to abstract some of the MAVLink complexities). 
Most of the examples [here](https://dev.px4.io/en/dronekit/example.html)  and
[here](https://github.com/dronekit/dronekit-python/tree/master/examples) make
use of the `command_long_send` method, but what other methods are available and
how to use them in Python?
The rest of
this post is about reverse engineering the module and uncovering how DroneKit actually communicates using the MAVLink protocol.

 
When a connection is made to a vehicle i.e.,
```python
vehicle = connect(connection_string, wait_ready=True)
```
which is located in `dronekit/__init__.py`, 
a `dronekit.mavlink.MAVConnection` instance is created. In this constructor the MAVLink instance is created from
`mavutil.mavlink.MAVLink` which is located in the [pymavlink](https://github.com/ArduPilot/pymavlink) Python module.


The variable `mavlink` in `mavlink` module is set dynamically using the
`set_dialects` method. There are different MAVLink interfaces depending on the
dialect. A dialect defines the messages exposed and are defined in an XML file. Details of
this are described in the [mavlink](https://github.com/mavlink/mavlink) repo. 

So how do we get the Python source code for this dialect? Well they are autogenerated from these XML files, mostly from  [https://github.com/mavlink/mavlink/blob/master/message_definitions/v1.0/common.xml](https://github.com/mavlink/mavlink/blob/master/message_definitions/v1.0/common.xml). 


To generate these bindings first clone the mavlink repo,
```
git clone --recursive git@github.com:mavlink/mavlink.git
```

Make sure to set the recursive flag to include the
[pymavlink](https://github.com/ArduPilot/pymavlink) submodule. Then generate the Python interface file,

```
cd mavlink/pymavlink/tools/
python mavgen.py -o mavlinkv10.py ../../message_definitions/common.xml
```
A good write up of this  process can be found
[here](http://qgroundcontrol.org/mavlink/pymavlink).

Once this file is generated you can see all of the methods you have at your
disposal for communicating with your MAV. 
Alternatively I've found this auto-generated  API doc for the module
[here](https://www.samba.org/tridge/UAV/pymavlink/apidocs/).

Now back in DroneKit once you have a vehicle object you can access any of the MAVLink
bindings. For example to send a
[SET_POSITION_TARGET_LOCAL_NED](http://mavlink.org/messages/common#SET_POSITION_TARGET_LOCAL_NED) message,

```
vehicle._master.mav.set_position_target_local_ned_send(...)
```

TLDR; Under the hood DroneKit uses the pymavlink module which includes an auto-generated Python MAVLink
implementation. If you have DroneKit/pymavlink installed you can find this API
in `pymavlink/dialects`. 

