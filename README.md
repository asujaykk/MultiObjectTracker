# MultiObjectTracker
This is multiobject tracker module designed to use with any object detectors. 
This tracker is designed with YOLOv7 object detector, but still work with other detectors with a slight change in the input parameters to the tracker.

Method used for tracking.
1. Nearest object search
2. Iou matching

How to use the tracker:




1. The tracker object need to be created with list of class names as constructor parameter.
2. Then process one  image/video frmae with the detector (lets say yolo object detector) and get the detections.
3. Then invoke tracker.track(im0,det) for starting the tracking process.
   The tracker.track() method accept two parameters and they are as follows 
      
      a. im0 : The frame/image being processed (it should be a numpy array)  
      b. det : The detection of the object detector for the image (list of detections) 
            a single detection should have the following format 
            
            (bbox,confidence,class)  
            bbox=bounding box of the object[x1,y1,x2,y2]   
               x1,y1= upper left corner   
               x2,y2= Lower right corner  
            confidence= confidence of the object detector (float value)  
            cls= integer ,represent the index position of the class in the class name list.  

4. The tracker.track() method return a list of the detections with label for each object.
   a single detection have the following format

         (bbox,label,class)
          bbox=bounding box of the object[x1,y1,x2,y2]
          label= string representing an objecrt "classname_<id>"   example truck_5, car_6
          class= integer representing the class 


