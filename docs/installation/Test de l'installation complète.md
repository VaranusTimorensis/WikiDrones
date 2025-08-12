---
sidebar_position: 6
---
### Test de l'installation complète

#### 1) Créer et builder le workspace ROS2

```bash
mkdir -p ~/ws_sensor_combined/src && cd ~/ws_sensor_combined/src
git clone https://github.com/PX4/px4_msgs.git
git clone https://github.com/PX4/px4_ros_com.git
cd ~/ws_sensor_combined
source /opt/ros/jazzy/setup.bash    # ou /opt/ros/foxy/setup.bash
sudo chmod 755 -R urs/local/
python -m pip install catkin_pkg lark
colcon build
```

A ce stade, si le colcon échoue, pensez à vérifier les deux points suivants :
- les droits d'accès à /usr/local/. Vérifiez les droits par `ll` ou en essayant d'accéder au dossier /usr/local/.
Si l'accès est impossible, exécutez les commandes suivantes :
```bash
sudo chmod 755 -R urs/local/
```
- Bibliothèque Python manquante (exemple : catkin_pkg et lark). Dans ce cas, il faut les installer dans l'environnement virtuel. Si `pip` ne fonctionne pas, installez `uv` (cf https://docs.astral.sh) puis exécutez `uv pip install <pkg>` dans l'environnement virtuel.

#### 2) Démarrer l'agent Micro XRCE‑DDS (uORB ↔ ROS2)
```bash
MicroXRCEAgent udp4 -p 8888 &
```

#### 3) Lancer QGroundControl
```bash
cd ~/Téléchargements
./QGroundControl.AppImage
```


#### 4) Lancer PX4 SITL avec Gazebo X500
```bash
cd ~/src/PX4-Autopilot
make px4_sitl gz_x500 &
```

#### 5) Lancer le listener ROS2
```bash
cd ~/ws_sensor_combined
source /opt/ros/jazzy/setup.bash    # ou /opt/ros/foxy/setup.bash
source install/local_setup.bash
ros2 launch px4_ros_com sensor_combined_listener.launch.py
```

#### 6) Faire décoller le drone virtuel

Dans l'invite de commande Nuttshel de px4_sitl lancer la commande suivante :
```bash
commander takeoff
```
Ou via QGroundControl : lancer takeoff depuis la barre d'outils.