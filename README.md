# MultiObjectTracker
This is multiobject tracker module designed to use with any object detectors. 
This tracker is designed with YOLOv7 object detector, but still work with other detectors with a slight change in the input parameters to the tracker.

## Tracking procedure
1. Detect Object from the video frame using an object detector and recognition model (recomended yolov7).
2. Create a list of objects with the new detections (current objects).
3. Find the best match for each objects from previous frmae (old objects) with the current objects, the matched objects will be updated with the old label.
4. Find new objects entries from temp objects those were not available in the previous frame.
5. identify missed objects (objects was there in the previous frames but not in the current framae).
6. Save matched objects and new objects for next stages and remove missed objects (only objects they are missing for more than a number of frmaes).
7. return the labels for all new objects and old objects in the current frame (label represent the objects id) back for plotting or further application.

## Method used for tracking.
1. Nearest object search
2. Iou matching

## How to use the tracker:

1. The tracker object need to be created with list of class names as constructor parameter.
2. Then process one  image/video frmae with the detector (lets say yolo object detector) and get the detections.
3. Then invoke tracker.track(im0,det) method for starting the tracking process.
   The tracker.track() method accept two parameters and they are as follows 
      
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
