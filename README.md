
# Facial Expression Recognition

It is a Tensorflow/Keras sample project using CNN. 

## Steps

- Download dataset (https://www.kaggle.com/msambare/fer2013) 

- Build and train the model 
 
```python
$python Classification_model_vgg.py 
```

- Load trained model and Run demo 

```python
$python Facial_Expressions_Recog.py
```



## Demo

```python
from keras.models import load_model
from time import sleep
from keras.preprocessing.image import img_to_array
from keras.preprocessing import image
import cv2
import numpy as np

face_classifier = cv2.CascadeClassifier(r'\haarcascade_frontalface_default.xml')
classifier =load_model(r'\my_model.h5')

class_labels = ['Angry','Happy','Disgust','Fear','Neutral','Sad','Surprise']

#cap = cv2.VideoCapture(0)
cap = cv2.VideoCapture('HumanFE.mp4')


while True:
    # Grab a single frame of video
    ret, frame = cap.read()
    labels = []
    gray = cv2.cvtColor(frame,cv2.COLOR_BGR2GRAY)
    faces = face_classifier.detectMultiScale(gray,1.3,5)

    for (x,y,w,h) in faces:
        cv2.rectangle(frame,(x,y),(x+w,y+h),(255,0,0),2)
        roi_gray = gray[y:y+h,x:x+w]
        roi_gray = cv2.resize(roi_gray,(48,48),interpolation=cv2.INTER_AREA)
        # rect,face,image = face_detector(frame)
        if np.sum([roi_gray])!=0:
            roi = roi_gray.astype('float')/255.0
            roi = img_to_array(roi)
            roi = np.expand_dims(roi,axis=0)

            # make a prediction on the ROI, then lookup the class

            preds = classifier.predict(roi)[0]
            label=class_labels[preds.argmax()]
            print(label)
            label_position = (x,y)
            cv2.putText(frame,label,label_position,cv2.FONT_HERSHEY_SIMPLEX,2,(0,255,0),3)
        else:
            cv2.putText(frame,'No Face Found',(20,60),cv2.FONT_HERSHEY_SIMPLEX,2,(0,255,0),3)
    cv2.imshow('Emotion Detector',frame)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()

```

# Demo output

<p float="left">
  <img src="demo 1.png" width="400" />
  <img src="demo 2.png" width="400" /> 
  <img src="demo 3.png" width="400" />
  <img src="demo 4.png" width="400" />
  <img src="5.png" width="400" />
  <img src="6.png" width="400" />
  <img src="7.png" width="400" />
</p>


## License
[MIT](https://choosealicense.com/licenses/mit/)
