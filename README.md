# Voice-Controlled-Fan-using-TinyML

The code in this repository listen's its surrounding environment and  based on the recieved audio classifies  the sound in three distinct classes that are, 

1. On 
2. Off
3. Noise

any other words beside on and off is marked as noise. The device is in listning mode while the blue light on board is on. Once the voice is recorded the dsp block calculates the spectrogram of the voice data. The spectrogram is then infered through a TinyML model to classify the recorded sound. If the classifier detects the "On" sound the red led blinks, whereas, if the recorded sound was " off" the green led blinks. Beside these on board leds a digital pin "D2" of the device also gets high or low on "on" and "off" sounds respectively. whereas,  the voltage level stays same in case of noise

https://user-images.githubusercontent.com/94599611/157850750-91dd55f0-a1ea-4a05-a44f-c9e8dca3d291.mp4

. 

## Nano 33 BLE Sense 
This repository shows how to use Arduino nano 33 BLE sense for Fan switching on and off. The Nano 33 BLE Sense (without headers) is Arduino’s 3.3V AI enabled board in the smallest available form factor: 45x18mm!

The Arduino Nano 33 BLE Sense is a completely new board on a well-known form factor. It comes with a series of embedded sensors:

1. 9 axis inertial sensor: what makes this board ideal for wearable device.
2. Humidity, and temperature sensor: to get highly accurate measurements of the environmental conditions.
3. Barometric sensor: you could make a simple weather station.
4. Microphone: to capture and analyse sound in real time  (used in this repository).
5. Gesture, proximity, light color and light intensity sensor : estimate the room’s luminosity, but also whether someone is moving close to the board.


The Arduino Nano 33 BLE Sense is an evolution of the traditional Arduino Nano, but featuring a lot more powerful processor, the nRF52840 from Nordic Semiconductors, a 32-bit ARM® Cortex®-M4 CPU running at 64 MHz. This will allow you to make larger programs than with the Arduino Uno (it has 1MB of program memory, 32 times bigger), and with a lot more variables (the RAM is 128 times bigger). The main processor includes other amazing features like Bluetooth® pairing via NFC and ultra low power consumption modes.The main feature of this board, besides the impressive selection of sensors, is the possibility of running Edge Computing applications (AI) on it using TinyML. You can create your machine learning models using TensorFlow™ Lite and upload them to your board using the Arduino IDE.
## Deployment

Take the arduino nano 33 BLE device and connect it with the relay board and Fan(Motor) as shown in the following Circuit diagram. To deploy this repository on arduino nano 33 BLE sense you must have installed the Arduino IDE and connect the device to the system using the standard USB cable that comes with the device. Press the reset button on the device twice to take it to boot loader mode. The yellow led on board starts blinking.  Once the Arduino IDE is installed open tools -->  Library manager and install all required libraries of arduino nano 33 BLE. In tools select arduino nano 33 BLE in boards. Then open sketch-->Include Library --> Add .Zip Library. Select the path of the zip library " ei-voice-data-arduino-1.0.zip"  present in this repository. Once the librray is successfully added to the path open File-->Examples-->voice_data_inferencing-->nano_ble33_sense_microphone. This will  open the code in another window  simply press upload button to write the binary file to the microcontroller. 


![Figure : Circuit Diagram](https://github.com/dlision/Voice-Controlled-Fan-using-TinyML/blob/main/Circuit%20.jpg "Figure : Circuit Diagram")

##  Data Acquistion
 60 Audio sampled at 16kHZ, 20ms each  is collected for each class through audrino nano 33 BLE and the edge impulse api. Arduino nano 33 BLE is connected with the edge impulse api using a firmware that takes device in data acquisition mode. The detailed steps for building this connection can be see on "https://docs.edgeimpulse.com/docs/arduino-nano-33-ble-sense". Among 60 samples the 85 % samples were used for training of the model whereas, the rest of the samples were used for the testing of the designed model. 
 ## Impulse Design ( Model Design) 
 The Traning data is divided in to a window size of 1000ms each. Spectrogram is calculated on each window using the dsp module for arduino. A spectrogram is a visual way of representing the signal strength, or “loudness”, of a signal over time at various frequencies present in a particular waveform. Not only can one see whether there is more or less energy at, for example, 2 Hz vs 10 Hz, but one can also see how energy levels vary over time.These spectrogram images are then fed into a machine learning model that classifies between three classes. The summary of the model is as follows


![Figure :Model_Summary](https://github.com/dlision/Voice-Controlled-Fan-using-TinyML/blob/dev/Model_Summary.png "Figure: Model Summary")

## Results 
The model is first trained and the over all traning accuracy of 86.1 % is achieved on 20 traning cycles. The loss of the model was redcued to 0.41 and the F1 score is 0.89, 0.82 and 0.87 for Noise, On and Off class respectively. The graphical representation of the confusion matrix is shown below

![Figure :Traning_results](https://github.com/dlision/Voice-Controlled-Fan-using-TinyML/blob/dev/Training%20Results.jpg)
 
 The test accuracy of the model is  78.99 % , with an F1 score of 0.93, 0.71 and 0.84 for Noise, on and off classes respectively the confusion matrix is of test is shown in the following figure. 
 ![Figure :Traning_results](https://github.com/dlision/Voice-Controlled-Fan-using-TinyML/blob/dev/TestResults.jpg) 

