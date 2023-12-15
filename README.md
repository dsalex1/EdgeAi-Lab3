# EdgeAi-Lab3
 
## Part 1 – Setup

### Task 1. Create a new project in Edge Impulse.

![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/a71739eb-2a1a-42e2-8b3e-d33a0fba31d5)

### Task 2. Install the Edge Impulse CLI and Arduino CLI 1

## Part 2 – Data Collection

### Task 3. Collect data

![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/daefa805-a5b9-4640-a97b-ce25c55452f3)

### Task 4. Look at your data.

#### Question 1. Which features/patterns are representative for each of your motions?

The O shape looks sinusodial in one or more dimensions. The S shape looks wavey, but with alterating amplutudes, also in one dimension the waves seem to have a small frequencies. The L shape looks bumpy with more sudden changes in accesselration.

#### Question 2. Is it sufficient to record data for your classes, or do you also need a baseline without any movement? Explain why you (don’t) need idle data.

yes we need an idle state, because the model tends to be over convident if it was no idle state to compare against, in our case it worked out better with an idle state

### Task 5. If you need idle data, record the same amount of data without moving the device as for each of the classes.

### Task 6. Split your data into train and test data.
![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/404fe33c-c847-4ed6-b8e0-91724e85727e)

## Part 3 – Impulse Design

### Task 7. Create an Impulse. 

#### Question 3. Which settings did you choose in the Time series data input block? Explain what these settings do and why you think that your choices are a good choice.

![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/3dcb881e-1217-4640-b738-cef6b66e5ae2)
within 4 seconds there is at least one full movement, so it captures all features neccessary to indentify the shape. 200ms movement allows us to generate many samples, also it was the smalles possible size that works within the free tier of edgeimpulse, also this allows for a reasonable fast inference time.

### Task 8. Explore the different preprocessing blocks and generate features.

#### Question 4. What do the different preprocessing blocks do? Explain them. Why should we use preprocessing blocks instead of feeding the data directly into the neural network like in the previous labs?

preprocessing allows our data to amplify the features that we actually care about, for example spectral analysis allows us to have the model notice 
the different frequencies present in the input data directly instead of haveing the model have to figure out that itself. Flatten flattens all data in an axis down to a single value, similar to an average. also there is raw data which doesnt add any preprocessing

#### Question 5. Which preprocessing block creates the best features separating the classes? Add screenshots. What kind of features did the blocks extract?

flatten:
![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/55c9af20-fef1-4b6e-a2de-fa3bed916883)
raw data:
![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/db5075b5-b3a9-4890-b9eb-d48eba1eda83)
spectral analysis:
![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/848c7708-34ac-411c-a900-58b1cfbf936b)

the spectral analysis seperated the samples based on how wavey or flat their data is.

### Task 9. Continue with at least the Spectral Analysis and the Raw Data blocks.

#### Question 6. What are 1D Convolutions? How do they differ from 2D convolutions used for images?

1d Consolution just use one row of the image, instead of a 2D block of hte image, this sort of applies the filter to each row of the image sperately, and stitches the results back together to form a whole 2d image, similar to the result of a 2d convolution.

#### Question 7. Briefly explain your models. Focus in your explanation on the parts of the code deviating from the network architectures you used in the previous labs. E.g., what is activity regularizer=tf.keras.regularizers.l1(0.00001)? Include the code for your models in your submission.

The code uses batches, which we previously did not explicitly. also the batches are not shuffled which makes the training deterministic, meaning that training with the same hyperparameters whould lead to the same model.

also we use an l1 regularization for the weights and parameters, which aims to prevent overfitting, by making certain values smaller to reduce the impact of noise.

#### Question 8. What is the training performance of the models? How much memory is your model expected to need? What execution time is estimated? You can add screenshots. Check also for the memory usage and execution time of your DSP blocks.

flattened: 
![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/d607af92-2bfd-4e2c-99be-9d9b1f86998a)
spectral:
![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/3e474135-ff02-4360-bfc7-a271873dd8a0)
raw data:
![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/b5b074c1-3704-4234-bfaa-18f5d97f6b0c)


### Task 10. Head to your dashboard and download the quantized models of your classifiers. Head to https://netron.app/ and open your models with it. Click on the input or output layer and take a look at the quantization equations.

