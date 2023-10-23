---
---


<br>
<br>
<br>
<br>
<br>
<br>


<div align="center">
	<h1>Adapting Grasps to Any Object Position</h1>
</div>

<br>
<br>

<div align="center">
	<font color="#b7b7b7" style="font-size:1.5rem"><b>Learning to Grasp: From Somewhere to Anywhere</b></font>
</div>

<div align="center">
	<font color="#b7b7b7" style="font-size:1.35rem">François Hélénon*, Johann Huber*,</font>
</div>
<div align="center">
	<font color="#b7b7b7" style="font-size:1.35rem">Faïz Ben Amar and Stéphane Doncieux (2023)</font>
</div>

<br>
<br>

<font color="#b7b7b7" style="font-size:1.15rem">*: Equal contribution.</font>


<br>
<br>
<br>
<br>
<br>
<br>



### Beyond the simulated initial conditions

<br>

<p align="justify" class="space"> 
<font style="font-size:1.35rem;font-family:'Charter',serif;">
Quality-Diversity methods can be used to <a href="https://qdgrasp.github.io/qdgrasp/generating_the_data/">automatically generate large grasping datasets</a> that can be <a href="https://qdgrasp.github.io/qdgrasp/sim2real_labelling/">exploited into the real world</a>. However, the generated reach-and-grasps trajectories are open-loop: a significant modification of the initial conditions makes a generated grasp fail. 
</font>
</p>

<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/mug_open_loop_aligned.gif" style="width:300px;"> 
	<img src="/assets/blog_posts/qd_adapt/mug_open_loop_shifted.gif" style="width:300px;"> 
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7">Automatically generated open-loop grasping trajectory under simulation conditions (left) or with shifted object pose (right).</font>
</div>

<br>
<br>

<p align="justify" class="space"> 
<font style="font-size:1.35rem;font-family:'Charter',serif;">
The grasping skill requires the ability to perform the grasp on objects regardless of their position and orientation – as long as they remain in the agent's operational space. <b>A grasping trajectory can easily be generalized</b> to any state in the operational space by <b>converting the corresponding positions and orientations from the world frame to the object frame</b>. This idea fixes the abovementioned constraint, making the grasping datasets suited to any object state <b>without requiring additional iteration</b> of the QD grasp generator algorithm.
</font>
</p>


<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/qd_grasp_adapt_pipeline_overview.png" style="width:600px;"> 
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7">Overview of the proposed approach.</font>
</div>


<br>

<br>

<p align="justify" class="space"> 
<font style="font-size:1.35rem;">
Applying this idea to the real world raises a big challenge: getting an accurate 6DoF (Degree-of-Freedom) pose of a targeted object is an open problem (Tyree et al., 2022). There is currently no off-the-shelf module for doing reliable 6DoF pose prediction from an RGB-D camera.
</font>

<br>
<br>

<font style="font-size:1.35rem;">
This paper proposes grasping trajectory adaptation for generalizing automatically generated grasps to any object's initial states. To do so, we have implemented an integration pipeline that performs accurate 6DoF pose estimation for YCB objects. Overall, our pipeline involves 4 publicly available research modules for generating the grasps, detecting the object pose, and allowing generalization with tracking.
</font>

</p>


<br>
<br>
<br>
<br>
<br>
<br>



### Grasp Adaptation Pipeline


<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/qd_grasp_adapt_pipeline_grasp_generation.png" style="width:600px;"> 
</div>

<br>
<br>




##### Grasp Generation

<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/qd_grasp_franka_mug_scs_crop.gif" style="width:300px;"> 
	<img src="/assets/blog_posts/qd_adapt/qd_grasp_schunk_mug_2_scs_crop.gif" style="width:300px;"> 
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7">Example of automatically generated trajectories used in this work.</font>
</div>

<br>
<br>

<p align="justify" class="space"> 

<font style="font-size:1.35rem;">
The reach-and-grasp trajectories are generated with QD algorithms, similarly to (Huber et al, 2023). The automatic generation of grasps with QD is described in this <a href="https://qdgrasp.github.io/qdgrasp/generating_the_data/">related blog post</a>.

</font>
</p>


<br>
<br>
<br>
<br>


##### 6DoF Pose Prediction


<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/qd_grasp_adapt_pipeline_perception.png" style="width:600px;"> 
</div>


<br>
<br>

