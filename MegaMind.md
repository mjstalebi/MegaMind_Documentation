# MegaMind installation!

In this document we provide step-by-step guide on how to install MegaMind on a new system. 


# Make directories and install  Alexa device SDKs
First lets make the base directories for the two device SDKs of MegaMind
```bash
	cd $HOME
	mkdir MegaMind
	cd MegaMind
	mkdir MegaMind_device_SDK1
	mkdir MegaMind_device_SDK2
```
## Install the first device SDK
Let's install Alexa device SDK for first SDK, we will repeat the same instructions for the second SDK later.
```bash
	cd MegaMind_device_SDK1
	mkdir build third-party application-necessities
	cd application-necessities
	mkdir sound-files
	
	cd $HOME/MegaMind/MegaMind_device_SDK1

	sudo apt-get install -y \
	git gcc cmake openssl clang-format libgstreamer1.0-0 gstreamer1.0-plugins-base \
	gstreamer1.0-plugins-good gstreamer1.0-plugins-bad \
	gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-doc gstreamer1.0-tools \
	pulseaudio doxygen libsqlite3-dev repo libasound2-de
```
We need to install more gstreamer packages as dependencies
```bash
	sudo  apt-get install -y \
	libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev \	
	gstreamer1.0-plugins-base gstreamer1.0-plugins-good \
	gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly \
	gstreamer1.0-libav libgstrtspserver-1.0-dev
```

```bash	
	
	sudo apt-get -y install build-essential nghttp2 libnghttp2-dev libssl-dev
	cd third-party
	
	wget https://curl.haxx.se/download/curl-7.63.0.tar.gz
	tar xzf curl-7.63.0.tar.gz
	cd curl-7.63.0
	./configure --with-nghttp2 --prefix=/usr/local --with-ssl
	make && sudo make install
	sudo ldconfig
```
To verify curl installation please run:

```bash
	curl -I https://nghttp2.org/
```
the successful response should look like this:

  ```
HTTP/2 200
date: Fri, 15 Dec 2017 18:13:26 GMT
content-type: text/html
last-modified: Sat, 25 Nov 2017 14:02:51 GMT
etag: "5a19780b-19e1"
accept-ranges: bytes
content-length: 6625
x-backend-header-rtt: 0.001021
strict-transport-security: max-age=31536000
server: nghttpx
via: 2 nghttpx
x-frame-options: SAMEORIGIN
x-xss-protection: 1; mode=block
x-content-type-options: nosniff
```

Next we install PortAudio

```bash
	cd $HOME/MegaMind/MegaMind_device_SDK1/third-party
	wget -c http://www.portaudio.com/archives/pa_stable_v190600_20161030.tgz
	tar xf pa_stable_v190600_20161030.tgz

	cd portaudio
	./configure --without-jack && make
```

Next we install the Kitt-AI wake-word detector

```bash
	cd $HOME/MegaMind/MegaMind_device_SDK1/third-party
	git clone https://github.com/Kitt-AI/snowboy.git 

```
Kitt-AI requires some packages
```bash
	sudo apt-get install -y libblas-dev liblapack-dev 
```
We need to copy one file:
```bash
	cd $HOME/MegaMind/MegaMind_device_SDK1/third-party/snowboy/resources
	cp alexa/alexa-avs-sample-app/alexa.umdl .
```

Now that we have all the packages let's clone the source code for  MegaMind_device_SDK1:
```bash
	cd $HOME/MegaMind/MegaMind_device_SDK1
	git clone -b MegaMind_SDK1  https://github.com/mjstalebi/MegaMind_DeviceSDKs.git source
```
In order to build the SDK:
```bash
	cd $HOME/MegaMind/MegaMind_device_SDK1/build/
	vim build.sh
```

