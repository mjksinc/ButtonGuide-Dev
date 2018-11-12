# teXXmoButon_IoTCentral

This document contains all the instructions for connecting your teXXmo IoT Button to Azure IoT Central

## Pre-requisites
- 1 x teXXmo Button
- A Microsoft Account or Azure Subscription
- A computer or tablet with a web browser
- A wifi connection requiring only an SSID and Password to connect

# Acknowledgements
+ [IoT Central Documentation](https://docs.microsoft.com/en-us/azure/iot-central/)

# Overview
Something as simple as a button can have a powerful actions when connected to Azure. In the example, you'll be connecting a physcial teXXmo button to Microsoft's new software-as-a-service application, IoT Central. The button will trigger an alert if pressed 3 times within a short time frame. You'll learn how to deploy your own IoT Central application, create device templates, and use the Device Provisioning Service to connect the teXXmo button to the cloud.

## Setting Up IoT Central
1. Head on over to the the [IoT Central Homepage](https://azure.microsoft.com/en-au/services/iot-central/) to learn more or select [Getting Started](https://apps.azureiotcentral.com/) to begin creating your application.
2. You'll be prompted to login in using your Microsoft account. Enter your credentials or create a new account using the links provided. 
3. Follow the prompts until you arrive at the Application Manager page. Select the "New Application" tile to get started.
4. Now you'll configure the propeorties of your new IoT Central application
  a. Select the *trial* payment plan (or the pay-as-you-go option if you want to keep the applciation lonfer than 7 days)
  b. Select *Custom Application* as the template
  c. Enter a suitable and unique *Application Name*
  d. Press *Create* when done
5. That's it! You should now the see the main dashboard of your new IoT Central Application.

## Creating a Device Template
1. Select *Create Device Templates* from the tile on the main dashboard or select *Application Builder > Create Device Template > Custom*
2. Enter a unique name for you Device Template and select *Create* when complete
3. This will generate a Simulated Device which you can use to create graphs, build dashboards, and get your application together before needing to connect a physcial device. Since we're using a physical device, we'll delete the simulated device. Select *Delete* to remove

## Creating and configuring a new physical Device
1. Go to the *Device Explorer* by selecting the icon on in the right-side menu
2. Select the Button template you created, the click *+ > Real* to create a new real device
3. Enter the following details to create the device:
  a. A unique identifier. This will be used for the Device Provisioning Service. A MAC Address or Serial Number are suitable IDs
  b. A name for your button. This could be the same as the Device ID, or something completely different like location the button will be placed
  c. Select *Create* when completed
4. You should now be on the device configuration page for the button you just created. If not, select *Device Explorer* in the right-side menu and navigate to your new device.
5. You'll now configure the device's measurements. Specifically, the device will transmit a click *event* . Select *edit template* to begin configuring the measurements
6. Make sure you're in the *measurements* tab, click *Edit Template* and select *+ New Measurement > Event* (you may need to scroll depending on the zoom level of your browser)
7. Now you'll configure the event parameters:
  a. Enter a *Display Name* for how the event will be labelled in your application
  b. Enter a *Field Name* for the measurement. __Note:__ This field name will need to be remembered for when you configure the button settings in future steps
  c. Set the *Default Severity* to any level for this use case
  d. Select *Save* when you're finished
8. A *Rule* needs to be created so your IoT Central application knows when to trigger an action. Select the *Rules* tab and click *Edit Template*.
9. To add a new rule, select *+ New Rule* then *Event*
10. Configure the rule with the following properties:
  a. *Name* should be descriptove of the rule's action to avoid confusion
  b. Select *Enable* for both the template and device options
  c. Click the *+* symbol next to *Conditions*
    i. Click the dropdown box under *Measurements* and choose the event you created earlier
    ii. For *Aggregator*, select *Count*
    iii. For *Operator*, select *is Greater Than*
    iv. Enter *3* for the threshold
  d. Select the *time duration* under the *Aggregation Time Window* as 10 minutes
  e. Click save to confirm properties
11. Now you've created the rule trigger, you'll need to create the action
  a. Scroll until you see *Actions* and click the adjacent *+* symbol.
  b. Select the email tile and enter the following details
    i. *Display Name*
    ii. Recipient addresses in the *To* field
    iii. A message to be included in the email under *Notes*
  c. Click save when complete
12. Congratualations! You've succssfully created a device in IoT Central and configured a rule to be action based on aggreagate events from the button

## Generating a SAS Token using Device Provisioning Service

# TeXXmo Button
## Configuring your button

## Connecting to Azure