---
sidebar_position: 5
---
### Installation QGroundControl

Installation copiée de : https://docs.qgroundcontrol.com/master/en/qgc-user-guide/getting_started/download_and_install.html


#### 1) Installer GStreamer et autres bibliothèques requises
```bash
sudo apt update
sudo apt install -y \
  gstreamer1.0-plugins-bad \
  gstreamer1.0-libav \
  gstreamer1.0-gl
sudo apt install -y libfuse2
sudo apt install -y libxcb-xinerama0 libxkbcommon-x11-0 libxcb-cursor-dev
```

#### 2) Télécharger l'AppImage de QGroundControl
```bash
cd ~/Téléchargements
wget https://d176tv9ibo4jno.cloudfront.net/latest/QGroundControl.AppImage \
     -O QGroundControl.AppImage
```

#### 3) Rendre l'AppImage exécutable et lancer
```bash
chmod +x QGroundControl.AppImage
./QGroundControl.AppImage
```