---
sidebar_position: 4
---
### Armer et désarmer
La méthode `arm` permet d'armer le drone. La voici isolée du reste du code :
```python
def arm(self):
    """Send an arm command to the vehicle."""
    self.publish_vehicle_command(
        VehicleCommand.VEHICLE_CMD_COMPONENT_ARM_DISARM, param1=1.0)
    self.get_logger().info('Arm command sent')
```

Pour désarmer, il suffit de donner à `param1` la valeur `0`.