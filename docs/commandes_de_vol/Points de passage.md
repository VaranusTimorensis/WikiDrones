---
sidebar_position: 5
---
### Contrôler un drone par des points de passage
La méthode `publish_position_setpoint` permet de contrôler le drone par des points de passages.

```python
def publish_position_setpoint(self, x: float, y: float, z: float):
    """Publish the trajectory setpoint."""
    msg = TrajectorySetpoint()
    msg.position = [x, y, z]
    msg.yaw = 1.57079  # (90 degree)
    msg.velocity = [0., 0., 0.]
    msg.acceleration = [float('nan')]*3
    msg.timestamp = int(self.get_clock().now().nanoseconds / 1000)
    self.trajectory_setpoint_publisher.publish(msg)
    self.get_logger().info(f"Publishing position setpoints {[x, y, z]}")
```
Voici la structure complète :
```bash
# Trajectory setpoint in NED frame
# Input to PID position controller.
# Needs to be kinematically consistent and feasible for smooth flight.
# setting a value to NaN means the state should not be controlled

uint32 MESSAGE_VERSION = 0

uint64 timestamp # time since system start (microseconds)

# NED local world frame
float32[3] position # in meters
float32[3] velocity # in meters/second
float32[3] acceleration # in meters/second^2
float32[3] jerk # in meters/second^3 (for logging only)

float32 yaw # euler angle of desired attitude in radians -PI..+PI
float32 yawspeed # angular velocity around NED frame z-axis in radians/second
```
Les booléens dans les messages `OffboardControlMode` doivent correspondre exactement aux données fournies dans les messages `TrajectorySetpoint`. 