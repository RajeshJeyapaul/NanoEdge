# NanoEdge
How to deploy the model created from IBM visual Insight (Powervision) onto Jetson Nano
>>>>>>>>>>>>>>>>Step 1: Increase the swap space:<<<<<<<<<<<<<<<<<<
a.	swapon -s 
b.	size – 1012820
c.	zram0 / zram1
Create additional memory using swap space:
•	https://github.com/JetsonHacksNano/installSwapfile...

•	Installswapfile.sh file
•	reboot
>>>>>>>>>>>>>Step 2: Pycuda Installation: <<<<<<<<<<<<<
 
To install pycuda:
https://devtalk.nvidia.com/default/topic/1056369/pycuda-installation-failure-on-jetson-nano/


export CPATH=$CPATH:/usr/local/cuda-10.0/targets/aarch64-linux/include

export LIBRARY_PATH=$LIBRARY_PATH:/usr/local/cuda-10.0/targets/aarch64-linux/lib

pip3 install pycuda –-user
	
NOTE: If bashrc is messed up: (https://askubuntu.com/questions/319882/problem-in-bashrc/319886#319886)

>>>>>>>>>>Step 3: Download the sample program:<<<<<<<<<<<<<<<
Clone the github - https://github.com/IBM/powerai

Go to the following folder - https://github.com/IBM/powerai/tree/master/vision/tensorrt-samples/samples/python/sampleFasterRCNN

DO the below changes: (Modified file provided here )
•	Do the following changes:
o	Engine.py Line 119 add the below line of code
o	Elif trt_engine_datatype == trt.DataType.INT8:
	Builder.int8_mode = True
o	Inference.py
	Add the below @ line 71
	trt.DataType.FLOAT: np.float32,
	trt.DataType.INT8: np.int8
o	paths.py
o	line @ 140
o	elif trt_engine_datatype == trt.DataType.INT8:
	return os.path.join(trt_results_path, ‘INT8’)
o	vision_model_deploy.py
o	comment #dnn_util.test import _get_image_blob
o	Add @ line57
o	32: trt.DataType.FLOAT,
o	8: trt.DataType.INT8
o	Add the path @ line # 251
o	Cv2.imwrite (‘/home/…../’+…_
o	Add @ line #258
o	Cv2.waitkey(0)
>>>>>>Step 4: Run the sample  with the required input :<<<<<<<<<<
  python3 vision_model_deploy.py  
Net_file – test_trt.prototxt
Model_file - .caffemodel
Json_file - .prop.json
lable_FILE – label.prototxt (create one with the class name) 
('face',) ---> if it is single class
('safety_vest', 'helmet', 'no_safety_vest', 'no_helmet')
Image_name - .jpg 