<p align="justify" class="space"> 
<font style="font-size:1.35rem;">
A given object name is set as input to an Open Vocabulary segmenter (Zhou et al., 2022) that provides a first guess on the object localization in the RGB-D image through the segmented region. 
</font>
</p>

<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/semantic_segmentation_crop_x4speedup.gif" style="width:450px;">
</div>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7">Open Vocabulary object segmentation (speed x5).</font>
</div>


<br>
<br>

<p align="justify" class="space"> 
<font style="font-size:1.35rem;">
The first 6DoF pose is then computed with MegaPose (Labbé et al. 2022) using the RGB-D image and the known object meshes.
</font>
</p>

<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/pose_estimate_crop.gif" style="width:450px;">
</div>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7">6DoF pose estimation</font>
</div>

<br>
<br>

<p align="justify" class="space"> 
<font style="font-size:1.35rem;">
Given the initial 6DoF pose, the object is then tracked with ICG (Stoiber et al., 2022).
</font>
</p>

<br>
<br>


<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/mug_tracking_crop.gif" style="width:450px;">
</div>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7">6DoF pose tracking</font>
</div>


<br>
<br>

<p align="justify" class="space"> 
<font style="font-size:1.35rem;">
The reach-and-grasp trajectory can finally be adapted using the tracked estimated pose.
</font>
</p>

<br>
<br>
<br>
<br>



##### Trajectory Adaptation

<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/qd_grasp_adapt_pipeline_adaptation.png" style="width:600px;"> 
</div>


<br>
<br>


<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/problem_definition.png" style="width:450px;">
</div>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7">Adaptation of the grasps trajectories to the object frame.</font>
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7">(See the paper for notation details)</font>
</div>



<br>
<br>

<p align="justify" class="space"> 
<font style="font-size:1.35rem;">
To compute the adaptation, a given grasping trajectory is first expressed as a sequence of positions and orientations in the world frame that describe the approach and the application of forces on the object. Those poses are then expressed relatively to the object frame, allowing the gripper to reach and grasp the object for any object's pose.
</font>
</p>

<br>

<p align="justify" class="space"> 
<font style="font-size:1.35rem;">
Such a straightforward approach modifies the initial position of the trajectory, which does not correspond to the initial end effector pose anymore. The trajectory is completed by reaching the first adapted position using a path planer.
</font>
</p>

<br>

<p align="justify" class="space"> 
<font style="font-size:1.35rem;">
The adapted grasping trajectory is not necessarily fully exploitable. Some poses might be out of the robot's operational space but might also lead to autocollisions or collisions with the environment (e.g., the table). The adapted trajectories are thus truncated to maximize the length of consecutive poses from the trajectory, which are reachable while assuring to preserve the prehension pose. In the worst case, the 6DoF prehension poses might be invalid. It is worth noting that the <a href="https://qdgrasp.github.io/qdgrasp/generating_the_data/">large diversity of QD generated grasps</a> guarantees to provide several grasping solutions for a considered object pose.
</font>
</p>



<br>
<br>
<br>
<br>

##### Scene and trajectory update

<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/qd_grasp_adapt_pipeline_all.png" style="width:600px;"> 
</div>


<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/mug_tracking_simu_x4speedup.gif" style="width:450px;"> 
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7">Adaptation of the grasping trajectory to the predicted object pose (speed x4).</font>
</div>

<br>
<br>

<p align="justify" class="space"> 
<font style="font-size:1.35rem;">
The above animation shows the adaptation of a QD-generated grasping trajectory to the predicted object 6DoF pose. In green is the object pose at which the grasp has been generated in simulation. The colored curve above the mug is the sequence of states the end effector must follow to grasp the object. The green, blue, and red arrows represent the corresponding orientations. The pipeline provides an <b>adapted trajectory</b> to follow for <b>any predicted object position</b>, <b>allowing the exploit of automatically generated datasets for any targeted position in the operational space.</b>
</font>
</p>


<br>
<br>
<br>
<br>
<br>
<br>




### Results


<br>
<br>


##### Generalisation to the whole operational space


<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/motion_planning_simulation.gif" style="width:400px;"> 
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7">Evaluation of the exploitability of a generated dataset all over the operational space.</font>
</div>

<br>
<br>


<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/plots/translation_only_count.png" style="width:600px;">
	<img src="/assets/blog_posts/qd_adapt/plots/mug_11250_trajectories_27_rotations_with_translation_count_cropped.png" style="width:600px;">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7">For 5 randomly sampled trajectories, how many of them are still valid for translated (top) or translated + rotated object state (bottom)? (See the paper for details.)</font>
