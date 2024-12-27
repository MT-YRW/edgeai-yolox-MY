# This readme is from MT-YRW
* A modified version of open-source project named edgeai-yolox. located in:https://github.com/TexasInstruments/edgeai-yolox.git
* Instead of training model based on ycbv and lmo dataset, I used self-made dataset which contain only one class of object(fire extinguisher) 


## Installation  
you can reference [README_ori.md](./README_ori.md) to get start

## Try to train your own datasets
### Step1. Preparation
* **dataset**
    * A dataset which contains your target models.If you dont know how to make a bop dataset, Refer to this for further details.
* **labels**
    * change the label of your models. the code is located in [lmo2coco](./tools/lmo2coco.py), [lmo.py](./yolox/data/datasets/lmo.py) and [object_pose_utils_onnx.py](./demo/ONNXRuntime/object_pose_utils_onnx.py).You can identify what modifications are needed by searching for "fire_extinguisher". 

### Step2. Convert your dataset to coco format
Rename your train file to "train_pbr" and your test file to "test_bop", build a new file named "annotations", and run this command:
```
python tools/lmo2coco.py --datapath './datasets/my' --split train                
                                                     --split test   
```
The above script will generate instances_train.json and instances_test_bop.json under the "annotations" file.

### Step3. Train your model
Run this command to start your training:
```
python -m yolox.tools.train -n yolox-s-object-pose-ti-lite --dataset my -c 'path to pretrained ckpt' -d 4 -b 64 --fp16 -o --task object_pose
```

### Step4. ONNX Export Including Detection and 6D Pose Estimation
Run this command to get your ONNX model:
```
python tools/export_onnx.py -n yolox-s-object-pose -c "path to ckpt" --output-name your_model.onnx --export-det --dataset my --task object_pose
```

### Step5. Test your model
Run the script as below to run inference with an ONNX model. The script runs inference and visualize the results.
```
cd demo/ONNXRuntime
# Run inference on a set of sample images
python onnx_inference_object_pose.py --model "path_to_onnx_model"  --image-folder "path-to-input-images" --output-dir "ops_onnxrt" 
```


# Note:
This readme described a fast and easy way to train your model,which can detect whatever you want. Although I used this frame successfully trained my model and get a preferable result,I'm not sure that my understanding of this method is accurate.If you find something wrong, welcome to correct me!




