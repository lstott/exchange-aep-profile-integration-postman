# exchange-aep-profile-integration-postman
A postman collection to assist Exchange partners with the required API calls used to build an integration with Adobe Experience Cloud (AEP) Profiles. This Postman collection includes API calls to... First, prepare the AEP instance for receiving new profile data...  Second, import the profile data (streaming and batch) and... Finally, query for specific profiles. 

## Getting set up

Before executing the API calls in this collection you will need to...

1. Download and import this collection into Postman
2. Create an Integration using the Adobe.io console
3. Download and import the environment from Adobe.io console
4. Modify the environment

### Download and import this collection into Postman

The collection can be downloaded [here](https://github.com/Adobe-Marketing-Cloud/exchange-aep-profile-integration-postman/blob/master/AEP%20-%20Profile%20integration%20for%20ISVs%202020-03-06.postman_collection.json). Import the collection in Postman using File > Import...

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

* PRIVATE_KEY = _full_text_of_private_key


