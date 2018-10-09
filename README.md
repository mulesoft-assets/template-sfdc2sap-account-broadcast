
# Anypoint Template: Salesforce to SAP Account Broadcast

# License Agreement
This template is subject to the conditions of the 
<a href="https://s3.amazonaws.com/templates-examples/AnypointTemplateLicense.pdf">MuleSoft License Agreement</a>.
Review the terms of the license before downloading and using this template. You can use this template for free 
with the Mule Enterprise Edition, CloudHub, or as a trial in Anypoint Studio.

# Use Case
This Anypoint Template should serve as a foundation for setting an online synchronization of accounts/customers from Salesforce to SAP.
Everytime there is a new account or a change in already existing one in Salesforce instance, the template will fetch it and send it as IDoc to SAP to update customer there.

Requirements have been set not only to be used as examples, but also to establish a starting point to adapt your integration to your requirements.

As implemented, this Anypoint Template leverage the [Batch Module](http://www.mulesoft.org/documentation/display/current/Batch+Processing).
The batch job is divided in Process and On Complete stages.
The integration is triggered by poll to Salesforce Accounts. New or modified accounts are passed to the batch as input.
In the batch the customer is fetched from SAP by its name. If it exists, its SAP Account Group is resolved against SAP.
If it doesn't exist, then new customer number is fetched from SAP. Afterwards every such account from Salesforce is sent to SAP in form of iDoc XML where it is asynchronously updated or created. 
Finally during the On Complete stage the Anypoint Template will log output statistics data into the console.

# Considerations

To make this Anypoint Template run, there are certain preconditions that must be considered. All of them deal with the preparations in both, that must be made in order for all to run smoothly.
**Failling to do so could lead to unexpected behavior of the template.**

Before continue with the use of this Anypoint Template, you may want to check out this [Documentation Page](http://www.mulesoft.org/documentation/display/current/SAP+Connector#SAPConnector-EnablingYourStudioProjectforSAP), that will teach you how to work 
with SAP and Anypoint Studio.

## Disclaimer

This Anypoint template uses a few private Maven dependencies from Mulesoft in order to work. If you intend to run this template with Maven support, you need to add three extra dependencies for SAP to the pom.xml file.


## SAP Considerations

Here's what you need to know to get this template to work with SAP.


### As a Data Destination

There are no considerations with using SAP as a data destination.
## Salesforce Considerations

Here's what you need to know about Salesforce to get this template to work.

### FAQ

- Where can I check that the field configuration for my Salesforce instance is the right one? See: <a href="https://help.salesforce.com/HTViewHelpDoc?id=checking_field_accessibility_for_a_particular_field.htm&language=en_US">Salesforce: Checking Field Accessibility for a Particular Field</a>
- Can I modify the Field Access Settings? How? See: <a href="https://help.salesforce.com/HTViewHelpDoc?id=modifying_field_access_settings.htm&language=en_US">Salesforce: Modifying Field Access Settings</a>

### As a Data Source

If the user who configured the template for the source system does not have at least *read only* permissions for the fields that are fetched, then an *InvalidFieldFault* API fault displays.

```
java.lang.RuntimeException: [InvalidFieldFault [ApiQueryFault [ApiFault  exceptionCode='INVALID_FIELD'
exceptionMessage='
Account.Phone, Account.Rating, Account.RecordTypeId, Account.ShippingCity
^
ERROR at Row:1:Column:486
No such column 'RecordTypeId' on entity 'Account'. If you are attempting to use a custom field, be sure to append the '__c' after the custom field name. Reference your WSDL or the describe call for the appropriate names.'
]
row='1'
column='486'
]
]
```











# Run it!
Simple steps to get Salesforce to SAP Account Broadcast running.


## Running On Premises
In this section we help you run your template on your computer.


### Where to Download Anypoint Studio and the Mule Runtime
If you are a newcomer to Mule, here is where to get the tools.

+ [Download Anypoint Studio](https://www.mulesoft.com/platform/studio)
+ [Download Mule runtime](https://www.mulesoft.com/lp/dl/mule-esb-enterprise)


### Importing a Template into Studio
In Studio, click the Exchange X icon in the upper left of the taskbar, log in with your
Anypoint Platform credentials, search for the template, and click **Open**.


### Running on Studio
After you import your template into Anypoint Studio, follow these steps to run it:

+ Locate the properties file `mule.dev.properties`, in src/main/resources.
+ Complete all the properties required as per the examples in the "Properties to Configure" section.
+ Right click the template project folder.
+ Hover your mouse over `Run as`
+ Click `Mule Application (configure)`
+ Inside the dialog, select Environment and set the variable `mule.env` to the value `dev`
+ Click `Run`
In order to make this Anypoint Template run on Mule Studio there are a few extra steps that needs to be made.
Please check this Documentation Page:

+ [Enabling Your Studio Project for SAP](https://docs.mulesoft.com/connectors/sap-connector#configuring-the-connector-in-studio-7)

### Running on Mule Standalone
Complete all properties in one of the property files, for example in mule.prod.properties and run your app with the corresponding environment variable. To follow the example, this is `mule.env=prod`. 


## Running on CloudHub
While creating your application on CloudHub (or you can do it later as a next step), go to Runtime Manager > Manage Application > Properties to set the environment variables listed in "Properties to Configure" as well as the **mule.env**.


### Deploying your Anypoint Template on CloudHub
Studio provides an easy way to deploy your template directly to CloudHub, for the specific steps to do so check this


## Properties to Configure
To use this template, configure properties (credentials, configurations, etc.) in the properties file or in CloudHub from Runtime Manager > Manage Application > Properties. The sections that follow list example values.
### Application Configuration
**Batch Aggregator configuration**
+ page.size `100`

+ scheduler.frequency `10000`
+ scheduler.start.delay `5000`

**Watermarking default last query timestamp e.g. 2016-12-13T03:00:59Z**
+ watermark.default.expression `2014-06-26T12:30:00.000Z`
		
**SalesForce Connector configuration**

+ sfdc.username `bob.dylan@sfdc`
+ sfdc.password `DylanPassword123`
+ sfdc.securityToken `avsfwCUl7apQs56Xq2AKi3X`

**SAP Connector configuration**

+ sap.jco.ashost `your.sap.address.com`
+ sap.jco.user `SAP_USER`
+ sap.jco.passwd `SAP_PASS`
+ sap.jco.sysnr `14`
+ sap.jco.client `800`
+ sap.jco.lang `EN`

+ sap.default.accountGroup `ZAG2`

# API Calls
There are no particular considerations for this Anypoint Template regarding API calls.


# Customize It!
This brief guide intends to give a high level idea of how this template is built and how you can change it according to your needs.
As Mule applications are based on XML files, this page describes the XML files used with this template.

More files are available such as test classes and Mule application files, but to keep it simple, we focus on these XML files:

* config.xml
* businessLogic.xml
* endpoints.xml
* errorHandling.xml


## config.xml
Configuration for connectors and configuration properties are set in this file. Even change the configuration here, all parameters that can be modified are in properties file, which is the recommended place to make your changes. However if you want to do core changes to the logic, you need to modify this file.

In the Studio visual editor, the properties are on the *Global Element* tab.


## businessLogic.xml
Functional aspect of the Anypoint Template is implemented on this XML, directed by a batch job that will be responsible for creations/updates. The several message processors constitute four high level actions that fully implement the logic of this Anypoint Template:
1. The integration is triggered by poll to Salesforce Accounts. New or modified accounts are passed to the batch as input.
2. During the Process stage, the customer is fetched from SAP by its name.
3. If customer exists, its SAP Account Group is resolved against SAP. If it doesn't exist, then new customer number is fetched from SAP. Afterwards every such account from Salesforce is sent to SAP in form of iDoc XML where it is asynchronously updated or created.
4. Finally during the On Complete stage the Template will log output statistics data into the console.



## endpoints.xml
This file is conformed by two Flows.
The first one we'll call it **scheduler** flow. This one contains the Scheduler endpoint that will periodically trigger **query** flow and then executing the batch job process.
The second one we'll call it **query** flow. This one contains watermarking logic that will be querying Salesforce for updated/created Accounts that meet the defined criteria in the query since the last polling. The last invocation timestamp is stored by using Objectstore Component and updated after each Salesforce query.



## errorHandling.xml
This is the right place to handle how your integration reacts depending on the different exceptions. 
This file provides error handling that is referenced by the main flow in the business logic.




