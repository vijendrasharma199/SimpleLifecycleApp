# Documentation for Sericom Module | v1.0.5

##### A guidance document for understanding the ```Sericom``` module which helps to communicate with hardware devices to any android application.

## Purpose:
The aim of this module is to establish a communication setup between the hardware and software, so that it can work smoothly as per the required inputs by integrating in any android application.

## Introduction:
Current version of _**Sericom**_  = 1.0.5

_**Sericom**_ refers to a medium to connect hardware devices to an android app by checking the required permission(to notify the user either for acceptance or decline) by showing the pop up for permission whenever a device connects to an android application through the cable, so that it takes care of user privacy.
If the user allows it, then the device is ready for communication and can start communication according to their need by sending the command.

## Features included:
* UsbDevice connection detection and its permission handling.
* Can provide custom filter option in response to commands.
* Explicitly provide custom device configuration like baud rate, start bit, stop bit, parity bit, etc.
* **Error handling** with their own status code.


## Functionalities included & its usage:

* **Installation** : For installing the sericom module, add the following line to build.gradle file of app level.
```implementation ("in.sunfox.healthcare.commons.android.sericom:sericom:1.0.5")```

* **Initialization** : For initialization of device, a method is provided named _**Sericom.initialize(applicationContext)**_, which takes application context reference as a parameter and is used at the root level of application.

* **Setup device configuration(Optional)** : This process includes multiple function like :
  - **Baud rate**: Rate at which information is transferred in a communication channel. By default it is 115200.
  - **Data bit**: The smallest unit of data that a computer can process and store. By default it is 0x08.
  - **Parity Bit**: A simple form of error detecting code. By default it is 0x00.
  - **Stop Bit**: A bit that signals the end of an asynchronous transmission. By default it is 0x00.
  - **Request Codet**: A particular code in hex format on which a hardware can understand for communication. It may vary according to hardware devices which you want to connect. By default it is 0x22.

* **Apply Delimiter(Optional)** : If you want to apply your own custom delimiter, then you can do so by using _**Sericom.setDelimiter(‘delimiter’)**_. By default it applies ‘Y’.

* **Implement onConnectionStateChange Interface** : Now, this is the main step which you need to implement, because it includes a function which helps to return the current device status whether it is attached or connected or receiving data or disconnected or any type of connection error. You can do so by calling _**Sericom.setOnConnectionChangeListener(context, onConnectionStateChange)**_, which accepts current context and OnConnectionStateChange Interface which includes the device related status.

* **Input/Send Command** : Now, come to the main part like on which input you need what output. After implementing the onConnectionStateChange interface, you need to send a command to the device and you can do so by using _**Sericom.sendCommand()**_, then you can get a response in ```OnConnectionStateChange``` interface.

* **Release the resources** : It is also a needful step that you can’t forget. After completing the device related work you have to release the device by calling _**Sericom.clearInstance()**_, which helps to release all the required memory used by the device.


## Practical implementation of ```Sericom``` Module

### Installation
For installing the sericom module, add the following line to [build.gradle](https://developer.android.com/build) file of app level.
```kotlin
implementation ("in.sunfox.healthcare.commons.android.sericom:sericom:1.0.5")
```
### Initialization
Initialize the Sericom in ```MyApplication.kt``` which is an Application class.
```kotlin
override fun onCreate(){
    ....
    SeriCom.initialize('applicationContext')
}
```

### Setup Device Configuration (Optional):
Call the below configuration methods before implementing device connectivity using ```setConnectionChangeListener()```.
```kotlin
SeriCom.setBaudRate(115200) //by default 115200
SeriCom.setDataBit(0x08) //by default 0x08
SeriCom.setParity(0x00) //by default 0x00
SeriCom.setStopBit(0x00) //by default 0x00
SeriCom.setRequestCode(0x22) //by default 0x22
```
### Set Delimiter (Optional):
If your output contains any delimited text that you want to separate, then use  ```setDelimiter(delimiter)```.
```kotlin
SeriCom.setDelimiter(‘Y’);
```
```diff
- Note: Set the delimiter before implementing device connectivity using setOnConnectionChangeListener().
```
### Implement ```onConnectionStateChange``` Interface
Implement ```setOnConnectionChangeListener()``` to get a callback for the Spandan device’s connection state changes and receiving data.
```kotlin
SeriCom.setOnConnectionChangeListener(listener: OnConnectionStateChangeListener);
```
```OnConnectionStateChangeListener``` is defined as:
```kotlin
interface OnConnectionStateChangeListener {
   fun onDeviceAttached() //call when device is attached
   fun onDeviceConnected() //call when device is connected
   fun onReceivedData(data: String) //call when data is received
   fun onDeviceDisconnected() //call when device is disconnected
   fun onConnectionError(errorCode: DeviceErrorState, errorMessage: String) //call   when any error comes
}
```

### Send/Input Command:
#### For sending commands from the user end by call ```sendCommand()``` method, use
```kotlin
SeriCom.sendCommand(command);
```

### Release the resources:
Clear the device connection by calling ```clearInstance()``` method on the ```onTerminate()``` of ```MyApplication.kt``` application class:
```kotlin
override fun onTerminate(){
    ....
    SeriCom.clearInstance()
}
```

## Availability of ```Sericom```:
_**Sericom**_ is currently available at organization level and for it we are using a _**maven repository on github**_ where all the dependencies such as jars, library files, plugins, or other artifacts that will be required by the projects are stored.


## Issues faced during development & its solution:
* Permission response handling of system generated pop up after device connected and to overcome this we remove the hardcore device configuration and custom calling of permission function for device usage.

* Memory leaks issues in ```Sericom.kt``` file when using a companion object and to overcome it we use a parent level class for e.g we need context so instead of it we took ```applicationContext```.

* The app hangs when receiving responses continuously from the device and to overcome it we use _**Coroutines(helps to manage long running tasks)**_ which helps to do the process into a separate thread which does not block the main User Interface.


## Issue which are currently facing and its possible solution:
* Sericom is not working properly when using it in multiple activities at the same time.
  * **Reason**: Because module initialization function ```Sericom.setOnConnectionChangeListener()``` is not calling when coming back from next activity to previous activity.
  * **Possible Solution**: 
    * We need to call the above function when coming back from another activity. It can be done by calling it in ```onResume()``` of activity.
    * Do not make the ```SericomIOManager.kt``` class an object class.(Currently we are using this solution).

## Conclusion:
As per the above documentation, we got to know how **Sericom** is beneficial for implementing the communication setup between the hardware device and android application. 
Its required functionality provides us with smooth communication with less effort and customized response(by applying filtration) based on input provided by the user according to their need.



## License

[MIT](https://choosealicense.com/licenses/mit/)
