- NVIDIA Jetson series
	- [Jetson Orin](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/jetson-orin/) is the latest iteration of the NVIDIA Jetson
- NVIDIA JetPack
	- like OS .iso file to be flashed to device
- NVIDIA Development Kit
	- Jetson Orin Nano Developer Kit
- ### NVIDIA Deep Learning Accelerator
	- DLA is a specialized hardware component **built into NVIDIA Jetson devices** that optimizes deep learning inference for energy efficiency and performance
		- ```bash
		  # Export a YOLO11n PyTorch model to TensorRT format with DLA enabled (only works with FP16 or INT8)
		  # Once DLA core number is specified at export, it will use the same core at inference
		  yolo export model=yolo11n.pt format=engine device="dla:0" half=True # dla:0 or dla:1 corresponds to the DLA cores
		  
		  # Run inference with the exported model on the DLA
		  yolo predict model=yolo11n.engine source='https://ultralytics.com/images/bus.jpg'
		  ```
- ## Common approaches
	- Flash JetPack to NVIDIA Jetson by one of:
	  collapsed:: true
		- If you own an official NVIDIA Development Kit such as the Jetson Orin Nano Developer Kit, you can [download an image and prepare an SD card with JetPack for booting the device](https://developer.nvidia.com/embedded/learn/get-started-jetson-orin-nano-devkit).
		- If you own any other NVIDIA Development Kit, you can [flash JetPack to the device using SDK Manager](https://docs.nvidia.com/sdk-manager/install-with-sdkm-jetson/index.html).
		- If you own a Seeed Studio reComputer J4012 device, you can [flash JetPack to the included SSD](https://wiki.seeedstudio.com/reComputer_J4012_Flash_Jetpack/) and if you own a Seeed Studio reComputer J1020 v2 device, you can [flash JetPack to the eMMC/ SSD](https://wiki.seeedstudio.com/reComputer_J2021_J202_Flash_Jetpack/).
		- If you own any other third party device powered by the NVIDIA Jetson module, it is recommended to follow [command-line flashing](https://docs.nvidia.com/jetson/archives/r35.5.0/DeveloperGuide/IN/QuickStart.html).
	- Install `torch 2.5.0` and `torchvision 0.20` according to JP6.1
	- Install `cuSPARSELt` to fix a dependency issue with `torch 2.5.0`
	- Install `onnxruntime-gpu`
	  collapsed:: true
		- The [onnxruntime-gpu](https://pypi.org/project/onnxruntime-gpu/) package hosted in PyPI does not have `aarch64` binaries for the Jetson. So we need to manually install this package.
	- Convert Model to TensorRT using `DLA`
- ## Best Practices
	- Enabling MAX Power Mode on the Jetson will make sure all CPU, GPU cores are turned on.
		- ```bash
		  sudo nvpmodel -m 0
		  ```
	- Enabling Jetson Clocks will make sure all CPU, GPU cores are clocked at their maximum frequency.
		- ```bash
		  sudo jetson_clocks
		  ```
	- Install Jetson Stats Application
		- ```bash
		  sudo apt update
		  sudo pip install jetson-stats
		  sudo reboot
		  jtop
		  ```