</div>

<br>
<br>

<p align="justify" class="space"> 
<font style="font-size:1.35rem;">
As previously mentioned, the simple adaptation method proposed in this article does not necessarily lead to valid grasp trajectories. To evaluate to what extent the automatically generated grasps can be exploited with the adapation without having to sample other grasps from a QD generated dataset, a specific experiment has been conducted in simulation. It consists of estimating the number of valid grasps after adaptation for a wide range of object positions and orientations. The above-presented results show that the adaptation almost always leads to valid grasps when translating the object (top row) – except when the positions are too far from the robot (out of range) or too close (leading to autocollision). When applying both translation and rotation (bottom row), the ratio of successful adaptation is lower. The rotations often require truncation of the approaching phase, making the grasp's success more dependent on the path planner. Plus, the 6-DoF prehension can be projected into obstacles, making them unreachable. Nevertheless, <b>a significant part of the trajectories can straightforwardly be applied to new object positions without compromising the success of the grasp</b>.
</font>
</p>

<br>
<br>


##### Real world reach-and-grasp trajectory adaptation

<br>
<br>

<p align="justify" class="space"> 
<font style="font-size:1.35rem;">
The approach has been validated on two real robots: a Franka Research 3 arm with a 2-fingers gripper and a UR-5 arm with an SIH Schunk Hand. Some grasping trajectories have been generated with the ME-scs QD algorithm (Huber et al., 2023). A subset of the YCB objects (Calli et al., 2015) has been used for the experiments.
</font>
</p>

<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/setup_experiment.png" style="width:500px;">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7">Experimental setups.</font>
</div>

<br>
<br>

<p align="justify" class="space"> 
<font style="font-size:1.35rem;">
It is worth noting that 3 different RGB-D camera has been used: a D435i camera positioned with a 45° angle point-of-view on the scene for the FR3 arm; a L515 with a 90° angle, and a D455 with a 0° angle point-of-view on the scene for the UR5 arm. The proposed approach exploits <b>a single RGB-D camera at a time</b>. What is notable is that the perception part of the proposed approach operates <b>similarly with any of those 3 kinds of RGD-D camera</b>, being similarly <b>robust to the 3 point-of-views</b>. We believe this property to be a key strength of the proposed approach, considering the challenges of 6-DoF object pose estimators on real scenes (Tyree et al., 2022).
</font>
</p>

<br>
<br>


<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/adapt_real/franka/franka_real_adapt_bc_2.gif" style="width:152px;">
	<img src="/assets/blog_posts/qd_adapt/adapt_real/franka/franka_real_adapt_bc_1.gif" style="width:160px;">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7">Adapted trajectory on the bleach cleanser. The heavy rotation resulted into the truncation of a significant part of the approach, leading to a more direct grasp.</font>
</div>

<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/adapt_real/franka/franka_real_adapt_mug_handle_1.gif" style="width:160px;">
	<img src="/assets/blog_posts/qd_adapt/adapt_real/franka/franka_real_adapt_mug_handle_2.gif" style="width:142px;">
	<img src="/assets/blog_posts/qd_adapt/adapt_real/franka/franka_real_adapt_mug_handle_3.gif" style="width:151px;">
	<img src="/assets/blog_posts/qd_adapt/adapt_real/franka/franka_real_adapt_mug_handle_4.gif" style="width:160px;">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7">Adapted trajectory on the mug for different example of perturbated poses.</font>
</div>


<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/adapt_real/schunk/schunk_real_adapt_pd_1.gif" style="width:160px;">
	<img src="/assets/blog_posts/qd_adapt/adapt_real/schunk/schunk_real_adapt_pd_2_crop.gif" style="width:149px;">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/adapt_real/schunk/schunk_real_adapt_pd_3_crop.gif" style="width:200px;">
	<img src="/assets/blog_posts/qd_adapt/adapt_real/schunk/schunk_real_adapt_pd_4_crop.gif" style="width:200px;">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7">Some example of the schunk SIH hand on the power drill.</font>
</div>

<br>
<br>

<p align="justify" class="space"> 
<font style="font-size:1.35rem;">
The results obtained on the real robot validate the simple adaptation strategy for two robotic manipulators of different natures, preserving the benefits of QD methods for <b>easy generalization to different plateforms</b>. It also <b> preserves the diversity of grasps regarding the quality</b> (more or less robust grasps) <b>and the affordances</b> (diverse application of forces on the object for accomplishing certain tasks afterward).

