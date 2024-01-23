# Camunda-test-1
Basic BPMN Test # 1 

## Camunda BPMN Test: Environment Configuration Options
1. Use a Free Camunda SaaS account
	* Sign up for a free Camunda account [Create Camunda Account](https://accounts.cloud.camunda.io/signup?)

	  * Required Connectors
		  * Amazon SQS Outbound Connector
		  * Rest Connector
		
2. 	Use Camunda Desktop Modeler
	* Download Camunda Desktop Modeler and follow the Get Started Instructions [Camunda Desktop Modeler](https://camunda.com/download/modeler/)
	
	  * Required Connectors
  		* Download "http-json-connector.json" [REST Connector](https://github.com/camunda/connectors/blob/main/connectors/http/rest/element-templates/http-json-connector.json)
  		* Download "aws-sqs-outbound-connector.json" [Amazon SQS Outbound Connector](https://github.com/camunda/connectors/blob/main/connectors/aws/aws-sqs/element-templates/aws-sqs-outbound-connector.json)
  		* Place the files in the Camunda Modeler directory; resources\element-templates

## Camunda BPMN Test: Task Description
	* Task completion will demonstrate the ability to configure the most basic Camunda setup
 	* Send the completed .BPMN files to Systalex HR contact you are working with
 	* Include potential errors if deployed to a Camunda engine and a process started
 	* List improvements the process could have added to it to reduce errors

## Objective: Build a simple Camunda BPMN that makes non-complex decisions based on data fetched from an REST Api GET, configure an AWS SQS outbound connector and call activity.

### Requirements:

1.  Create a New Project
	*	Add a business process model notation process called process-activity-status-evaluation- “your First name” “Your last Name”
 
2.  Process Modeling
	*	This process is assuming it will start by being invoked from another Camunda process or the start event may become an inbound connector in the future.

3.  Process Details.
    *	Assume your process receives “activityStatus” a process variable when it is invoked
        *	If activityStatus is “A” Continue the process
        *	If activityStatus is not “A” End the process

    *	Get Data from a REST API
        *	API URL: "https://jsonplaceholder.typicode.com/todos/10"
        *	Result Expression should include; "userId", "id", "completed" from the API body


    *	Add decisions gateway to check to see if completed is True or False
        *	If "completed" = false end the process
        *	If "completed" = true Continue the process


    *	Continued process consists of two objectives
        *	Start another Camunda process with ProcessID:"completedActiveStatus"
		    *	Post a message to Amazon SQS with fifo enabled
            *	MessageGroupId= 1234
            *	Message:
            *	Include variables received from the GET REST API call above and add a "completedBy" = "Your Name"


    *	Use the following secrets
        *	Access Key = SYS_AWS_KEY
        *	Secret Key = SYS_SECRET_KEY
        *	Queue URL = SYS_QUEUE_URL
        *	Region = us-east-1
          
	* End the process


4.  Create process referenced by Call Activity		
    *	Add an additional business process model notation process for the Call Activity above
    *	Name the process file process-completed-activity-status
    *	Ensure the process is configured to start when invoked by the call activity created in process-activity-status-evaluation...



6.  Process Details
    *	This process is expected to use variables from the initiating process
    *	This process will have a start event, script task, and an end event
        *	Script task will reference userId
		    *	If userId = 1 then set new variable expectedUser = true
		    *	If userId != 1 then set new variable expectedUser = false	
8.    Styling
    *  Use BPMN best practices and labeling.
   
9.  Deployment Management
    * This process would not be expected to execute without being deployed into a environment without proper secrets being created.
	
10.   Error Handling
    * Implement or describe basic error handling in the email back to Systalex with the completed .BPMN file.
