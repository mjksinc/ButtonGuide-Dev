# teXXmoButon_IoTCentral

This document contains all the instructions for connecting your teXXmo IoT Button to Azure IoT Central and triggering an action based on the number of clicks.

## Pre-requisites
- 1 x teXXmo Button
- A Microsoft Account or Azure Subscription
- A computer or tablet with a web browser
- A wifi connection requiring only an SSID and Password to connect

# Acknowledgements
+ [IoT Central Documentation](https://docs.microsoft.com/en-us/azure/iot-central/)
+ [teXXmo Button Page](https://catalog.azureiotsolutions.com/details?title=teXXmo-IoT-Button&source=home-page)
+ [Device Provisioning Service GitHub](https://github.com/Azure/dps-keygen)

# Overview
Something as simple as a button can have powerful actions when connected to Azure. In this example, you'll be connecting a physcial teXXmo button to Microsoft's new software-as-a-service application, IoT Central. The button will trigger an alert if pressed 3 times within a short time frame. You'll learn how to deploy your own IoT Central application, create device templates, and use the Device Provisioning Service to connect the teXXmo button to your application.

## Setting Up IoT Central
1. Head on over to the the [IoT Central Homepage](https://azure.microsoft.com/en-au/services/iot-central/) to learn more or select [Getting Started](https://apps.azureiotcentral.com/) to begin creating your application.

![IoT Central Main Page](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image1.1.png)

2. You'll be prompted to login in using your Microsoft account. Enter your credentials or create a new account using the links provided. 

![Microsoft Account login page](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image1.2.png | width=10)

3. Follow the prompts until you arrive at the Application Manager page. Select the **New Application** tile to get started.

![IoT Central - New Application](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image1.3.png)

4. Now you'll configure the propeorties of your new IoT Central application
  a. Select the *trial* payment plan (or the pay-as-you-go option if you want to keep the applciation longer than 7 days)
  b. Select *Custom Application* as the template
  c. Enter a suitable *Application Name* and use a unique *URL*
  d. Press *Create* when done

![IoT Central - Configuration Page](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image1.4.png)

5. That's it! You should now the see the main dashboard of your new IoT Central Application.

![IoT Central - Application Home Page](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image1.5.png)

## Creating a Device Template
1. Select *Create Device Templates* from the tile on the main dashboard or select *Application Builder > Create Device Template > Custom*

![IoT Central - Application Builder, Template](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.1.png)

2. Enter a unique name for you Device Template and select *Create* when complete

![IoT Central - Create Template](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.1.png)

3. This will generate a Simulated Device which you can use to create graphs, build dashboards, and get your application together before needing to connect a physcial device. Since we're using a physical device, we'll delete the simulated device. Select *Delete* to remove

![IoT Central - Delete Simulated](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.2.png)

## Creating and configuring a new physical Device
1. Go to the *Device Explorer* by selecting the icon on in the right-side menu

- [ ] *TODO* Fix Flow

2. Select the Button template you created, the click *+ > Real* to create a new real device

- [ ] *TODO* Add Image of creating real device

3. Enter the following details to create the device:
  a. A unique identifier. This will be used for the Device Provisioning Service. A MAC Address or Serial Number are suitable IDs
  b. A name for your button. This could be the same as the Device ID, or something completely different like location the button will be placed
  c. Select *Create* when completed

![IoT Central - Create Device Template](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.4.png)

4. You should now be on the device configuration page for the button you just created. If not, select *Device Explorer* in the right-side menu and navigate to your new device.

![IoT Central - View Device Template](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.5.png)

5. You'll now configure the device's measurements. Specifically, the device will transmit a click *event*. Make sure you're in the *measurements* tab, click *Edit Template*

![IoT Central - Edit Measurement](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.5.1.png)

6. Select *+ New Measurement > Event* (you may need to scroll depending on the zoom level of your browser)

![IoT Central - Create Event](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.6.png)

7. Now you'll configure the event parameters:
  a. Enter a *Display Name* for how the event will be labelled in your application
  b. Enter a *Field Name* for the measurement. __Note:__ This field name will need to be remembered for when you configure the button settings in future steps
  c. Set the *Default Severity* to any level for this use case
  d. Select *Save* when you're finished

![IoT Central - Event Details](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.7.png)

8. A *Rule* needs to be created so your IoT Central application knows when to trigger an action. Select the *Rules* tab and click *Edit Template*.

![IoT Central - Create Rule](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.8.png)

9. To add a new rule, select *+ New Rule* then *Event*

![IoT Central - Edit Rule](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.9.png)

10. Configure the rule with the following properties:
  a. *Name* should be descriptove of the rule's action to avoid confusion
  b. Select *Enable* for both the template and device options
  c. Click the *+* symbol next to *Conditions*
    i. Click the dropdown box under *Measurements* and choose the event you created earlier
    ii. For *Aggregator*, select *Count*
    iii. For *Operator*, select *is Greater Than or Equal to*
    iv. Enter *2* for the threshold
  d. Select the *time duration* under the *Aggregation Time Window* as 5 minutes
  e. Click save to confirm properties

![IoT Central - Rule Details](https://github.com/mjksinc/ButtonGuide-Dev/blob/master/images/Image2.10.png)

11. Now you've created the rule trigger, you'll need to create the action
  a. Scroll until you see *Actions* and click the adjacent *+* symbol.
  b. Select the email tile and enter the following details
    i. *Display Name*
    ii. Recipient addresses in the *To* field
    iii. A message to be included in the email under *Notes*
  c. Click save when complete
12. Congratualations! You've succssfully created a device in IoT Central and configured a rule to be actioned based on events from the button

## Generating a SAS Token using Device Provisioning Service
1. Now you'll need to copy some credentials to connect your device. Return to the *Device Explorer* page by selecting the icon in the right-side menu
2. Select the device template, then the device you created previously
3. Click *Connect* on the device screen. This will generate credentials for you to create a connection string through the Azure Device Provisioning Service (DPS).
4. Save the following credentials for later use:
  a. Under **Device Connection**, copy the *Scope ID* and *Device ID*
  b. Under **Credentials**, select *Shared Access Signature*, then copy the *Primary ID*
5. Select *Close* when complete

- [ ] *TODO* add Device Provisioning Steps

X. Now you've generated your connection string, it's time to connect your teXXmo button

# TeXXmo Button
Note: these steps are also available under "*Getting Started*" [on the teXXmo page](https://catalog.azureiotsolutions.com/details?title=teXXmo-IoT-Button&source=home-page) of the Azure Device Catalog

## Configuring your button
1. Put the teXXmo button into Access Point (AP) mode by pressing and holding the button for 6 seconds. The LED will change to a yellow flashing strobe, then to a pulsing red.
    __Note:__ if the button begins to rapidly flash green, wait until it stops, then try again.
2. Open the network connection settings on your desktop and select the wifi network beginning with *ESP_XX:XX:XX* (The numbers will match the MAC address of your button). The light will continue to pulse red while in AP mode. 
    __Note:__ this will disconnect you from the internet
3. Open a web browser and go to *192.168.4.1*. You should arrive at the homepage for your button.
4. Select the *IoT Hub Configuration* tab at the top left of the screen. This is where we'll break down the connection string generated in the previous section:
  a. Under *Azure IoT Hub* copy the *Hostname*
  b. Under *IoT device name* copy the *Device ID*
  c. Under *IoT device secret* copy the *Shared Access Key*
5. Select the *WIFI* tab at the top of the screen and enter the SSID and Password for your wifi network to connect the button to the internet. Select *Save* when complete
6. Next, click the *User JSON* tab. This is where you can write the JSON message the device will deliver when clicked. Enter the text below and click *Save* when complete
  `{"click": "1"}
  __Note:__ the key (in this case, "click") should match the Field Name you entered into IoT Central previously
7. Now select the *Shutdown* tab so exit the homepage and save your configuration to the button
8. Reconnect to your original wifi network and return to your IoT Central Application

## Sending Telemetry and Visualising in IoT Central
1. Now the physical button has been connected, return to the device page in your IoT Central Application by clicking *Device Explorer*, then the device template and device created previously
2. Click the *Measurements* tab to display the previously-created Event measurment.
3. Push and hold the physical teXXmo button for ~1 second then release. You should see the green LED frantically flash, then emit a single green pulse to signify successful data transmission
4. After a few seconds, the event should plot onto the Measurements graph. Congratulations - you sent your first message to your IoT Central Application!
5. Press the buttton several more times to trigger the rule you created. This should trigger the email action you created earlier
__Note:__IoT Central checks aggregation in tumbling windows starting on the hour. This means you may not see an emaail alert for at least 11 minutes.
6. Now you've set up your IoT Central Application, explore some of other features such as the [Creating a Webhook Alert](https://docs.microsoft.com/en-au/azure/iot-central/howto-create-webhooks) or [Exporting Data](https://docs.microsoft.com/en-au/azure/iot-central/howto-export-data)