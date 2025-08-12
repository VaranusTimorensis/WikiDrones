---
sidebar_position: 2
---
### Installation de Gazebo

Installation copiée de : https://gazebosim.org/docs/harmonic/install_ubuntu/

```bash
sudo apt-get update
sudo apt-get install curl lsb-release gnupg

sudo curl https://packages.osrfoundation.org/gazebo.gpg --output /usr/share/keyrings/pkgs-osrf-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/pkgs-osrf-archive-keyring.gpg] http://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/gazebo-stable.list > /dev/null
sudo apt-get update
sudo apt-get install gz-harmonic
```

#### Tester que c'est OK

```bash
gz sim
```

#### Si ça ne marche pas

```bash
sudo apt remove gz-harmonic && sudo apt autoremove
```