
PX4 receives MAVLink messages in `src/modules/mavlink/mavlink_receiver.cpp`
In partiuclar the `handle_message_set_attitude_target` method receives and
proceses the 
[SET_ATTITUDE_TARGET](http://mavlink.org/messages/common#SET_ATTITUDE_TARGET)
MAVLink messages. 

This MAVLink message allows for a mask to ignore certain field. If attitude and
thrust are not ignored than these values are published to the
`vehicle_attitude_setpoint` topic. 

If the body rates and thrust are not ignored then publish the values to the 
`vehicle_rates_setpoint` topic.



The attitude controller, `mc_att_control_main.cpp` obtains new attitidue
setpoints through uORB through subscribing to the `vehicle_attitude_setpoint`
topic .

These values are polled from task main and control attitude


[Attitude Tuning](http://autoquad.org/wiki/wiki/configuring-autoquad-flightcontroller/tuning-and-troubleshooting/attitude-tuning/)
