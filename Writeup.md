## RoboND-Perception-Project

### Overview
  * Exercise 1 RANSAC
  * Exercise 2 Segmentation
  * Exercise 3 Classifier
  * Pick and Place Implementation
  
### Exercise 1, RANSAC:
 Developed filtering techniques to generate point clouds of invidual objects.
  * [Voxel Grid Filter](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise1/Pictures/001_ex1_voxel.png)
  * [Passthrough Filter](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise1/Pictures/002_ex1_pass_through_filtered.png)
  * [Extracted Inliers](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise1/Pictures/003_ex1_extracted_indices.png)
  * [Extracted Outliers](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise1/Pictures/005_extracted_outliers.png)
  * [Cloud Filtered](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise1/Pictures/004_cloud_filtered.png)
  * [Link to Code](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise1/RANSAC.py)

### Exercise 2, Segmentation:
 Developed segmentation routine to seperate objects from enviroment. Will be use later to assist with classificaiton.
  * [Point Cloud](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise2/pictures/001_ex2_pcl.png)
  * [PCL Clustering](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise2/pictures/002_ex2_pcl_cluster.png)
  * [PCL Objects](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise2/pictures/003_ex2_pcl_objects.png)
  * [PCL Table](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise2/pictures/004_ex2_pcl_table.png)
  * [Link to Code](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise2/segmentation.py)
  
### Exercise 3, Classifier
 Utilized sensor stick to train and develop SVM model for classifying objects
  * [Sensor Stick Pic](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise3/pictures/001_ex3_sensor_stick.png)
  * [Object Recognition Pic](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise3/pictures/002_ex3_object_recognition.png)
  * [Confusion Matrix](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise3/pictures/004_ex3_conf_matrix_1.png)
  * [Training_Set](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise3/training_set.sav)
  * [Model](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise3/model.sav)
  * [All Code](https://github.com/ejbkdb/RoboND-Perception-Project/tree/master/Exercise3)

### Pick and Place Implementation
 Implemented process chain developed in exercises 1-3. Utilized sensor stick training to train a model 
 using the objects that would be present in the test worlds. See below for confusion matrix, classifier results 
 from each world, and the .yaml outputs.
  * [Confusion Matrix](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/PnP_project/Pictures/001_world3_conf_matrix_1.png)
  * [Pic World 1](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/PnP_project/Pictures/001_world1_object_detection.png)
  * [Pic World 2](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/PnP_project/Pictures/002_world2_objectdetection.png)
  * [Pic World 3](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/PnP_project/Pictures/003_world3_object_detection.png)
  * [YAML_1](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/PnP_project/YAML/output1.yaml)
  * [YAML_2](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/PnP_project/YAML/output2.yaml)
  * [YAML_3](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/PnP_project/YAML/output3.yaml)
  * [Code](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/PnP_project/project.py)
  * [model](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/PnP_project/model.sav)
