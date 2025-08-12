---
sidebar_position: 4
---
### Installation DDS

```bash
git clone https://github.com/eProsima/Micro-XRCE-DDS-Agent.git
cd Micro-XRCE-DDS-Agent
mkdir build
cd build
cmake ..
make
sudo make install
sudo chmod 755 -R urs/local/
sudo ldconfig /usr/local/lib/
```

Test de  MicroXRCEAgent
```bash
MicroXRCEAgent udp4 -p 8888
```