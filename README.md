# MMPK-Deployment-Results
This repository serves as a supplemental documentation of the test scenarios and results presented for our 
novel end-to-end data-driven (modeling/planning/control) framework called the Multi-Model Parameterized Koopman (MMPK)
framework.


### Abstract:
This research introduces the Multi-Model Parameterized Koopman (MMPK) framework, a novel data-driven approach for enabling autonomous navigation in autonomous ground vehicles. 
MMPK builds upon the Koopman Extended Dynamic Mode Decomposition (KEDMD) algorithm, offering a flexible modeling and control solution. 
Unlike traditional methods, MMPK addresses challenges such as overfitting and reliance on a single model by adopting a pose-agnostic representation of positional data and employing a set of parameterized models based on curvature. 
Additionally, the MMPK approach effectively mitigates data bias and reduces dependency on a singular global model. 
Our paper presents an end-to-end unified pipeline encompassing offline MMPK modeling and an online outer-loop control design consisting of model-based trajectory planning and linear Model Predictive Control adapted to switched Koopman dynamics. 
Comparative evaluations demonstrate MMPK's superior path-tracking capabilities and the effectiveness of its local planning strategy in bridging the model-sim-real gap. </br>


**Paper (Preprint)**: </br>
[Data-Driven Modeling and Experimental Validation of Autonomous
Vehicles using Koopman Operator](https://www.researchgate.net/publication/369737963_Data-Driven_Modeling_and_Experimental_Validation_of_Autonomous_Vehicles_using_Koopman_Operator)

### Repository Organization
- **Figures:** This folder contains all the plots from the experiment. 
- **Videos:** Folder for storing video results from MMPK hardware deployment.

### MMPK Framework

![Framework](Figures\Framework_digram_brief.png)

The framework can be broken down into following process flows.</br>

- **Data-gathering/Model-training (Offline):** 
  - First, we collect data of a vehicle undergoing maneuvers resulting in roll/yaw plane excitation.
  - The data is then converted into body-frame representation.
  - Finally, it is split into distinct buckets based on the curvature bins.
  
- **Parameterized family of Koopman models (Offline):** 
  - For each of the curvature bins, the associated set of state, control, and output matrix are approximated.
  - Once trained, these models are directly used for the control loop.
  
- **Outer control loop (Online):** 
  - For the online real-time deployment, the reference trajectory and the pose estimate of the vehicle are sent to the local motion planner.
  - The local planner samples all the feasible trajectories across models and chooses the one closest to the reference trajectory.
  - This reference trajectory along with the selected curvature is passed forward to the linear MPC.
  - The linear MPC produces high-level commands (velocity steering) for the vehicle.



### Experiment Setup
The following diagram presents pictorial representation for host of experimental stipulations for thorough
validation of MMPK framework in simulation and hardware deployment settings.

![Expsetup](Figures\Experimental_setup.png)

The experiments are divided into two sections:
**Experiment I** for deployment in the simulation
environment and **Experiment II** for hardware
deployment.<br>
**Experiment I-A to I-D:** <br>
In this setup, we are evaluating
model v/s simulation disparity and effect of pose
feedback and parameterized Koopman models
and their effect on tracking performance across
reference trajectories in the test dataset. Following
are the stipulations for each of the test cases:
- _Experiment I-A:_ Single model with quintic
sampling and model feedback from MPC as
pose estimate.
- _Experiment I-B:_ Single model with quintic
sampling and pose feedback from simulation
as pose estimate.
- _Experiment I-C:_ Parameterized model with
reachability based motion planning and
model feedback from MPC as pose estimate.
- _Experiment I-D:_ Parameterized model with
reachability based motion planning and
feedback from simulation as pose estimate .

**Experiment II-A to II-D:** <br>
In the following experiments , we evaluate
the disparity between model and hardware and
the impact of pose feedback and parameterized
Koopman models on tracking performance across
reference trajectories in the test dataset. The
stipulations for each test case are as follows:
- _Experiment II-A and II-C:_ Similar to Experiments _I-A and I-C_, the feedback is agnostic to
the actual realization of motion for the robot
in simulation and on hardware.
- _Experiment II-B and II-D:_ Similar to Experiments _I-B and I-D_, the only difference is that
the pose feedback is achieved from LiDAR- based odometry

we have considered 8 models as a baseline for MMPK 
It means that the entire operational range of the vehicle is broken down into 8 discrete bins. 
For each of the data points, curvature of instantaneous reference trajectory is captured through the motion planner and fit into one of the 8 bins of operation. 
Thus, all the box and whisker plots are split in these 8 bins even where the number of models and their associated curvature bins of operation vary but are approximated with 8 reference bins to maintain consistency in comparison as represented visually in the figure below.
![Test_methodology](D:\MMPK-Deployment-Results\Figures\Test_comparison_methodology.png)



### Results
**Simulation deployment (Experiment I-A to I-D):**
![ExpIA-IB](https://github.com/ajinkya-joglekar/MMPK-Deployment-Results/blob/main/Figures/Experiment%20I-A_vs_Experiment%20I-B.png)
![ExpIC-ID](D:\MMPK-Deployment-Results\Figures\Experiment I-C_vs_Experiment I-D.png)
![ExpIB-ID](D:\MMPK-Deployment-Results\Figures\Experiment I-B_vs_Experiment I-D.png)

**Hardware deployment (Experiment II-A to II-D):**
![ExpIIA-IIB](D:\MMPK-Deployment-Results\Figures\Experiment II-A_vs_Experiment II-B.png)
![ExpIIC-IID](D:\MMPK-Deployment-Results\Figures\Experiment II-C_vs_Experiment II-D.png)
![ExpIIB-IID](D:\MMPK-Deployment-Results\Figures\Experiment II-B_vs_Experiment II-D.png)


**Optimizing number of MMPK models:**
![ExpID-6models-ID-8models](D:\MMPK-Deployment-Results\Figures\Experiment_I-D_6 models_vs_Experiment_I-D_8 models.png)
![ExpID-8models-ID-12models](D:\MMPK-Deployment-Results\Figures\Experiment_I-D_8 models_vs_Experiment I-D_12 models.png)
![ExpIID-8models-IID-12models](D:\MMPK-Deployment-Results\Figures\Experiment_II-D_8 models_vs_Experiment II-D_12 models.png)



**Hardware deployment trajectory plot**
![Traj_ff](D:\MMPK-Deployment-Results\Figures\Traj_freeze_frame_plots.png)