# ExerciseRecorder

# What is this? 

This is me trying to build a model that can that can detect what strength training exercise is being performed using the data from an IMU sensor on the wrist and also count the number of reps. 

Admittedly, I have no idea what I am doing in most of the code. 

# Recording Details :

1) Each file is one set of one strength training exercise. It is named by the following scheme. "ddmmyy_CODE_Wxx_Sx_Rxx"
   Where ddmmyy is the date, CODE is the unique exercise code ( list of which is provided in a separate csv file next to this document )
   The xx is Wxx is the weight, The x next to S is set number, The x next to R is the number of reps.
   You won't have to worry about any of this as all the information processing from the name is done in the processing file. 

2) Data was recorded using the sensor logger app on an appleWatch SE at 100Hz.

3) The raw files have a lot of unnecessary columns that come from the app also recording the phone data it is running on.
   The only relevant columns are filtered in the processed folder. 
   All the motion data from the apple watch is titled wristMotion, the other stuff is nonsense. 

4) Whales1 is the first few days of recording data and has around 90 sets of data. 
   Whales2 is the next few days and has around 74 set ofdata. 


# Basic Data processing :

1) Firstly, all the information from the name of the file like weight, sets, reps is put into a column in the file. 

2) The first 1.5s and last 1.5s of data are removed because it adds unnecessary noise from me when I was pressing the record or stop button

3) The data is filtered to remove anything above 1Hz (aggressive, I know, I just don't see any reason to have higher freq when reps are performed at <0.5Hz) 
   This is done using a Butterworth filter. 
   You can change this if needed.

4) The data is then combined into one large continuous file. 
   (This just made it a easier for me to train my model, no idea if this is right )


# Exercise Classification : 

1) I am using some combination of a DNN I found on Kaggle for Human Activity Recog and my own insights. 
   It takes 6 parameters 3 accel and 3 rotationrate

2) The code is self explanatory but it's probably better you use the raw or filtered data and do your own thing to try and classify the data. 

3) The model I am using divides the combined file into 1000 length timeperiods and 500 length steps.
   This means that it is looking at windows of 10s each and moving the window by 5s each. 
   Felt this was appropriate because each set on average lasts around 50-100s

4) The maximum accuracy I was able to get from my model was 65%, however this is changing for different runs so I don't know if it's something wrong with the way I am doing it.


# Peak Detection :

1) I attempted to count reps from IDBC or Incline Dumbell Bicep Curls. I was partially successful for the few sets that I checked. 

2) It is basic peak detection on filtered data with min spacing and hysteresis requirements to count a peak. 
