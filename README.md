# bevel_arm

## Imports
  - rospy
  - ros msgs like sensor_msgs, std_msgs

## Subscribers
  - `/joy_arm`: Joystick input

## Publishers
  - `stm_write`: Purblishes an array of integers containing the required data for the motor controller

## Control flow
  1. Initialization:
  - `Node` class is instantiated.
  - Subscribed to neccessary ROS topics.
  - Registers this script as a ROS node named `arm_drive`.
  - `self.outbuff`: Stores the processed output as an array of 6 integers.

  2. Processing Joystick Input:
  - `joyCallback()`: Takes joy stick input through `joy_arm` and stores it in `self.outbuff`.
  - It creates a dummy variable `outbuff`.
  - It reads the axes and buttons input and scales them from (-1.0 to 1.0) to (-255.0 to 255.0) and converts them to int.
  - It combines button inputs to control arm functions.
  - Mapping:
    `outbuff[0]`: Shoulder movement.
    `outbuff[1]`: Base rotation.
    `outbuff[2]`: Combination of two buttons for roll/pitch control.
    `outbuff[3]`: Elbow movement.
    `outbuff[4]`: Gripper control.
    `outbuff[5]`: Additional roll/pitch logic.
  - `self.outbuff` is updated using variable `outbuff`.

  3. Main loop:
  - `run()`: Publishes the processed output, which is a msg created from `self.outbuff`, to `stm_write`.
  - It runs as 50Hz.
  - It keeps running until ROS in shutdown.
  - It pauses the loop for a fixed time to publish the msg.

  4. Message creation:
  - `createMsg()`: It creates a msg out of `self.outbuff` and returns it back to the main loop (`run()`).
  - It creates a ROS msg of type `Int32MultiArray`.
  - It copies the `self.outbuff` to the msg.
  - It sets up the layout for multi-array, labelling the dimension as `write`.
