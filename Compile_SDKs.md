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
	pulseaudio doxygen libsqlite3-dev repo libasound2-dev
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
	cmake $SDK_FOLDER/source \
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
	cmake $SDK_FOLDER/source \
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
