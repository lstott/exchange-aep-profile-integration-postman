# exchange-aep-profile-integration-postman
A [postman collection](../master/AEP%20-%20Profile%20integration%20for%20ISVs%202020-03-06.postman_collection.json) to assist Exchange partners with the required API calls for integration with Adobe Experience Cloud (AEP) Profiles. This Postman collection includes API calls to... First, prepare the AEP instance for receiving new Loyalty profile data...  Second, import the profile data (streaming and batch) and... Finally, query for specific profiles. 

## Getting set up

Before executing the API calls in this collection you will need to...

1. Download and import this collection into Postman
2. Create an Integration using the Adobe.io console
3. Download and import the environment from Adobe.io console
4. Modify the environment

### Download and import this collection into Postman

The collection can be downloaded [here](../master/AEP%20-%20Profile%20integration%20for%20ISVs%202020-03-06.postman_collection.json). Import the collection in Postman using File > Import...

### Create an Integration using the Adobe.io console

Follow [this guide](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/integrations.md) -- specifically the section titled "Creating an Integration in Adobe I/O Console"

### Download and import the environment from Adobe.io console

The Adobe.io console will provide you with a Postman environment that contains most of the configuration you need. 

To download the environment file...

1. Navigate to your Integration Overview page at https://console.adobe.io/integrations
2. Click on the "Export Details to Postman" button
3. Save the environment json file to your local machine
4. In Postman use File > Import... to import the environment file

### Modify the environment

You will need to make a few changes to the enviroment download from Adobe.io console. If needed, you can learn how to [manage Postman environment variables](https://learning.postman.com/docs/postman/variables-and-environments/variables/#defining-variables).

Add the following variables to your environment.  

* PLATFORM_GATEWAY = https://platform.adobe.io
* SANDBOX_NAME = _your_sandbox_name_

Modify the following variables.

* PRIVATE_KEY = _full_text_of_private_key_


## Running the API calls

The calls in this collection are organized in numbered folders to indicate the sequence. Many of the calls are very similar for treatment of the Profile data and the Event (time-series) data. Folders with an (a) are for the Profile data, and folders with (b) are for the Event data. 

### 0: Initialize and Authenticate (folder) 

#### INIT: Load Crypto Library for RS256
Loads a javascript library into the environment. Used to create the JWT token for authentication. Sets a global variable called "jsrsasign-js"

#### AUTH: Generate JWT and Access Token
Generates a JWT token using the Crypto RS256, then sends to IMS to retrieve an access_token which will be valid for 24 hours. Sets an environment variable called "ACCESS_TOKEN". 


#### Schema: Return {TENANT_ID} and Schema Registry usage information
This call will retrieve the tenantId, which is needed for Schema related API calls. Sets an environment variable called "TENANT_ID". 

#### Import: Create Real-time Data Inlet
Creates a streaming endpoint for real-time data ingestion. The same inlet works for both the PROFILE and EVENT data. Sets three environment variables (INLET_ID, INLET_URL, INLET_SOURCE_ID) to be used in the "3: Real-time import" calls. 

#### Event Config: Create a new custom event type (loyaltyBalanceUpdate)
Adds a custom event for tracking Loyalty account balance updates

#### Event Config: Create a new custom event type (loyaltyLevelUpdate)
Adds a custom event for tracking Loyalty membership level updates


### 1a: Create Schema for PROFILE data (folder)

#### Schema: Create Mixin containing Loyalty PROFILE details
The mixin containing the Loyalty details will be created first. This mixin will then be part of the Schema to be created next. Sets an environment variable called "MIXIN_ID_PROFILE". 

#### Schema: Create PROFILE Schema for Loyalty data
Create a Schema for importing Loyalty data into the profile. Schema uses the "XDM Individual Profile" class. Sets an environment variable called "SCHEMA_ID_PROFILE".

#### Schema: Set Primary Identity Descriptor for Loyalty Profile Schema
Defines which field in the Schema is the primary identifier. 


### 1b: Create Schema for EVENT data (folder)

#### Schema: Create Mixin containing Loyalty EVENT details
The mixin containing the Loyalty details will be created first. This mixin will then be part of the Schema to be created next. Sets an environment variable called "MIXIN_ID_EVENT". 

#### Schema: Create EVENT Schema for Loyalty data
Create a Schema for importing Loyalty event data into the profile. Schema uses the "XDM ExperienceEvent" class. Sets an environment variable called "SCHEMA_ID_EVENT".

#### Schema: Set Primary Identity Descriptor for Loyalty Event Schema
Defines which field in the Schema is the primary identifier. 

### 2a: Create DataSet for PROFILE data (folder)

#### DataSet: Create Dataset for Loyalty PROFILE Records
Creates a new Dataset for collecting the Loyalty profile records, using streaming and/or batch. Uses the Loyalty profile schema that was previously created. 

### 2b: Create Dataset for EVENT data (folder)

#### DataSet: Create Dataset for Loyalty EVENT Records
Creates a new Dataset for collecting the Loyalty event records, using streaming and/or batch. Uses the Loyalty event schema that was previously created. 

### 3a: Real-time import for PROFILE data (folder)

#### Import: Stream to PROFILE DataSet

### 3b: Real-time import for EVENT data (folder)

#### Import: Stream to EVENT DataSet

### 4a: Batch import for PROFILE data (folder)

#### Import: Create a Batch for PROFILE data

This is the first call of a multi step process. Batches must be created, uploaded, then completed before they can fully process. 

#### Import: Upload file to Batch for PROFILE data

Before hitting "Send" for this call you should select the data file by clicking on the "Body" tab, then choose "binary" then clicking "Select Fille". This [sample Profiles file](../master/AEP%20loyalty%20profiles.json) can be used if you first download it to your local machine. More sample data for this import process can be generated with this [Mockaroo schema - 22f6f910](https://mockaroo.com/22f6f910)

#### Import: Complete a Batch for PROFILE data

#### Import: Check the Status of a Batch for PROFILE data

### 4b: Batch import for EVENT data (folder)

#### Import: Create a Batch for EVENT data

This is the first call of a multi step process. Batches must be created, uploaded, then completed before they can fully process. 

#### Import: Upload file to Batch for EVENT data

Before hitting "Send" for this call you should select the data file by clicking on the "Body" tab, then choose "binary" then clicking "Select Fille". This [sample Events file](../master/AEP%20loyalty%20events.json) can be used if you first download it to your local machine.  More sample data for this import process can be generated with this [Mockaroo schema - d419b8a0](https://mockaroo.com/d419b8a0)


#### Import: Complete a Batch for EVENT data

#### Import: Check the Status of a Batch for EVENT data

### 5a: Real-time lookup PROFILE data (folder)

#### Export: Retrieve PROFILE data for a customer profile

Query a profile based on the email address in the PROFILE_ID environment variable. This is the same profile that should have been created by the streaming import call made previously.

### 5b: Real-time lookup EVENT data (folder)

#### Export: Retrieve EVENT data for a customer profile

Query a profile based on the email address in the PROFILE_ID environment variable. This returns only the time-series Events associated with the profile.  This is the same profile that should have been created by the streaming import call made previously.

### X: Supplementary calls (folder)

This folder contains several optional calls related to the configuration and data used in the calls previously made. 



