## 1⚙️ Environment Setup and Running

### Build the Docker Environment (Using Ubuntu 22.04 / IsaacSim 5.0)

```shell
sudo docker pull nvidia/cuda:12.2.0-runtime-ubuntu22.04

cd unitree_sim_isaaclab

sudo docker build -t unitree-sim:latest -f Dockerfile .

# If you need to use a proxy, please fill in
#   --build-arg http_proxy=http://127.0.0.1:7890 \
#   --build-arg https_proxy=http://127.0.0.1:7890 \

### Build the Docker Environment (Using Ubuntu 22.04 / IsaacSim 5.0)
```
### Build the Docker Environment

```shell
xhost +local:docker

sudo docker run --gpus all -it \
  --name unitree-sim \
  --network host \
  -e NVIDIA_VISIBLE_DEVICES=all \
  -e NVIDIA_DRIVER_CAPABILITIES=compute,utility,video,graphics,display \
  -e LD_LIBRARY_PATH=/usr/local/nvidia/lib:/usr/local/nvidia/lib64:$LD_LIBRARY_PATH \
  -e DISPLAY=$DISPLAY \
  -e VK_ICD_FILENAMES=/usr/share/vulkan/icd.d/nvidia_icd.json \
  -v /usr/share/vulkan/icd.d:/usr/share/vulkan/icd.d:ro \
  -v /tmp/.X11-unix:/tmp/.X11-unix:rw \
  -v /dev/dri:/dev/dri \
  -v /home/unitree/newDisk/unitree_sim_isaaclab_usds:/home/code/isaacsim_assets \
  -v /home/unitree/workspace:/home/code/workspace \
  unitree-sim /bin/bash
```

### Asset Download

```
sudo apt update

sudo apt install git-lfs

. fetch_assets.sh
```
## 2 Start the Program
#### Teleoperation

```shell
python sim_main.py \
  --device cpu \
  --enable_cameras \
  --task Isaac-PickPlace-Cylinder-G129-Dex1-Joint \
  --enable_dex1_dds \
  --robot_type g129 \

# --headless
```
- --task: Task name, corresponding to the task names in the table above

- --enable_dex1_dds/--enable_dex3_dds: Represent enabling DDS for two-finger gripper/three-finger dexterous hand respectively  
- --robot_type: Robot type, currently has 29-DOF unitree g1 (g129),27-DoF H1-2

- --headless: This allows running without launching the simulation window. Add this parameter if you're using a Docker environment.