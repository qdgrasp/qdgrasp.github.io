---
---

<!-- LOAD ModelViewer libraries for 3D display and opening of .glb file compressed with draco -->
<script src="https://www.gstatic.com/draco/versioned/decoders/1.5.7/draco_wasm_wrapper.js">
</script>
<script>
self.ModelViewerElement = self.ModelViewerElement || {};
self.ModelViewerElement.dracoDecoderLocation = "https://www.gstatic.com/draco/versioned/decoders/";
</script>
<script type="module"  src="/assets/scripts/model-viewer.min.js"></script>


<br>
<br>
<br>
<br>
<br>
<br>

<div align="center">
	<h1>QDGset: A Large Scale Grasping Dataset Generated with QD</h1>
</div>

<br>
<br>

<div align="center">
	<font color="#b7b7b7" style="font-size:1.5rem"><b>QDGset: A Large Scale Grasping Dataset Generated with Quality-Diversity</b></font>
</div>

<div align="center">
	<font color="#b7b7b7" style="font-size:1.35rem">Johann Huber, François Hélénon, Mathilde Kappel, Ignacio de Loyola Páez-Ubieta, Santiago T. Puente, Pablo Gil, Faïz Ben Amar and Stéphane Doncieux (2024)</font>
</div>

<br>
<br>



<br>
<br>
<br>
<br>
<br>
<br>


### Augmenting grasping datasets

<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
Recent studies are increasingly leveraging automatically generated grasping datasets to train more generalizable grasping policies. These approaches utilize advanced techniques such as diffusion models (Urain et al., 2023), Variational Autoencoders (Barad et al., 2023), and neural representations (Chen et al., 2024), paving the way for more robust robotic grasping systems.
</font>
</p>


<br>
<br>

<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
The most commonly used dataset is ACRONYM, a large set of object-centric 2-finger grasp poses (Eppner et al., 2021). ACRONYM has been generated with basic sampling schemes, which <a href="{{site.url}}/generating_grasp_poses/">were proven significantly less sample efficient than a quality-diversity-based approach</a>. The corresponding framework, QDG-6DoF, combines Quality-Diversity optimisation (Cully et al., 2022) with robotic priors to accelerate the discovery of diverse and robust grasps. 
</font>
</p>

<br>
<br>

<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
Some works on grasping datasets rely on data augmentation to increase the amount of high-quality data. But most of these methods concern images (Mahler et al., 2017)(Van Molle et al., 2018). In this work, <b>we argue that object-centric grasping data can also be augmented</b>. To do so, we proposed to create new object 3D models of objects out of known ones and to leverage previously found grasps to transfer them to the produced objects.
</font>
</p>



<br>
<br>
<br>
<br>
<br>
<br>


### Proposed approach

<br>
<br>

<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qdg_set/qdgset_aug_principle.png" style="width:550px;">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7"><b>Augmentation framework overview.</b></font>
</div>

<br>
<br>

<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
Grasps are first generated on a given object with QDG-6DoF (Huber et al., 2024). The object's 3D model is modified by applying randomly sampled linear perturbation along the Cartesian axes. Grasp poses on the resulting objects are then produced by initializing the QDG-6DoF optimization with the success archive of the reference object.
</font>
</p>

<br>
<br>

<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
This framework relies on the assumption that a set of realistic 3D models of objects is available. We gathered 3D models coming from KIT (Kasper et al., 2012), 3DNet (Wohlkinger et al., 2012), YCB (Calli et al., 2015), GraspNet-1Billion (Fang et al., 2020), and ShapeNet (Chang et al., 2015) datasets. EGAD (Morrison et al., 2020) is also included, even if it contains unrealistic objects. These models have been generated to explore grasping capabilities on adversarial objects—this is why it is included, too. 
</font>
</p>

<br>
<br>


<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qdg_set/vis_aug_obj.png" style="width:620px;">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7"><b>Example of augmented objects.</b> In red are the reference objects (KIT n°5, n°22, n°111) and in grey a subsample of 25 resulting augmentations.</font>
</div>


<br>
<br>

<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
The follow-up assumption is that applying linear perturbations on the geometrical axes of a realistic object resultsresults in a new, still realistic object. The above image shows three examples that support that view.
</font>
</p>


<br>
<br>



