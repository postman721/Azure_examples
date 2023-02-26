#### Preparing Debian based Linux

		curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg

		sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg

		sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/debian/$(lsb_release -rs | cut -d'.' -f 1)/prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'

		sudo apt update
		
		sudo apt install azure-functions-core-tools-4 python3-venv -y
		
		
#### Create virtual environment & Install requirements

		cd <into_content_directory_on_the_upper_level>

		python3 -m venv  .env

		source .env/bin/activate		

		python -m pip install -r requirements


<p> Example output when installing requirements:

python -m pip install -r requirements
Collecting azure-functions
  Downloading azure_functions-1.13.2-py3-none-any.whl (162 kB)
     |████████████████████████████████| 162 kB 2.4 MB/s 
Collecting azure-functions-durable
  Downloading azure_functions_durable-1.2.2-py3-none-any.whl (215 kB)
</p>


#### Create a function project

		func init example --worker-runtime python
		
		cd example


##### Create a new durable function set

We will need: 

		orchestrator, which is the organizer of things.

		activity function, which is called by the orchestrator to start a function process and return something.
		
		client function, which is just a regular Azure trigger function.
		

#### Creating functions via cli:


		func new --template DurableFunctionsorchestrator --name orchestrator

		func new --template DurableFunctionsactivity --name activity

		func new --template DurableFunctionsHttpstarter --name client


#### Run Azurite blob store 


		sudo apt install npm -y && sudo npm install -g azurite


Start azurite with the command:  <b> azurite </b>

NOTICE. Azurite can be a pain at times. I managed to corrupt its storage multiple times. When this happened client functions could not run etc.
Fixed this by making sure that azurite ran in the current folder only. On cli, I executed:  

		azurite  --location . 


#### Error and how to handle it
		

Upon initial func start, you will get an error:

Microsoft.Azure.WebJobs.Extensions.DurableTask: Unable to resolve the Azure Storage connection named 'Storage'.
[2023-02-25T10:07:41.749Z] Using the default storage provider: AzureStorage.
Value cannot be null. (Parameter 'provider')


Add: 
		
		"AzureWebJobsStorage": "UseDevelopmentStorage=true", 
		"AzureWebJobsSecretStorageType": "files"

to local.settings.json


#### Running things after the lines have been added

		func start  --verbose 
				
== Will start the durable function set on a verbose mode.

Output will be:

Functions:

	client: [POST,GET] http://localhost:7071/api/orchestrators/{functionName}

	activity: activityTrigger

	orchestrator: orchestrationTrigger

[2023-02-25T11:04:59.893Z]  INFO: Using allowed directories for shared memory: ['/dev/shm'] from App Setting: FUNCTIONS_UNIX_SHARED_MEMORY_DIRECTORIES
[2023-02-25T11:04:59.912Z]  INFO: Successfully opened gRPC channel to 127.0.0.1:46109 
[2023-02-25T11:04:59.912Z]  INFO: Detaching console logging.
[2023-02-25T11:04:59.956Z] Switched to gRPC logging.
...


If I would navigate to http://localhost:7071/api/orchestrators/orchestrator, I would see endpoints. By going to the orchestrator url, the durable function would get started.

If I would copy the statusQueryGetUri url line and navigate to it, I would essentially be seeing the durable function in action. 
