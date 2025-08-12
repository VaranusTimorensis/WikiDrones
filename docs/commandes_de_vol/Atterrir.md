---
sidebar_position: 6
---
### Atterrir
On peut ordonner l'atterrissage par la méthode suivante :
```python
def land(self):
    msg = VehicleCommand()
    msg.timestamp        = int(self.get_clock().now().nanoseconds / 1000)
    msg.command          = VehicleCommand.VEHICLE_CMD_NAV_LAND
    self.pub_vehicle_command.publish(msg)
```