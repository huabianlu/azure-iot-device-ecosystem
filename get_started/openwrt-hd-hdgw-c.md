---
platform: openwrt
device: hd industrial remote communication module hdgw series
language: c
---

Run a simple C sample on HD Industrial Remote Communication Module HDGW Series device running OpenWrt
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Tips](#tips)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect HD Industrial Remote Communication Module HDGW Series device running OpenWrt with Azure IoT SDK. This multi-step process includes:  
-   Configuring Azure IoT Hub  
-   Registering your IoT device  
-   Build and deploy Azure IoT SDK on device  

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

  1.1 Set up the ubuntu x64 machine (for cross compiling)  
  1.2 Create an Azure IoT hub, install DeviceExplorer tool, add a IoT devicce  
  1.3 Hardware - HD Industrial Remote Communication Module HDGW Series -  http://www.hdim.com.cn/infocenter/info_detail.aspx?topclass=acf17ffbe9af44228cfe8593054c5702&GUID=bbf0b35d7d7c4c06ac90a11f9cbbfd78



<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
  2.1 Use PUTTY tool connect HD Industrial Remote Communication Module HDGW Series  device to a usable network



<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
    3.1 Build SDK on ubuntu machine
    
        3.1.1 Install cmake and gcc
        
            sudo add-apt-repository ppa:george-edison55/cmake-3.x      # cmake ppa
            sudo add-apt-repository ppa:ubuntu-toolchain-r/test        # gcc ppa
            sudo apt-get update
            sudo apt-get install cmake
            sudo apt-get install gcc-4.8 g++-4.8
            sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 60 --slave /usr/bin/g++ g++ /usr/bin/g++-4.8
			
        3.1.2 Clone github repository
		
			git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git  

		3.1.3 Copy HD Industrial Remote Communication Module HDGW Series device sdk to folder ~/openwert/sdk  
		3.1.4 Navigate to the folder ~/azure-iot-sdk-c, update the following references in the CMakeLists.txt  
			...
			if(IN_OPENWRT)
    			ADD_DEFINITIONS("$ENV{TARGET_LDFLAGS}" "$ENV{TARGET_CPPFLAGS}" "$ENV{TARGET_CFLAGS}")
    			INCLUDE_DIRECTORIES("$ENV{TOOLCHAIN_DIR}/usr/include")
			endif()
			...
			
		
		3.1.5 Navigate to the folder ~/azure-iot-sdk-c/build_all/arduino, update the following references in the Makefile.iot
			
				...
				cmake -DCMAKE_FIND_ROOT_PATH="$(TOOLCHAIN_DIR)" $(PKG_BUILD_DIR)/CMakeLists.txt -DIN_OPENWRT=1 -Duse_amqp:bool=OFF -Duse_http:bool=OFF -Duse_mqtt:bool=ON -Duse_floats:bool=OFF -Duse_condition:bool=OFF
				...
				
        3.1.6 Build sdk and sample
        
            cd ~/azure-iot-sdk-c/build_all/arduino
            sudo ./build.sh
        
        3.1.7 Use "scp" command copy files to the board
		
			scp ~/openwrt/sdk/build_dir/target-mipsel_24kec+dsp_musl-1.1.11/azure-iot-sdks-1/iothub_client/samples/iothub_client_sample_mqtt/iothub_client_sample_mqtt root@192.168.11.1:/root
			
	3.2 Run the command on board

	
		3.2.1 Install libuuid
		
			opkg update
			opkg install libuuid
		
		3.2.2 Change the files as a executable file
		
			chmod +x ./iothub_client_sample_mqtt
   
    3.3 Send Device Events to IoT Hub
            
        3.3.1 Run the sample and send message on board
			
			/root/iothub_client_sample_mqtt  

    3.4 Receive messages from IoT Hub

        3.4.1 Use DeviceExplorer send message
		
			"My command to the device"
			
<a name="tips"></a>
# Tips

- If you just want to build the serializer samples, run the following commands:

  ```
  cd ./c/serializer/build/linux
  make -f makefile.linux all
  ```

<a name="NextSteps"></a>
# Next Steps

You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

-   [Manage cloud device messaging with iothub-explorer]
-   [Save IoT Hub messages to Azure data storage]
-   [Use Power BI to visualize real-time sensor data from Azure IoT Hub]
-   [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]
-   [Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]
-   [Remote monitoring and notifications with Logic Apps]   

[Manage cloud device messaging with iothub-explorer]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging
[Save IoT Hub messages to Azure data storage]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage
[Use Power BI to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi
[Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps
[Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning
[Remote monitoring and notifications with Logic Apps]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps
[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../../setup_iothub.md
[lnk-manage-iot-hub]: ../../manage_iot_hub.md

