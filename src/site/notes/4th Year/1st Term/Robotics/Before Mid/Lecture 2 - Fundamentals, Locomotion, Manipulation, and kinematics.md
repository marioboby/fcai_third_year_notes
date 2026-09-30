---
{"dg-publish":true,"permalink":"/4th-year/1st-term/robotics/before-mid/lecture-2-fundamentals-locomotion-manipulation-and-kinematics/","dg-note-properties":{}}
---

# Robotics Fundamentals

## Traditional vs. Modern Definitions

- **Traditional Definition:** Robots are electromechanical machines that utilize electrical control systems and programming to perform specific tasks, requiring collaboration across mechanical, electrical, and computer engineering.
    
      
    
- **Modern Definition:** Due to advancements in computer vision, artificial intelligence, networks, and electronics, robots are now defined as computers (or machines) that can independently sense, think, move, and interact.
    
      
    

## Core Capabilities of Modern Robots

- **Sense:**
    
      
    - The process of acquiring data and signals through sensors to understand the surrounding world's status.
        
          
        
    - Robots use sensors to measure distances, check temperature, capture images, or communicate with other data sources.
        
          
        
    - Sensor selection depends on the application; for instance, digital cameras are mandatory for mobile robots.
        
          
        
- **Think:**
    
      
    - Involves processing the acquired signals or images to extract information and understand the environment.
        
          
        
    - Thinking requires making logical and correct decisions to accomplish tasks, heavily relying on artificial intelligence and high computational facilities.
        
          
        
- **Interact:**
    
      
    - A highly challenging task that combines sensing, thinking, and the use of actuators (mechanical parts) to engage with surrounding objects.
        
          
        
- **Move:**
    
      
    - Allows mobile robots to physically navigate their environment, facilitating both Robot-Human and Robot-Robot interactions.
        
          
        

## Classification of Robots

Robots can be classified by their operational environment:

  

- **Aquatic:** Remotely Operated Vehicles (ROV).
    
      
    
- **Terrestrial:** Wheeled mobile robots and legged robots.
    
      
    
- **Airborne:** Fixed-wing aircraft and Unmanned Aerial Vehicles (UAV).
    
      
    

## Locomotion and Manipulation

- **Locomotion:** The ability of a robot to move itself by using motors to exert forces on its environment.
    
      
    - Includes rolling, walking, running, jumping, sliding, crawling, climbing, swimming, and flying.
        
          
        
    - These methods vary significantly in energy consumption, kinematics, stability, and required capabilities.
        
          
        
    - For example, crawling and sliding require high power at low speeds, while a railway wheel on steel requires low power at higher speeds.
        
          
        
- **Manipulation:** The ability of a robot to move objects within its environment by using motors to exert forces on those objects.
    
      
    

## Mechanics and Kinematics

- **Classification of Mechanics:** Mechanics is divided into Rigid Bodies (which includes Statics and Dynamics) and Soft (Deformable) bodies. Dynamics is further split into Kinematics and Kinetics.
    
      
    
- **Kinematics Definition:** The branch of mechanics concerned with the motion of objects—specifically how individual parts move relative to each other and the environment—without reference to the forces causing the motion.
    
      
    
- Kinematics deals only with position and speed (the first derivative of position), not dynamics or acceleration.
    
      
    

## Forward Kinematics (Robot Arm)

- **Purpose:** Forward kinematics calculates the robot's resulting position in the environment based on the actuators' control parameters.
    
      
    
- The position and angle of each joint dictate the end-effector's location using trigonometry.
    
      
    
- **Degrees of Freedom:** Objects can move in terms of Up/Down, Left/Right, Forward/Back, Roll, Pitch, and Yaw.
    
      
    
- **Kinematic Equations (2-Link Arm):** Using a local coordinate system with the origin at the robot's base:
    
      
    - $x_1 = \cos(\alpha)l_1$
        
          
        
          
        
    - $y_1 = \sin(\alpha)l_1$
        
          
        
          
        
    - $x_2 = \cos(\alpha+\beta)l_2 + x_1$
        
          
        
          
        
    - $y_2 = \sin(\alpha+\beta)l_2 + y_1$
        
          
        
          
        
    - Final Position: $x = \cos(\alpha+\beta)l_2 + \cos(\alpha)l_1$ and $y = \sin(\alpha+\beta)l_2 + \sin(\alpha)l_1$
        
          
        
          
        
- **Spaces:**
    
      
    - **Configuration Space:** The set of angles each actuator can be set to (e.g., $0 \le \alpha \le \pi$, $-\pi < \beta < \pi$).
        
          
        
    - **Workspace:** The physical space the robot can move to, calculated using the forward kinematic equations.
        
          
        

