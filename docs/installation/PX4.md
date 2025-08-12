---
sidebar_position: 3
---
### Installation PX4

Installation adaptée de : https://docs.px4.io/main/en/ros2/user_guide

```bash
cd
git clone -b v1.16.0-rc3 https://github.com/PX4/PX4-Autopilot.git --recursive
```

→ Remplacer le `./PX4-Autopilot/Tools/setup/ubuntu.sh` par le `ubuntu.sh` du dépôt https://gitlab.utils.troie.ia/cel_dev/c2

Patch nécessaire car nouveau mécanisme d'authentification.

```bash
bash ./PX4-Autopilot/Tools/setup/ubuntu.sh

cd PX4-Autopilot/
make px4_sitl
```

#### Pour tester

```bash
make px4_sitl gz_x500
```

Si ça échoue, vous pouvez supprimer le dossier PX4.