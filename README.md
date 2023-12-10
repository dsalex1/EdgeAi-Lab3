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

# TODO: 

### Task 12. If your models didn’t perform well, head back to the data acquisition and record additional data and/or modify your models with the knowledge you have to avoid overfitting.

### Task 13. After finding well-functioning models, you will use next an automated method for finding a good model. Use the EON Tuner2 to automatically search for a good model. This might take a while.

#### Question 11. Did the EON Tuner come up with a better model than you? If so, in which regard is it better? Is it still better when you limit it to using only accelerometer data? (To answer the latter question, first answer Question 12.)
![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/832cd71d-e160-4c36-bf78-a7ac1b9b8058)
![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/ce15e9e0-6650-47bf-afd9-f01aed9ad4ec)

# TODO: 

#### Question 12. If the EON tuner resulted in a better model, use this model as well in the Model Testing section and add its results to the plots you created in Question 10. Please note: Before doing so, save your current version with the versioning tool on the left.

![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/a60a288d-63cc-4c89-b39e-d7a438f627c8)
![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/c620b12a-62cb-4c7d-84bd-de1211bf4965)
![image](https://github.com/dsalex1/EdgeAi-Lab3/assets/25539263/feaf35bd-f617-4429-b101-ca10d25a9be8)

# TODO: 

### Task 14. From this task onward, we will only use your best performing model. Save your previous state with the Versioning tool in Edge Impulse, and afterward remove all blocks no longer needed from your impulse design. Train your impulse one more time.

## Part 5 – Live Classification

### Task 15. Head to Live classification and test your model with the device. Try each of your motions.

#### Question 13. Explain the output of the classification results. Does your model work? Did it misclassify some timestamps? Is this a bad thing? Why (not)?

### Task 16. Try the live classification with another Arduino board.

#### Question 14. Does the classification also work for another board? How does the performance compare when you use a board other than the one you used for collecting training data?

### Task 17. Head to Devices and add your phone as an additional device. You can just scan the QR Code for this. Now perform live classification with your phone. Question 15. Does the classification also work for your phone? If it doesn’t, why not? Does the performance change when you change the orientation of your phone? In which orientation do you have to hold your phone for it to work best?

## Part 6 – Deployment

### Task 18. Build two Arduino Libraries for your model – one with enabling the EON Compiler and one without. For both libraries, use the quantized version.

#### Question 16. What is the EON Compiler, and why is the memory usage so different between the two libraries? Compare the models included in the two libraries (in src > tflite-model). How do they differ? What makes one of them smaller?

### Task 19. Include the libraries into your Arduino IDE (Add .ZIP Library...). Open theaccelerometer example that comes with your library and flash it to your board. Open a serial monitor.

#### Question 17. Explain the Arduino program and the output of the serial monitor. Also, why is there a reference to numpy in the Arduino program? How is that possible in C++? Evaluate how well and how fast the classification works for each of your motions. Is there a difference in performance between the two Arduino libraries?

### Task 20. Open the continuous accelerometer example that comes with your library and flash it to your board. Open a serial monitor.

#### Question 18. Explain the Arduino program and the output of the serial monitor. How does it differ from the previous program? Does it use multithreading? If so, how? How well does the continuous mode work? Under which circumstances would you use either of the two modes? What applications would require the one or the other mode?

#### Question 19. Discuss the pros and cons of Edge Impulse. When should one use it and when rather not? Also elaborate on the privacy and ethical aspects of using Edge Impulse versus building a model locally.

