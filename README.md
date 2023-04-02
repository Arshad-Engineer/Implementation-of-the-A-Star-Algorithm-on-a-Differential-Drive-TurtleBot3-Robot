# Implementation-of-the-A-Star-Algorithm-on-a-Differential-Drive-TurtleBot3-Robot
## Description:
+ Navigate a differential drive robot (TurtleBot3 Burger) in the given map environment from a given start point to a given goal point.
  * If the start and/or goal nodes are in the obstacle space, the user should be informed by a
message and the user should input the nodes again until valid values are entered.
  * The user input start and goal coordinates shall be be w.r.t. the origin shown in the map.
+ Differential drive constraints are considered while implementing the A* algorithm, with 8 set of action
space.

## User Input
+ The code will take the following values from the user:
+ Start Point Coordinates (3-element vector): (Xs, Ys, Θs).
  * Θs - The orientation of the Robot at the start point.
+ Goal Point Coordinates (2-element vector): (Xg, Yg)
  * To simplify the path explored, final orientation input is not required.
+ Wheel RPMs (2-element vector) => (RPM1, RPM2)
  * RPM: Revolutions per Minute
  * 2 possible values for the wheel RPMs
+ Clearance (in mm)

## Parameters
+ The following parameters are defined in the code for the TurtleBot 3 Burger robot.
+ These parameters are NOT user defined but rather are constants which can be defined in
the Python code.
	* Robot Wheel Radius (R)
		* From the robot’s datasheet/documentation
	* Robot Radius (r)
		* From the robot’s datasheet/documentation
		* This needs to be added to the clearance value to create the obstacle map.
	* Wheel Distance (L)
		* From the robot’s datasheet/documentation
    
## Action Set
+ Let the two RPMs provided by the user be RPM1 and RPM2. Then the action space consisting
of 8 possible actions for the A* algorithm is:
    1. [0, RPM1]
    2. [RPM1, 0]
    3. [RPM1, RPM1]
    4. [0, RPM2]
    5. [RPM2, 0]
    6. [RPM2, RPM2]
    7. [RPM1, RPM2]
    8. [RPM2, RPM1]
    .... where, the 1st element in each vector above corresponds to the Left Wheel’s RPM and 
    the 2nd element in each vector above corresponds to the Right Wheel’s RPM.
    
# Part 01 : 2D Implementation
## Differential Drive Constraints
+ For this project it is considered that the robot as a non-holonomic robot which means the robot
cannot move in the y-direction independently.
+ Smooth moves are defined for the robot by providing Left and Right wheel
velocities. The time for each move is fixed.
+ The equations for a Differential Drive robot are:
![image](https://user-images.githubusercontent.com/112987383/229331139-c130a5fc-775d-45eb-9882-bb282cae2b92.png)
![image](https://user-images.githubusercontent.com/112987383/229331171-e66343ea-a8bc-4fb7-b3c1-c62b59c07d5c.png)
+ Step 01: Function for non-holonomic constraints
  * A function will take 2 arguments (Rotational velocities of the Left wheel and Right wheel)
and returns the new coordinate of the robot, i.e. (x, y, theta).
    * where x and y are the translational coordinates of the robot and theta shows the orientation of
the robot with respect to the x-axis.
+ Step 02: Modify the Map to consider the geometry of the Rigid Robot
  * Dimensions of the robot are available in the Official Documentation which can be used to define the
clearance value.
  * The map with obstacles can be setup using any inbuilt function of OpenCV/Matplotlib/Pygame.
However, the half-plane equations method is used here.
+ Step 03: Generate the tree using non-holonomic constraints
  * Consider the configuration space as a 3 dimensional space.
  * Follow the same step from Project 3- Phase 2 to check for duplicate nodes (consider threshold
as per your own need, but make sure the 8-action space is not violated)
![image](https://user-images.githubusercontent.com/112987383/229331675-d7f99ad4-d921-4eb8-810e-656c6aa61d21.png)
+ Step 04: Display the tree in the configuration space
  * Use curves that address the non-holonomic constraints to connect the new node to previous
nodes and display it on the Map.
![image](https://user-images.githubusercontent.com/112987383/229331691-e219bf54-1129-436d-8630-9135dc77d4f4.png)
+ Step 05: Implement A* search algorithm to search the tree and to find the optimal path
  * Consider Euclidean distance as a heuristic function.
  * Note:- Define a reasonable threshold value for the distance to the goal point. Do to the limited
number of moves the robot cannot reach the exact goal location. So, use a threshold distance
to check the goal.
![image](https://user-images.githubusercontent.com/112987383/229331727-53bd9186-9d7c-4eb0-bbd3-f236749d1a7f.png)
+ Step 06: Display the optimal path in the Map
  * To plot the final path, you can use the plot_curve function as shown in the given
Howplotcurves.py Python file. You may also use other libraries such as quiver to help you plot
the path lines of the robot.
  * For Part 01 2D Implementation, reuse the Project 03 Phase 01 map.
![image](https://user-images.githubusercontent.com/112987383/229331746-d678e563-7af0-478d-9060-7116ea1f30a0.png)

# Part 02: Gazebo Visualization
+ Implement Gazebo Visualization
  * Simulate the path planning implementation on Gazebo with the TurtleBot 3 Burger robot.
  * The Gazebo environment has been provided for the map (map.world), which is different from
that used in Part 01 2D Implementation.
![image](https://user-images.githubusercontent.com/112987383/229331589-5950a049-8666-4c54-9cd1-402ba723d886.png)

+ Gazebo Map
  * The approximate start point is shown in red and goal point is shown in green in the Figure below.
    * You are free to choose any start point in the space to the left of the 1st rectangle and the goal
point anywhere in the space above/below/to the right of the shown black circle.
  * The video should show the TurtleBot motion in Gazebo environment for these points. The motion of
the TurtleBot should be as a result of the solution generated by your A* algorithm and the
corresponding velocity commands published to the appropriate ROS topic.
  * Choose the start and goal points by your best judgement along with other user inputs, specifically
the wheel RPMs and clearance.
![image](https://user-images.githubusercontent.com/112987383/229331456-3a4207bb-1f6d-48f7-9bb6-7e82675a7f2f.png)

## Simulation Video Links:
+ For Part 01: Give a start point (near the bottom left) and end point (near the bottom right)
  * Google Drive/YouTube: ?????????????
+ For Part 02: TurtleBot3 Gazebo simulation in the given map with start point (left of the 1st rectangle) and goal point (in the
space above/below/to the right of the shown black circle)
  * Google Drive/YouTube: ?????????????

## Folder Structure:
+ Part01 folder
  * Source Code (.py)
+ Part02 folder
  * ROS Package
  * src
    * <your-node-script>.py
  * launch
    * <your-launch-file>.launch
  * world
    * map.world
+ README file (.md or .txt)

## How to run the code:
## Libraries / dependancies:
## Problems Encountered:
