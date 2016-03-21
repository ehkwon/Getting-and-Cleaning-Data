1.Variable list and descriptions

subject 	         ID the subject who performed the activity for each window sample. Its range is from 1 to 30.
activity 	         Activity name
featDomain 	       Feature: Time domain signal or frequency domain signal (Time or Freq)
featAcceleration 	 Feature: Acceleration signal (Body or Gravity)
featInstrument 	   Feature: Measuring instrument (Accelerometer or Gyroscope)
featJerk 	         Feature: Jerk signal
featMagnitude 	   Feature: Magnitude of the signals calculated using the Euclidean norm
featVariable 	     Feature: Variable (Mean or SD)
featAxis 	         Feature: 3-axial signals in the X, Y and Z directions (X, Y, or Z)
Count 	           Feature: Count of data points used to compute average
Average 	         Feature: Average of each variable for each activity and each subject

2.Dataset structure

'data.frame':	11881 obs. of  11 variables:
 $ V1 : Factor w/ 31 levels "1","10","11",..: 31 1 1 1 1 1 1 1 1 1 ...
 $ V2 : Factor w/ 7 levels "activity","LAYING",..: 1 2 2 2 2 2 2 2 2 2 ...
 $ V3 : Factor w/ 3 levels "featDomain","Freq",..: 1 3 3 3 3 3 3 3 3 3 ...
 $ V4 : Factor w/ 3 levels "Body","featAcceleration",..: 2 NA NA NA NA NA NA NA NA NA ...
 $ V5 : Factor w/ 3 levels "Accelerometer",..: 2 3 3 3 3 3 3 3 3 3 ...
 $ V6 : Factor w/ 2 levels "featJerk","Jerk": 1 NA NA NA NA NA NA NA NA 2 ...
 $ V7 : Factor w/ 2 levels "featMagnitude",..: 1 NA NA NA NA NA NA 2 2 NA ...
 $ V8 : Factor w/ 3 levels "featVariable",..: 1 2 2 2 3 3 3 2 3 2 ...
 $ V9 : Factor w/ 4 levels "featAxis","X",..: 1 2 3 4 2 3 4 NA NA 2 ...
 $ V10: Factor w/ 46 levels "36","38","39",..: 46 13 13 13 13 13 13 13 13 13 ...
 $ V11: Factor w/ 11521 levels "-0.000143542544507042",..: 11521 282 1115 10851 5297 6398 5443 5300 5239 1690 ...


3.Show a few of the dataset
       V1       V2         V3               V4             V5
1 subject activity featDomain featAcceleration featInstrument
2       1   LAYING       Time             <NA>      Gyroscope
3       1   LAYING       Time             <NA>      Gyroscope
4       1   LAYING       Time             <NA>      Gyroscope
5       1   LAYING       Time             <NA>      Gyroscope
6       1   LAYING       Time             <NA>      Gyroscope
        V6            V7           V8       V9   V10             V11
1 featJerk featMagnitude featVariable featAxis count         average
2     <NA>          <NA>         Mean        X    50 -0.016553093978
3     <NA>          <NA>         Mean        Y    50 -0.064486124088
4     <NA>          <NA>         Mean        Z    50   0.14868943626
5     <NA>          <NA>           SD        X    50  -0.87354386782
6     <NA>          <NA>           SD        Y    50   -0.9510904402

4.Summary of variables
       V1                        V2                V3      
 1      : 396   activity          :   1   featDomain:   1  
 10     : 396   LAYING            :1980   Freq      :4680  
 11     : 396   SITTING           :1980   Time      :7200  
 12     : 396   STANDING          :1980                    
 13     : 396   WALKING           :1980                    
 14     : 396   WALKING_DOWNSTAIRS:1980                    
 (Other):9505   WALKING_UPSTAIRS  :1980                    
                V4                    V5              V6      
 Body            :5760   Accelerometer :7200   featJerk:   1  
 featAcceleration:   1   featInstrument:   1   Jerk    :4680  
 Gravity         :1440   Gyroscope     :4680   NA's    :7200  
 NA's            :4680                                        
                                                              
                                                              
                                                              
             V7                  V8              V9      
 featMagnitude:   1   featVariable:   1   featAxis:   1  
 Magnitude    :3240   Mean        :5940   X       :2880  
 NA's         :8640   SD          :5940   Y       :2880  
                                          Z       :2880  
                                          NA's    :3240  
                                                         
                                                         
      V10                          V11       
 51     : 726   -0.000971394711666667:    2  
 54     : 726   -0.0117536873784314  :    2  
 47     : 660   -0.0128062276916667  :    2  
 57     : 594   -0.0135771208822581  :    2  
 59     : 594   -0.0156098198829787  :    2  
 48     : 528   -0.0197686685018519  :    2  
 (Other):8053   (Other)              :11869  

