## RoboND-Perception-Project

### Overview
  * Exercise 1 RANSAC
  * Exercise 2 Segmentation
  * Exercise 3 Classifier
  * Pick and Place Implementation
  
### Exercise 1, RANSAC:
 Developed filtering techniques to generate point clouds of invidual objects.
  * [Voxel Grid Filter](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise1/Pictures/001_ex1_voxel.png)
  
    Voxel grid filtering is used to downsample the point cloud, while still leaving enough points to clearly identify the cloud. This reduction in cloud size reduces the computation complexity of future tasks.
    
    I utilized a leaf size of 0.01. The results can be seen in the linked image above.
    ```python
          # Voxel Grid filter
      # Create a VoxelGrid filter object for our input point cloud
      vox = cloud.make_voxel_grid_filter()

      # Choose a voxel (also known as leaf) size
      # Note: this (1) is a poor choice of leaf size
      # Experiment and find the appropriate size!
      LEAF_SIZE = .01

      # Set the voxel (or leaf) size
      vox.set_leaf_size(LEAF_SIZE, LEAF_SIZE, LEAF_SIZE)

      # Call the filter function to obtain the resultant downsampled point cloud
      cloud_filtered = vox.filter()

  * [Passthrough Filter](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise1/Pictures/002_ex1_pass_through_filtered.png)
  
    Pass Through filter is used to remove un-need parts of the image. It's used to 'focus' the image only on the items that are deemed important
    to future steps. In my filter I removed the lower portion of the table. The results can be seen in the linked image above. 
    
    ```python
  
      # Call the filter function to obtain the resultant downsampled point cloud
      cloud_filtered = vox.filter()
      filename = 'voxel_downsampled.pcd'
      pcl.save(cloud_filtered, filename)

      # PassThrough filter
      # Create a PassThrough filter object.
      passthrough = cloud_filtered.make_passthrough_filter()

      # Assign axis and range to the passthrough filter object.
      filter_axis = 'z'
      passthrough.set_filter_field_name(filter_axis)
      axis_min = 0.6
      axis_max = 1.1
      passthrough.set_filter_limits(axis_min, axis_max)

      # Finally use the filter function to obtain the resultant point cloud.
      cloud_filtered = passthrough.filter()
        
  * [Extracted Inliers](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise1/Pictures/003_ex1_extracted_indices.png)
  * [Extracted Outliers](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise1/Pictures/005_extracted_outliers.png)
   
   The next step is to seperate the table from the objects using RANSAc segmentation. To do so I utilized SACMODEL_PLANE to help with 
   the task of seperating the objects from the table. Extracted inliers are the items on top of the table, and the extracted outliers 
   is the seperated table. I assigned my distance threshold value to .01.
   
   ```python
           # RANSAC plane segmentation
        # Create the segmentation object
        seg = cloud_filtered.make_segmenter()

        # Set the model you wish to fit
        seg.set_model_type(pcl.SACMODEL_PLANE)
        seg.set_method_type(pcl.SAC_RANSAC)

        # Max distance for a point to be considered fitting the model
        # Experiment with different values for max_distance
        # for segmenting the table
        max_distance = .01

        seg.set_distance_threshold(max_distance)

        # Call the segment function to obtain set of inlier indices and model coefficients
        inliers, coefficients = seg.segment()

        # Extract inliers
        extracted_inliers = cloud_filtered.extract(inliers, negative=True)
        filename = 'extracted_inliers.pcd'
        pcl.save(extracted_inliers, filename)

        # Save pcd for table
        # pcl.save(cloud, filename)
        # Much like the previous filters, we start by creating a filter object:
        outlier_filter = cloud_filtered.make_statistical_outlier_filter()

        # Set the number of neighboring points to analyze for any given point
        outlier_filter.set_mean_k(50)

        # Set threshold scale factor
        x = 1.0

        # Any point with a mean distance larger than global (mean distance+x*std_dev) will be considered outlier
        outlier_filter.set_std_dev_mul_thresh(x)

        # Finally call the filter function for magic
        cloud_filtered = outlier_filter.filter()
        filename = 'cloud_filtered.pcd'
        pcl.save(cloud_filtered,filename)


        # Extract outliers
        extracted_outliers = cloud_filtered.extract(inliers, negative=True)
   ```
   
   
  * [Cloud Filtered](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise1/Pictures/004_cloud_filtered.png)
  * [Link to Code](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise1/RANSAC.py)

### Exercise 2, Segmentation:
 Developed segmentation routine to seperate objects from enviroment. Will be use later to assist with classificaiton.
  * [Point Cloud](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise2/pictures/001_ex2_pcl.png)
  
    Cloud object I started with. Utilized code from exercise 1 to generate point cloud of relevant objects
  * [Euclidean Clustering](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise2/pictures/002_ex2_pcl_cluster.png)
    
    After seperating the objects from the table, I now needed to distinguish the different objects from each other. To do so I used
    Euclidean Cluster Extraction function.
    
    ```python
        white_cloud = XYZRGB_to_XYZ(cloud_objects)
    tree = white_cloud.make_kdtree()

    ec = white_cloud.make_EuclideanClusterExtraction()
    ec.set_ClusterTolerance(LEAF_SIZE*2)
    ec.set_MinClusterSize(10)
    ec.set_MaxClusterSize(2500)
    ec.set_SearchMethod(tree)
    cluster_indices = ec.Extract()
    ```
  * [PCL Objects](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise2/pictures/003_ex2_pcl_objects.png)
  * [PCL Table](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise2/pictures/004_ex2_pcl_table.png)
  
    After extracting the objects using Euclidean clustering. We need to create a new color for the object. See above links for pictures of 
    colored clustered objects.
  
      ```python
          # TODO: Create Cluster-Mask Point Cloud to visualize each cluster separately
    cluster_color = get_color_list(len(cluster_indices))
    color_cluster_point_list = []
    for j, indices in enumerate(cluster_indices):
        for i, indice in enumerate(indices):
            color_cluster_point_list.append([white_cloud[indice][0],
                                             white_cloud[indice][1],
                                             white_cloud[indice][2],
                                             rgb_to_float(cluster_color[j])])
    cluster_cloud = pcl.PointCloud_PointXYZRGB()
    cluster_cloud.from_list(color_cluster_point_list)
    ```
  
  * [Link to Code](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise2/segmentation.py)
  
  
### Exercise 3, Classifier
 Utilized sensor stick to train and develop SVM model for classifying objects.
 
     While utilizing the sensor stick each model was placed infront of the stick and rotated to several points to create a fully developed
    model of the object. Capture_Features.py was utilized for this task. After all models had their features captured, Train_SVM.py was utilized
    to create the model and caluclate the confusion matrix. After the model was trained it was time to test the model to see how it performed.
    
  * [Sensor Stick Pic](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise3/pictures/001_ex3_sensor_stick.png)
  
    Object being placed infront of sensor stick and rotated.  
  * [Object Recognition Pic](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise3/pictures/002_ex3_object_recognition.png)
    Classification results from after training
    
  * [Confusion Matrix](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise3/pictures/004_ex3_conf_matrix_1.png)
    
    Confusion Matrix output
  * [Training_Set](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise3/training_set.sav)
    
    Training set developed using capture features
  * [Model](https://github.com/ejbkdb/RoboND-Perception-Project/blob/master/Exercise3/model.sav)
    
    Model developed using Train_SVM
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
