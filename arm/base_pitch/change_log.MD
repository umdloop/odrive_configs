### Dec 17, 2024

#### V-0.1
State before config edits:
Motor attached to gearbox in isolation setup. Testing absolute encoder settings.
Absolute encoder on output of gearbox works form positional control, but there 
are issues with speed. The odrive accellerates way too fast, and the motor ends up
going at basically its max speed.

Changes:
- Created this folder.
- "axis0.config.I_bus_hard_min": -0.05,
- "axis0.config.I_bus_soft_min": -0.02,
- "axis0.config.startup_motor_calibration": true,
- "axis0.controller.config.commutation_vel_scale": 0.0014285714,
  - Note: Didn't set use_commutation_vel to true, so this change shouldn't affect anything
- "axis0.controller.config.vel_ramp_rate": 0.001,


Values to potentially change:
- "axis0.config.torque_soft_max": 1.0,
- "axis0.config.torque_soft_min": -1.0,



#### V-0.3
State before edits:
Velocity control doesn't work. Positional control works, but the motor always trips the 
motor regen limit. I think the axis0.config.I_bus_xxx_min controls how much regen current 
the motor is allowed to put into the brake resistor, not into the bus itself

Changes:
- "axis0.config.I_bus_hard_min": -Infinity,
- "axis0.config.I_bus_soft_min": -Infinity,

The above fixed the issue with regen current.

##### V-0.3.1
ODrive GUI changes:
- "axis0.trap_traj.config.accel_limit": 0.04,
- "axis0.trap_traj.config.decel_limit": 0.025,
  
This fixed the issue with the motor accelerating wayy too much. 
I also tried modifying the velocity integrator gain. My thinking was that since this value
is pretty much the torque gain, then lowering it will mean that the motor doesn't apply as 
crazy a torque increase. Since the rapid increases only happen a certain time after the input
request, I also think that this value is accumulating until it reaches high enough torque to 
overcome the sloppy/sticky design of the 3d printed gear box. Then, when it finally starts 
to move, the vel gain also kicks in, adding torque until it reaches the final destination

Making the vel_int gain 0 makes it so that the motor cannot move in this sticky gearbox

##### V-0.5
State before edits:
velocity control over CAN worked, but encoder position was not being reported. 

Changes:
- checked Send Feedback every 10 ms
- "axis0.controller.config.control_mode": 2, it used to be 3. Don't know why.
- looks like brake resistor has been disabled