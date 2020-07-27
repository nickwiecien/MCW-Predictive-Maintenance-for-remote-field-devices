![Microsoft Cloud Workshops](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png 'Microsoft Cloud Workshops')

<div class="MCWHeader1">
Predictive Maintenance for remote field devices
</div>

<div class="MCWHeader2">
Hands-on lab step-by-step
</div>

<div class="MCWHeader3">
May 2020
</div>

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2020 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [Predictive Maintenance for remote field devices hands-on lab step-by-step](#predictive-maintenance-for-remote-field-devices-hands-on-lab-step-by-step)
  - [Abstract and learning objectives](#abstract-and-learning-objectives)
  - [Overview](#overview)
  - [Solution architecture](#solution-architecture)
  - [Requirements](#requirements)
  - [Before the hands-on lab](#before-the-hands-on-lab)
  - [Exercise 1: Configuring IoT Central with devices and metadata](#exercise-1-configuring-iot-central-with-devices-and-metadata)
    - [Task 1: Model the telemetry data](#task-1-model-the-telemetry-data)
      - [Telemetry schema](#telemetry-schema)
    - [Task 2: Create an IoT Central application](#task-2-create-an-iot-central-application)
    - [Task 3: Create the Device Template](#task-3-create-the-device-template)
    - [Task 4: Create and provision real devices](#task-4-create-and-provision-real-devices)
    - [Task 5: Delete the simulated device](#task-5-delete-the-simulated-device)
  - [Exercise 2: Run the Rod Pump Simulator](#exercise-2-run-the-rod-pump-simulator)
    - [Task 1: Generate device connection strings](#task-1-generate-device-connection-strings)
    - [Task 2: Open the Visual Studio solution, and update connection string values](#task-2-open-the-visual-studio-solution-and-update-connection-string-values)
    - [Task 3: Run the application](#task-3-run-the-application)
    - [Task 4: Interpret telemetry data](#task-4-interpret-telemetry-data)
    - [Task 5: Restart a failing pump remotely](#task-5-restart-a-failing-pump-remotely)
  - [Exercise 3: Creating a device set](#exercise-3-creating-a-device-set)
    - [Task 1: Create a device set using a filter](#task-1-create-a-device-set-using-a-filter)
  - [Exercise 4: Creating a useful dashboard](#exercise-4-creating-a-useful-dashboard)
    - [Task 1: Clearing out the default dashboard](#task-1-clearing-out-the-default-dashboard)
    - [Task 2: Add your company logo](#task-2-add-your-company-logo)
    - [Task 3: Add a list of Texas Rod Pumps](#task-3-add-a-list-of-texas-rod-pumps)
    - [Task 4: Add a map displaying the power state of DEVICE001](#task-4-add-a-map-displaying-the-power-state-of-device001)
  - [Exercise 5: Create an Event Hub and continuously export data from IoT Central](#exercise-5-create-an-event-hub-and-continuously-export-data-from-iot-central)
    - [Task 1: Create an Event Hub](#task-1-create-an-event-hub)
    - [Task 2: Configure continuous data export from IoT Central](#task-2-configure-continuous-data-export-from-iot-central)
  - [Exercise 6: Use Azure Databricks and Azure Machine Learning service to train and deploy predictive model](#exercise-6-use-azure-databricks-and-azure-machine-learning-service-to-train-and-deploy-predictive-model)
    - [Task 1: Run the Anomaly Detection notebook](#task-1-run-the-anomaly-detection-notebook)
  - [Exercise 7: Create an Azure Function to predict pump failure](#exercise-7-create-an-azure-function-to-predict-pump-failure)
    - [Task 1: Create an Azure Function Application](#task-1-create-an-azure-function-application)
    - [Task 2: Create a notification table in Azure Storage](#task-2-create-a-notification-table-in-azure-storage)
    - [Task 3: Create a notification queue in Azure Storage](#task-3-create-a-notification-queue-in-azure-storage)
    - [Task 4: Create notification service in Logic Apps](#task-4-create-notification-service-in-logic-apps)
    - [Task 5: Obtain connection settings for use with the Azure Function implementation](#task-5-obtain-connection-settings-for-use-with-the-azure-function-implementation)
    - [Task 6: Create the local settings file for the Azure Functions project](#task-6-create-the-local-settings-file-for-the-azure-functions-project)
    - [Task 7: Review the Azure Function code](#task-7-review-the-azure-function-code)
    - [Task 8: Run the Function App locally](#task-8-run-the-function-app-locally)
    - [Task 9: Prepare the Azure Function App with settings](#task-9-prepare-the-azure-function-app-with-settings)
    - [Task 10: Deploy the Function App into Azure](#task-10-deploy-the-function-app-into-azure)
  - [After the hands-on lab](#after-the-hands-on-lab)
    - [Task 1: Delete Lab Resources](#task-1-delete-lab-resources)

<!-- /TOC -->

# Predictive Maintenance for remote field devices hands-on lab step-by-step

## Abstract and learning objectives

In this hands-on-lab, you will build an end-to-end industrial IoT solution. We will begin by leveraging the Azure IoT Central SaaS offerings to quickly stand up a fully functional remote monitoring solution. Azure IoT Central provides solutions built upon recommendations found in the [Azure IoT Reference Architecture](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/iot/). We will customize this system specifically for rod pumps. Rod pumps are industrial equipment that is heavily used in the oil and gas industry. We will then establish a model for the telemetry data that is received from the pump systems in the field and use this model to deploy simulated devices for system testing purposes.

Furthermore, we will establish threshold rules in the remote monitoring system that will monitor the incoming telemetry data to ensure all equipment is running optimally, and can alert us whenever the equipment is running outside of normal boundaries - indicating the need for alternative running parameters, maintenance, or a complete shutdown of the pump. By leveraging the IoT Central solution, users can also issue commands to the pumps from a remote location in an instant to automate many operational and maintenance tasks which used to require staff on-site. This lessens operating costs associated with technician dispatch and equipment damage due to a failure.

Above and beyond real-time monitoring and mitigating immediate equipment damage through commanding - you will also learn how to apply the historical telemetry data accumulated to identify positive and negative trends that can be used to adjust daily operations for higher throughput and reliability.

## Overview

The Predictive Maintenance for Remote Field Devices hands-on lab is an exercise that will challenge you to implement an end-to-end scenario using the supplied example that is based on Azure IoT Central and other related Azure services. The hands-on lab can be implemented on your own, but it is highly recommended to pair up with other members at the lab to model a real-world experience and to allow each member to share their expertise for the overall solution.

## Solution architecture

![The architecture diagram shows the components of the preferred solution.](media/preferred-solution.png "High-level architecture")

[Azure IoT Central](https://docs.microsoft.com/azure/iot-central/overview-iot-central) is at the core of the preferred solution. It is used for data ingest, device management, data storage, and reporting. IoT field devices securely connect to IoT Central through its cloud gateway. The continuous export component sends device telemetry data to [Azure Blob storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) for cold storage, and the same data to [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-about) for real-time processing. Azure Databricks uses the data stored in cold storage to periodically re-train a Machine Learning (ML) model to detect oil pump failures. It is also used to deploy the trained model to a web service hosted by [Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/) (AKS) or [Azure Container Instances](https://docs.microsoft.com/azure/container-instances/) (ACI), using [Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/overview-what-is-azure-ml). An [Azure function](https://docs.microsoft.com/azure/azure-functions/functions-overview) is triggered by events flowing through Event Hubs. It sends the event data for each pump to the web service hosting the deployed model, then sends an alert through [Azure Logic Apps](https://azure.microsoft.com/en-us/services/logic-apps/) if an alert has not been sent within a configurable period of time. The alert is sent in the form of an email, identifying the failing oil pump with a suggestion to service the device.

_Azure IoT Central architecture_

![The architecture diagram illustrates the components of Azure IoT Central.](media/arch-diagram.png "IoT Central architecture")

The diagram above shows the components of IoT Central's architecture that pertain to Fabrikam's use case. Because it is a SaaS-based solution, the Azure services IoT Central uses are hidden from view. IoT field devices securely connect to IoT Central through its cloud gateway. The gateway uses Azure IoT Hub Device Provisioning Service (DPS) to streamline device management, and the underlying IoT Hub service facilitates bi-directional communication between the cloud and IoT devices. All device telemetry is stored in a time series data store, based on Azure Time Series Insights. The application data store persists the IoT Central application and its customizations. This application provides a user interface shell, through which Fabrikam's users manage devices and associated metadata, view dashboards and reports, and configure rules and actions to react to device telemetry that indicates possible rod pump failure. Pervasive throughout the end-to-end solution is security in transit and at rest for devices and the web-based management application. The continuous data export feature is used to enable hot and cold path workloads on real-time and batch device telemetry and metadata, using external services. Batch data is exported to Azure Blob storage in Apache Avro file format each minute, and real-time data is exported to either Azure Event Hubs or Azure Service Bus.

## Requirements

1. Microsoft Azure subscription (non-Microsoft subscription, must be a pay-as-you subscription).
2. [.NET Core 3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1)
3. [Visual Studio Code](https://code.visualstudio.com/) version 1.39 or greater
4. [C# Visual Studio Code Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
5. [Azure Functions Core Tools version 2.x (using NPM or Chocolatey - see readme on GitHub repository)](https://github.com/Azure/azure-functions-core-tools)
6. [Azure Functions Visual Studio Code Extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
7. An Azure Databricks cluster running Databricks Runtime 5.1 or above.

## Before the hands-on lab

Refer to the [Before the hands-on lab setup guide](./Before%20the%20HOL%20-%20Predictive%20Maintenance%20for%20Remote%20Field%20Devices.md) manual before continuing to the lab exercises.

## Exercise 1: Configuring IoT Central with devices and metadata

Duration: 45 minutes

[Azure IoT Central](https://azure.microsoft.com/en-us/services/iot-central/) is a Software as a Service (SaaS) offering from Microsoft. The aim of this service is to provide a frictionless entry into the Cloud Computing and IoT space. The core focus of many industrial companies is not on cloud computing; therefore, they do not necessarily have the personnel skilled to provide guidance and to stand up a reliable and scalable infrastructure for an IoT solution. It is imperative for these types of companies to enter the IoT space not only for the cost savings associated with remote monitoring, but also to improve safety for their workers and the environment.

Fabrikam is one such company that could use a helping hand entering the IoT space. They have recently invested in sensor technology on their rod pumps in the field, and they are ready to implement their cloud-based IoT Solution. Their IT department is small and unfamiliar with cloud-based IoT infrastructure; their IT budget also does not afford them the luxury of hiring a team of contractors to build out a solution for them.

The Fabrikam CIO has recently heard of Azure IoT Central. This online offering will streamline the process of them getting their sensor data to the cloud, where they can monitor their equipment for failures and improve their maintenance practices and not have to worry about the underlying infrastructure. A [predictable cost model](https://azure.microsoft.com/en-us/pricing/details/iot-central/) also ensures that there are no financial surprises.

### Task 1: Model the telemetry data

The first task is to identify the data that the equipment will be sending to the cloud. This data will contain fields that represent the data read from the sensors at a specific instant in time. This data will be used downstream systems to identify patterns that can lead to cost savings, increased safety and more efficient work processes.

The telemetry being reported by the Fabrikam rod pumps are as follows, we will be using this information later in the lab:

#### Telemetry schema

| Field          | Type     | Description                                                                                                                                                                        |
| -------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SerialNumber   | String   | Unique serial number identifying the rod pump equipment                                                                                                                            |
| IPAddress      | String   | Current IP Address                                                                                                                                                                 |
| TimeStamp      | DateTime | Timestamp in UTC identifying the point in time the telemetry was created                                                                                                           |
| PumpRate       | Numeric  | Speed calculated over the time duration between the last two times the crank arm has passed the proximity sensor measured in Strokes Per Minute (SPM) - minimum 0.0, maximum 100.0 |
| TimePumpOn     | Numeric  | Number of minutes the pump has been on                                                                                                                                             |
| MotorPowerkW   | Numeric  | Measured in Kilowatts (kW)                                                                                                                                                         |
| MotorSpeed     | Numeric  | including slip (RPM)                                                                                                                                                               |
| CasingFriction | Numeric  | Measured in PSI (psi)                                                                                                                                                              |

### Task 2: Create an IoT Central application

1. Access the [Azure IoT Central](https://azure.microsoft.com/en-us/services/iot-central/) website.

2. Select the _Build a solution_ button.

    ![The browser screen shows the Azure products filtered to Azure IoT Central. An arrow points to the Build a solution button.](media/azure-iot-central-website.png "IoT Central Getting started")

3. If you are not currently logged in, you will be prompted to log in with your Microsoft Azure Account.

4. Select **My Apps** from the left-hand menu, then select the **New application** button.

    ![The screens displays the list of my applications and options for creating a new application.](media/azure-iot-central-new-application.png "New Application")

5. Select **Custom app**.

    ![The custom app option is selected.](media/azure-iot-central-custom-app.png "Custom app")

6. Fill in the provisioning form.

    ![The screens displays the new application configuration options. The Trial payment plan is selected.](media/custom-app-creation-form.png "Provision application form")

    a. *Application Name* - give your application a name of your choice, in this example, we used *fabrikam-oil*.

    b. *URL* - this will be the URL for your application, it needs to be globally unique.

    c. *Application Template* - select *Custom application (legacy)*.

    d. *Pricing plan* - select the *Free* option to enable the free 7-day trial.

    e. Fill out your contact information (First Name, Last Name, Email Address, Phone Number, Country/Region).

    f. Select the *Create* button to provision your application.

### Task 3: Create the Device Template

1. Once the application has been provisioned, we need to define the type of equipment we are using, and the data associated with the equipment. In order to do this, we must define a _Device Template_. Select either the _Create Device Templates_ button, or the _Device Templates_ menu item from the left-hand menu.

    ![The screen displays the options available for a dashboard. Red arrows point to Create Device Templates.](media/create-device-templates.png "Device Templates")

2. Select _Custom_ to define our own type of hardware.

    ![The screen shows the Device Template menu item is selected. The device template tile option for the Custom template is circled.](media/new-template-custom.png "Custom Template")

3. For the device template name, enter _Rod Pump_, then select the _Create_ button.

    ![The screen shows the form to create new custom device templates.](media/rod-pump-template-create.png "Create Rod Pump Template")

4. The next thing we need to do is define the measurements that will be received from the device. To do this, select the _New_ button at the top of the left-hand menu.

    ![The screen displays the configuration menu for the Rod Pump 1. The Measurements menu item is selected and the Add New menu is selected.](media/new-measurement.png "New Measurement")

5. From the context menu, select _Telemetry_.

    ![The screen displays the configuration menu for the Rod Pump 1. The Measurements menu item is selected and the Add New menu displays the options available. The Telemetry item is selected.](media/new-telemetry-measurement.png "New Telemetry Measurement")

6. Create Telemetry values as follows:

    ![The screen displays the Create Telemetry configuration options.](media/telemetry-data.png "Telemetry Data")

    | Display Name    | Field Name     | Units   | Min. Value | Max. Value | Decimal Places |
    | --------------- | -------------- | ------- | ---------- | ---------- | -------------- |
    | Pump Rate       | PumpRate       | SPM     | 0          | 100        | 1              |
    | Time Pump On    | TimePumpOn     | Minutes | 0          |            | 2              |
    | Motor Power     | MotorPowerKw   | kW      | 0          | 90         | 2              |
    | Motor Speed     | MotorSpeed     | RPM     | 0          | 300        | 0              |
    | Casing Friction | CasingFriction | PSI     | 0          | 1600       | 2              |

    ![A line chart displays the Rod Pump 1 telemetry measurements.](media/telemetry-defined.png "Telemetry Defined")

7. Remaining on the measurement tab, we also need to define the current state of the pump, whether it is running or not. Select _New_ and select _State_.

    ![The screen displays the configuration menu for the Rod Pump 1. The Measurements menu item is selected and the Add New menu displays the options available. The State item is selected.](media/device-template-add-state.png "Add State")

8. Add the state with the display name of _Power State_, field name of _PowerState_ with the values _Unavailable_, _On_, and _Off_, then select the _Save_ button.

    ![The screen displays the state configuration options.](media/power-state-definition.png "Power State")

9. In the device template, Properties are read-only metadata associated with the equipment. For our template, we will expect a property for Serial Number, IP Address, and the geographic location of the pump. From the top menu, select _Properties_, then _Device Property_ from the left-hand menu.

    ![The screen displays the configuration menu for the Rod Pump 1. The Properties menu item is selected and the Device Property menu item link is circled.](media/device-properties-menu.png "Device Properties Menu")

10. Define the device properties as follows:

    ![The screen displays the configure device properties.](media/device-properties-form.png "Device Properties Form")

    | Display Name  | Field Name   | Data Type | Description                       |
    | ------------- | ------------ | --------- | --------------------------------- |
    | Serial Number | SerialNumber | text      | The Serial Number of the rod pump |
    | IP Address    | IPAddress    | text      | The IP address of the rod pump    |
    | Pump Location | Location     | location  | The geo. location of the rod pump |

11. Operators and field workers will want to be able to turn on and off the pumps remotely. In order to do this, we will define commands. Select the _Commands_ tab and select the _New_ button to add a new command.

    ![The screen displays the configuration menu for the Rod Pump 1. The Commands menu item is selected and the New Command link is circled.](media/device-template-add-command.png "Add Command")

12. Create a command as follows, and select _save_:

    1. _Display Name_ - _Toggle Motor Power_
    2. _Field Name_ - _MotorPower_
    3. _Default Timeout_ - _30_
    4. _Data Type_ - _toggle_
    5. _Description_ - _Toggle the motor power of the pump on and off_

    ![The screen shows the configure command options. The save button is circled.](media/template-configure-command.png "Configure Command")

13. Now, we can define the dashboard by pressing the _Dashboard_ option in the top menu and selecting _Line Chart_ from the left-hand menu. Define a line chart for each of the telemetry fields (PumpRate, TimePumpOn, MotorPower, MotorSpeed, CasingFriction) - keeping all the default values:

    ![The screen shows the Dashboard menu item is selected and circled. The Library menu has the Line Chart menu item selected.](media/line-chart.png "Line Chart")

    ![The line chart configurations are displayed. The Pump Rate measurements has focus.](media/line-chart-form.png "Line Chart Form")

14. A map is also available for display on the dashboard. Remaining on the Dashboard tab, select _Map_.

    ![The device template library menu items are displayed. The Map menu item is circled.](media/dashboard-map.png "Dashboard Map")

15. Fill out the values for the map settings as follows:

    ![The device template options are displayed. The save button is circled.](media/dashboard-map-settings.png "Dashboard Map Form")

    After saving, the map will appear on the dashboard as shown:

    ![The dashboard configuration options are displayed.](media/dashboard-charts-definition.png "Dashboard Charts Definition")

16. Finally, we can add an image to represent the equipment. Select on the circle icon left of the template name and select an image file. The image used in this lab can be found on [PixaBay](https://pixabay.com/photos/pumpjack-texas-oil-rig-pump-591934/).

    ![A Rod Pump 1 device is displayed. The device picture has focus.](media/device-template-thumbnail.png "Device Template Thumbnail")

17. Review the application template by viewing its simulated device. IoT Central automatically creates a simulated device based on the template you've created. From the left-hand menu, select _Devices_. In this list you will see a simulated device for the template that we have just created. Select the link for this simulated device, the charts will show a sampling of simulated data.

    ![A rod pump list is displayed. Rod Pump 1 is circled.](media/iot-central-simulated-rod-pump.png "Device List - Simulated")

    ![The Rod Pump 1.0.0 telemetry measurements are displayed in line graph.](media/simulated-measurements.png "Simulated Measurements")

### Task 4: Create and provision real devices

Under the hood, Azure IoT Central uses the [Azure IoT Hub Device Provisioning Service (DPS)](https://docs.microsoft.com/en-us/azure/iot-dps/). The aim of DPS is to provide a consistent way to connect devices to the Azure Cloud. Devices can utilize Shared Access Signatures, or X.509 certificates to securely connect to IoT Central.

[Multiple options](https://docs.microsoft.com/en-us/azure/iot-central/concepts-connectivity) exist to register devices in IoT Central, ranging from individual device registration to [bulk device registration](https://docs.microsoft.com/en-us/azure/iot-central/concepts-connectivity#connect-devices-at-scale-using-sas) via a comma delimited file. In this lab we will register a single device using SAS.

1. In the left-hand menu of your IoT Central application, select _Devices_.

2. Select the _Rod Pump (1.0.0)_ template. This will now show the list of existing devices which at this time includes only the simulated device.

3. Select the _+_ button to add a new device, select _Real_.

    ![The device explorer screen shows the devices that are unassociated. The filter for real and simulated devices is circled.](media/add-real-device-menu.png "Add a real device menu")

4. A modal window will be displayed with an automatically generated Device ID and Device Name. You are able to overwrite these values with anything that makes sense in your downstream systems. We will be creating three real devices in this lab. Create the following as real devices:

    ![The create new device manager configuration fields are displayed.](media/real-device-id.png "Real Device ID and Name")

    | Device ID | Device Name          |
    | --------- | -------------------- |
    | DEVICE001 | Rod Pump - DEVICE001 |
    | DEVICE002 | Rod Pump - DEVICE002 |
    | DEVICE003 | Rod Pump - DEVICE003 |

5. Return to the Devices list by selecting _Devices_ in the left-hand menu. Note how all three real devices have the provisioning status of _Registered_.

    ![All of the devices are listed and their associated provisioning status. The devices show as registered.](media/new-devices-registered.png "Real Devices Registered")

### Task 5: Delete the simulated device

Now that we have registered real devices, we will no longer be needing the simulated pump that was created for us when we defined our template.

1. From the Devices list, check the checkbox next to the simulated pump, then select the **Delete** button.

    ![All of the devices are listed.  The simulated device is selected and circled for deletion. The delete link is circled.](media/delete-simulated-device.png "Delete Simulated Device")

## Exercise 2: Run the Rod Pump Simulator

Duration: 30 minutes

Included with this lab is source code that will simulate the connection and telemetry of three real pumps. In the previous exercise, we have defined them as DEVICE001, DEVICE002, and DEVICE003. The purpose of the simulator is to demonstrate real-world scenarios that include a normal healthy rod pump (DEVICE002), a gradually failing pump (DEVICE001), and an immediately failing pump (DEVICE003).

### Task 1: Generate device connection strings

1. In IoT Central, select _Devices_ from the left-hand menu. Then, from the devices list, select the link for _Rod Pump - DEVICE001_, and select the _Connect_ button located in the upper right corner of the device's page. Make note of the Scope ID, Device ID, as well as the primary and secondary key values.

    ![The device connection key information is displayed.](media/device-connection-info.png "Device Connection Info")

2. Utilizing one of the keys from the values you recorded in #5, we will be generating a connection string to be used within the source code running on the device. We will generate the connection string using command line tooling. Ensure you have Node v.8+ installed, open a command prompt, and execute the following to globally install the key generator utility:

    ```
    npm i -g dps-keygen@0.3.3
    ```

    ![The command prompt displays the npm dps-keygen installation package result message.](media/global-install-keyutil.png "Global Install of Key Generator Utility")

    Next, generate the connection string using the key generator utility:

    ```
    dps-keygen -di:<Device ID> -dk:<Primary Key> -si:<Scope ID>
    ```

    ![The command prompt displays the output message from dps-keygen command.](media/generated-device-connectionstring.png "Generated Device Key")

    Make note of the connection string for the device.

3. Repeat steps 1 and 2 for DEVICE002 and DEVICE003.

### Task 2: Open the Visual Studio solution, and update connection string values

1. Using Visual Studio Code, open the `C:\MCW-Predictive-Maintenance-for-remote-field-devices-master\Hands-on lab\Resources\FieldDeviceSimulator` folder.

    > If you are prompted by Visual Studio code to install additional components to enable debugging, please select the option to install the components.

2. Expand the `Fabrikam.FieldDevice.Generator` folder, then open _appsettings.json_ and copy & paste the connection strings that you generated in Task 1 into this file.

    ![The screenshot shows the connection strings for all three devices.](media/appsettings-updated.png "Updated appsettings.json")

3. Open _Program.cs_, go to line 141 and you will find the _SetupDeviceRunTasks_ method. This method is responsible for creating the source code representations of the devices that we have defined earlier in the lab. Each of these devices is identified by its connection string. Note that DEVICE001 is defined as the pump that will gradually fail, DEVICE002 as a healthy pump, and DEVICE003 as a pump that will fail immediately after a specific amount of time. Line 164 also adds an event handler that gets fired every time the Power State for a pump changes. The power state of a pump gets changed via a cloud to device command - we will be visiting this concept later on in this lab.

4. Open _PumpDevice.cs_, this class represents a device in the field. It encapsulates the properties (serial number and IP address) that are expected in the properties for the device in the cloud. It also maintains its own power state. Line 86 shows the _SendDevicePropertiesAndInitialState_ method which updates the reported properties from the device to the cloud. This is also referred to as _Device Twins_. Line 131 shows the _SendEvent_ method that sends the generated telemetry data to the cloud.

### Task 3: Run the application

1. Within Visual Studio Code, expand the **.vscode** sub-folder, then open **launch.json**. Update the `console` setting to **externalTerminal**. This will cause the debugger to launch the console window into an external terminal instead of within Visual Studio Code. This is a required step since the internal terminal does not support entering values (ReadLine).

    ![The console setting is highlighted in launch.json.](media/vs-code-launch-settings.png "launch.json")

2. Using Visual Studio Code, Debug the current project by pressing _F5_.

3. Once the menu is displayed, select option 1 to generate and send telemetry to IoT Central.

    ![The command prompt displays the choice to generate and send telemetry to IoT Central.](media/generate-and-send-telemetry.png "Generate and Send Telemetry to IoT Central")

4. Allow the simulator to start sending telemetry data to IoT Central, you will see output similar to the following:

    ![A command prompt shows a few provisioned devices and their corresponding measurements.](media/telemetry-data-generated.png "Sample Telemetry")

5. Allow the simulator to run while continuing with this lab.

6. After some time has passed, in IoT Central select the _Devices_ item in the left-hand menu. Note that the provisioning status of DEVICE001, DEVICE002, and DEVICE003 now indicate _Provisioned_.

    ![A list of provisioned devices is displayed.](media/provisioned-devices.png "Provisioned Devices")

### Task 4: Interpret telemetry data

DEVICE001 is the rod pump that will gradually fail. Upon running the simulator for approximately 10 minutes (or 1100 messages), you can take a look at the Motor Power chart in the Device Dashboard, or on the measurements tab and watch the power consumption decrease.

1. In IoT Central, select the _Devices_ menu item, then select the link for _Rod Pump - DEVICE001_ in the Devices list.

2. Ensure the _Measurements_ tab is selected, then you can toggle off all telemetry values except for Motor Power so the chart will be focused solely on this telemetry property.

    ![Motor power telemetry graphing options are displayed. The graph shows a line with a drop off.](media/device001-focus-telemetry-chart.png "Focus Telemetry Chart to Motor Power")

3. Observe how the Motor Power usage of DEVICE001 gradually degrades.  **Note**: You may not yet see the degradation at this stage. The motor power will gradually decrease after running for several minutes.

    ![The average Motor Power measurements are displayed in a graph. The graph shows Motor Power usage degradation.](media/device001-gradual-failure-power.png "Motor Power usage degradation")

4. Repeat 1-3 and observe that DEVICE002, the non-failing pump, remains above the 30 kW threshold. DEVICE003 is also a failing pump but displays an immediate failure versus a gradual one.

    ![Low motor measurements are being displayed in a graph. Power measurements show steady.](media/device002-normal-operation.png "DEVICE002 Motor Power Rules Chart")

    ![Low motor measurements are being displayed in a graph. There is a steep power drop off around 3:52 PM.](media/device003-immediate-failure.png "DEVICE003 Immediate Failure")

### Task 5: Restart a failing pump remotely

After observing the failure of two of the rod pumps, you are able cycle the power state of the pump remotely. The simulator is setup to receive the Toggle Motor Power command from IoT Central and will update the state accordingly and start/stop sending telemetry to the cloud.

1. In IoT Central, select _Devices_ from the left-hand menu, then select _Rod Pump - DEVICE001_ from the device list. Observe that even though the pump has in all purposes failed, that there is still power to the motor - indicated by the Power State bar at the bottom of the device's Measurements chart.

    ![A graph of device telemetry measurements is displayed. The Motor Power measurement is highlighted. DEVICE001 Power State is in a failure.](media/device001-powerstate-in-failure.png "DEVICE001 Power State in Failure")

2. In order to recover DEVICE001, select the _Commands_ tab. You will see the _Toggle Motor Power_ command. Select the _Run_ button on the command to turn the pump motor off.

    ![Device command options are displayed.  The Run command is circled.](media/device001-run-toggle-command.png "DEVICE001 Run Toggle Motor Power Command")

3. The simulator will also indicate that the command has been received from the cloud. Note in the output of the simulator, that DEVICE001 is no longer sending telemetry due to the pump motor being off.

    ![Command prompt displays several messages. Simulator showing DEVICE001 received the cloud message. Device power cycle is displayed.](media/device001-simulator-power-off.png "Simulator showing DEVICE001 received the cloud message")

4. After a few moments, return to the _Measurements_ tab of _DEVICE001_ in IoT Central. Note the receipt of telemetry has stopped, and the state indicates the motor power state is off.

    ![Device DEVICE001 is in a Power State Off with no telemetry coming in.](media/device001-stopped-telemetry-power-state-off.png "Rod Pump DEVICE001 Measurements")

5. Return to the _Commands_ tab and toggle the motor power back on again by pressing the _Run_ button once more. On the measurements tab, you will see the Power State switch back to online, and telemetry to start flowing again. Due to the restart of the rod pump - it has now recovered and telemetry is back into normal ranges!

    ![A graph of device telemetry measurements is displayed. DEVICE001 recovered after Pump Power State has been cycled.](media/device001-recovered-1.png "Instance of pump recovery")

    ![Another graph of device telemetry measurements is displayed. DEVICE001 recovered after Pump Power State has been cycled.](media/device001-recovery-2.png "Another Instance of Pump Recovery")

## Exercise 3: Creating a device set

Duration: 10 minutes

Device sets allow you to create logical groupings of IoT Devices in the field by the properties that defined them. In this case, we will want to create a device set that contains only the rod pumps located in the state of Texas (this will exclude the simulated Rod Pump).

### Task 1: Create a device set using a filter

1. In the left-hand menu, select the _Device sets_ menu item. You will see a single default device set in the list. Select the _+ New_ button in the upper right-hand side of the listing.

    ![A screen displays the current device sets.  There is a add new button circled.](media/device-set-list.png "Device set listing")

2. In the field, all Texas pumps are located in the *192.168.1.* subnet, so we will create a filter to include only those pumps in this device set. Create the Device set with a condition as follows:

    ![The screen shows the device set creation options.  There is a list of devices and their settings displayed on the right.](media/new-device-set.png "New device set")

    | Field               | Value                      |
    | ------------------- | -------------------------- |
    | Device Set Name     | Texas Rod Pumps            |
    | Description         | Rod pumps located in Texas |
    | Device Template     | Rod Pump (1.0.0)           |
    | Condition: Property | IP Address                 |
    | Condition: Operator | contains                   |
    | Condition: Value    | 192.168.1.                 |

3. Note how the device list for this device set is automatically filtered to include only the real devices based on their IP Address. You are now able to act upon this group of devices as a single unit in IoT Central.

    ![The screen shows the device set configuration values.  There is a list of devices and their settings displayed on the right.](media/texas-rod-pump-devices.png "Texas Rod Pumps")

4. Similar to devices, you are also able to create a dashboard specific to this Device Set. Select the _Dashboard_ tab from the top menu, then select the _Edit_ button on the right-hand side of the screen.

    ![The edit device menu is presented. The dashboard item is circled.](media/device-set-dashboard-edit.png "Device Set Dashboard Edit")

5. In this case, we will add a map that will show the location and current power state of each rod pump in the device set. From the _Library_ menu, select _Map_.

    ![The editing dashboard options are displayed.  The map menu item is circled.](media/device-set-add-map.png "Device Set Dashboard Add Map")

6. Configure the map as follows, and select _Save_:

    | Field             | Value                       |
    | ----------------- | --------------------------- |
    | Title             | Texas Rod Pumps |
    | Location          | Pump Location           |
    | State Measurement | Power State                 |

    ![The device configurations are displayed. The save button is circled.](media/device-set-configure-map.png "Device set configure map")

7. End Dashboard editing by selecting the _Done_ button on upper-right corner of the dashboard editor.

    ![A map of the Texas rod pumps is displayed. The done button is circled in the upper right-hand corner.](media/device-set-dashboard-done.png "Device set dashboard complete editing")

8. Observe how the device set now has a map displaying markers for each device in the set. Feel free to adjust to zoom to better infer their location.

## Exercise 4: Creating a useful dashboard

Duration: 30 minutes

One of the main features of IoT Central is the ability to visualize the health of your IoT system at a glance. Creating a customized dashboard that best fits your business is instrumental in improving business processes. In this exercise, we will go over adding a main dashboard that will be displayed upon entry to the IoT Central application.

### Task 1: Clearing out the default dashboard

1. In the left-hand menu, select the _Dashboard_ item. Then, in the upper right corner of the dashboard - select the _Edit_ button.

    ![The dashboard editing options are presented to the user.](media/dashboard-edit-button.png "Edit Dashboard")

2. Select the _X_ on each tile that you do not wish to see on the dashboard to remove them. The _X_ will display when you hover over the tile.

    ![The create device template card is displayed. The close button in the upper right-hand corner is selected.](media/delete-dashboard-card.png "Delete Dashboard Tile")

### Task 2: Add your company logo

1. Remaining in the edit mode of the dashboard, select _Image_ from the _Library_ menu.

    ![The Library menu items are displayed. The Image menu item is circled.](media/dashboard-library-image.png "Dashboard library Image")

2. Configure the logo with the following file _C:\MCW-Predictive-Maintenance-for-remote-field-devices-master\Hands-on lab\media\fabrikam-logo.png_.

    ![The company logo configuration options are displayed.](media/configure-dashboard-logo.png "Configure Logo Image")

3. Resize the logo on the dashboard using the handle on the lower right of the logo tile.

    ![The company logo is displayed and option to resize tool is selected.](media/logo-resize.png "Resize the Logo")

### Task 3: Add a list of Texas Rod Pumps

In the previous exercise, we created a device set that contains the devices located in Texas. We will leverage this device set to display this filtered information.

1. Remaining in the edit dashboard mode, select _Device Set Grid_ from the _Library_ menu.

2. Configure the device list by selecting the _Texas Rod Pumps_ Device Set and assigning it the title of _Texas Rod Pumps_.

3. Add columns by selecting the _Add/Remove_, we will add _Device ID_ and _IP Address_.

4. Select the _Save_ button to add the tile to the dashboard.

    ![The panel for device list configuration is displayed.](media/device-list-configure1.png "Configure list")

    ![The dashboard shows company logo and the device IDs and IP addresses.](media/dashboard-inprogress-1.png "Dashboard in progress")

### Task 4: Add a map displaying the power state of DEVICE001

It is beneficial to see the location and power state of certain critical Texas rod pumps. We will add a map that will display the location and current power state of DEVICE001.

1. Select _Map_ from the _Library_ menu, configure the map as follows, and select _Save_:

    | Field             | Value                       |
    | ----------------- | --------------------------- |
    | Device Template   | Rod Pump (1.0.0)            |
    | Device Instance   | Rod Pump - DEVICE001        |
    | Title             | Rod Pump - DEVICE001 Status |
    | Location          | Rod Pump Location           |
    | State Measurement | Power State                 |

    ![The map configurations are displayed.](media/dashboard-configure-map.png "Configure Map")

    ![The Rod Pump location is displayed in a map. A list of devices and their IP addresses are listed on the right-hand side.](media/completed-dashboard.png "Completed Dashboard")

2. Select the _Done_ button in the upper right corner of the Dashboard to finish editing.

    ![The Rod Pump location is displayed in a map. A list of devices and their IP addresses are listed on the right-hand side. The done button is circled.](media/done-dashboard-editing.png "Done dashboard editing")

## Exercise 5: Create an Event Hub and continuously export data from IoT Central

Duration: 15 minutes

IoT Central provides a great first stepping stone into a larger IoT solution. Earlier in this lab, we responded to a crossed threshold by initiating an email sent to Fabrikam field workers through Logic Apps directly from IoT Central. While this approach certainly does add value, a more mature IoT solution typically involves a machine learning model that will process incoming telemetry to logically determine if a failure of a pump is imminent. The first step into this implementation is to create an Event Hub to act as a destination for IoT Centrals continuously exported data.

### Task 1: Create an Event Hub

The Event Hub we will be creating will act as a collector for data coming into IoT Central. The receipt of a message into this hub will ultimately serve as a trigger to send data into a machine learning model to determine if a pump is in a failing state. We will also create a Consumer Group on the event hub to serve as an input to an Azure Function that will be created later on in this lab.

1. Log into the [Azure Portal](https://portal.azure.com).

2. In the left-hand menu, select **Resource Groups**.

3. Open your **Fabrikam_Oil** resource group.

4. On the top of the screen, select the **Add** button. When the marketplace screen displays, search for and select **Event Hubs**. This will allow you to create a new Event Hub Namespace resource.

   ![Searching for the Event Hubs in the Azure Marketplace.](media/search-event-hubs.png "Search Event Hubs")

5. Configure the event hub as follows, select the *Review + create** button, and then **Create**:

   | Field          | Value                                 |
   | -------------- | ------------------------------------- |
   | Name           | _anything (must be globally unique)_    |
   | Pricing Tier   | Standard                              |
   | Subscription   | _select the appropriate subscription_ |
   | Resource Group | Fabrikam_Oil                          |
   | Location       | _select the location nearest to you_  |

   ![The creating event hub namespace options are displayed.](media/create-eventhub-namespace-form.png "Configure Event Hub Namespace")

6. Once the Event Hubs namespace has been created, open it and select the **+ Event Hub** button at the top of the screen.

   ![The Event Hub namespace screen is displayed. The create event hub button is circled.](media/add-eventhub-menu.png "Add new Event Hub")

7. In the Create Event Hub form, configure the hub as follows and select the **Create** button:

   | Field        | Value            |
   | ------------ | ---------------- |
   | Name         | iot-central-feed |
   | Partition Count | 2                |
   | Message Retention | 1                |
   | Capture      | Off              |

   ![The event hub creation screen is displaying the configuration options.  The create button is circled.](media/create-eventhub-form.png "Configure Event Hub")

8. Once the Event Hub has been created, open it by selecting _Event Hubs_ in the left-hand menu, and selecting the hub from the list.

   ![The event hubs are listed. IOT central feed is shown and circled.](media/event-hub-listing.png "Event Hub Listing")

9. From the top menu, select the **+ Consumer Group** button to create a new consumer group for the hub. Name the consumer group _ingressprocessing_ and select the **Create** button.

   ![The event hub screen shows the ability to create a consumer group. The name is entered and the create button is circled.](media/create-consumer-group-form.png "Create Consumer Group")

### Task 2: Configure continuous data export from IoT Central

1. Return to the IoT Central application, from the left-hand menu, select **Data Export**.

   ![The dashboard is displayed and the left-hand menu has the Data Export link circled.](media/data-export-menu.png "Data Export Menu")

2. From the _Data Export_ screen, select the **+ New** button from the top menu, and select **Azure Event Hubs**.

   ![The new Data Export options are displayed. The Azure Event Hubs option is circled.](media/ce-eventhubs-menu.png "New Event Hubs export")

3. IoT Central will automatically retrieve Event Hubs namespaces and Event Hubs from the connected Azure Account. Configure the data export as follows and select the **Save** button:

   | Field                | Value                                            |
   | -------------------- | ------------------------------------------------ |
   | Display Name         | Event Hub Feed                                   |
   | Enabled              | On                                               |
   | Event Hubs Namespace | _select the namespace you created in Exercise 6_ |
   | Event Hub            | iot-central-feed                                 |
   | Measurements         | On                                               |
   | Devices              | Off                                              |
   | Device Templates     | Off                                              |

   ![The data export configuration fields are displayed.  The save button is circled.](media/create-data-export-form.png "Configure Data Export")

   > **Note**: If you cannot browse your subscription for the Event Hubs namespace due to using a trial app instead of pay-as-you-go, perform the following steps:

   ![The error stating the user is unable to browse Event Hubs is highlighted.](media/iot-central-cannot-browse-event-hubs.png "Cannot browse event hubs")

   1. Navigate to your Event Hubs namespace in the Azure portal.

   2. Select **Shared access policies** on the left-hand menu, then select the **RootManageSharedAccessKey** and copy the **Connection string-primary key**.

        ![The Event Hubs connection string is highlighted.](media/event-hubs-connection-string.png "Event Hubs connection string")

   3. Return to IoT Central and paste the connection string into the **Connection string** field, then select the `iot-central-feed` event hub you created.

        ![The data export configuration form is updated with the manual connection string for Event Hubs.](media/create-data-export-form-connection-string.png "Connection string added")

4. The Event Hub Feed export will be created, and then started (it may take a few minutes for the export to start)

   ![The Data Export screen displays the status of Event Hub creation. The status of starting is displayed.](media/ce-eventhubfeed-starting.png "Event Hub Export Starting")

   ![The Data Export screen displays the status of Event Hub creation. The status of running is displayed.](media/ce-eventhubfeed-running.png "Event Hub Export Running")

## Exercise 6: Use Azure Databricks and Azure Machine Learning service to train and deploy predictive model

Duration: 15 minutes

In this exercise, we will use Azure Databricks to train a deep learning model for anomaly detection by training it to recognize normal operating conditions of a pump. We use three data sets for training: A set of telemetry generated by a pump operating under normal conditions, a pump that suffers from a gradual failure, and a pump that immediately fails.

After training the model, we validate it, then register the model in your Azure Machine Learning service workspace. Finally, we deploy the model in a web service hosted by Azure Container Instances for real-time scoring from the Azure function.

### Task 1: Run the Anomaly Detection notebook

1. In the [Azure portal](https://portal.azure.com), open your lab resource group, then open your **Azure Databricks Service**.

2. Select **Launch Workspace**. Azure Databricks will automatically sign you in through its Azure Active Directory integration.

   ![Launch Workspace option is displayed.](media/databricks-launch-workspace.png "Launch Workspace")

3. In Azure Databricks, select **Workspace**, select **Users**, then select your username.

4. Select the `Anomaly Detection` folder, then select the **Anomaly Detection** notebook to open it.

   ![The Anomaly Detection notebook is highlighted.](media/databricks-anomaly-detection-notebook.png "Workspace Folder")

5. Before you can execute the cells in this notebook, you must first attach your Databricks cluster. Expand the dropdown at the top of the notebook where you see **Detached**. Select your lab cluster to attach it to the notebook. If it is not currently running, you will see an option to start the cluster.

   ![The screenshot displays the lab cluster selected for attaching to the notebook. The lab configuration values are displayed.](media/databricks-notebook-attach-cluster.png "Attach cluster")

6. You may use keyboard shortcuts to execute the cells, such as **Ctrl+Enter** to execute a single cell, or **Shift+Enter** to execute a cell and move to the next one below.

7. Run all of the cells in the notebook and read the instructions and explanations to understand how the model is trained and deployed. You will need to provide values in the `Cmd 60` cell.

8. Copy the scoring web service URL from the last cell's result after executing it. You will use this value to update a setting in your Azure function in the next exercise to let it know where the model is deployed.

> You are not required to run the _Generated-signal-visualizations_ notebook for this lab. This notebook only contains the visualizations for the training data and is available for your reference.

## Exercise 7: Create an Azure Function to predict pump failure

Duration: 45 minutes

We will be using an Azure Function to read incoming telemetry from IoT Hub and send it to the HTTP endpoint of our predictive maintenance model. The function will receive a 0 or 1 from the model indicating whether or not a pump should be maintained to avoid possible failure. A notification will also be initiated through Logic Apps to notify Field Workers of the maintenance required.

### Task 1: Create an Azure Function Application

1. Return to the [Azure Portal](https://portal.azure.com).

2. Open your resource group for this lab.

3. From the top menu, select the **+ Add** button, and search for Function App.

4. Configure the Function App as follows, then select **Next: Hosting >**:

    | Field         | Value                                           |
    | ------------- | ----------------------------------------------- |
    | App Name      | _your choice, must be globally unique_          |
    | Subscription  | _select the appropriate subscription_           |
    | Resource Group | use existing, and _select Fabrikam_Oil_           |
    | Publish       | _select Code_ |
    | Runtime Stack | _select .Net Core_                                       |
    | Version | _select 3.1_ |
    | Location      | _select the location nearest to you_            |

    ![The create Function App blade is displayed. All of the Function App configurations are displayed.  The create button is circled.](media/create-azure-function-app-form.png "Create Azure Function App")

5. Configure the Hosting options as follows, then select **Review + create**:

    | Field         | Value                                           |
    | ------------- | ----------------------------------------------- |
    | Storage Account | _retain the default value of create new_ |
    | Operating System | Windows |
    | Plan Type | Consumption (Serverless)                                     |

    ![The create Function App blade is displayed. All of the Function App configurations are displayed.  The create button is circled.](media/create-azure-function-app-form2.png "Create Azure Function App")

6. On the Review blade, select **Create**, then **wait until the Function App is created** before continuing.

### Task 2: Create a notification table in Azure Storage

One of the things we would like to avoid is sending repeated notifications to the workforce in the field. Notifications should only be sent once every 24 hour period per device. To keep track of when a notification was last sent for a device, we will use a table in a Storage Account.

1. In the [Azure portal](https://portal.azure.com), select **Resource groups** from the left-hand menu, then select the **Fabrikam_Oil** link from the listing.

2. Select the link for the storage account that was created with the Function App in Task 1. The name will start with "storageaccount".

   ![The resources are listed in the blade. The storage account is circled.](media/select-function-storage-account.png "Select the Storage Account")

3. From the Storage Account left-hand menu, select **Tables** from the _Table service_ section, then select the **+ Table** button, and create a new table named **DeviceNotifications**.

   ![The storage account table administration blade is displayed. The tables link is circled on the left-hand side and the Add Table button is circled in the header.](media/create-storage-table-menu.png "Create Table in the Storage Account")

4. Keep the Storage Account open in your browser for the next task.

### Task 3: Create a notification queue in Azure Storage

There are many ways to trigger workflows in Logic Apps. One of them is having a logic app monitor an Azure Queue. We will use a Queue in our Azure Storage Account to host this queue.

1. From the Storage Account left-hand menu, select **Queues** located beneath the _Queue service_ section, then select the **+ Queue** button, and create a new queue named **flownotificationqueue**.

   ![The queues administration blade is displayed. The Add Queue button is circled.](media/create-storage-queue-menu.png "Create Queue in the Storage Account")

2. Navigate to the storage account. Obtain the Shared Storage Key for the storage account by selecting the left-hand menu item **Access keys**. Copy the _Key_ value of _key1_ and retain this value. We will be using it later in the next task. Also, retain the name of your Storage Account. (in the image below, the name of the Storage Account is _storageaccountfabribce9_)

   ![The pump function access key information is displayed. Key 1 is circled.](media/copy-function-storage-access-key.png "Copy access key for the Storage Account")

### Task 4: Create notification service in Logic Apps

We will be using [Azure Logic Apps](https://azure.microsoft.com/en-us/services/logic-apps/) as a means to email the workforce in the field. This flow will respond to new messages placed on the queue that we created in Task 3.

1. Return to the [Azure Portal](https://portal.azure.com).

2. Open your resource group for this lab.

3. From the top menu, select the **+ Add** button, and search for Logic App.

4. Configure your logic app as follows, select the *Review + create* button, and then **Create**:

   | Field          | Value                                 |
   | -------------- | ------------------------------------- |
   | Logic App name           | _anything (must be globally unique)_    |
   | Select the location   | Region                              |
   | Subscription   | _select the appropriate subscription_ |
   | Resource Group | Fabrikam_Oil                          |
   | Location       | _select the location nearest to you_  |
   | Log Analytics      | Off  |

   ![The creating logic app options are displayed.](media/logic-app-create.png "Configure Logic App")

5. Go to the resource and from the Logic Apps Designer, scroll to **Templates** and click **Blank Logic App**.

   ![The available templates are displayed, click Blank Logic App.](media/logic-app-blank-template.png "Select Blank Template")

6. From the Choose an action panel type _azure queues_ in the search box. Select the **When there are messages in a queue** item from the filtered list of triggers.

   ![Available actions are displayed, enter Azure queues and click When there are messages in queue option.](media/logic-app-messages-in-queue.png "Select Messages in Queue")

7. When prompted to connect to a storage account, select the storage account used as a notification queue in Task 3. 

   ![Available storage accounts to connect to.](media/logic-app-queue-connection.png "Connect to Storage Account")

8. Once your connection has been created, select *flownotificationqueue* under Queue Name, then click **+ New step**.

   ![Messages in queue action configuration.](media/logic-app-confirm-queue-setup.png "Finalize Queue Action Configuration.")

9. From the search bar, type _outlook 365_ to filter connectors and triggers. Then, select the **Send an email (V2)** item from the filtered list of actions.

   ![Available actions are displayed for Office Outlook 365.](media/logic-app-send-email.png "Select Send an Email")

10. From the Office 365 Outlook action panel, click **Sign in** to create a connection to your outlook account. 

   ![Office 365 Outlook sign in required.](media/logic-app-email-sign-in.png "Click Sign in")

11. When prompted, connect to your existing account.

   ![Sign in prompt is displayed.](media/logic-app-email-credentials.png "Sign in to your Outlook account")

12. In the Send an email (V2) form, fill it out as follows then click **+New step**.

  | Field      | Value                                                                                |
   | ---------- | ------------------------------------------------------------------------------------ |
   | To         | _enter your email address_                                                           |
   | Subject    | Action Required: Pump needs maintenance                                              |
   | Email Body | _put cursor in the field, then select **Message Text** from the Dynamic Content menu_ |

   ![Send email configuration is displayed.](media/logic-app-email-config.png "Create email notification action")

13. In the search bar for the next step, search for _azure queues_ once more, then select the **Delete message** item from the filtered list of Actions.

   ![The action pane is displayed. The queue text is listed in the search field.](media/logic-app-delete-message.png "Delete queue message step")

14. In the Delete message form, fill it out as follows, then select the **Save** button.

    | Field       | Value                                                                               |
    | ----------- | ----------------------------------------------------------------------------------- |
    | Queue Name  | flownotificationqueue                                                               |
    | Message ID  | _put cursor in the field, then select **Message ID** from the Dynamic Content menu_  |
    | Pop Receipt | _put cursor in the field, then select **Pop Receipt** from the Dynamic Content menu_ |

    ![The dialog displays the Delete message step. Message ID and Pop Receipt fields are populated. The Save button is circled. The available dynamic content pane has the available fields listed.](media/logic-app-delete-message-config.png "Delete queue message form")

15. Your completed logic app should look like what is shown below. Click the **Save** button from the top of the designer page.

    ![Completed logic app with three steps is displayed. Save button is circled in red.](media/logic-app-full-setup.png "Full logic app setup")

### Task 5: Obtain connection settings for use with the Azure Function implementation

1. Once the Function App has been provisioned, open the **Fabrikam_Oil** resource group and select the link for the storage account that was created with the Function App in Task 1. The name will start with "storageaccount".

   ![The resources are listed in the blade. The storage account is circled.](media/select-function-storage-account.png "Select the Storage Account")

2. From the left-hand menu, select **Access Keys** and copy the key 1 Connection String, keep this value handy as we'll be needing it in the next task.

   ![The pump function access keys are displayed. Key 1 is circled. The copy button for Connection string value is circled.](media/copy-function-storage-access-key-connection-string.png "Copy Function Storage Connection String")

3. Return to the **Fabrikam_Oil** resource group and select the link for the Event Hubs Namespace.

   ![A screen displays Azure resources. Event hub is circled.](media/select-event-hubs-namespace.png "Select the Event Hubs Namespace")

4. With the Event Hubs Namespace resource open, in the left-hand menu select the **Shared access policies** item located in Settings, then select the **RootManageSharedAccessKey** policy.

   ![The root connection string screen is displayed. Shared access policies and the RootManageSharedAccessKey link are circled.](media/open-event-hub-namespace-access-keys-menu.png "Open the Event Hub Namespace Shared Access Policies")

5. A blade will open where you will be able to copy the Primary Connection string. Keep this value handy as we'll be needing it in the next task.

   ![SAS Policy blade is displayed. Sample primary connection is displayed.  Copy button is circled.](media/copy-event-hub-namespace-access-key.png "Copy the Event Hub Namespace Connection String")

### Task 6: Create the local settings file for the Azure Functions project

It is recommended that you never check in secrets, such as connection strings, into source control. One way to do this is to use settings files. The values stored in these files mimic environment values used in production. The local settings file is never checked into source control.

1. Using Visual Studio Code, open the `C:\MCW-Predictive-Maintenance-for-remote-field-devices-master\Hands-on lab\Resources\FailurePredictionFunction` folder.

2. Upon opening the folder in Visual Studio Code, you may be prompted to restore unresolved dependencies. If this is the case, select the **Restore** button.

   ![A sample Restore Unresolved Dependencies dialog is displayed. The Restore button is available.](media/unresolveddependencies.png "Restore Unresolved Dependencies Dialog")

3. In this folder, create a new file named _local.settings.json_ and populate it with the values obtained in the previous task as follows, then save the file (note: prediction model endpoint was obtained in Exercise 6, Task 1 - step 8):

   ```json
   {
      "IsEncrypted": false,
      "Values": {
        "AzureWebJobsStorage": "<paste storage account connection string>",
        "FUNCTIONS_WORKER_RUNTIME": "dotnet",
        "fabrikam-oil_RootManageSharedAccessKey_EVENTHUB": "<paste event hub namespace connection string>",
        "PredictionModelEndpoint": "<paste prediction model endpoint>"
      }
   }
   ```

### Task 7: Review the Azure Function code

1. Observe the static _Run_ function located in **PumpFailurePrediction.cs**. It identifies that the function will run every time the _iot-central-feed_ event hub receives a message. It also uses a specific Consumer Group. These groups are used when there is a possibility that more than one process may be utilizing the data received in the hub. This ensures that dependent processes receive all messages once - thus avoiding any contingency problems. The method receives an array (batch) of events from the hub on each execution, as well as an instance of a logger in case you wish to see values in the console (locally or in the cloud).

   ```c#
   public static async Task Run([EventHubTrigger("iot-central-feed", Connection = "fabrikam-oil_RootManageSharedAccessKey_EVENTHUB",
                                ConsumerGroup = "ingressprocessing")] EventData[] events, ILogger log)
   ```

2. On line 29, the message body received from the event is deserialized into a Telemetry object. The Telemetry class matches the telemetry sent by the pumps. The Telemetry class can be found in the `Models/Telemetry.cs` file.

3. On line 31, the Device ID is pulled from the system properties of the event. This will let us know from which device the telemetry data came and allows us to group the telemetry by device so we can perform aggregates on the sensor data.

4. From lines 42 - 54, we group the telemetry by Device ID and calculate the averages for each sensor reading. This helps us reduce the number of calls we send to the scoring service that contains our deployed prediction model.

5. Lines 67 through 69 sends the received telemetry to the scoring service endpoint. This service will respond with a 1 - meaning the pump requires maintenance, or a 0 meaning no maintenance notifications should be sent.

6. Lines 75 through 101 checks Table storage to ensure a notification for the specific device hasn't been sent in the last 24 hours. If a notification is due to be sent, it will update the table storage record with the current timestamp and send a notification by queueing a message onto the _flownotificationqueue_ queue.

### Task 8: Run the Function App locally

1. Select <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the Azure Function code.

2. After some time, you should see log statements indicating that a message has been queued (indicating that the Logic App will send a notification email).

   ![A sample function log is displayed. A notification of email has been sent is circled.](media/azure-function-output.png "Azure Function Output")

3. Once a message has been placed on the _flownotificationqueue_, it will trigger the notification flow that we created and send an email to the field workers. These emails are sent in 5 minute intervals.

   ![A sample Logic App pump maintenance message is displayed.](media/flow-email-receipt.png "Notification email received")

4. You can now exit the locally running functions by selecting the Terminal window by pressing the <kbd>Ctrl</kbd>+<kbd>c</kbd> keys.

### Task 9: Prepare the Azure Function App with settings

1. In Task 6, we created a local settings file to hold environment variables that are used in our function code. We need to mirror these values in the Azure Function App as well. In the Azure portal, access the **Fabrikam_Oil** resource group, and open the **pumpfunctions** Function Application.

2. Select the **Configuration** option in the left-hand menu.

   ![The Azure Function Application Overview window is displayed with the Configuration item highlighted.](media/functionconfigurationsettingsmenu.png "Azure Function Application Overview window")

3. In the **Application Settings** section, we will add the following application settings to mimic those that are in our *local.settings.json* file. Add a new setting by selecting the **New application setting** button.

    | Setting     | Value                                                                               |
    | ----------- | ----------------------------------------------------------------------------------- |
    | fabrikam-oil_RootManageSharedAccessKey_EVENTHUB  | _event hub shared access key value from the local.settings.json file_                  |
    | PredictionModelEndpoint  | _prediction model endpoint value from local.settings.json file_   |

    ![The Application Settings tab is selected and the new application setting button is highlighted.](media/functionconfigurationnewsetting.png "New Application Setting")

    ![The form to add an application setting is displayed, consisting of a name and value textbox.](media/functtionappaddremoveappsetting.png "Add an Application Setting")

4. Once complete, select the **Save** button from the top menu to commit the changes to the application configuration.

   ![The Application Settings tab is selected and the Save button is highlighted.](media/functionconfigurationsave.png "Save Application Setting")

### Task 10: Deploy the Function App into Azure

1. Now that we have been able to successfully run our Functions locally, we are ready to deploy them to the cloud. The first step to deployment is to ensure that you are logged in to your Azure Account. To log into your Azure account, select the following shortcut to display the command palette: <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>p</kbd>.

2. In the textbox of the command palette, type in *Azure:Sign In*, and select enter (or select the command from the list). This will open a Microsoft Authentication webpage in your default browser. Logging into this window will authenticate Visual Studio Code with your ID.

   ![The Visual Studio Code command palette is displaying Azure:Sign In as a command option. The Azure:Sign In command is highlighted.](media/commandpalettesignin.png "Azure Sign In Command")

3. Once authenticated, we are ready to deploy - once again select <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>p</kbd> to open the command palette. Type *Azure Functions: Deploy* and select the *Azure Functions: Deploy to Function App* command from the list.

   ![The Visual Studio Code command palette is displaying Azure Functions. The Deploy to Function App command is circled.](media/commandpalettedeploytofunctionapp.png "Deploy to Function App Command")

4. The first step of this command is to identify where we are deploying the function to. In our case, we have already created a Function App to house our function called **pumpfunctions**. Select this value from the list of available choices.

   ![The Visual Studio Code command palette is displaying potential destinations for deployment of our function, the pumpfunctions function app is highlighted](media/funcdeploydestinationapp.png "pumpfunctions function app is highlighted")

5. You may be prompted if you want to deploy to **pumpfunctions**, select the **Deploy** button in this dialog.

   ![A dialog asking if we are sure we want to deploy to pump functions is displayed. The Deploy button is highlighted.](media/funcdeploymentdeploydialog.png "Deploy to Pump Functions")

6. After some time, a notification window will display indicating the deployment has completed.

   ![A notification is shown indicating the deployment to pumpfunctions has completed.](media/funcdeploycompleted.png "Deployment to pumpfunctions completed")

7. Returning to the Azure Portal, in the **Fabrikam_Oil** resource group, open the **pumpfunctions** function app and observe that our function that we created in Visual Studio Code has been deployed.

   ![The Azure Portal Function Application overview window is displayed with the pumpfunctions application expanded. The functions node is also expanded and the function named PumpFailurePrediction is highlighted.](media/azurefunctiondeployed.png "Functions node is expanded")

## Watch it in action

Nice work getting to this point! To observe your predictive maintenance solution in action using all of the deployed Azure services complete the following:

1. Stop execution of your Field Device Simulator by entering **Ctrl+C** or clicking the stop button inside Visual Studio Code.

2. From the Azure Portal, navigate to the storage account which contains your notification table. The name will start with "storageaccount" and was referenced in Exercise 7 - Task 2.

3. From the storage account, select **Storage Explorer (preview)** from the sidebar menu, and then click into the DeviceNotifications table underneath *TABLES*. For each row of data listed, right-click and select **Delete**. This step removes the records of any alerts being sent and will allow another set of notifications to be sent within a 24-hour time period.

4. Restart your Field Device Simulator by entering **F5** inside Visual Studio Code.

5. From IoT Central, you should be able to observe your streaming data and devices performing at high levels soon after restarting the simulator. Note: if your device is performing at a "low" level try cycling the power under the *Commands* tab. Once performance begins to degrade you should expect to receive automated alerts.

## After the hands-on lab

Duration: 10 minutes

### Task 1: Delete Lab Resources

1. In IoT Central, select _Administration_ from the left-hand menu. In the _Application Settings_ screen, delete the application by pressing the _Delete_ button. This will automate the removal of the IoT Application as well as all of its resources.

   ![The Administration panel is displayed and circled in the image. The delete button is highlighted as well.](media/delete-application.png "Delete the IoT Central application")

2. In the [Azure Portal](https://portal.azure.com), select **Resource Groups**, open the resource group that you created in Exercise 6, and select the **Delete resource group** button.

   ![The Azure Resource Group panel is displayed. The Delete resource group link is circled.](media/delete-resource-group.png "Delete the Resource Group")

You should follow all steps provided *after* attending the Hands-on lab.
