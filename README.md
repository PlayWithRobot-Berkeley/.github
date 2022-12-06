# Play with Robot
_UC Berkeley EE C206/106A 2022 Fall Group 5' final project_

## Teach a Robot to Do Maths

The project aims to utilise the Sawyer robotic arm to solve mathematical problems. 
The Sawyer robotic arm is expected to read the handwritten mathematical problems on
a random whiteboard placed inside its working space, interpret the problems, generate
the corresponding solutions and write them down on whiteboards. 

<img width='30%' src='https://github.com/PlayWithRobot-Berkeley/.github/blob/main/IMG_20221206_095615.jpg'/>

In order to acheive this, the team developed two packages, each in a git repository: 


### [FormulaRec](https://github.com/PlayWithRobot-Berkeley/FormulaRec)

The package provides the functionality to connect to the Sawyer's intrisic cameras, 
parse the images into mathematical expressions and evaluate the answers. It can be
started as a ROS service server, so that the controller and path planner can query
it to retrive what to be written. 

### [PathPlanning](https://github.com/PlayWithRobot-Berkeley/PathPlanning)

The package plan the path to write down answers to mathematical expressions and
relies on MoveIt! to control a robotic arm to follow the path. The answers to be
written by it comes from [FormulaRec](https://github.com/PlayWithRobot-Berkeley/FormulaRec). 



## How to Deploy

Both the repository contain individual packages instead of **not** ROS workspace.
Hence, it would be advisable to **first create a workspace**, 
clone both repositories into the `src` directory and then `catkin_make`, so that
both packages can reside in the same workspace, making the life easier (and neater). 

Since the Sawyer robotic arm are to be used as well, do not forget to include the
`intera.sh` as well. 

In summary, basicall you need to: 

```sh
mkdir final_project # the workspace
cd final_project
mkdir src
cd src
git clone https://github.com/PlayWithRobot-Berkeley/FormulaRec.git
git clone https://github.com/PlayWithRobot-Berkeley/PathPlanning.git

# THEN FOLLOW THE README in FormulaRec to complete the CV setup

cd .. # back to final_project
ln -s /opt/ros/eecsbot_ws/intera.sh .
catkin_make
```

The README files in
[FormulaRec](https://github.com/PlayWithRobot-Berkeley/FormulaRec)
and
[PathPlanning](https://github.com/PlayWithRobot-Berkeley/PathPlanning)
gives a detailed instructions and are strongly recommended to go through. 


### Run the codes

1. Enable the robotic arm
    ```sh
    cd [workspace dir]
    ./intera.sh # Enter the SSH session
    rosrun intera_interface enalbe_robot.py -e
    exit # exit the SSH session
    ```
1. Test the robotic arms' movibility and the cameras
    ```sh
    roslaunch intera_examples sawyer_tuck.launch
    rosrun intera_examples camera_display.py -c right_hand_camera
    ```
1. Start the action server
    ```
    rosrun intera_interface joint_trajectory_action_server.py
    ```
1. Run MoveIt! via RVIZ **in a new terminal**
    ```sh
    roslaunch sawyer_moveit_config sawyer_moveit.launch electric_gripper:=true
    ```
1. Start the CV server node **in a new terminal**
    ```sh
    roslaunch formula_rec server.launch
    ```
1. Finally, run the path planning node **in a new terminal**
    ```sh
    rosrun path_planning cartesian_test.py
    ```

## Bingo! 

<img width='40%' src='https://github.com/PlayWithRobot-Berkeley/.github/blob/main/Bingo.png'/>