## Holonomic vs. Non-Holonomic Systems

- **Holonomic Kinematics:** Closed trajectories in the configuration space result in closed trajectories in the workspace (e.g., a train).
    
    ![Pasted image 20260930233846.png](/img/user/imgs/Pasted%20image%2020260930233846.png)

> [!Configuration Space vs. Workspace Analysis]
> 
> This image illustrates the concept of **holonomic kinematics** for a 2-link robotic arm, demonstrating that a closed trajectory in the robot's configuration space (its joint angles) results in a closed trajectory in its physical workspace.
> 
>   
> 
> ## Graph Overview
> 
> - **Left Graph (Configuration Space):** Plots the internal joint angles of the robot, with α (alpha) representing the angle of the first link in radians on the y-axis, and β (beta) representing the angle of the second link relative to the first on the x-axis.
>     
>       
>     
> - **Right Graph (Workspace):** Plots the physical x and y coordinates of the robot arm in meters, showing the actual path the arm and its end-effector trace in the real world.
>     
>       
>     
> 
> ## Step-by-Step Arm Movement
> 
> The movement follows the closed loop shown in the configuration space, mapping to the physical movements on the right:
> 
>   
> 
> - **Initial State (0, 0):** Both α and β are at 0 radians. The arm is fully extended horizontally along the x-axis.
>     
>       
>     
> - **Movement 1 (Diagonal line to π/2, π/2):** Both joint angles increase simultaneously. Link 1 rotates 90 degrees upwards to align with the y-axis, while Link 2 simultaneously rotates 90 degrees relative to Link 1 (pointing leftwards). The end-effector traces the large, outer upward curve.
>     
>       
>     
> - **Movement 2 (Horizontal line to β=π):** The angle α remains at π/2 (Link 1 stays pointing straight up), while β increases from π/2 to π. Link 2 rotates another 90 degrees, folding completely downward to overlap with Link 1. The end-effector traces the arc moving inwards toward the y-axis.
>     
>       
>     
> - **Movement 3 (Vertical line to α=0):** The angle β remains at π (Link 2 stays completely folded), while α decreases from π/2 back to 0. Link 1 rotates 90 degrees back down to the x-axis, carrying the folded Link 2 with it.
>     
>       
>     
> - **Movement 4 (Horizontal line back to 0, 0):** The angle α remains at 0 (Link 1 stays resting on the x-axis), while β decreases from π back to 0. Link 2 unfolds by rotating 180 degrees outward. The end-effector traces the lower semi-circle path, returning the arm to its exact starting, fully-extended position.
    
- **Non-Holonomic Kinematics:** Closed trajectories in the configuration space may not result in closed trajectories in the workspace.
    
    ![Pasted image 20260930234417.png](/img/user/imgs/Pasted%20image%2020260930234417.png)

> [!Configuration Space vs. Workspace Analysis (Non-Holonomic)]
> 
> This image illustrates the concept of **non-holonomic kinematics** using a differential-drive mobile robot (a robot with a left and right wheel). It demonstrates that for non-holonomic systems, a closed trajectory in the configuration space (wheel rotations) does **not** result in a closed trajectory in the physical workspace.
> 
>   
> 
> ## Graph Overview
> 
> - **Left Graph (Configuration Space):** Plots the accumulated rotation of the robot's wheels. The y-axis represents the left wheel's rotation in radians, and the x-axis represents the right wheel's rotation in radians.
>     
>       
>     
> - **Right Graph (Workspace):** Plots the physical $x$ and $y$ coordinates of the robot in meters, showing the actual path the robot travels on the ground.
>     
>       
>     
> 
> ## Step-by-Step Robot Movement
> 
> The movement follows the closed rectangular loop shown in the configuration space, mapping to the physical movements on the right:
> 
>   
> 
> - **Initial State (0, 0):** Both wheels are at their starting rotation of 0 radians. The robot starts at the physical origin (0,0) facing along the x-axis.
>     
>       
>     
> - **Movement 1 (Diagonal line up and right):** Both the left wheel and the right wheel rotate forward at the exact same rate. Because both wheels move equally, the robot drives in a straight line forward along the x-axis in the workspace.
>     
>       
>     
> - **Movement 2 (Horizontal line moving right):** The left wheel stops rotating (its value on the y-axis remains constant), while the right wheel continues to rotate forward (its value on the x-axis increases). Because only the right wheel is driving forward, the robot pivots to the left around the stationary left wheel.
>     
>       
>     
> - **Movement 3 (Vertical line moving down):** The right wheel stops rotating (x-axis value is constant), while the left wheel's rotation decreases (y-axis value drops). This means the left wheel is driving in reverse while the right wheel is locked, causing the robot to pivot backward.
>     
>       
>     
> - **Movement 4 (Horizontal line moving left back to 0,0):** The left wheel remains stopped at 0, while the right wheel's rotation decreases back to 0 (moving left along the x-axis). The right wheel drives in reverse, causing another pivoting motion.
>     
>       
>     
> 
> ## The Non-Holonomic Proof
> 
> At the end of this sequence, the configuration space graph returns exactly to (0,0), meaning the total net rotation of both wheels is zero. However, looking at the right graph, the physical robot **does not** return to the origin. It ends up stranded further down the x-axis, rotated in a different orientation.
> 
>   
> 
> This visualizes the rule of mobile robots: you cannot determine a vehicle's final physical location simply by looking at the total number of wheel rotations; the specific sequence and timing of those rotations dictate the final path.
    
