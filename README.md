# Autonomous Tree Planter

## Background and problem statement

Forest planting is an important part of establishing and regenerating forests. However, planting small trees requires manual labor, and it can be challenging to place the seedlings at consistent distances and at the correct depth. Automation may be a possible way to make this part of the process more efficient.

At the same time, the development of automated solutions requires that the robot can move in a controlled manner, handle the seedlings, and place them in the correct location (Regulations on the Testing of Self-Driving Motor Vehicles, 2017, § 10). A LEGO robot can be used to investigate how such functions can be implemented in a smaller, controlled model.


### Problem statement

How can an autonomous LEGO robot be developed and programmed to plant small trees with even spacing and correct placement, in order to investigate how automation can contribute to more efficient and sustainable forest planting?



## Getting started

### Installing
The program for this project will be executed in Lego SPIKE, where we use the scripting language Python. There will be a .llsp3 file added to this GitHub page, whereas the execution of the program will be explained under.

### Executing program

#### How to run the program (robot positioning and optimal conditions)
Connect the hardware to the SPIKE hub:
 - Port A: Left drive motor
 - Port E: Right drive motor
 - Port D: Arm motor
 - Port C: Force sensor (The button)
 - Port F: Distance sensor (mounted at the front of the robot, pointing forward)

Place the robot on a flat, level surface with enough free space in front of it. The robot drives 2.0-3.0 m between trees depending on the tree type, for life scale models, but the prototype has the distances divided by 10.
The starting position for the arm has to be all the way forward (pickup position)
Load the first tree so it is in the pickup position at the front of the robot. The arm picks up the following trees by itself when it returns to the front.
Make sure the hub battery is charged and the wheels are clean, so the distances stay accurate.
Open tree_planter.py in LEGO SPIKE, connect to the hub and run the program.
During the run, press the button to switch tree type. The light matrix shows the selected type: G (gran), F (furu) or B (bjørk). After a press, the robot drives a bit further and then plants the new type with that type's spacing.
If an obstacle is in front of the robot when it is about to drive, it tries to go around it by stepping to the side (see the obstacle avoidance step below).

#### Step by step code explanation
Configuration: the ports, wheel diameter, tree spacings, obstacle avoidance settings and speeds are set at the top of the file. Spacings are in millimeters and can be changed per tree type in TREE_TYPES.

Start: main() pairs the two drive motors, sets the arm's current position as zero and shows the first tree type (G) on the light matrix.

Planting a tree: plant_one_tree() flips the arm 229 degrees from the front to the back, where the tree is dropped in the ground, and then back to the front, where it picks up the next tree.

Obstacle avoidance: while the robot stands still, before every drive, avoid_obstacle() reads the distance sensor. If something is closer than OBSTACLE_DISTANCE_MM in front of the robot, it turns 90 degrees to the right and checks that side. If the right side is blocked too, it turns to the left and checks again. It then drives SIDESTEP_MM to the free side and turns back to face forward. If both sides are blocked, it faces forward again and drives on without avoiding anything. The robot only checks while standing still, so it does not react to obstacles that appear during a drive.

Driving: after the obstacle check, drive() moves the robot forward the spacing for the selected tree type. mm_to_degrees() converts the distance to wheel rotation.

Button: watch_button() runs at the same time as main(). When the button is pressed, it switches to the next type (gran, furu, bjørk, then back to gran), updates the light matrix and asks for an extra drive of SWITCH_EXTRA_DISTANCE_MM before the next tree.

Repeat and stop: the loop in main() repeats until MAX_SEEDLINGS trees are planted. The robot then stops and the button watcher ends so the program can exit.

## Help
If the robot drives backwards or turns, swap the drive motor ports in the configuration.
If the arm doesn't reach the back or goes too far, adjust ARM_FLIP_DEG.
If trees don't have time to settle or feed in, adjust RELEASE_PAUSE_MS.
If the light matrix shows nothing or the wrong letter, check that the button is on the port set as BUTTON.
If the robot does not detect obstacles, check that the distance sensor is on the port set as OBSTACLE_SENSOR and adjust OBSTACLE_DISTANCE_MM. If the robot does not turn exactly 90 degrees when avoiding an obstacle, adjust TURN_90_DEG. If it turns the wrong way, swap the turn directions in avoid_obstacle().


## Authors
Noah Sigmundsen - noahbsi@ntnu.no | Henrik Vea - henrikvj@ntnu.no | Ida Marie Bakke Karlsen - imkarls@ntnu.no | Thirisan Yogachanrda - thirisay@ntnu.no

## Links to rapports and other supplementary documents

Overleaf report: https://www.overleaf.com/project/6aec30d9e7a2a47e137433d4
