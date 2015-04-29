###Setting up Visual Studio 2015 Preview on your PC

We have created a versions file describing the supported versions of the required tools.  Use this as a blueprint for installing the required tools on your PC:

* Install Windows 10 from [here]({{site.downloadurl}})

* Install Visual Studio 2015 Preview from [here]({{site.downloadurl}}).  Choose the Custom option when you kick off the installer, and then check the option to install the Windows 10 tools.

* If you are planning to develop drivers, install the WDK from [here]({{site.downloadurl}})

* Install the IoT tools MSI from [here]({{site.downloadurl}})

* At this point, you are ready to develop apps.  Notice that the Windows IoT Core Watcher application automatically starts when you log on.  It can be used to find available Windows 10 IoT Core devices to deploy apps to.

    <img class="device-images" src="{{site.baseurl}}/images/IoTCoreWatcher.png">

### Connecting your Windows 10 IoT Core device directly to your PC & setting up Internet Connection Sharing (ICS)

In order to connect and share the internet connection in your PC with your IoT Core device, you must have the following:

* A spare Ethernet port on your development machine.  This can be either an extra PCI Ethernet card or a USB-to-Ethernet dongle.
* An Ethernet cable to link your development machine to your IoT Core device.

Follow the instructions below to enable Internet Connection Sharing (ICS) on your PC

1. Open up the control panel by right-clicking on the Windows button and selecting **Control Panel**, or by opening up a command prompt window and typing ***control.exe***
2. In the search box of the control panel, type ***adapter***
3. Under **Network and Sharing Center**, click **View network connections**
4. Right-click the connection that you want to share, and click **Properties**
5. Click the **Sharing** tab, and select the **Allow other network users to connect through this computer's Internet connection** box.

After you have enabled ICS on your PC, you can now connect your Windows 10 IoT Core device directly to your PC.  You can do it by plugging in one end of the spare Ethernet cable to the extra Ethernet port on your PC, and the other end of the cable to the Ethernet port on your IoT Core device.

Note:

* The **Sharing** tab won't be available if you have only one network connection.