#### Question 9. Are the quantization equations the same for each of your classifiers? Why (not)?

spectral:
![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/ca1108d7-1758-4a2a-bb0d-ed856ab3a221)

flattened:
![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/adcc92c0-4e60-4cc1-81b6-0dd3f672956b)

raw data:
![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/2698a8f3-798b-43aa-ab69-4649e76a7016)

the equation equations are different for each model, which is since the data ranges going into these layers are vastly different due to the preprocessing.

## Part 4 – Model Testing

### Task 11. Evaluate your models in the Model Testing section.

#### Question 10. How good do your models classify the test data? Do the models generalize well? Create plots comparing your models. Also explain F1 Score and Uncertainty displayed in the confusion matrix.

![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/59fbcea3-70d5-4d12-83d6-c7eb8c9582a1)
spectral features: ![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/c3d3ab65-303d-4360-abdf-56f80a0071f1)
flatten: ![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/4ceab9ed-8fde-4b4d-bc2c-d9322a85bc28)
raw data: ![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/cf169bc9-0164-433a-9ab3-cb2a9e313045)
result: ![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/ca278f1c-fb64-4cdd-9ae8-165f75bfe9aa)

the F1 score is a metric for assessing classification performance, it is helpful when using unbalanced data.

### Task 12. If your models didn’t perform well, head back to the data acquisition and record additional data and/or modify your models with the knowledge you have to avoid overfitting.

### Task 13. After finding well-functioning models, you will use next an automated method for finding a good model. Use the EON Tuner2 to automatically search for a good model. This might take a while.

#### Question 11. Did the EON Tuner come up with a better model than you? If so, in which regard is it better? Is it still better when you limit it to using only accelerometer data? (To answer the latter question, first answer Question 12.)

![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/832cd71d-e160-4c36-bf78-a7ac1b9b8058)
![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/ce15e9e0-6650-47bf-afd9-f01aed9ad4ec)

The EON models didnt perform better than our model, however, it presented changes that lead to the manual discovery of a better architecture for our model. 

#### Question 12. If the EON tuner resulted in a better model, use this model as well in the Model Testing section and add its results to the plots you created in Question 10. Please note: Before doing so, save your current version with the versioning tool on the left.

![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/a60a288d-63cc-4c89-b39e-d7a438f627c8)
![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/c620b12a-62cb-4c7d-84bd-de1211bf4965)
![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/feaf35bd-f617-4429-b101-ca10d25a9be8)

### Task 14. From this task onward, we will only use your best performing model. Save your previous state with the Versioning tool in Edge Impulse, and afterward remove all blocks no longer needed from your impulse design. Train your impulse one more time.

## Part 5 – Live Classification

### Task 15. Head to Live classification and test your model with the device. Try each of your motions.

#### Question 13. Explain the output of the classification results. Does your model work? Did it misclassify some timestamps? Is this a bad thing? Why (not)?

The Model does generally work, some edge cases especially L-Shapes sometimes have misclassified timestamps. This doesnt pose any problem since the detection is by majority vote. It only becomes a problem if too many frames are misclassified.

### Task 16. Try the live classification with another Arduino board.

#### Question 14. Does the classification also work for another board? How does the performance compare when you use a board other than the one you used for collecting training data?

It does work very similar to exactly the same on other boards, since the accelerometer is likely the exact same for different boards, and even if they differ given that the sensors are calibrated properly, they will output the same values.

### Task 17. Head to Devices and add your phone as an additional device. You can just scan the QR Code for this. Now perform live classification with your phone. Question 15. Does the classification also work for your phone? If it doesn’t, why not? Does the performance change when you change the orientation of your phone? In which orientation do you have to hold your phone for it to work best?

### Question 15. Does the classification also work for your phone? If it doesn’t, why not? Does the performance change when you change the orientation of your phone? In which orientation do you have to hold your phone for it to work best?

It does work similarly on smartphones, it is more sensitive to the orientation of the phone, this is maybe due to sensor fusion being used in the phone or higher inertia of the phone when being moved. It works best in a unaligned orientation.

## Part 6 – Deployment

### Task 18. Build two Arduino Libraries for your model – one with enabling the EON Compiler and one without. For both libraries, use the quantized version.

