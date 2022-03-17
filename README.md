# Vehicle_Classification_with_NumberPlate_Detection

<p>
    <img src="https://img.shields.io/npm/l/color-calendar?style=flat-square" alt="license" />
</p>

<p>
    A vehicle detection, classification and counting model is used to maintain a record of all the vehicles passing an area at a particular time. This comes with license plate detection which can be used to keep a record of all those who break some traffic rule.

</p>

## QUICK DEMO

---
### Counting of Different Types of Vehicles:
<p align="center">
   <video src="https://user-images.githubusercontent.com/41968942/158681333-74d3d972-8229-4e6f-99b5-338aa7c4f707.mp4" | width=300>
</p>

- [Features](#features)
- [Installation](#Installation)
- [Usage](#usage)
  - [Tracker.py](#Tracker)
  - [Vehicle_Count.py](#Count)
  - [CoCo.names](#Coco)
- [Methods](#methods)
- [YOLO](#yolo)
- [Bug Reporting](#bug)
- [Feature Request](#feature-request)
- [Release Notes](#release-notes)
- [License](#license)

<a id="features"></a>

## 🚀 Features

- Zero dependencies
- Detection of vehicles in a dynamic setting i.e. Video
- Detection of Vehicles in a static setting i.e. a particular frame
- Highlighting the vehicle along with the prediction accuracy.
- Counter for different types of vehicles i.e. Cars, Trucks, Bikes.
- Number plate Detection System for those who break any rules. (Upcoming)
- Alert for all the vehicles who break any rules. (Upcoming)
- Storing the data about the number of vehicles passed segregated by their category. 
- [Request more features](#feature-request)...

<a id="Installation"></a>
  ## Installation
### Directly 
```bash
pip install -r requirements.txt
```
#### Or
### Use a Virtual Environment

```bash
    virtualenv <virtual_environment_name>
    
    cd <virtual_environment_name>\Scripts
    
    activate.bat
    
    pip install -r requirements.txt
```



<a id="usage"></a>
## 🔨 Usage
    
<a id="Tracker"></a>
### 1) Tracker.py
*This uses the Euclidean_distance concept to keep track of an object. It calculates the difference between two center points of an object in the current frame vs the previous frame, and if the distance is less than the threshold distance then it confirms that the object is the same object of the previous frame.*

![Tracker_Euclidean](https://user-images.githubusercontent.com/41968942/158663021-1ec8d1b0-d939-4d5e-89ec-88526d1ae7bf.png)

```
    Import math
    # Get center point of new object
    for rect in objects_rect:
      x, y, w, h, index = rect
      cx = (x + x + w) // 2
      cy = (y + y + h) // 2
      # Find out if that object was detected already
      same_object_detected = False
      for id, pt in self.center_points.items():
          dist = math.hypot(cx - pt[0], cy - pt[1])
          if dist < 25:
              self.center_points[id] = (cx, cy)
              # print(self.center_points)
              objects_bbs_ids.append([x, y, w, h, id, index])
              same_object_detected = True
              break
```   

<a id="Count"></a>                           
### 2) Vehicle_Count.py
<ul>
    <li> This controls the initialisation of models and network models.</li>
    <li> Selecting the video source to be used for prediction. </li>
    <li> Pre-process the frame and run the detection. </li>
    <li> Post-process the output data. </li>
    <li> Track and count all vehicles on the road. </li>
    <li> Save the final data to a CSV file. </li>
</ul>
              
<a id="Coco"></a>                           
### 3) CoCo.names
<ul>
    <li> YOLOv3 is trained on the COCO dataset, so we read the file that contains all the class names and store the names in a list.</li>
    <li> The COCO dataset contains 80 different classes.</li>
    <li> We need to detect only cars, motorbikes, buses, and trucks for this project, that’s why the required_class_index contains the index of those classes from the coco dataset. </li>
</ul>
   
    
    
<a id="methods"></a>

## 🔧 Methods

### `Import Libraries`
```
    import cv2
    import csv
    import collections
    import numpy as np
    from tracker import *
```
    
### `Positing the Lines`
```
    # Middle cross line position
    middle_line_position = 225   
    up_line_position = middle_line_position - 15
    down_line_position = middle_line_position + 15
```

### `Defining the Model and Setting up the Network`
```
    ## Model Files
    modelConfiguration = 'yolov3-320.cfg'
    modelWeights = 'yolov3-320.weights'
    # configure the network model
    net = cv2.dnn.readNetFromDarknet(modelConfiguration, modelWeights)
    # Configure the network backend
    net.setPreferableBackend(cv2.dnn.DNN_BACKEND_CUDA)
    net.setPreferableTarget(cv2.dnn.DNN_TARGET_CUDA)
    # Define random colour for each class
    np.random.seed(42)
    colors = np.random.randint(0, 255, size=(len(classNames), 3), dtype='uint8')
```
   
### `Defining the video source and reading the frames `
<ul>
    <li>cap.read() reads each frame from the capture object.</li>
    <li>Using cv2.reshape() we reduced our frame by 50 percent.</li>
    <li>Then using the cv2.line() function we draw the crossing lines in the frame.</li>
    <li>And finally, we used cv2.imshow() function to show the output image.</li>
</ul>
    
```
    # Initialize the videocapture object
      cap = cv2.VideoCapture('video.mp4')
      def realTime():
        while True:
           ...
    if __name__ == '__main__':
          realTime()
```
    
### `Pre-Processing and Running Detection`
<ul>
    <li>YOLO takes 320×320 image objects as input. The input to the network is a blob object. The function dnn.blobFromImage() takes an image as input and returns a resized and normalized blob object.</li>
    <li>The net.forward() is used to feed the image to the network. And it returns an output.</li>
    <li>Finally, we call our custom postProcess() function to post-process the output.</li>
</ul>  
    
```
   input_size = 320
   blob = cv2.dnn.blobFromImage(img, 1 / 255, (input_size, input_size), [0, 0, 0], 1, crop=False)
   # Set the input of the network
   net.setInput(blob)
   ...
   # Feed data to the network
   outputs = net.forward(outputNames)
   # Find the objects from the network output
   postProcess(outputs,img)
```    
    
### `Post-process the output data`
The network forward output contains 3 outputs. Each output object is a vector of length 85.
<ul> 
    <li>4x the bounding box (centerX, centerY, width, height)</li>
    <li>1x box confidence</li>
    <li>80x class confidence</li>
</ul>

```
    detected_classNames = []
    def postProcess(outputs,img):
    ...
```
    
### `Track and count all vehicles on the road`
<ul>
    <li>After getting all the detection, we keep track of those objects using the tracker object. The tracker.update() function keeps track of every detected object and updates the position of the objects.</li>
    <li>Count_vehicle is a custom function that counts the number of vehicles that crossed through the road.</li>
</ul>
    
```
    # Update the tracker for each object
    boxes_ids = tracker.update(detection)
    for box_id in boxes_ids:
        count_vehicle(box_id)
    def count_vehicle(box_id):
    ...
```

### `Marking the Vehicles.`
Cv2.circle() draws a circle in the frame. In our case, we’re drawing the center point of the car.
    
### `Save the final data to a CSV file`
<ul>
    <li> With the open function, we opened a new file data.csv with write permission only.</li>
    <li> Then we write 3 rows, the first row contains class names and directions, and the 2nd and 3rd row contains up and down route counts respectively.</li>
    <li> The writerow() function writes a row to the file.</li>
</ul>
    
```
    with open("data.csv", 'w') as f1:
    ...
```

<a id="yolo"></a>
    
## YOLO
### Working
    
<ul>
    <li> Residual Blocks – Basically, it divides an image into NxN grids.</li>
    <li> Bounding Box regression – Each grid cell is sent to the model. Then YOLO determines the probability of the cell contains a certain class and the class with the maximum probability is chosen.
    <li>  Intersection Over Union (IOU) – IOU is a metric that evaluates intersection between the predicted bounding box and the ground truth bounding box.</li>
    </ul>
    
![YOLO_1](https://user-images.githubusercontent.com/41968942/158674134-3caed9e8-5493-4833-8e58-d08c177ee944.png)

### Architecture
<ul>
    <li> The YOLO network has 24 convolutional layers followed by 2 fully connected layers. The convolutional layers are pre-trained on the ImageNet classification task at half the resolution (224 × 224 input image) and then double the resolution for detection.</li>
    <li> The layers Alternating 1 × 1 reduction layer and 3×3 convolutional layer to reduce the feature space from preceding layers.</li>
    <li> The last 4 layers are added to train the network for object detection. </li>
    <li> The last layer predicts the object class probability and the bounding box probability.</li>
</ul>

![YOLO_2](https://user-images.githubusercontent.com/41968942/158674162-18d6a763-ef42-4a7a-86f6-c0f074bf02b4.png)
    

<a id="bug"></a>

## 🐛 Bug Reporting

Feel free to [open an issue](https://github.com/deepanshug4/Vehicle_Classification_with_NumberPlate_Detection/issues) on GitHub if you find any bug.


<a id="feature-request"></a>

## ⭐ Feature Request

- Feel free to [Open an issue](https://github.com/deepanshug4/Vehicle_Classification_with_NumberPlate_Detection/issues) on GitHub to request any additional features you might need for your use case.
- Connect with me on [LinkedIn](https://www.linkedin.com/in/deepanshug4/). I'd love ❤️️ to hear where you are using this library.

<a id="release-notes"></a>

## 📋 Release Notes

Check [here](https://github.com/deepanshug4/Vehicle_Classification_with_NumberPlate_Detection/releases) for release notes.

<a id="license"></a>

## 📜 License

This software is open source, licensed under the [MIT License](https://github.com/PawanKolhe/color-calendar/blob/master/LICENSE).