</font>
</p>

<br>
<br>

##### Limits

<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/plots/failure_orientation_powerdrill_ambiguity.png" style="width:400px;"> 
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7">A single RGB-D camera can lead to ambiguities on the object's orientation.</font>
</div>


<br>
<br>

<p align="justify" class="space"> 
<font style="font-size:1.35rem;">
This work has some limitations. First, the vision noise can lead to wrong 6DoF pose estimation and limit the transferability of some grasps. Moreover, some object surfaces are slippery, especially with only two fingers, when applying too much force on the object surface. The proposed approach is also not close-loop adaptation: as soon as the grasping trajectory deployment is started, the modification of the object state will not be taken into account on the flight.
</font>
</p>



<br>
<br>
<br>
<br>



### Conclusions


<br>
<br>


<p align="justify" class="space"> 
<font style="font-size:1.35rem;">
This work proposes an <b>integration pipeline to allow vision-based adaptation of automatically generated grasping trajectories</b>. Results on several robots – including a 2-fingers gripper and a dexterous hand – show that trajectories that were initially restricted to the simulated object position <b>can be exploited all over the operational space</b>. This work demonstrates that QD-generated grasping datasets can be used for vision-based open-loop grasping. It also suggests that QD-generated datasets can be generalized to a wide range of object positions and orientations for producing large-scale grasping datasets that can be used for learning close-loop policies.
</font>
</p>


<br>
<br>
<br>
<br>


### Acknowledgement

<p align="justify">
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
Authors deeply thank Pr. Sven Behnke andthe members of the AIS lab of Bonn for their warm welcome and support.
</font>
</p>

<br>

<p align="justify">
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
This work was supported by the Sorbonne Center for Artificial Intelligence, the German Ministry of Education and Research (BMBF) (01IS21080), the French Agence
Nationale de la Recherche (ANR) (ANR-21-FAI1-0004) (Learn2Grasp), the European Commission’s Horizon Europe Framework Programme under grant No 101070381 and
from the European Union’s Horizon Europe Framework Programme under grant agreement No 101070596. This work used HPC resources from GENCI-IDRIS (Grant 20XX-
AD011014320).
</font>
</p>


<br>
<br>
<br>
<br>

### References

<i>Tyree, S., Tremblay, J., To, T., Cheng, J., Mosier, T., Smith, J., & Birchfield, S. (2022, October). 6-DoF pose estimation of household objects for robotic manipulation: An accessible dataset and benchmark. In 2022 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS) (pp. 13081-13088). IEEE.</i>

<br>

<i>Huber, J., Hélénon, F., Coninx, M., Ben Amar, F., Doncieux, S. (2023). Quality Diversity under Sparse Reward and Sparse Interaction: Application to Grasping in Robotics. arXiv:2308.05483</i>

<br>

<i>X. Zhou, R. Girdhar, A. Joulin, P. Krähenbühl, and I. Misra, “Detecting Twenty-Thousand Classes Using Image-Level Supervision,” in Computer Vision – ECCV 2022, S. Avidan, G. Brostow, M. Cissé, G. M. Farinella, and T. Hassner, Eds. Cham: Springer Nature Switzerland, 2022, vol. 13669, pp. 350–368.</i>

<br>

<i>B. Calli, A. Singh, A. Walsman, S. Srinivasa, P. Abbeel, and A. M. Dollar, “The YCB object and Model set: Towards common benchmarks for manipulation research,” in 2015 International Conference on Advanced Robotics (ICAR), July 2015, pp. 510–517.</i>

<br>

Y. Labbé, L. Manuelli, A. Mousavian, S. Tyree, S. Birchfield, J. Tremblay, J. Carpentier, M. Aubry, D. Fox, and J. Sivic, “MegaPose: 6D Pose Estimation of Novel Objects via Render & Compare,” in 6th Annual Conference on Robot Learning, Aug. 2022.

<br>

M. Stoiber, M. Sundermeyer, and R. Triebel, “Iterative Corresponding Geometry: Fusing Region and Depth for Highly Efficient 3D Tracking of Textureless Objects,” in 2022 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR). New Orleans, LA, USA: IEEE, June 2022, pp. 6845–6855.


<br>
<br>
<br>
<br>
<br>












