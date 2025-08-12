---
sidebar_position: 2
---
# Structure d'un package ROS2
## Qu'est-ce qu'un workspace ?
*cf https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.html*

A workspace is a directory containing ROS 2 packages. Before using ROS 2, it’s necessary to source your ROS 2 installation workspace in the terminal you plan to work in. This makes ROS 2’s packages available for you to use in that terminal.

You also have the option of sourcing an “overlay” - a secondary workspace where you can add new packages without interfering with the existing ROS 2 workspace that you’re extending, or “underlay”. Your underlay must contain the dependencies of all the packages in your overlay. Packages in your overlay will override packages in the underlay. It’s also possible to have several layers of underlays and overlays, with each successive overlay using the packages of its parent underlays.

## Qu'est-ce qu'un package ROS 2 ?
*cf https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.html*
A package is an organizational unit for your ROS 2 code. If you want to be able to install your code or share it with others, then you’ll need it organized in a package. With packages, you can release your ROS 2 work and allow others to build and use it easily.

Package creation in ROS 2 uses ament as its build system and colcon as its build tool. You can create a package using either CMake or Python, which are officially supported, though other build types do exist.

The simplest possible package may have a file structure that looks like:
```
my_package/
     CMakeLists.txt
     include/my_package/
     package.xml
     src/
```
- CMakeLists.txt file that describes how to build the code within the package
- include/<package_name> directory containing the public headers for the package
- package.xml file containing meta information about the package
- src directory containing the source code for the package

A single workspace can contain as many packages as you want, each in their own folder. You can also have packages of different build types in one workspace (CMake, Python, etc.). You cannot have nested packages.

Best practice is to have a `src` folder within your workspace, and to create your packages in there. This keeps the top level of the workspace “clean”.

A trivial workspace might look like:
```
workspace_folder/
    src/
      cpp_package_1/
          CMakeLists.txt
          include/cpp_package_1/
          package.xml
          src/

      py_package_1/
          package.xml
          resource/py_package_1
          setup.cfg
          setup.py
          py_package_1/
      ...
      cpp_package_n/
          CMakeLists.txt
          include/cpp_package_n/
          package.xml
          src/
```



## Exemple : créer un workspace avec un package

### 1. Sourcer l'environnement ROS 2
`source /opt/ros/foxy/setup.bash`

### 2. Créer un nouveau dossier
Best practice is to create a new directory for every new workspace. The name doesn’t matter, but it is helpful to have it indicate the purpose of the workspace. Let’s choose the directory name ros2_ws, for “development workspace”:
```
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
```
Another best practice is to put any packages in your workspace into the src directory. The above code creates a src directory inside ros2_ws and then navigates into it.

### 3. Create a package
The command syntax for creating a new package in ROS 2 is:
```
ros2 pkg create --build-type ament_cmake --license Apache-2.0 --node-name my_node my_package
```
You will now have a new folder within your workspace’s src directory called my_package.

### 4. Build a package
Putting packages in a workspace is especially valuable because you can build many packages at once by running colcon build in the workspace root. Otherwise, you would have to build each package individually.

Return to the root of your workspace:
```
cd ~/ros2_ws
```
Now you can build your packages:
```
colcon build
```
To build only the my_package package next time, you can run:
```
colcon build --packages-select my_package
```
### 5. Construire le workspace avec colcon

From the root of your workspace (ros2_ws), you can now build your packages using the command:
```
colcon build
```
Once the build is finished, enter ls in the workspace root (~/ros2_ws) and you will see that colcon has created new directories:
```
build  install  log  src
```
The `install` directory is where your workspace’s setup files are, which you can use to source your overlay.

### 6. Source the overlay

Before sourcing the overlay, it is very important that you open a new terminal, separate from the one where you built the workspace. Sourcing an overlay in the same terminal where you built, or likewise building where an overlay is sourced, may create complex issues.

In the new terminal, source your main ROS 2 environment as the “underlay”, so you can build the overlay “on top of” it:
```
source /opt/ros/foxy/setup.bash
```
Go into the root of your workspace:
```
cd ~/ros2_ws
```
In the root, source your overlay:
```
source install/local_setup.bash
```
---
**NOTE**

Sourcing the local_setup of the overlay will only add the packages available in the overlay to your environment. setup sources the overlay as well as the underlay it was created in, allowing you to utilize both workspaces.

So, sourcing your main ROS 2 installation’s setup and then the ros2_ws overlay’s local_setup, like you just did, is the same as just sourcing ros2_ws’s setup, because that includes the environment of its underlay.

--- 

Now you can run the package from the overlay:
```
ros2 run <package> <node>
```

### 7. Customization of package.xml and CMakeLists.txt
**TODO**