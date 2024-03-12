---
---


<br>
<br>
<br>
<br>
<br>
<br>

<div align="center">
	<h1>Automatically Generating 6DoF Grasps with QD</h1>
</div>

<br>
<br>

<div align="center">
	<font color="#b7b7b7" style="font-size:1.5rem"><b>Speeding up 6-DoF Grasp Sampling with Quality-Diversity</b></font>
</div>

<div align="center">
	<font color="#b7b7b7" style="font-size:1.35rem">Johann Huber, François Hélénon, Mathilde Kappel, Elie Chelly, Mahdi Khoramshahi, Faïz Ben Amar and Stéphane Doncieux (2024)</font>
</div>

<br>
<br>


<br>
<br>
<br>
<br>
<br>
<br>


### The need for 6DoF grasp datasets 

<br>

<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
An increasing number of works exploit automatically generated grasping datasets to train generalizing grasping policies. Those works involve the usage of diffusion models (Urain et al., 2023), Variational Autoencoders (Barad et al. 2023), or neural representations (Chen et al., 2024).
</font>
</p>

<br>
<br>

<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
The most commonly used grasping dataset of the past few years is ACRONYM (Eppner et al., 2021). It consists of a large dataset of diverse 2-finger gripper poses, provided as 6D vectors. The diversity of the proposed dataset aims to provide both robust and fragile examples to allow efficient supervised learning.
</font>
</p>


<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_6dof/acronym_screen.png" style="width:350px;">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7">ACRONYM (Eppner et al., 2021).</font>
</div>

<br>
<br>

<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
However, ACRONYM grasps have been generated with basing sampling schemes (Eppner et al., 2019). A search space is designed such that each element of that space can be projected into the SE(3) space of 6D grasp poses. The search space is randomly sampled: what makes this framework efficient is the usage of priors to generate successful grasps more easily.
</font>
</p>


<br>
<br>

<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
The recent works on <b>Quality-Diversity (QD) algorithms</b> for the generation of reach-and-grasps trajectories (Huber et al., 2023) suggest that those methods <b>could be applied to 6DoF grasp generation</b> too. As QD is meant to efficiently explore an outcome space to produce diverse and high-performing solutions, those methods are <b>likely to be more efficient than the prior-based random search</b> used in the literature. Exploring this idea is the purpose of this paper.
</font>
</p>


<br>
<br>
<br>
<br>
<br>
<br>

### Proposed framework

<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
Instead of using the standard random search, the proposed framework relies on a population-based optimization method to evaluate an evolving set of grasp poses.
</font>
</p>

<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_6dof/screens_overview_qdg6dof/qdg6dof_overview_video_step_2.png" style="width:300px;">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7"><b>Population-based exploration of a search space:</b> Elements from the search space are projected in the space of grasps poses with respect to standard priors (Eppner et al., 2019).</font>
</div>

<br>
<br>


<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
Evaluating each grasp pose in simulation results in a quality, as well as a behavior vector that describes the nature of the grasp in low dimension. A structured archive of solutions is optimized by <b>keeping in memory the best-performing grasp for each behavior</b>.
</font>
</p>


<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_6dof/screens_overview_qdg6dof/qdg6dof_overview_video_step_4.png" style="width:380px;">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7"><b>Optimization of a structured archive:</b>This archive acts as a memory that contains the best-performing solutions for each found behavior.</font>
</div>


<br>
<br>
 
<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
A mutation-selection process drives the optimization process. The output is an <b>outcome archive of diverse and high-performing solutions</b> (Huber et al., 2023).
</font>
</p>


<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_6dof/screens_overview_qdg6dof/qdg6dof_overview_video_step_7.png" style="width:460px;">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7"><b>QD-Grasp-6DoF (QDG-6DoF) framework overview:</b> By combining priors and QD optimization, this framework allows to generate large datasets of diverse and high-performing grasp poses.</font>
</div>


<br>

<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
The same framework can easily be applied to different objects and grippers (see the paper for details): what is needed is a 3D model of the gripper considered and a collision mesh of the targeted objects.
</font>
</p>


<br>
<br>
<br>
<br>
<br>
<br>


### QDG-6DoF vs standard methods


<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
An experiment was conducted in the Pybullet simulator to evaluate the proposed framework. The coverage of the set of possible successful grasps has been measured throughout the search using different methods.
</font>
</p>


<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_6dof/screen_results/qdg_6dof_exp1.png" style="width:600px;">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7"><b>QDG-6DoF vs standard methods:</b>The proposed QD methods outperform the prior-based sampling schemes by a large margin.</font>
</div>


<br>
<br>