- Mobile robots (like cars or differential-wheel robots) are generally non-holonomic.
    
      
    
- For mobile robots, encoder values refer to wheel orientation and must be integrated over time; it matters _when_ a movement was executed, not just how far a wheel traveled.
    
      
    
- Executing a straight line then a right turn yields the same amount of wheel rotation as a right turn followed by a straight line, but results in a completely different final location.
    
      
    
- The speed of each wheel as a function of time is required for mobile robots, unlike a manipulating arm whose pose is uniquely determined by joint positions alone.
    
      
    
> [!Holonomic vs. Non-Holonomic Systems]
> 
> To understand the difference, you must first distinguish between **Configuration Space** (the internal state of the robot, such as the total number of rotations its wheels or joints have made) and the **Workspace** (the robot's actual physical position and orientation in the real world).
> 
>   
> 
> ## Holonomic Systems
> 
> A system is holonomic if its controllable degrees of freedom equal its total degrees of freedom. Intuitively, this means the robot can move in any direction at any given moment without needing to reorient itself first.
> 
>   
> 
> - **The Trajectory Rule:** In a holonomic system, a closed trajectory in the configuration space always results in a closed trajectory in the workspace. If you move the actuators through a sequence of states and return them to their exact starting values, the robot will always return to its exact physical starting location.
>     
>       
>     
> - **Examples:**
>     
>       
>     - A robotic manipulator arm: The pose of the arm is uniquely determined by the position of its joints.
>         
>           
>         
>     - A train on a track: It can only move forward and backward along a fixed rail.
>         
>           
>         
>     - An omnidirectional robot (e.g., using Mecanum wheels) that can slide sideways.
>         
>           
>         
> 
> ## Non-Holonomic Systems
> 
> A system is non-holonomic if its movement is constrained, meaning it has fewer controllable degrees of freedom than total degrees of freedom. It cannot move instantaneously in every direction (e.g., a standard car cannot move horizontally sideways).
> 
>   
> 
> - **The Trajectory Rule:** Closed trajectories in the configuration space may _not_ result in closed trajectories in the workspace. Returning the wheels to their starting rotation counts does not guarantee the robot has returned to its starting physical location.
>     
>       
>     
> - **Examples:** Standard cars and differential-wheel mobile robots.
>     
>       
>     
> 
> ## The "Path Dependency" Intuition
> 
> The lecture highlights this difference using a driving example that proves why most mobile robots are non-holonomic.
> 
>   
> 
> Imagine two different sequences of movement using the exact same total amount of wheel rotation:
> 
>   
> 
> 1. **Sequence A:** Drive straight forward for 5 meters, then turn 90 degrees to the right.
>     
>       
>     
> 2. **Sequence B:** Turn 90 degrees to the right _first_, then drive straight forward for 5 meters.
>     
>       
>     
> 
> In both sequences, the robot's wheels rotated the exact same number of times (identical changes in the configuration space). However, the robot ends up in completely different physical locations in the room (different workspaces).
> 
>   
> 
> ## Why This Matters for Mobile Robotics
> 
> Because of this non-holonomic property, you cannot simply look at a mobile robot's wheel encoders (how many times the wheels turned) to know where the robot is.
> 
>   
> 
> To determine a non-holonomic robot's position, it is not sufficient to measure the total distance each wheel traveled; you must know exactly _when_ each movement was executed. The system must continuously integrate the speed of each wheel as a function of time to track the robot's true path.
## Inverse Kinematics and Odometry

- **Inverse Kinematics:** The process of mapping a desired Cartesian space position back into joint space, calculating the necessary position for each joint or wheel to achieve that outcome.
    
      
    
- **Odometry:** The use of motion sensor data to estimate changes in position over time, which is essential for mobile robots to estimate their location relative to a starting point.