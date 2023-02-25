#### Preparing Debian based Linux

		curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg

		sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg

		sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/debian/$(lsb_release -rs | cut -d'.' -f 1)/prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'

		sudo apt update
		
		sudo apt install azure-functions-core-tools-4 -y

##### Telemetry
---------
<p> The Azure Functions Core tools collect usage data in order to help us improve your experience.
The data is anonymous and doesn't include any user specific or personal information. The data is collected
by Microsoft.

You can opt-out of telemetry by setting the FUNCTIONS_CORE_TOOLS_TELEMETRY_OPTOUT environment variable to 
'1' or 'true' using your favorite shell.

</p>

##### Create and go to folder

		mkdir -p example && cd example 



#### Create a function project

		func init example --worker-runtime python


<p> Pure func init, <b> without worker-runtime specified </b> will output:

Select a number for worker runtime:
1. dotnet
2. dotnet (isolated process)
3. node
4. python
5. powershell
6. custom
</p>

You will have a folder called example inside a folder called example.

cd example



##### Create a new function

		func new --template HttpTrigger


<p> Pure func new will output:

Will output:

Select a number for template:
1. Azure Blob Storage trigger
2. Azure Cosmos DB trigger
3. Durable Functions activity
4. Durable Functions entity
5. Durable Functions HTTP starter
6. Durable Functions orchestrator
7. Azure Event Grid trigger
8. Azure Event Hub trigger
9. HTTP trigger
10. Kafka output
11. Kafka trigger
12. Azure Queue Storage trigger
13. RabbitMQ trigger
14. Azure Service Bus Queue trigger
15. Azure Service Bus Topic trigger
16. Timer trigger



Select a number for template:HttpTrigger
Function name: [HttpTrigger] 
</p>

I could have also specified --name instead of using the offered HttpTrigger. 

New folder should appear, according to the specified name. It contains __init__.py and function.json

function.json == This file will define functions configuration. Azure has two directions for a function: in and out. In is the classical python way: We have a function and the function gets parameters. Out is what is
going outside == In to the world.


__init__.py  == This is the Python function itself.

On this file we see something like this: def main(req: func.HttpRequest) -> func.HttpResponse

req is a parameter, which is also specified in function.json. Similarly, func.HttpRequest also specifies that we are dealing with a function called HttpRequest, which is also specified in the function.json

-> func.HttpResponse is a commentlike structure, which we could also remove.

#### Other files of interest:


host.json == This file will contain extension bundles (NuGet extensions).

local.settings.json  == Local settings needed by functions. Danger: Might contain secrets. Do not store carelessly.



#### Running things

		func start HttpTrigger --verbose 
		
== Will start a function called HttpTrigger on a verbose mode.




