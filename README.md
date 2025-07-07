# Jetracer

The Jetracer is a robot designed by Waveshare based on an NVIDIA Jetson Nano board. It is mainly used to familiarize users with autonomous driving based on neural networks.

## Accessing the robot interface

There are several methods for connecting to the robot and developing autonomous driving algorithms.

### Physical access

The first method is the recommended method for the development phase. To do this, you need a monitor, mouse, and keyboard. Connect the keyboard and mouse to the USB ports on the NVIDIA Jetson Nano. Then connect the monitor to the HDMI port or Display port. It is also advisable to connect the NVIDIA Jetson Nano to a power source other than the batteries during the development phase.

### Remote access

It is also possible to access the robot's interface remotely from the Jupyter server available on the NVIDIA Jetson Nano. To do this, the robot and the computer from which you are attempting to access the robot must be on the same local network. When the robot is connected to a network, its IP address is displayed on the robot's screen. This IP address must then be copied into a web browser, followed by port number 8888. For example, the IP address of the Jetracer used was `192.168.0.104`, so the following line is entered into a web browser:

```bash
http://192.168.0.104:8888/
``` 
## Camera calibration 

When used for the first time, the robot's camera may have a reddish tint. To correct this problem, apply a patch that can be downloaded [here.](https://files.waveshare.com/upload/e/eb/Camera_overrides.tar.gz)

Then run the following command lines: 
```bash
tar zxvf Camera_overrides.tar.gz 
sudo cp camera_overrides.isp /var/nvidia/nvcam/settings/
sudo chmod 664 /var/nvidia/nvcam/settings/camera_overrides.isp
sudo chown root:root /var/nvidia/nvcam/settings/camera_overrides.isp
```

## Getting started with the robot

It is strongly recommended that you familiarize yourself with the robot before you start working on it seriously. To do this, there are two codes (created by NVIDIA) in the `basic code` directory. The first, `basic_motion.ipynb`, allows you to familiarize yourself with the Jetracer's driving system, and the code `teleoperation.ipynb` allows you to control the robot remotely with a joystick. This code will be necessary during the data collection phases.

## Trajectory tracking

Now that you know how to use the robot, we can move on to the autonomous driving algorithm available in the `Road following` folder. To do this, you need to train, optimize, and then infer the neural network.
For training, use the `interactive_regression.ipynb` code, which allows you to collect data, annotate it, and start training.
Next, for the optimization and inference stage, you will need to use the `road_following.ipynb` code.
Once the first inference is successful, you can use the `RF_inference.ipynb` code, which contains optimized code for inference based on the previous one.

## Obstacle avoidance

The codes used for obstacle avoidance can be found in the `Obstacle avoidance` directory. As with trajectory tracking, you must first create a database using the `Data_Collection.ipynb` code. 
Once the data has been collected, you can train the model using the `Obstacle_training.ipynb` code. Then you can move on to optimization with the code `live_demo_resnet18_build_trt.ipynb`. And finally, once the inference engine is ready, you can launch an inference from the code `live_demo_resnet18_trt.ipynb`. 
Note that the output of the obstacle detection model is a probability between 0 and 1, normally close to 0.95 when an obstacle is detected and close to 0 when there is no obstacle. If this is not the case, it means that the database is incomplete. Images of the obstacle must be collected on different backgrounds so that certain background patterns are not mistaken for obstacles.

## Combined script

Once the two models are ready, we can move on to the final step. This algorithm will allow us to follow a default trajectory and, if an obstacle is detected, it will avoid it and then resume the initial trajectory. The corresponding code is called `DEMO1.ipynb` and can be found in `Combined script`. 

## Network

For the purposes of the V2X project, it is useful to make driving connected. The necessary codes for this can be found in the `Network` directory. The code for sending the camera feed to a remote server is called `Video_flask.ipynb`.



## Credit

Much of the code developed in this project is based on NVIDIA code. This is particularly true of the codes `basic_motion.ipynb` `teleoperation.ipynb` `interactive_regression.ipynb` `road_following.ipynb` `Data_Collection. ipynb` `Obstacle_training.ipynb` `live_demo_resnet18_build_trt.ipynb` `live_demo_resnet18_trt.ipynb` and are available on [NVIDIA's GitHub](https://github.com/NVIDIA-AI-IOT/jetracer)
