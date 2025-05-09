# Autonomous-Vehicle-Navigation-Using-Deep-Learning

**[Paper Link](https://drive.google.com/file/d/1-GqUQUW696-wRDpZN61SrFRGXc5QeCao/view?usp=sharing)**

## Introduction
The development of safe and reliable autonomous driving technology remains a critical research area, with numerous studies exploring various approaches to enhancing the safety of self-driving vehicles. When it comes to beyond level 4 in autonomous driving technology, the vehicle should be able to deal with various kinds of situations around the self-driving vehicle in order to arrive at the targeted destination successfully. Different sensor and algorithm-based approaches have been tried and developed to navigate safely to the target destination in the CARLA simulator. Conventional control systems are based on mathematical models, but they only control the vehicle in a limited range of situations. Therefore, Machine Learning (ML) algorithms have been applied in autonomous systems to better control vehicles in varied situations.



## Problem Statement
The deployment of autonomous vehicles on road to safely navigate in uncertain environments to the final destination require fast computation, low latency, and high accuracy. Therefore, there is a need to develop efficient techniques to process high-dimensional input data from sensors and images to reduce the state space and improve the training and testing efficiency. This paper proposes a Deep RL method using DQN, which includes sensor data pre-processing steps to reduce the state space and demonstrates its effectiveness in successfully navigating through 4 traffic scenarios with high levels of safety and accuracy.

<table>
  <tr>
    <td>
      <img src="/images/trajs.png" alt="Trajectories" width="100%">
      <p align="center">Trajectories</p>
    </td>
    <td>
      <img src="images/schemes.png" alt="Schematic" width="100%">
      <p align="center">Vehicles FoV & Waypoints</p>
    </td>
  </tr>
</table>


<!-- <img src="/images/trajs.png" alt="Trajectories" width=50% style="display: block; margin: 0 auto;">
 -->


## Installtion
1. Clone this repo: `git clone https://github.com/varunpratap222/Autonomous-Vehicle-Navigation-Using-Deep-Learning.git`
2. Download Carla 0.9.13 from [source](https://github.com/carla-simulator/carla/releases). 
3. Install required packages: `pip install -r requirements.txt`
4. Requirements file contains packages compatible with Ubuntu 22.0



## Run
1. Run Carla Server using: `./CarlaUE4.sh`
2. Run `config.py` file to load Town02
3. Generate traffic either using `generate_traffic.py` or spawn pedestrians at random location along Trajectory 1 and 2 using the `pedestrians_1.py` and `pedestrians_2.py` script. Change the number of vehicles and pedestrians to spawn using `generate_traffic.py` by passing the corresponding arguments.
4. Select an existing trajectory (Trajectory 1, Trajectory 2, Trajectory 3, Trajectory 4) or set custom trajectory using the format given in `test_everything.py` arguments.
5. To find the initial and final locations of the custom trajectory, make use of `get_location.py` file and navigate the map using W,A,S,D, E, and Q keys. 
6. Enter locations or select existing trajectories and run the `test_everything.py` file.



## Methodology
Model Architecture Encapsulating the workflow:

<center><img src="/images/model.jpg" alt="Model Architecture"></center>

To build a model that allows a car to drive safely and autonomously in traffic, we need to combine the braking and driving DQN models that are developed using the `braking_dqn.py` and `driving_dqn.py` scritps. We use a hierarchical approach where the braking model is used as a safety net to prevent collisions when an obstacle is too close. The final model takes as input the values of $d$, $\phi$, $d_{obs}$, and $v$, obtained from the observations of the agent. Before making predictions on which action to take, the agent checks if there is a traffic light. If it is red, then, it brakes. If the light is green, the braking model uses $v$ and $d_{obs}$ to make a prediction on whether it is safe to drive or if the car should brake. If it is safe to drive, the driving model uses $d$ and $\phi$ to make a prediction on which driving action to take. This approach ensures that the car can safely navigate through traffic while following its path and reaching its final destination.

The image below shows the pre-processing of Segmentation and Depth Images to compute the distance to the closest obstacle $d_{obs}$

<center><img src="/images/preprocessing_step.jpg" alt="Model Architecture"></center>


## Case Scenarios and Results
The trained DQN models are tested on 4 different trajectories in a traffic scenario containing 35 other vehicles and 80 pedestrians. The model is trained on a different set of trajectories and below selected trajectories demonstrate the effectiveness of our models to generalize on unseen data (refer to trajectories from the first image). The below table shows the percentage of success and failure cases for each trajectory when tested for 25 runs each.


## Trajectory Statistics

| Trajectory Name | Path Distance (m) | Time (s) | % Success | % Collision with Vehicles | % Collision with Pedestrians | Deadlocks |
| --------------- | ----------------- | -------- | --------- | ------------------------ | ---------------------------- | --------- |
| Trajectory 1    | 258               | 321.5    | 96        | 0                        | 4                            | 0         |
| Trajectory 2    | 163               | 177.4    | 92        | 4                        | 0                            | 4         |
| Trajectory 3    | 150               | 148.18   | 92        | 4                        | 4                            | 0         |
| Trajectory 4    | 104               | 136.48   | 96        | 0                        | 4                            | 0         |
| Average         | 168.75            | 195.89   | 94        | 2                        | 3                            | 1         |

A run is considered to be successful if the car safely navigates to the final destination without colliding with an obstacle (vehicle, pedestrian, and sidewalk). Successful runs for trajectories 1, 2, 3, and 4 are shown in [Videos 4, 5, 6, and 7](https://drive.google.com/drive/folders/1dLaCMtb7UOpxs6karu0RshLH0Opd3Izp?usp=sharing) respectively. We achieved a success rate of 94\% when we ran our model on 4 different trajectories for a total of 100 runs. 

Video for all trajectories and failure cases can be found [here](https://drive.google.com/drive/folders/1dLaCMtb7UOpxs6karu0RshLH0Opd3Izp?usp=sharing). 


## File Overview

| File Name          | Overview                                                                                       |
| ------------------| ----------------------------------------------------------------------------------------------|
| braking_dqn.py     | Script for training the braking model                                                        |
| car_env.py         | Class containing environmental setup and complete process                                    |
| config.py          | To reset the town (pre-existing file)                                                         |
| driving_dqn.py     | Script for training the driving model                                                         |
| front_car.py       | To check the effectiveness of the braking model                                               |
| generate_traffic.py| To generate traffic (pre-existing file)                                                        |
| get_location.py    | To get the coordinates of initial and final destination                                       |
| pedestrians_1.py   | To spawn pedestrians at predefined locations on Trajectory 1                                  |
| pedestrians_2.py   | To spawn pedestrians at predefined locations on Trajectory 2                                  |
| test_braking.py    | To test the braking model                                                                     |
| test_driving.py    | To test the driving model                                                                     |
| test_everything.py | Main file to run the selected trajectories                                                     |
