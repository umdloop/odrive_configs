## Dec 18 2024

### V 0.2

#### Summary before changes
I tried to control the motor with the odrive can packages. I think that the heartbeat 
is causing the motor to block. I'm going to try disabling the heartbeat and see what 
happens. Specifically, I'm wondering if candump can0 will show messages being published,
since as of now the odrive isn't even sending a message on the CAN bus

#### Changes
- "axis0.config.can.heartbeat_msg_rate_ms": 0,

Still didn't work. I added in logger text to the trouble function which is the function 
callback for the request axis state (such as closed loop control)

Going to change the heartbeat back to the default setting of 10 hz


### V 0.3
#### Behavior before changes
Doesn't work, so the I'm going to implement the above changes

#### Changes
- "axis0.config.can.heartbeat_msg_rate_ms": 10,


## Dec 19 2024

### V 0.4
I found the error
I didn't configure the can bus rate to 1Mbps, instead it was set to 250kbps.

#### Changes
- "can.config.baud_rate": 1000000,
- "can.config.data_baud_rate": 1000000,
