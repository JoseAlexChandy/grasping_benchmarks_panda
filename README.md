# grasping-benchmarks-panda


## Modifications of this fork
This fork allows the use of the latest version of Dexnet. It also allows Graspnet to work without a graphics interface, and fixes other problems faced when running the containers and libraries at CMS

* docker/run.sh: Doesn't roslaunch the service automatically, which was giving me problems
* docker/build_images/Makefile: A different image is chosen in order to install ROS Noetic wuth Python3
* docker/dexnet/Dockerfile: installs ROS Noetic, fixes ownership
* grasping_benchmarks/dexnet/dexnet_grasp_planner.py: fixes functions from SciPy which were updated in the library's latest version
* docker/dexnet/Dockerfile: pulls forked graspnet library which works without graphics interface
* grasping_benchmarks/graspnet/graspnet_grasp_planner.py: allows use without graphics interface, discards use of aruco marker

## Instructions:
1. Git-clone this fork:
```
git clone https://github.com/PabloLopezCustodio/grasping-benchmarks-panda-1.git
cd grasping-benchmarks-panda-1
```
2. set SSH_AUTH_SOCK:
```
eval $(ssh-agent)
```
3. build each container separetely 
```
cd docker/build_images
make USER_NAME=<user_name> dexnet
make USER_NAME=<user_name> 6dgraspnet
make USER_NAME=<user_name> gdp
```
or build them all at once:
```
cd docker/build_images
sh build_all.sh
```
4. run a container for the first time
```
cd docker
bash run.sh <user_name> 6dgraspnet_container <user_name>/benchmark_6dgraspnet
bash run.sh <user_name> gpd_container <user_name>/benchmark_gpd
bash run.sh <user_name> dexnet_container <user_name>/benchmark_dexnet
```
5. Inside the container source the workspace and fix the IP of your ROS master if it's different to the local machine. Finally, launch the service:
```
source /workspace/catkin_ws/devel/setup.bash
export ROS_MASTER_URI=http://10.0.2.3:11311
export ROS_IP=10.0.2.11
roslaunch grasping_benchmarks_ros grasp_planning_benchmark.launch
```
