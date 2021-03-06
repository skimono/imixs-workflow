# Imixs Docker

The Imixs-Workflow project supports several Docker Images to run Imixs-Workflow in a containerized infrastructure. 
The Imixs Docker Images are hosted on [Docker Hub](https://hub.docker.com/r/imixs/) and can be installed and extended in various ways.


## The Imixs Microservice

The subproject [Imixs-Microservice](https://github.com/imixs/imixs-microservice) provides a WebService Interface which can be used to interact with the Imixs-Workflow-Engine over the [Imixs Rest API](./restapi/index.html). The 'Imixs-Microsoervice' is a Java EE Web Module which extends the Imixs-Workflow Engine providing a Rest Service for Human Centric Workflow Applications. This service can be used as a single Microservice or bundled with a Java EE Business Application. See also the [Deployment Guide](./deployment/index.html) for details.

## How to run a Imixs Docker Container

To use Imixs-Workflow out of the box, the Imixs-Microservice is also available as a [docker image on Docker Hub](https://hub.docker.com/r/imixs/imixs-microservice/) which can be deployed into any [Docker Environment](https://www.docker.com/). 
To run Imixs-Workflow in a container start the Docker Image imixs/workflow:

	docker run --name="imixs-workflow" -d -p 8080:8080 -p 9990:9990 \
	         -e WILDFLY_PASS="adminadmin" \
	         --link imixs-workflow-db:postgres \
	         imixs/imixs-workflow

The container need to be linked to the postgres container providing a database name 'workflow-db'. See the [docker project home](https://hub.docker.com/r/imixs/imixs-microservice/) for more information. 

### ...via docker-compose

You can simplify the start process of Imixs-Workflow by using 'docker-compose'.
The following example shows a docker-compose.yml file to run imixs-workflow:

	postgres:
	  image: postgres
	  environment:
	    POSTGRES_PASSWORD: adminadmin
	    POSTGRES_DB: workflow-db
	
	imixsworkflow:
	  image: imixs/workflow
	  environment:
	    WILDFLY_PASS: adminadmin
	  ports:
	    - "8080:8080"
	    - "9990:9990"
	  links: 
	    - postgres:postgres
    
Take care about the link to the postgres container. The host 'postgres' name need to be used in the standalone.xml configuration file in wildfly to access the postgres server.

Run start imixs-wokflow with docker-compose run:

	docker-compose up


## Testing the Imixs-Microservice

Using the command-line tool '[curl](http://curl.haxx.se/)' makes it easy to test the Imixs-Microservice. Here are some examples.

**NOTE**: As Imixs-Workflow is a human-centric Workflow Engine onyl authenticated users (Actors) can interact with the engine. Therefore it is necessary to authenticate against the Imixs-Rest service API. Imixs-Microservice provides a User-Management-Service to register and authenticate users. The default user has the userid 'admin' and the default password 'adminadmin'. This user is used  in the following examples. Run the setup url of Imixs-Micorservice to initalize the admin user acount:

	http://localhost:8080/imixs-microservice/setup

### Deploy a new BPMN model

With the following command a BPMN model created with the [Imixs-BPMN Modeling Tool](./modelling/index.html) can be deployed into the Imixs-Microservce.

    curl --user admin:adminadmin --request POST \
    	-Tticket.bpmn \
    	http://localhost:8080/imixs-microservice/model/bpmn

### Request the Deployed Model Version

The following command returns the model versions deployed into the service: 

    curl --user admin:adminadmin -H \
    	"Accept: application/xml" \
    	http://localhost:8080/imixs-microservice/model/

### Request the Task List

To request the current task list for the user 'admin' run:

    curl --user admin:adminadmin -H \
    	"Accept: application/json" \
    	http://localhost:8080/imixs-microservice/workflow/tasklist/creator/admin


### Create a new Process Instance
The next example shows how to post a new Workitem in JSON Format. The request post a JSON structure for a new workitem with the txtWorkflowGroup 'Ticket', the ProcessID 1000 and ActivityID 10. The result is a new process instance controlled by Imixs-Workflow Engine


	curl --user admin:adminadmin -H "Content-Type: application/json" -d \
	       '{"item":[ \
	                 {"name":"type","value":{"@type":"xs:string","$":"workitem"}}, \
	                 {"name":"$modelversion","value":{"@type":"xs:string","$":"1.0.0"}}, \
	                 {"name":"txtworkflowgroup","value":{"@type":"xs:string","$":"Ticket"}}, \
	                 {"name":"$processid","value":{"@type":"xs:int","$":"1000"}}, \
	                 {"name":"$activityid","value":{"@type":"xs:int","$":"10"}}, \
	                 {"name":"txtname","value":{"@type":"xs:string","$":"test-json"}}\
	         ]}' \
	         http://localhost:8080/imixs-microservice/workflow/workitem.json


### Read a Process Instance
After posting a new process instance the Imixs-Workflow engine will retrun a datastructure includign the uniqueid of the created process instance.
The uniqueid is used to request a single Workitem from the Imixs-Microservice. See the following curl example to request a workitem by it's $UniqueID in JSON format:

    curl --user admin:adminadmin -H \
    	"Accept: application/json" \
    	http://localhost:8080/imixs-microservice/workflow/workitem/14b65352f58-259f4f9b

This example returns the content of the Workitem with the UniqueID '14b65352f58-259f4f9b'. You can also restrict the result to a subset of properties when you add the query parameter 'items':


For more details read the section [Imixs-Rest API](./restapi/index.html)


