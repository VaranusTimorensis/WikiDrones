---
sidebar_position: 2
---
### Message UORB VehicleCommand
Code associé :
```python
def publish_vehicle_command(self, command, **params) -> None:
    """Publish a vehicle command."""
    msg = VehicleCommand()
    msg.command = command
    msg.param1 = params.get("param1", 0.0)
    msg.param2 = params.get("param2", 0.0)
    msg.param3 = params.get("param3", 0.0)
    msg.param4 = params.get("param4", 0.0)
    msg.param5 = params.get("param5", 0.0)
    msg.param6 = params.get("param6", 0.0)
    msg.param7 = params.get("param7", 0.0)
    msg.target_system = 1
    msg.target_component = 1
    msg.source_system = 1
    msg.source_component = 1
    msg.from_external = True
    msg.timestamp = int(self.get_clock().now().nanoseconds / 1000)
    self.vehicle_command_publisher.publish(msg)
```
VehicleCommand est un message UORB, qui est un middleware interne à PX4. UORB est utilisé pour envoyer des commandes à l'intérieur du système PX4 (navigation, armement, changement de mode, etc.).

Voici sa structure générique : 
```
uint64 timestamp # [us] Time since system start.
float32 param1 # Parameter 1, as defined by MAVLink uint16 VEHICLE_CMD enum.
float32 param2 # Parameter 2, as defined by MAVLink uint16 VEHICLE_CMD enum.
float32 param3 # Parameter 3, as defined by MAVLink uint16 VEHICLE_CMD enum.
float32 param4 # Parameter 4, as defined by MAVLink uint16 VEHICLE_CMD enum.
float64 param5 # Parameter 5, as defined by MAVLink uint16 VEHICLE_CMD enum.
float64 param6 # Parameter 6, as defined by MAVLink uint16 VEHICLE_CMD enum.
float32 param7 # Parameter 7, as defined by MAVLink uint16 VEHICLE_CMD enum.
uint32 command # Command ID.
uint8 target_system # System which should execute the command.
uint8 target_component # Component which should execute the command, 0 for all components.
uint8 source_system # System sending the command.
uint16 source_component # Component / mode executor sending the command.
uint8 confirmation # 0: First transmission of this command. 1-255: Confirmation transmissions (e.g. for kill command).
bool from_external
```

Détaillons plus précisément les paramètres à renseigner :
- `timestamp` représente le temps écoulé depuis le démarrage du système PX4 en microsecondes (et non pas un timestamp UNIX). Ce paramètre est essentiel pour la cohérence temporelle du système. Il permet notamment à PX4 de savoir quand la commande a été générée, et donc si elle est encore pertinente, et de reconstituer la chronologie des événements.
- `command` : commande à exécuter. Les différentes possibilités sont décrites dans le fichier VehicleCommand.msg situé dans le dossier `px4_msgs/msg/`.
- `param1` à `param7` : Chaque commande peut exiger de 1 à 7 paramètres. Les paramètres à renseigner sont précisés dans VehicleCommand.msg.
- `from_external` : PX4 distingue les commandes internes et des commandes externes pour des raisons de :
    - Sécurité (certaines commandes critiques ne doivent pas être acceptées à distance sans autorisation explicite),
    - Priorité (les modules internes peuvent avoir priorité sur certaines commandes externes),
    - Auditabilité (savoir si une commande vient d'un opérateur externe ou d'un contrôleur interne),
    - Filtrage (le système peut ignorer certaines commandes externes en fonction du mode de vol, etc.).
- `target_system` désigne l'id unique du système (ex : drone entier) que l'on veut commander. Dans un réseau MAVLink (ex : drone + companion + station sol), chaque système a un `system_id`. PX4 (le contrôleur de vol) utilise généralement `system_id = 1`. 0 permet de diffuser la commande à tous les systèmes. Certains modules ignorent cette valeur.
- `target_component` cible le composant du système, i.e. le module à l'intérieur du système qui doit exécuter la commande. Par exemple, 1 désigne l'autopilote (PX4, ArduPilot), 100 la caméra.
- `source_system` : id du système émetteur.
- `source_component` : id du composant émetteur dans le système émetteur. 
- `confirmation` indique si la commande est :
    - une nouvelle requête (`confirmation = 0`)
    - une répétition de la même commande, pour s'assurer qu'elle a bien été reçue (`confirmation > 0`). Avec VehicleCommand, la valeur par défaut est 0.

*Remarque* : `source_system = source_component = target_system = target_component = 1` signifie que la commande vient de l'autopilote et s'adresse à l'autopilote. C'est logiquement incompatible à `from_external = true` et ne devrait pas être utilisé si la commande provient du companion (ex : Jetson).