<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qdg_set/qdgset_vis_bootstrap_heatmap.png" style="width:620px;">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7"><b>Transferred grasping archive.</b> (Left) Example of a core object and the associated generated grasps (left) for KIT n°111, and the resulting grasping archives after transferring it on 3 augmented objects (right). Each dot corresponds to the end effector position for a given grasp pose. The color describes the quality, the hotter the better.</font>
</div>


<br>
<br>

<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
These new objects are, however, relatively close to the reference model from which they were generated. Thus, grasp poses that are successful on the reference object are likely to be successful on the new one. The success archives of a QDG-6DoF optimization should therefore be a good starting point for a new optimization on an augmented object.
</font>
</p>


<br>
<br>
<br>
<br>
<br>
<br>


### Experimental results


<br>
<br>


<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qdg_set/qdgset_res_diff_archive_size.png" style="width:450px;">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7"><b>Effect of the bootstrap on the generation of grasps.</b> Are distinguished the objects for which the bootstrap result to a better sample efficiency (<i>Better</i>) and those for which it has a null effect (<i>Constant</i>).</font>
</div>

<br>
<br>

<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
In an experimental study, 76% over the 431 runs lead to more robust grasps when stopping after the bootstrapping step. However, the bootstrap can have a detrimental effect on the long run for some objects. The above plot presents the evolution of the number of successful grasps found in runs of QDG-6DoF with and without bootstrap. These results show that the bootstrapping approach has a positive - or at least null - effect on the sample efficiency of the optimization process. However, using a previously found archive as input for initializing has a null, or at worse, negative impact on the sample efficiency. Consequently, the proposed approach is efficient in speeding up QD optimization if the process is stopped right after the bootstrapping step.
</font>
</p>





<br>
<br>


<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qdg_set/qdgset_res_transferability_fit.png" style="width:620px;">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7"><b>Transferability.</b> (Left) Transfer rate from core object to augmented one - successful if at least 500 grasps are transferred; (center) grasp quality distribution on KIT-* data; (right) same on 3DNet-* data.</font>
</div>


<br>
<br>


<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
For 78% of the 1304 considered reference objects, 10 out of 10 transfers lead to at least 500 successful grasps. Moreover, the distributions of grasp qualities are similar for the reference grasp sets and the augmented ones. This shows that the proposed bootstrapping method leads to successful transfer from core objects to augmented ones while maintaining the same quality distribution.
</font>
</p>

<br>
<br>

<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
In an experimental study, 76% over the 431 runs lead to more robust grasps when stopping after the bootstrapping step. However, the bootstrap can have a detrimental effect on the long run for some objects. The above plot presents the evolution of the number of successful grasps found in runs of QDG-6DoF with and without bootstrap. These results show that the bootstrapping approach has a positive - or at least null - effect on the sample efficiency of the optimization process. However, using a previously found archive as input for initializing has a null, or at worse, negative impact on the sample efficiency. Consequently, the proposed approach efficiently speeds up QD optimization if the process is stopped right after the bootstrapping step. 
</font>
</p>



<br>
<br>
<br>
<br>
<br>
<br>


### QDGset



<br>
<br>



<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
Building on these results, we have created QDGset, a large-scale dataset of 62.000.000 object-centric grasp poses for about 40.000 objects. Each grasp is labeled with a probability to transfer in the real world, using the domain-randomization-based criterion <a href="{{site.url}}/sim2real_labelling/">introduced in a previous work</a>.
</font>
</p>


<br>
<br>


<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qdg_set/qdgset_pie_charts.png" style="width:720px;">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7"><b>QDGset objects.</b> (Left) Number of objects per dataset; (center) ratio of augmented objects; (right) object categories.</font>
</div>


<br>
<br>


<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
QDGset contains objects from a wide range of object datasets—including primitive, daily, complex, and adversarial objects. Some of them are widely used for benchmarking in the physical world, like the YCB set (Calli et al., 2015), or can be 3d printed (Morrison et al., 2020). Only 27% of the current set comes from augmentations: the size of the dataset can thus easily be scaled by orders of magnitude.
</font>
</p>


<br>
<br>


<div align="center" style="vertical-align:bottom ; text-align:center">
	<img src="/assets/blog_posts/qdg_set/qdset_n_scs_per_objects.png" style="width:720px;">
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7"><b>Number of successful grasps per object. Most of the distributions lie between 1000 and 5000 grasps per objects. This reflects the complexity of the subset of objects.</b></font>
</div>


<br/>

