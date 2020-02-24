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
