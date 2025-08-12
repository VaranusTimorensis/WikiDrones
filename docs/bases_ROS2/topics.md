---
sidebar_position: 3
---
# Topics
ROS 2 breaks complex systems down into many modular nodes. Topics are a vital element of the ROS graph that act as a bus for nodes to exchange messages.

![Publishers et subscribers](/img/topic_1.gif)

A node may publish data to any number of topics and simultaneously have subscriptions to any number of topics.

![An image from the static](/img/topic_2.gif)

Topics are one of the main ways in which data is moved between nodes and therefore between different parts of the system.

Nous pouvons voir les *Topics* existants dans un terminal :
`ros2 topic list`

Nous pouvons écouter un *Topic* :
`ros2 topic echo <topic>`