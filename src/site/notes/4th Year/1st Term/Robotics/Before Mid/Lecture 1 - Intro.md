---
{"dg-publish":true,"permalink":"/4th-year/1st-term/robotics/before-mid/lecture-1-intro/","dg-note-properties":{}}
---

# **History and Origins of Robotics**

  

- Robotics is the branch of technology dealing with physical, programmable machines capable of executing autonomous or semi-autonomous actions.
    
      
    
- The term "robot" originates from the Czech word "robota," which translates to slave.
    
      
    
- The word was first used in a 1921 play featuring mechanical men built to work on factory assembly lines who eventually rebel against their human masters.
    
      
    
- The first commercial robot, Unimate, was introduced 65 years ago in 1961.
    
      
    
- Unimate was a programmable machine that demonstrated capabilities such as opening and pouring a bottle of beer, and putting a golf ball into a hole.
    
      
    
- Modern robots have only recently achieved the ability to perform Unimate's original demonstrations autonomously, relying on affordable, powerful sensors and computation to independently detect objects, plan motions, and grasp them.
    
      
    

# **The Three Ethical Laws of Robotics (1942)**

  

- A robot may not injure a human being or, through inaction, allow a human being to come to harm.
    
      
    
- A robot must obey orders given by humans, except when those orders conflict with the First Law.
    
      
    
- A robot must protect its own existence as long as doing so does not conflict with the First or Second Law.
    
      
    

# **Formal Definitions of Robotics**

  

- **Robot Institute of America (1979):** A reprogrammable, multifunctional manipulator designed to move materials, parts, tools, or specialized devices through programmed motions to perform varied tasks.
    
      
    
- **Webster's Dictionary:** An automatic device performing functions normally ascribed to humans, or a machine in human form.
    
      
    
- **British Department of Industry:** A reprogrammable manipulator device.
    
      
    
- **Mike Brady:** The field concerned with the intelligent connection of perception to action.
    
      
    

# **Modern Autonomous Robots and System Architecture**

  

- Unlike preprogrammed machines, autonomous robots make decisions in response to their environment.
    
      
    
- They integrate techniques from signal processing, control theory, and artificial intelligence with the robot's mechanics, sensors, and actuators.
    
      
    
- The basic architecture of an autonomous robot forms a continuous loop between the real-world environment and internal processing:
    
      
    - **Perception:** Sensors gather raw data from the environment, which is extracted into information to build a local map and localize the robot.
        
          
        
    - **Cognition:** The system uses mission commands, a knowledge database, an environment model, and a global map to calculate a path.
        
          
        
    - **Acting:** The planned path is executed via motion control, driving the actuators to interact with the physical world.
        
          
        

# **The Role of a Roboticist**

  

- A roboticist is responsible for designing, programming, constructing, and testing smart machines.
    
      
    
- They may specialize in specific development stages, such as software creation, hardware building, or troubleshooting, and do not need in-depth knowledge of every single phase of development.
    
      
    

# **Core Challenges in Autonomous Robotics**

  

- **Mobility:** Controlling wheel-speed accurately to reach desired positions.
    
      
    
- **Perception:** Selecting appropriate sensors to monitor the robot's own status and extracting structured information from massive amounts of raw sensor data.
    
      
    
- **Localization:** Determining the robot's exact position in the world while representing errors and reasoning under uncertainty.
    
      
    
- **Manipulation:** Successfully grasping, holding, and moving objects.
    
      
    
- **Handling Unreliability:** Robotics differs from pure artificial intelligence because robots must operate with unreliable sensing, actuation, and communication links.
    
      
    
- **Probabilistic Reasoning:** Due to hardware and environmental unreliability, robotic systems require probabilistic models to manage and function safely despite uncertainty.