#### Question 16. What is the EON Compiler, and why is the memory usage so different between the two libraries? Compare the models included in the two libraries (in src > tflite-model). How do they differ? What makes one of them smaller?

EON is a compiler that transforms tensorflow lite models to c++ code. It archives a smaller file size by removing meta data. 

The data preprocessing contributes the majority of the total time, also the RAM usage is larger for the preprocessing compared to the inference. because of this the total inference time and ram usage does not differ between the EON and non-EON model, only the flash usage is significantly smaller, since 

No Eon (int8): |prep. | inf.  | total
---------------|------|-------|--------
Inference:| 16ms | 1ms   | 17ms
RAM:    |   3.1K | 3.1K  | 3.1K
FLASH:  |        | 36.5k|
Accuracy: ||    |93.69%
    
EON (int8):      |prep. | inf.  | total
-----------------|------|-------|------
Inteference:| 16ms | 1ms | 17ms
RAM:        | 3.1K | 1.4K | 3.1K
FLASH:      |      | 16.8K|
Accuracy: ||   |93.69%
    

### Task 19. Include the libraries into your Arduino IDE (Add .ZIP Library...). Open theaccelerometer example that comes with your library and flash it to your board. Open a serial monitor.

#### Question 17. Explain the Arduino program and the output of the serial monitor. Also, why is there a reference to numpy in the Arduino program? How is that possible in C++? Evaluate how well and how fast the classification works for each of your motions. Is there a difference in performance between the two Arduino libraries?

The Setup of the Arduino program Starts Serial and the IMU. In the loop the IMU is read and preprocessed, and the run through our classifier. also the time taken for preprocessing, classifying, anomaly and then classification results are printed

EON:

    Starting inferencing in 2 seconds...
    Sampling...
    Predictions (DSP: 22 ms., Classification: 0 ms., Anomaly: 0 ms.): 
        Idle: 0.99609
        L-Shape: 0.00000
        O-Shape: 0.00000
        S-Shape: 0.00000
NO-EON:

    Starting inferencing in 2 seconds...
    Sampling...
    Predictions (DSP: 22 ms., Classification: 1 ms., Anomaly: 0 ms.): 
        Idle: 0.00000
        L-Shape: 0.00781
        O-Shape: 0.96094
        S-Shape: 0.03516

The EON and non-EON variant ebhave almost the same mainly because the processing time is cominated by the signal preprocessing. The serial output contains a reference time from which a sample is taken. the output it provides includes the timing used for the DSP, classification and anomaly detection (a feature we are not using). After which it gives us the classification results for all our classes. All classifications work as accurate as predicted. 

Numpy is available in c++ because the Edge Impulse SDK implemented it porting the python interfaces for its native c code.

### Task 20. Open the continuous accelerometer example that comes with your library and flash it to your board. Open a serial monitor.

#### Question 18. Explain the Arduino program and the output of the serial monitor. How does it differ from the previous program? Does it use multithreading? If so, how? How well does the continuous mode work? Under which circumstances would you use either of the two modes? What applications would require the one or the other mode?

The Setup of the Arduino program Starts Serial, the IMU, and the inference_thread used for multi threading. In the loop the IMU is sampled into a rolling buffer. the started thread runs the inference on the sampled data. THe sampling thread and inference thread run concurrently but not parallel. The continuous mode works well since the sampling window is far larger than our inference time, the continious mode would work well for scenarious where data may not be missed such as key word spotting. non continous mode would work for scenarios where the start of an event is known, such as a sensory input of some other kind, especially also when the inference time is larger than the sampling window

Predictions (DSP: 25 ms., Classification: 3 ms., Anomaly: 0 ms.): O-Shape  [ 0, 0, 7, 0, 3, 0, ]
the various timing are displayed again in a different format, the resulting inferred class is printed, and the class predictions, certainties and anomaly detection is printed in the array.

#### Question 19. Discuss the pros and cons of Edge Impulse. When should one use it and when rather not? Also elaborate on the privacy and ethical aspects of using Edge Impulse versus building a model locally.

Edge impulse makes it easier to develop edge ai models and allows for rapid prototyping. however it may lack customization (for the free version at least) and the cloud processing might a problem for sensitive data.

therefore it works well, for scenarios where rapid development is desireable while sensitive data and extensive customization without payment are not required.
