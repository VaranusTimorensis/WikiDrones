---
sidebar_position: 6
---
### Piloter par contrôle d'attitude
Les messages VehicleAttitudeSetpoint permettent de contrôler l'attitude du drone, comme dans la méthode ci-dessous :
```python
def track_target(self):
    self.publish_offboard(attitude=True)
    dt = 0.1
    ex = (self.x_img - self.cx) / self.cx     # [-1,1]
    ey = (self.y_img - self.cy) / self.cy

    roll_cmd   = self.pid_roll.update(ex, dt)
    yaw_cmd    = self.pid_yaw.update(ex, dt)   # même erreur ex
    thrust_cmd = self.pid_thrust.update(ey, dt)

    pitch_cmd  = math.radians(self.pitch_deg)

    # Yaw absolu = yaw courant + correction
    q_xyzw = [self.q[1], self.q[2], self.q[3], self.q[0]]
    yaw_now = R.from_quat(q_xyzw).as_euler('xyz')[2]
    yaw_sp  = yaw_now + yaw_cmd

    q_wxyz = quaternion_from_euler(roll_cmd, pitch_cmd, yaw_sp)

    sp = VehicleAttitudeSetpoint()
    sp.timestamp   = int(self.get_clock().now().nanoseconds / 1e3)
    sp.q_d         = [float(v) for v in q_wxyz]
    sp.thrust_body = [0., 0., self.nominal_thrust + thrust_cmd]
    self.pub_att_sp.publish(sp)

    self.get_logger().info(
        f"ex={ex:+.3f} roll={roll_cmd:+.3f} yaw∆={yaw_cmd:+.3f} "
        f"thrust={self.nominal_thrust + thrust_cmd:+.3f}"
    )
```
La structure attendue du message est la suivante :
```bash
uint32 MESSAGE_VERSION = 1

uint64 timestamp		# time since system start (microseconds)

float32 yaw_sp_move_rate	# rad/s (commanded by user)

# For quaternion-based attitude control
float32[4] q_d			# Desired quaternion for quaternion control

# For clarification: For multicopters thrust_body[0] and thrust[1] are usually 0 and thrust[2] is the negative throttle demand.
# For fixed wings thrust_x is the throttle demand and thrust_y, thrust_z will usually be zero.
float32[3] thrust_body		# Normalized thrust command in body FRD frame [-1,1]

# TOPICS vehicle_attitude_setpoint mc_virtual_attitude_setpoint fw_virtual_attitude_setpoint
```

Avec ces messages, il faut penser à passer à True le booléen msg.attitude dans les messages `OffboardControlMode`.