---
sidebar_position: 8
---
### Piloter en contrôlant la puissance des moteurs
#### Messages OffboardControlMode
```python
def publish_offboard_mode(self):
    msg = OffboardControlMode()
    msg.position = False
    msg.velocity = False
    msg.acceleration = False
    msg.attitude = False
    msg.body_rate = False
    msg.thrust_and_torque = False
    msg.direct_actuator = True
    msg.timestamp = int(time.time() * 1e6)
    self.offboard_ctrl_pub.publish(msg)
    self.get_logger().info('OffboardControlMode publié')
```

#### Publier les puissances des moteurs
```python
def publish_motors(self, motors_control):
    motors = ActuatorMotors()
    motors.control = motors_control
    self.actuator_pub.publish(motors)
    self.get_logger().info(f'ActuatorMotors publié: {motors_control}')
```

#### Message UORB
```
# Motor control message
#
# Normalised thrust setpoint for up to 12 motors.
# Published by the vehicle's allocation and consumed by the ESC protocol drivers e.g. PWM, DSHOT, UAVCAN.

uint32 MESSAGE_VERSION = 0

uint64 timestamp # [us] Time since system start
uint64 timestamp_sample # [us] Sampling timestamp of the data this control response is based on

uint16 reversible_flags # Bitset indicating which motors are configured to be reversible

uint8 ACTUATOR_FUNCTION_MOTOR1 = 101

uint8 NUM_CONTROLS = 12
float32[12] control # [@range -1, 1] Normalized thrust. where 1 means maximum positive thrust, -1 maximum negative (if not supported by the output, <0 maps to NaN). NaN maps to disarmed (stop the motors)
```