<style>
.grid-container {
  display: grid;
  grid-template-columns: auto auto;
  <!-- background-color: #2196F3; -->
  padding: 10px;
}
.grid-item {
  background-color: rgba(255, 255, 255, 0.8);
  border: 1px solid rgba(0, 0, 0, 0.8);
  padding: 20px;
  font-size: 30px;
  text-align: center;
}
</style>
<p>
 <div class="grid-container">
  <div class="grid-item"> Power drill (ycb) 
  <model-viewer alt="Power drill object and associated grasps where color of the gripper varies depending on robustness. Dark blue represent the less robust grasps while light red represents the most robust" src="/assets/meshes/power_drill.glb" shadow-intensity="1" camera-controls touch-action="pan-y"></model-viewer></div>
  <div class="grid-item">Mug (ycb) 
  <model-viewer alt="Mug object and associated grasps where color of the gripper varies depending on robustness. Dark blue represent the less robust grasps while light red represents the most robust" src="/assets/meshes/mug.glb" shadow-intensity="1" camera-controls touch-action="pan-y"></model-viewer></div>
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7"><b>
Grasps subsample</b> for two ycb objects (dark blue represents grasps that are less robust while light red represents grasps that are more robust i.e more likely to transfer)</font>
</div>
</p>

<br/>

<p>
 <div class="grid-container">
  <div class="grid-item"> kit_obj_25 (kit) 
  <model-viewer alt="Kit object number 25 and associated grasps where color of the gripper varies depending on robustness. Dark blue represent the less robust grasps while light red represents the most robust" src="/assets/meshes/kit_obj_25.glb" shadow-intensity="1" camera-controls touch-action="pan-y"></model-viewer></div>
  <div class="grid-item">kit_obj_25_da_2 (kit_augmented) 
  <model-viewer alt="Kit object number 25, augmented, and associated bootstrapped grasps where color of the gripper varies depending on robustness. Dark blue represent the less robust grasps while light red represents the most robust" src="/assets/meshes/kit_obj_25_da_2.glb" shadow-intensity="1" camera-controls touch-action="pan-y"></model-viewer></div>
<div class="grid-item">kit_obj_25_da_5 (kit_augmented) 
  <model-viewer alt="Kit object number 25, augmented, and associated bootstrapped  grasps where color of the gripper varies depending on robustness. Dark blue represent the less robust grasps while light red represents the most robust" src="/assets/meshes/kit_obj_25_da_5.glb" shadow-intensity="1" camera-controls touch-action="pan-y"></model-viewer></div>
<div class="grid-item">kit_obj_25_da_10 (kit_augmented) 
  <model-viewer alt="Kit object number 25, augmented, and associated bootstrapped  grasps where color of the gripper varies depending on robustness. Dark blue represent the less robust grasps while light red represents the most robust" src="/assets/meshes/kit_obj_25_da_10.glb" shadow-intensity="1" camera-controls touch-action="pan-y"></model-viewer></div>
</div>
<div align="center" style="vertical-align:bottom ; text-align:center">
	<font color="#b7b7b7"><b>
Grasps and bootstrapped grasps subsample</b> for one kit objects and some of its augmentations (dark blue represents grasps that are less robust while light red represents grasps that are more robust i.e more likely to transfer)</font>
</div>
</p>



<br>
<br>

<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
Most of the objects yield from 1000 to 5000 grasp poses. The obtained distributions of number of success found per objects shows how diverse in complexity the objects are: many more grasps were found on the KIT objects than the EGAD. This allows us to fully explore the capability of a given gripper on objects depending on their geometry. Leveraging such a large dataset for learning grasping policies, as it has been done on ACRONYM, is a promising perspective of this work.
</font>
</p>

<br>
<br>


<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
The ease of grasp generation with QDG-6DoF allows anyone to extend QDGset with more grasp for the panda 2-finger gripper. As QDG-6DoF is versatile to the gripper type, QDGset can similarly be extended with other kinds of grippers using the same codebase. The current approach, however, is limited to exploring the interaction between the contact 3D models of a gripper and an object. It assumes that the object consists of a unique material, which is not always true in the real world.
</font>
</p>

<br>
<br>


<br>
<br>
<br>
<br>


### Conclusions


