# BOREALIS Drone Documentation

## Prerequisites

- PX4 Firmware version: 1.11
- Companion Computer OS: Ubuntu 18.04
- ROS1 Melodic
- ROS2 Dashing
- Fast RTPS 1.8.2 and FastRTPSGen 1.0.4 or later
- Refer to [the official documentation](https://dev.px4.io/v1.11/en/middleware/micrortps.html) for installation instructions

## Testing the PX4-FastRTPS bridge with ROS2

1. Connect the Pixhawk to QGroundControl. Flash the Pixhawk with the corresponding RTPS version firmware

2. Set up PixHawk-Companion Computer connection following [this documentation](https://dev.px4.io/v1.11/en/companion_computer/pixhawk_companion.html)

   - Additionally, set the RTPS related parameters (I can't remember the names lmao). Enable RTPS, and set the port as `TELEM 2`.
   - Remember to set up the UDEV rule on the companion computer for the FTDI device, and re-login before continuing

3. Start the microRTPS agent on the companion computer

   **IMPORTANT**: The microRTPS agent must be started before the client!!!

   - On a terminal of the companion computer, source the `px4_ros_com` workspace

     ```bash
     $ source ~/px4_ros_com_ros2/install/setup.bash
     ```

   - Then launch the `micrortps_agent` using UART, specify the device as `/dev/ttyPixhawk` (previously set in UDEV rule), baudrate as 921600 (previously set in Pixhawk parameters)

     ```bash
     $ micrortps_agent -t UART -d /dev/ttyPixhawk -b 921600
     ```

4. Start the microRTPS client on the Pixhawk

   - On QGroundControl, go to the command line for the Pixhawk, start the `micrortps_client` using UART, specify device as `/dev/ttyS2`(`TELEM 2`), baudrate as 921600

     ```
     > micrortps_client start -t UART -d /dev/ttyS2 -b 921600
     ```

5. Check the ROS2 topic and nodes on the companion computer. Or run the `sensor_combined_listener` node

   ```bash
   $ source ~/px4_ros_com_ros2/install/local_setup.bash
   $ ros2 launch px4_ros_com sensor_combined_listener.launch.py
   ```