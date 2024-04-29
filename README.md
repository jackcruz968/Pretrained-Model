# Pretrained-Model

The project consists of two major parts and is a modified version of the 3DObjDet_SensFusion_Trackg
Link: https://github.com/Shruti-Bansal/3DObjDet_SensFusion_Trackg

Object detection: In this part, a deep-learning approach is used to detect vehicles in LiDAR data based on a birds-eye view perspective of the 3D point-cloud. Also, a series of performance measures is used to evaluate the performance of the detection approach.
Object tracking : In this part, an extended Kalman filter is used to track vehicles over time, based on the lidar detections fused with camera detections. Data association and track management are implemented as well.
The following diagram contains an outline of the data flow and of the individual steps that make up the algorithm.



Also, the project code contains various tasks, which are detailed step-by-step in the code. More information on the algorithm and on the tasks can be found in the Udacity classroom.

Project File Structure
📦project
┣ 📂dataset --> contains the Waymo Open Dataset sequences
┃
┣ 📂misc
┃ ┣ evaluation.py --> plot functions for tracking visualization and RMSE calculation
┃ ┣ helpers.py --> misc. helper functions, e.g. for loading / saving binary files
┃ ┗ objdet_tools.py --> object detection functions without student tasks
┃ ┗ params.py --> parameter file for the tracking part
┃
┣ 📂results --> binary files with pre-computed intermediate results
┃
┣ 📂student
┃ ┣ association.py --> data association logic for assigning measurements to tracks incl. student tasks
┃ ┣ filter.py --> extended Kalman filter implementation incl. student tasks
┃ ┣ measurements.py --> sensor and measurement classes for camera and lidar incl. student tasks
┃ ┣ objdet_detect.py --> model-based object detection incl. student tasks
┃ ┣ objdet_eval.py --> performance assessment for object detection incl. student tasks
┃ ┣ objdet_pcl.py --> point-cloud functions, e.g. for birds-eye view incl. student tasks
┃ ┗ trackmanagement.py --> track and track management classes incl. student tasks
┃
┣ 📂tools --> external tools
┃ ┣ 📂objdet_models --> models for object detection
┃ ┃ ┃
┃ ┃ ┣ 📂darknet
┃ ┃ ┃ ┣ 📂config
┃ ┃ ┃ ┣ 📂models --> darknet / yolo model class and tools
┃ ┃ ┃ ┣ 📂pretrained --> copy pre-trained model file here
┃ ┃ ┃ ┃ ┗ complex_yolov4_mse_loss.pth
┃ ┃ ┃ ┣ 📂utils --> various helper functions
┃ ┃ ┃
┃ ┃ ┗ 📂resnet
┃ ┃ ┃ ┣ 📂models --> fpn_resnet model class and tools
┃ ┃ ┃ ┣ 📂pretrained --> copy pre-trained model file here
┃ ┃ ┃ ┃ ┗ fpn_resnet_18_epoch_300.pth
┃ ┃ ┃ ┣ 📂utils --> various helper functions
┃ ┃ ┃
┃ ┗ 📂waymo_reader --> functions for light-weight loading of Waymo sequences
┃
┣ basic_loop.py
┣ loop_over_dataset.py

Installation Instructions for Running Locally
Cloning the Project
In order to create a local copy of the project, please click on "Code" and then "Download ZIP". Alternatively, you may of-course use GitHub Desktop or Git Bash for this purpose.

Python
The project has been written using Python 3.7. Please make sure that your local installation is equal or above this version.

Package Requirements
All dependencies required for the project have been listed in the file requirements.txt. You may either install them one-by-one using pip or you can use the following command to install them all at once: pip3 install -r requirements.txt

Waymo Open Dataset Reader
The Waymo Open Dataset Reader is a very convenient toolbox that allows you to access sequences from the Waymo Open Dataset without the need of installing all of the heavy-weight dependencies that come along with the official toolbox. The installation instructions can be found in tools/waymo_reader/README.md.

Waymo Open Dataset Files
This project makes use of three different sequences to illustrate the concepts of object detection and tracking. These are:

Sequence 1 : training_segment-1005081002024129653_5313_150_5333_150_with_camera_labels.tfrecord
Sequence 2 : training_segment-10072231702153043603_5725_000_5745_000_with_camera_labels.tfrecord
Sequence 3 : training_segment-10963653239323173269_1924_000_1944_000_with_camera_labels.tfrecord
To download these files, you will have to register with Waymo Open Dataset first: Open Dataset – Waymo, if you have not already, making sure to note "Udacity" as your institution.

Once you have done so, please click here to access the Google Cloud Container that holds all the sequences. Once you have been cleared for access by Waymo (which might take up to 48 hours), you can download the individual sequences.

The sequences listed above can be found in the folder "training". Please download them and put the tfrecord-files into the dataset folder of this project.

Pre-Trained Models
The object detection methods used in this project use pre-trained models which have been provided by the original authors. They can be downloaded here (darknet) and here (fpn_resnet). Once downloaded, please copy the model files into the paths /tools/objdet_models/darknet/pretrained and /tools/objdet_models/fpn_resnet/pretrained respectively.

Using Pre-Computed Results
In the main file loop_over_dataset.py, you can choose which steps of the algorithm should be executed. If you want to call a specific function, you simply need to add the corresponding string literal to one of the following lists:

exec_data : controls the execution of steps related to sensor data.

pcl_from_rangeimage transforms the Waymo Open Data range image into a 3D point-cloud
load_image returns the image of the front camera
exec_detection : controls which steps of model-based 3D object detection are performed

bev_from_pcl transforms the point-cloud into a fixed-size birds-eye view perspective
detect_objects executes the actual detection and returns a set of objects (only vehicles)
validate_object_labels decides which ground-truth labels should be considered (e.g. based on difficulty or visibility)
measure_detection_performance contains methods to evaluate detection performance for a single frame
In case you do not include a specific step into the list, pre-computed binary files will be loaded instead. This enables you to run the algorithm and look at the results even without having implemented anything yet. The pre-computed results for the mid-term project need to be loaded using this link. Please use the folder darknet first. Unzip the file within and put its content into the folder results.

exec_tracking : controls the execution of the object tracking algorithm

exec_visualization : controls the visualization of results

show_range_image displays two LiDAR range image channels (range and intensity)
show_labels_in_image projects ground-truth boxes into the front camera image
show_objects_and_labels_in_bev projects detected objects and label boxes into the birds-eye view
show_objects_in_bev_labels_in_camera displays a stacked view with labels inside the camera image on top and the birds-eye view with detected objects on the bottom
show_tracks displays the tracking results
show_detection_performance displays the performance evaluation based on all detected
make_tracking_movie renders an output movie of the object tracking results
Even without solving any of the tasks, the project code can be executed.

The final project uses pre-computed lidar detections in order for all students to have the same input data. If you use the workspace, the data is prepared there already. Otherwise, download the pre-computed lidar detections (~1 GB), unzip them and put them in the folder results.
