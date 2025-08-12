---
sidebar_position: 1
---
### Installation de ROS2
Installation copiée de : https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debs.html

```bash
sudo apt install software-properties-common
sudo add-apt-repository universe

sudo apt update && sudo apt install curl -y
export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F\" '{print $4}')
curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo $VERSION_CODENAME)_all.deb" # If using Ubuntu derivates use $UBUNTU_CODENAME
sudo dpkg -i /tmp/ros2-apt-source.deb

sudo apt update && sudo apt install ros-dev-tools

sudo apt update && sudo apt upgrade

sudo apt install ros-jazzy-desktop
```

Pour éviter de sourcer `/opt/ros/jazzy/setup.bash` à chaque nouvelle ouverture de terminal, exécutez la commande
```bash
echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc
```
### Vérifier l'installation

Dans terminal 1 lancer le publisher cpp :
```bash
source /opt/ros/jazzy/setup.bash
ros2 run demo_nodes_cpp talker
```

Dans terminal 2 lancer le subscriber py :
```bash
source /opt/ros/jazzy/setup.bash
ros2 run demo_nodes_py listener
```
### Si l'installation a échoué

```bash
sudo apt remove ~nros-jazzy-* && sudo apt autoremove
```