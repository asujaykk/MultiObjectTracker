# MultiObjectTracker
This is multiobject tracker module designed to use with any object detectors. 
This tracker is designed with YOLOv7 object detector, but still work with other detectors with a slight change in the input parameters to the tracker.

## Tracking procedure
1. Detect object from the video frame using an object detection and recognition model (recomended yolov7).
2. Create a list of objects with the new detections (current objects).
3. Find the best match for each objects from previous frmae (old objects) with the current objects, the matched objects will be updated with the old label.
4. Find new objects entries from temp objects those were not available in the previous frame.
5. Identify missed objects (objects was there in the previous frames but not in the current framae).
6. Save matched objects and new objects for next stages and remove missed objects (only objects they are missing for more than a number of frmaes).
7. Return the labels for all new objects and old objects in the current frame (label represent the objects id) back for plotting or further application.

## Methods used for tracking.
1. Nearest object search
2. Iou matching

## How to use the tracker with object detection and recognition model:

1. The tracker object need to be created with list of class names as constructor parameter.
2. Then process one  image/video frmae with the detector (lets say yolo object detector) and get the detections.
3. Then invoke tracker.track(im0,det) method for starting the tracking process. The tracker.track() method accept two parameters and they are as follows.    
      a. im0 : The frame/image being processed (it should be a numpy array)  
      b. det : The detection of the object detector for the image (list of detections) 
         a single detection should have the following format 
            
       (bbox,confidence,class)  
        bbox=bounding box of the object[x1,y1,x2,y2]   x1,y1= upper left corner , x2,y2= Lower right corner  
        confidence= confidence of the object detector (float value)  
        cls= integer ,represent the index position of the class in the class name list.  

4. The tracker.track() method return a list of the detections with label for each object.
   a single detection have the following format

       (bbox,label,class)
       bbox=bounding box of the object[x1,y1,x2,y2]  x1,y1= upper left corner , x2,y2= Lower right corner 
       label= string representing an objecrt "classname_<id>"   example truck_5, car_6
       class= integer representing the class 

5. The tracker object keep track of the objects detected in the previous frame and  return the new positions of the objects in the new frame based on the new detections in the new frame.
6. The following code template shows how to use tracker in a image detection and recognition code.

   Lets say if a normal object detection and tracking code template is as follows,
 
         class_names=[ list of class labels (string) ]       # class labels supported by the object detector         
         for frame in Video :                                # loop to process frames in a video
             detections =object_detector(frmae)              # object detector process current frmae for detecting objects and return detection list
             detection_plotting_function (frame,detection)   #plotting function or code for drawing detection in the image with labels
             
    Then the tracker can be added to the above code as follows,
    
         from MultiObjectTracker.tracker_v3 import tracker   # importing tracker module
         class_names=[ list of class labels (string) ]       # class labels supported by the object detector 
         mo_tracker= tracker(class_names)                    # create tracker object with list of class name as input
         for frame in Video :                               # loop to process frames in a video
             detections =object_detector(frmae)              # object detector process current frmae for detecting objects and return detection lis        
             tracker_detection=mo_tracker.track(frame,detections)   # apply track method on current detection and get tracked detection as output      
             detection_plotting_function (frame,tracker_detection)  # pass tracker_detection to detection_plotting_function (a slight change might be required in plotting function based on the tracker output and plotiing function)             

## Major tracker modules of the MultiObjectTracker
There are two tarckers available in this package and they are v2 and v3. These trackers have similar interface and both can be used with different detectors. The tracker v2 is a lite version and use nearest search and first match approach to track the object. And tracker v3 is the upgraded version and which use multiple object matching methods and improved cross matching algorithm for identifying best matches.

### tracker_v2 :
Tracker v2 is the basic version. It purely works based on the nearest object search. The tracker search the nearest object based on the movement of object centre point in consecutive frames.

### tracker_v3 :
Tracker v3 is a bit advanced version of tracker v2 and it work based on the following approaches.
1. Nearest object search.
2. Iou matcthing.
3. Best cross matching algorithm.  
It is computationally expensive but provide much accurate tracking. This tracker handle detections as objects and the user can add their own matching algorithms in "object" class to achive customized tracking.


## Aplications.
1. Traffic monitoring.
2. Vehicle spped detection from traffic camera.
3. Accident detection.
4. Environment monitoring for self driving cars.
5. Object tracking drones.



         
         
