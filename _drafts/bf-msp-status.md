Eventually it would be great to have MSP support in
[dronekit](https://github.com/dronekit/dronekit-python). I've used this library
when working with [PX4](http://px4.io/) and it provided a high level API to
work. However I found my self having to work with the underlying MAVLink Python
library, [pymavlink](https://github.com/ArduPilot/pymavlink).  Until then I'll
be continually adding features to
[pyMultiWii](https://github.com/wil3/pyMultiWii). This library has been a good
start but still needs work. Something I've needed lately is to read status from
the FC to determine if the aircraft has been armed. Essentially we need an
is_armed function. Without this your only option is to arm, wait for some amount
of time, assume the arm occurred and then start executing your commands. Not as
elegant as I would like. 

Looking at the
[MSP](http://www.multiwii.com/wiki/index.php?title=Multiwii_Serial_Protocol) API
there looks to be a command MSP_BOXIDS	 that will return which "boxes" that are
enabled. Not really sure what "boxes" mean or how to interpret the response but
the API documentation mentions something about arm status so looks like a good
place to start to me. This post will answer the following:

1. What does boxes mean?
2. How to decode response from MSP_BOXIDS?



# 1. What does boxes mean?



fc/fc_tasks.c: taskHandleSerial
   msp/msp_serial.c: mspSerialProcess



After implementing the return in PyMultiWii to support MSP_BOXIDS I received the
raw response from the FC, (256, 1026, 1798, 4877, 6938, 7708, 8480, 9250).
