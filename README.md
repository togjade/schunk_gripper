# schunk_gripper

1) Before using the Schunk gripper connect it to the controller.![alt text](https://github.com/togjade/schunk_gripper/blob/master/schunk_connection_2_controller.jpeg?raw=true)

2) Connect "+" of power supply (voltage limit: 24, current limit: 1.8) to the V_mot and V_log.

3) Connect "com" of power supply to the GND and common ground of power supply to the common ground on the controller

4) Copy the [folder](https://github.com/togjade/schunk_gripper/tree/master/schunk_ezn64) to the your ROS catkin workspace and compile it

5) Run " roslaunch schunk_ezn64 ezn64_usb_control.launch "

6) ALWAYS run schunk_ezn64/reference using the rosservice!

7) [ezn64_usb_control_lib.cpp](https://github.com/togjade/schunk_gripper/blob/master/schunk_ezn64/src/ezn64_usb_control_lib.cpp) contains subscriber. So you can create the publisher that publishes [position, velocity] in the given order. The message type was set to Float32MultiArray, but you can change it yourselve. 


        void
        EZN64_usb::subPositionCallback(const std_msgs::Float32MultiArray::ConstPtr& posSp){

            if((posSp->data[1] >= MIN_GRIPPER_POS_LIMIT) && (posSp->data[1] <= MAX_GRIPPER_POS_LIMIT)
              && (posSp->data[0] >= 0) && (posSp->data[0] <= MAX_GRIPPER_VEL_LIMIT)){

                setPosition(ezn64_handle_,posSp->data[1], posSp->data[0]);

            } else {

                ROS_WARN("EZN64: Goal position rejected!");

            }

        }
        
8) [Here](https://github.com/togjade/schunk_gripper/blob/master/gripper.py) you can find the example of the publisher.
