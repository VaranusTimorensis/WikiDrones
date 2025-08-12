---
sidebar_position: 7
---
### Piloter en mode ACRO
Voici un exemple de méthode pour piloter un drone en mode ACRO, c'est-à-dire en contrôlant la poussée et les vitesses angulaires.
```python
def attitude_control(roll_rate, pitch_rate, yaw_rate, thrust):
    msg = VehicleRatesSetpoint()
    msg.timestamp = int(self.get_clock().now().nanoseconds / 1e3)  # µs

    msg.roll = roll_rate
    msg.pitch = pitch_rate
    msg.yaw = yaw_rate

    # thrust_body est un tableau 3 éléments, en NED
    # On contrôle uniquement la poussée verticale (axe z)
    msg.thrust_body = [0.0, 0.0, thrust]
    self.pub_rates.publish(msg)
```
Utiliser `msg.body_rate = True` dans les messages `OffboardControlMode`.