And insert the following lines to build.sh:
 ```bash
	SDK_FOLDER=$HOME/MegaMind/MegaMind_device_SDK1
	cmake $SDK_FOLDER/source/avs-device-sdk \
	-DCMAKE_BUILD_TYPE=DEBUG \
	-DSENSORY_KEY_WORD_DETECTION=OFF \
	-DKITTAI_KEY_WORD_DETECTOR=ON \
	-DKITTAI_KEY_WORD_DETECTOR_LIB_PATH=$SDK_FOLDER/third-party/snowboy/lib/ubuntu64/libsnowboy-detect.a \
	-DKITTAI_KEY_WORD_DETECTOR_INCLUDE_DIR=$SDK_FOLDER/third-party/snowboy/include \
	-DGSTREAMER_MEDIA_PLAYER=ON \
	-DPORTAUDIO=ON \
	-DPORTAUDIO_LIB_PATH=$SDK_FOLDER/third-party/portaudio/lib/.libs/libportaudio.a \
	-DPORTAUDIO_INCLUDE_DIR=$SDK_FOLDER/third-party/portaudio/include \
	-DACSDK_EMIT_SENSITIVE_LOGS=ON   && make
```
  
## Install the second device SDK

```bash
	cd MegaMind_device_SDK2
	mkdir build third-party application-necessities
	cd application-necessities
	mkdir sound-files
	cd $HOME/MegaMind/MegaMind_device_SDK2
  ```
```bash
	cd $HOME/MegaMind/MegaMind_device_SDK2/third-party
	wget -c http://www.portaudio.com/archives/pa_stable_v190600_20161030.tgz
	tar xf pa_stable_v190600_20161030.tgz

	cd portaudio
	./configure --without-jack && make
```

```bash
	$HOME/MegaMind/MegaMind_device_SDK2/third-party
	git clone https://github.com/Kitt-AI/snowboy.git 

```
```bash
	cd $HOME/MegaMind/MegaMind_device_SDK2/third-party/snowboy/resources
	cp alexa/alexa-avs-sample-app/alexa.umdl .
```
```bash
	cd $HOME/MegaMind/MegaMind_device_SDK2
	git clone -b MegaMind_SDK2  https://github.com/mjstalebi/MegaMind_DeviceSDKs.git source
```
```bash
	cd $HOME/MegaMind/MegaMind_device_SDK2/build/
	vim build.sh
```

And insert the following lines to build.sh:
 ```bash
	SDK_FOLDER=$HOME/MegaMind/MegaMind_device_SDK2
	cmake $SDK_FOLDER/source/avs-device-sdk \
	-DCMAKE_BUILD_TYPE=DEBUG \
	-DSENSORY_KEY_WORD_DETECTION=OFF \
	-DKITTAI_KEY_WORD_DETECTOR=ON \
	-DKITTAI_KEY_WORD_DETECTOR_LIB_PATH=$SDK_FOLDER/third-party/snowboy/lib/ubuntu64/libsnowboy-detect.a \
	-DKITTAI_KEY_WORD_DETECTOR_INCLUDE_DIR=$SDK_FOLDER/third-party/snowboy/include \
	-DGSTREAMER_MEDIA_PLAYER=ON \
	-DPORTAUDIO=ON \
	-DPORTAUDIO_LIB_PATH=$SDK_FOLDER/third-party/portaudio/lib/.libs/libportaudio.a \
	-DPORTAUDIO_INCLUDE_DIR=$SDK_FOLDER/third-party/portaudio/include \
	-DACSDK_EMIT_SENSITIVE_LOGS=ON   && make
```


# Create Amazon developer account and register two devices with it

you can use your normal Amazon account to log-in to https://developer.amazon.com/ 

After logging-in to your Amazon account go-to https://developer.amazon.com/alexa/console/avs/products


