# Experimental_robotics_ASSIGNMENT-1
 

# 1) Introduction
The assignment's goal is to create an algorithm that will allow the robot to move and display a surveillance behavior so that its location in the environment can be ascertained. Depending on the user's preference, the environment is made up of a number of places that are connected. The goal is to use SMACH to build a Finite State Machine that enables the robot to choose its course of action based on the circumstances. The robot has a battery that must be recharged after a predetermined amount of use, which is periodically checked.
Rooms with one door and passageways with at least two doors are the two categories into which the places fall. Doors are the objects that connect two places, and Robot1 is the name of the robot that moves around the area.
This package was developed on a [docker image](https://hub.docker.com/r/carms84/exproblab). with all the dependencies needed pre-installed on the image.

Utilizing the below two packages, this package was primarily developed.

1)[Armor package](https://github.com/EmaroLab/armor) which helps to link ontology and some packages to build an experimental envornment.

2)And the finite state machine with the help of  [Smach package](http://wiki.ros.org/smach) is used to organize things.
  

# 2)Architecture 
## I) Behaviour of the robot:  
The robot initially waits for Room E to finish creating the ontology (map). Starting with the (walk in corridor) condition, it then decides whether the battery is not low and whether any rooms require immediate attention. In this case, the robot just travels around the two corridors before stopping briefly. The robot enters the (charging) mode when the battery is low, which confines it to Room E until the battery is fully charged. On the other hand, if the robot requires emergency assistance while the battery is being recharged, it visits and stays in a room for a while (visitroom state)
The graphic below shows an example loaded ontology map that the robot produces:
![MAP](https://user-images.githubusercontent.com/73067092/218581036-60d4779c-8891-4b07-881b-baa953429f72.png)

# 3)Explanation of the codes

### 1)fsm.py
This is the hub of all nodes showing state changes, where a two-action server assists the robot in achieving a certain state in accordance with the necessary condition.

### 2)robot_states.py:
This node facilitates getting pose responses from the controller and planner and simulating battery state.

### 3)ontology_build.py:
The finite state machine's structure is specified using this script. Here, all of its states and associated transitions are initialized.
To prevent the system from being stuck or experiencing faults during changes, it is defined how to behave for each state during each transition that is received.
The primary function of the helper, however, is to share the mutex that is required to access shared variables without difficulty. Of fact, there is only one mutex employed in this attempt to achieve complete synchronization between the state and the reading and writing operations.

### 4)helper.py and query_helper.py:
In order to facilitate communication with the Finie state machine node, this node has interfacehelper and actionclient helper. and in query helper node is used for querying the robot's position, holding everything in a list, and positioning the robot appropriately.

### 5)controller.py
The FSM uses this class as the server to simulate the robot's movement from a beginning position to a target position. The client specifies the route to take. As soon as the planner completes computing the path, the controller kicks off. The server mimics the robot's motion before publishing the outcome. Because of the preemption, if the process is halted by any signals (a low battery, for example), it doesn't return anything.

### 6)Planner.py:
The FSM uses this class as the server to determine the robot's path from a beginning location to a target position.
According to the list in the file:mod:'name mapper,' each position in the environment used has a point coordinate [float x, float y] linked with it.
A linear space on 'n' points between the two coordinates is used to calculate the plan (the number of points is set in the same file as before).
The path is calculated by the server, who then broadcasts the outcome. Because of the preemption, if the process is halted by any signals (a low battery, for example), it doesn't return anything.


# 4)How to run the code

## How to run the code 

Open the terminal and go to src of your ROS workspace and run -

<pre><code>https://github.com/krishanttharun98/experimental_assignment_1.git</code></pre>

Step 2 : Now to build your clonned code run the command -

<pre><code>catkin_make</code></pre>

step 3 : Open a each new terminal for the following commands seperately -
 
Terminal-1. 

<pre><code>roslaunch assignment assignment.launch</code></pre>

Terminal-2.

<pre><code>rosrun smach_viewer smach_viewer.py</code></pre>