<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
Regardless of the used gripper, the proposed <b>QD variant with contact-based prior</b> (<i>contact_ME_scs</i>) <b>outperforms all the considered grasp sampling schemes</b> (Eppner et al., 2019). Moreover, <b>the exploitation of the contact prior increases the performances of the QD state-of-the-art method</b> – compared to its raw version, which directly encodes the 6DoF position in the genotype (<i>ME-scs</i>). It is worth noting that <i>ME-scs</i> exploit limited priors: the search space is constrained such that the gripper is close to the object surface. But the standard priors – aligning normal to the gripper plan with the normal at the targeted point on the object surface or antipodal grasps – are not exploited here.
</font>
</p>


<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_6dof/screen_results/qdg_6dof_exp2.png" style="width:600px;">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7"><b>QDG-6DoF variants:</b> ME-scs variants are the best-performing QD methods (Huber et al., 2023). Antipodal quickly plateaus. Contact- and approach-based variants cannot be distinguished using the same QD method.</font>
</div>

<br>
<br>

<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
The best-performing method among the QD state-of-the-art ones is <i>ME-scs</i>, with and without the usage of priors. It extends the results from reach-and-grasp trajectories to the 6DoF paradigm (Huber et al., 2023). However, the antipodal-based variant of <i>ME-scs</i> quickly plateaus. It shows that this prior is too conservative to generate a large diversity of grasps. A last salient observation is that contact- and approach-based variants cannot be distinguished using the same QD method. Contact variants have been implemented as a baseline that prevents the gripper from being too far from the object's surface. In practice, it is similar to the approach-based variant without constraints on the normal to-the-palm alignment.
</font>
</p>


<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_6dof/screen_results/qdg_6dof_exp2_2.png" style="width:600px;">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7"><b>Distribution of angles between the normal to the gripper plan and the normal of the object surface at the targeted contact point for contact-based QD variants:</b> The found grasps mostly validate the approach criteria.</font>
</div>

<br>
<br>