![Pic1](https://www.ics.uci.edu/~sseyedta/Pics/Alexa1.png)
Then click on Create Product then. Fill the form as following
![enter image description here](https://www.ics.uci.edu/~sseyedta/Pics/Alexa2.png)

![enter image description here](https://www.ics.uci.edu/~sseyedta/Pics/Alexa3.png)Click Next.
Then
![enter image description here](https://www.ics.uci.edu/~sseyedta/Pics/Alexa4.png)In this page click on CREATE NEW PROFILE
![enter image description here](https://www.ics.uci.edu/~sseyedta/Pics/Alexa5.png)Fill the form as above and click Next
![enter image description here](https://www.ics.uci.edu/~sseyedta/Pics/Alexa6.png)
Copy the highlighted Client ID and keep it on your machine.
Choose I agree .. and click Finish. 
Repeat the whole process for the second device SDK. this time replace all the "1"s in the pictures with 2.  
Record the Client ID for device SDK 2, too.

# Run the device SDK

First we need to provide the Client ID for each SDK for authentication purposes.
```bash
	cd $HOME/MegaMind/MegaMind_device_SDK1/build/Integration
	vim AlexaClientSDKConfig_backup.json
```

And insert this information in it
```json
{
   "deviceInfo":{
     "deviceSerialNumber":"123456",
     "clientId":"XXXX CLIENT_ID XXXX",
     "productId":"MegaMind_device1"
   },  
   "cblAuthDelegate":{
       "databaseFilePath":"XXHOMEXX/MegaMind/MegaMind_device_SDK1/application-necessities/cblAuthDelegate.db"
   },  
   "miscDatabase":{
       "databaseFilePath":"XXHOMEXX/MegaMind/MegaMind_device_SDK1/application-necessities/miscDatabase.db"
   },  
   "alertsCapabilityAgent":{
       "databaseFilePath":"XXHOMEXX/MegaMind/MegaMind_device_SDK1/application-necessities/alerts.db"
   },  
   "settings":{
       "databaseFilePath":"XXHOMEXX/MegaMind/MegaMind_device_SDK1/application-necessities/settings.db",
       "defaultAVSClientSettings":{
           "locale":"en-US"
       }
   },  
   "certifiedSender":{
      "databaseFilePath":"XXHOMEXX/MegaMind/MegaMind_device_SDK1/application-necessities/certifiedSender.db"
   },  
   "notifications":{
       "databaseFilePath":"XXHOMEXX/MegaMind/MegaMind_device_SDK1/application-necessities/notifications.db"
   }   
}

```

Replace the "XXXX CLIENT_ID XXXX" with the client ID from the previous step. 
And replace "XXHOMEXX" with the absolute address of where you created MegaMind folder on your machine.
Repeat the same steps for Device SDK2. 

Then to run each of the Device SDKs first go to the build directory.
```bash
	cd $HOME/MegaMind/MegaMind_device_SDK1/build
	vim run.sh
```
and insert the following code into the run.sh
```bash
	rm Integration/AlexaClientSDKConfig.json
	cp Integration/AlexaClientSDKConfig_backup.json Integration/AlexaClientSDKConfig.json
	./SampleApp/src/SampleApp Integration/AlexaClientSDKConfig.json ../third-party/snowboy/resources/  NONE

```

repeat the same steps for the second SDK.

Now you can run each device SDK by only issuing run.sh

Note: the first time you run each SDK. It ask you to login to your amazon account and insert a code to complete the two step verification.

#  Install Alexa Skills Kit SDK for Python
The ASK SDK for Python requires **Python 2 (>= 2.7)** or **Python 3 (>= 3.6)**. Before continuing, make sure you have a supported version of Python installed

First you need to clone the our repository for MegaMind Skill:
```bash
cd $HOME/MegaMind
git clone https://github.com/mjstalebi/MegaMind_Skill.git
```
Then we can install the sdk and use our Skill.
```bash
cd $HOME/MegaMind/MegaMindSkill
python3 setup.py build
source run_MegaMind.sh
```
then open another terminal and 
```bash
cd $HOME/MegaMind/MegaMindSkill
source get_ngrok.sh
source ngrok_run.sh
```
Then we use the public url generated by ngrok to register our Skill with Amazon.



