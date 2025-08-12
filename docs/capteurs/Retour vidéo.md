---
sidebar_position: 1
---
### Afficher et traiter un retour vidéo
1) Vérifier que le modèle dispose d'une caméra
Exemple : x650_mono_cam_down (~/PX4-Autopilot/Tools/simulation/gz/models/x500_mono_cam_down/model.sdf)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<sdf version='1.9'>
  <model name='x500_mono_cam_down'>
    <self_collide>false</self_collide>
    <include merge='true'>
      <uri>x500</uri>
    </include>
    <include merge='true'>
      <uri>model://mono_cam</uri>
      <pose>0 0 .10 0 1.5707 0</pose>
      <name>mono_cam</name>
    </include>
    <joint name="CameraJoint" type="fixed">
      <parent>base_link</parent>
      <child>camera_link</child>
      <pose relative_to="base_link">0 0 0 0 1.5707 0</pose>
    </joint>
  </model>
</sdf>          
```
2) Noter le topic sur lequel Gazebo publie le flux vidéo
Pendant la simulation, exécutez dans un terminal la commande `gz topic -l`.
Le topic peut avoir un nom ressemblant à `"/world/baylands/model/x500_mono_cam_down_1/link/camera_link/sensor/imager/image`
3) Bridge
Dans le fichier de lancement `<nom>.launch.py`, ajouter le noeud suivant :
```python
Node(
    package="ros_gz_bridge",
    executable="parameter_bridge",
    name="camera_image_bridge",
    arguments=[
        "/world/baylands/model/x500_mono_cam_down_1/link/camera_link/sensor/imager/image@sensor_msgs/msg/Image@ignition.msgs.Image",
    ],
    remappings=[
        (
            "/world/baylands/model/x500_mono_cam_down_1/link/camera_link/sensor/imager/image",
            "/px4_1/camera",
        )
    ],
    output="screen",
    ),
```
4) (Eventuellement) renommer à l'aide de l'argument `remapping` comme ci-dessus.
5) Ecouter le topic. Ici, après remapping, il s'agit du topic `/px4_1/camera`.

### Modifier l'angle de la caméra
