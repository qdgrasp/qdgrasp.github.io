---
---

<br>
<br>

<div align="center">
	<h1>Papers</h1>
</div>

<br>
<br>


##### Generating the grasps

<br>

<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">QD methods need a relevant behavioral characterization to address a given problem. But inferring it from the grasping success criterion results in a too high dimensional behavioral space, making the problem untractable. This paper introduces a simple behavioral characterization to address robotic grasping with QD. This design choice led to a new setup called sparse interaction problems, in which the evaluations may not return a behavioral signal. It appears that sparse interaction problems lead to unexpected properties for standard QD methods, raising new challenges for addressing manipulation tasks with QD.</font>
</p>

<br>


<font color="#b7b7b7"><i>Quality Diversity under Sparse Reward and Sparse Interaction: Application to Grasping in Robotics, Huber, J., Hélénon, F., Coninx, M., Ben Amar, F., Doncieux, S. (2023)</i></font>

<br>
<br>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/qd_grasp_franka_mug_scs_crop.gif" style="width:300px;"> 
	<img src="/assets/blog_posts/qd_adapt/qd_grasp_schunk_mug_2_scs_crop.gif" style="width:300px;"> 
</div>


<br>

<div align="center">
	<a href="https://arxiv.org/abs/2308.05483" target="_blank"><b>Paper</b></a> &bull; <a href="https://qdgrasp.github.io/qdgrasp/generating_the_data/"><b>Blog post</b></a>
</div>


<br>
<br>
<br>





##### Computing labels for sim2real transfer

<br>

<p align="justify">
<font style="font-size:1.3rem;font-family:'Georgia',serif;">Generating thousands of diverse grasping trajectories in simulation does not guarantee their exploitability in the real world. The differences between simulation and reality can make a high-performing solution in simulation inefficient in reality (<i>reality gap</i>). This paper aims to bridge the reality gap for automatically generated grasps by identifying quality criteria computed in simulation that are correlated with the sim2real transferability. Domain Randomization can be leveraged for computing such quality criteria, allowing the selection of the grasps with a high probability of successful transfer. One can also rely on these criteria to make the generated solution even more robust to sim2real transfer or to identify the simulation weaknesses that have a critical impact on the reality gap.</font>
</p>

<br>

<font color="#b7b7b7"><i>Domain Randomization for Sim2real Transfer of Automatically Generated Grasping Datasets, Huber, J., Hélénon, F., Watrelot, H., Ben Amar, F., Doncieux, S. (2023)</i></font>

<br>
<br>


<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_qual/deployements/panda_bowl_1_crop.gif" style="width:100px;">
	<img src="/assets/blog_posts/qd_qual/deployements/bx_straw_1_crop.gif" style="width:100px;">
	<img src="/assets/blog_posts/qd_qual/deployements/schunk_mug_1.gif" style="width:320px;">
</div>

<br>

<div align="center">
	<a href="https://arxiv.org/abs/2310.04517" target="_blank"><b>Paper</b></a> &bull; <a href="https://qdgrasp.github.io/qdgrasp/sim2real_labelling/"><b>Blog post</b></a>
</div>

<br>
<br>
<br>



##### Adapting reach-and-grasp trajectories to the whole operational space



<br>

<p align="justify">
<font style="font-size:1.3rem;font-family:'Georgia',serif;">The targeted object's state is fixed during the generation of the data in simulation. However, the reach-and-grasps trajectories can easily be adapted to any object pose in the operational space by applying simple rigid transformations. Such an adaptation requires being able to accurately get the 6-DoF position of a target object in reality, which is a very challenging task. This problem has been addressed by designing an integration pipeline that exploits several state-of-the-art image processing techniques, including an open-vocabulary object detector, a 6-DoF object pose detector, and an object pose tracker.</font>
</p>


<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_adapt/mug_tracking_simu_x4speedup.gif" style="width:450px;"> 
</div>

<br>

<font color="#b7b7b7"><i>Learning to grasp: From Somewhere to Anywhere, Hélénon, F., Huber, J., Ben Amar, F., Doncieux, S. (2023)</i></font>


<br>

<div align="center">
	<a href="https://arxiv.org/abs/2310.04349" target="_blank"><b>Paper</b></a> &bull; <a href="https://qdgrasp.github.io/qdgrasp/object_pose_adaptation/"><b>Blog post</b></a>
</div>




<br>
<br>
<br>
<br>
<br>
<br>