<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
Plotting the distribution of angles between the normal to the gripper plan and the normal of the object surface at the targeted contact point for contact-based QD variants shows that most of <b>the grasps validates the approach criteria</b> (i.e. <img src="https://latex.codecogs.com/svg.image?\large&space;\nu\in [0, \pi/4]"/>. By focusing on the most promising part of the search space to produce diverse and successful grasps, <b>QD methods automatically find the approach prior</b>.
</font>
</p>





<br>
<br>
<br>
<br>
<br>
<br>


### Generated repertoires

<p align="justify">
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
The same framework can easily be applied to different objects and grippers. Here are some examples for the Panda 2-fingers gripper, the Barrett 3-fingers gripper, the Allegro 4-fingers hand, and the Shadow 5-fingers hand: 
</font>
</p>

<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_6dof/panda_pd_2.gif" ><img src="/assets/blog_posts/qd_6dof/panda_cracker.gif" >
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_6dof/barrett_banana.gif" ><img src="/assets/blog_posts/qd_6dof/barrett_spatula.gif" >
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_6dof/allegro_chips.gif" ><img src="/assets/blog_posts/qd_6dof/allegro_cube.gif" >
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_6dof/shadow_spatula.gif" ><img src="/assets/blog_posts/qd_6dof/shadow_tennis.gif" >
</div>

<br>

<p align="justify">
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
The exploration capabilities of QD methods allow the production of both robust and fragile grasps. Robustness can also be increased with quality criteria dedicated to simulation-to-reality transfer (Huber et al., 2024).
</font>
</p>

<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_6dof/allegro_cube_fragile.gif" ><img src="/assets/blog_posts/qd_6dof/allegro_cube_robust.gif" >
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7">(Left) a fragile grasp; (Right) a robust one generated with Domain-Randomization-based fitness (Huber et al., 2024).</font>
</div>


<br>
<br>
<br>
<br>
<br>
<br>


### Experiments in the physical world

<p align="justify">
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
To estimate the exploitability of the generated grasps, some of the found solutions have been deployed on a Panda 2-fingers gripper and on an Allegro hand.
</font>
</p>

<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_6dof/réelles/exp_réelles_panda_bowl_1_low_qual.gif"><img src="/assets/blog_posts/qd_6dof/réelles/exp_réelles_panda_mug_1_low_qual.gif"><img src="/assets/blog_posts/qd_6dof/réelles/exp_réelles_panda_pd_1_low_qual.gif">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7">Examples of deployed grasps on the Franka Emila Panda gripper.</font>
</div>

<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_6dof/screen_results/qdg6dof_table_res.png" style="width:500px;">
</div>

<br>

<p align="justify">
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
All the trajectories were successfully deployed on the mug, bowl, and bleach cleanser. The reported failures happened on the power drill. We attribute those failures to a misalignment between the forces applied on the object's surface in simulation (which can be arbitrarily high) and in the real world. Synergies involving grasps with 2 fingers struggled to lift the heavy power drill.
</font>
</p>
  

<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_6dof/réelles/exp_réelles_panda_pd_failure_1_low_qual.gif"><img src="/assets/blog_posts/qd_6dof/réelles/exp_réelles_allegro_pd_failure_1_low_qual.gif">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7"><b>Limitations.</b> Failure in sim-to-real transfer can occur due to too fragile grasp (left) or overestimation of the finger strengths in simulation compared to reality (right).</font>
</div>

<br>

<p align="justify">
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
Nevertheless, the diversity of grasps found by QD methods maintains a high sim-to-real transferability ratio – even for a multi-fingered hand.</font>
</p>

<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_6dof/réelles/exp_réelles_allegro_mug_1_low_qual.gif"><img src="/assets/blog_posts/qd_6dof/réelles/exp_réelles_allegro_mug_2_low_qual.gif"><img src="/assets/blog_posts/qd_6dof/réelles/exp_réelles_allegro_mug_3_low_qual.gif">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7">Diversity of grasps found on the Allegro hand for the mug.</font>
</div>

<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qd_6dof/réelles/exp_réelles_allegro_pd_1_low_qual.gif"><img src="/assets/blog_posts/qd_6dof/réelles/exp_réelles_allegro_pd_2_low_qual.gif">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7">Diversity of grasps found on the Allegro hand for the power drill.</font>
</div>

<br>
<br>
<br>
<br>


### Conclusions

<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
This work proposes a <b>QD framework for generating large datasets of diverse and high-performing 6DoF grasps</b>. This framework can easily be adapted to several grippers, including dexterous hands. The conducted experiments showed that by combining QD with robotic priors, the proposed framework <b>speeds up the generation of diverse grasps in simulation, compared to state-of-the-art methods</b>. Almost all of the deployed grasps <b>successfully transferred to real robots</b>. We believe this work to be a significant step toward the generation of large datasets for learning generalizing grasping policies.
</font>
</p>

<br>
<br>

<p align="justify">
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
Results can easily be reproduced with the <a href="https://gitlab.isir.upmc.fr/l2g/qd_grasp_6dof">publicly available code</a>!
</font>
</p>

<br>
<br>
<br>
<br>


### Acknowledgement

<p align="justify">
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
This work was supported by the Sorbonne Center for Artificial Intelligence, the German Ministry of Education and Research (BMBF) (01IS21080), and the French Agence Nationale de
la Recherche (ANR) (ANR-21-FAI1-0004) - Learn2Grasp. It has received funding from the European Commission’s Horizon Europe Framework Programme under grant agreement No
101070381 and from the European Union’s Horizon Europe Framework Programme under grant agreement No 101070596. This work was performed using HPC resources from GENCI-IDRIS
(Grant 20XX-AD011014320).
</font>
</p>


<br>
<br>
<br>
<br>


### References


<i>Urain, J., Funk, N., Peters, J., \& Chalvatzaki, G. (2023, May). Se (3)-diffusionfields: Learning smooth cost functions for joint grasp and motion optimization through diffusion. ICRA 2023.</i>

<br>

<i>Barad, K. R., Orsula, A., Richard, A., Dentler, J., Olivares-Mendez, M., \& Martinez, C. (2023). GraspLDM: Generative 6-DoF Grasp Synthesis using Latent Diffusion Models. arXiv preprint.</i>

<br>

<i>Chen, H., Xu, B., \& Leutenegger, S. (2024). FuncGrasp: Learning Object-Centric Neural Grasp Functions from Single Annotated Example Object. arXiv preprint.</i>

<br>

<i>Eppner, C., Mousavian, A., Fox, D. (2021, May). Acronym: A large-scale grasp dataset based on simulation. ICRA 2021.</i>

<br>

<i>Eppner, C., Mousavian, A., \& Fox, D. (2019, October). A billion ways to grasp: An evaluation of grasp sampling schemes on a dense, physics-based grasp data set. ISRR 2019.</i>

<br>

<i>Huber, J., Hélénon, F., Coninx, M., Ben Amar, F., Doncieux, S. (2023). Quality Diversity under Sparse Reward and Sparse Interaction: Application to Grasping in Robotics. arXiv preprint.</i>

<br>

<i>Huber, J., Hélénon, F., Watrelot, H., Amar, F. B., \& Doncieux, S. (2024). Domain Randomization for Sim2real Transfer of Automatically Generated Grasping Datasets. ICRA 2024.</i>


<br>
<br>
<br>