<p align="justify"> 
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
This work introduces QDGset, a large-scale parallel jaw grasp dataset provided as object-centric poses. It was produced by extending QDG-6DoF to data augmentation for grasping. This approach combines the creation of new simulated objects from reference ones with the transfer of previously found grasps. This method can reduce the required number of evaluations for finding robust grasps by up to 20% while maintaining the same quality distributions. The obtained results demonstrate how QD can be used to generate new data for a particular grasping scenario. We believe that such a tool makes possible the gathering of a large collaborative dataset of simulated grasps that can be successfully used in the real world.
</font>
</p>




<br>
<br>
<br>
<br>


### Acknowledgement

<p align="justify">
<font style="font-size:1.3rem;font-family:'Georgia',serif;">
This work was supported by the Sorbonne Center for Artificial Intelligence, the German Ministry of Education and Research (BMBF) (01IS21080), the French Agence Nationale de la Recherche (ANR) (ANR-21-FAI1-0004) (Learn2Grasp), the European Commission's Horizon Europe Framework Programme under grant No 101070381, by the European Union's Horizon Europe Framework Programme under grant agreement No 101070596, by Grant PID2021-122685OB-I00 funded by MICIU/AEI/10.13039/501100011033 and by the European Union NextGenerationEU/PRTR. This work used HPC resources from GENCI-IDRIS (Grant 20XX-AD011014320).
</font>
</p>


<br>
<br>
<br>
<br>


### References


<i>Urain, J., Funk, N., Peters, J., Chalvatzaki, G. (2023, May). Se (3)-diffusionfields: Learning smooth cost functions for joint grasp and motion optimization through diffusion. ICRA 2023.</i>

<br>

<i>Barad, K. R., Orsula, A., Richard, A., Dentler, J., Olivares-Mendez, M., Martinez, C. (2023). GraspLDM: Generative 6-DoF Grasp Synthesis using Latent Diffusion Models. arXiv preprint.</i>

<br>

<i>Chen, H., Xu, B., Leutenegger, S. (2024). FuncGrasp: Learning Object-Centric Neural Grasp Functions from Single Annotated Example Object. arXiv preprint.</i>

<br>

<i>Eppner, C., Mousavian, A., Fox, D. (2021, May). Acronym: A large-scale grasp dataset based on simulation. ICRA 2021.</i>


<br>

<i>Cully, A., Mouret, J-B., Doncieux, S. (2022). Quality-diversity optimisation. Proceedings of the Genetic and Evolutionary Computation Conference Companion.</i>


<br>

<i>Mahler, J., Liang, J., Niyaz, S., Laskey, M., Doan, R., Liu, X., Aparicio-Ojea, J., Goldberg, K. (2017). Dex-net 2.0: Deep learning to plan robust grasps with synthetic point clouds and analytic grasp metrics. In 2017 Robotics: Science and Systems (RSS).</i>


<br>

<i>Van Molle, P., Verbelen, T., De Coninck, E., De Boom, C., Simoens, P., & Dhoedt, B. (2018). Learning to grasp from a single demonstration.</i>


<br>

Kasper, A., Xue, Z. R, Dillmann, R. (2012). The kIT object models database: An object model database for object recognition, localization and manipulation in service robotics. The International Journal of Robotics Research (IJRR), 31(8), 927-934.

<br>

Wohlkinger, W. Aldoma, A., Rusu, R.B., Vincze, M. (2012). 3DNET: Large-scale object class recognition from cad models. In 2012 IEEE International Conference on Robotics and Automation (ICRA) (pp. 5384-5391). IEEE.

<br>

Calli, B., Walsman, A., Singh, A., Srinivasa, S., Abbeel, P., Dollar, A. M. (2015). Benchmarking in manipulation research: The ycb object and model set and benchmarking protocols. IEEE Robotics & Automation Magazine (RAM), 22(3), 36-52.

<br>

Fang, H. S., Wang, C., Gou, M., Lu, C. (2020). Graspnet-1billion: A large-scale benchmark for general object grasping. In 2020 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR) (pp. 11441-11450). IEEE.

<br>

Chang, A.X., Funkhouser, T., Guibas, L., Hanrahan, P., Huang, Q., Li, Z., Savarese, S., Savva, M., Song, S., Su, H., Xiao, J., Yi, L., Yu, F. (2015). ShapeNet: an information-rich 3D model repository.

<br>

Morrison, D., Corke, P. Leitner, J. (2020). Egad! an evolved grasping analysis dataset for diversity and reproducibility in robotic manipulation. IEEE Robotics and Automation Letters (RA-L), 8(3), 4368-4375.




<br>
<br>
<br>











