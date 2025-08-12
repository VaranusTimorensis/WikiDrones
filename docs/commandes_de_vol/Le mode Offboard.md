---
sidebar_position: 3
---
### Le mode Offboard
Le passage en mode Offboard se fait par la méthode `engage_offboard_mode` reportée ci-dessous : 
```python
def engage_offboard_mode(self):
    """Switch to offboard mode."""
    self.publish_vehicle_command(
        VehicleCommand.VEHICLE_CMD_DO_SET_MODE, param1=1.0, param2=6.0)
    self.get_logger().info("Switching to offboard mode")
```

Une fois le mode Offboard engagé, il faut publier à une fréquence suffisante (?) un message Offboard afin de se maintenir dans le mode. C'est ce qui est fait par la méthode `publish_offboard_control_heartbeat_signal` :

```python
def publish_offboard_control_heartbeat_signal(self):
    """Publish the offboard control mode."""
    msg = OffboardControlMode()
    msg.position = True
    msg.velocity = False
    msg.acceleration = False
    msg.attitude = False
    msg.body_rate = False
    msg.timestamp = int(self.get_clock().now().nanoseconds / 1000)
    self.offboard_control_mode_publisher.publish(msg)
```
Cette commande précise comment le drône est contrôlé : en position, en vitesse, en accélération, etc.