#The Workflow Event Properties
When a Imixs BPMN Event Element is selected in the Drawing Canvas, different settings can be
 configured from the tabbed property sheets displayed in the Property View.
 
<img src="../images/modelling/bpmn_screen_05.png"/>

The property settings are grouped into different sections.
 
##General Properties
The property tab 'General' defines basic informations for an Imixs-BPMN Event element. 

<img src="../images/modelling/bpmn_screen_19.png"  />

###Name 

The name of the Event element is used to identify the processing action. The name typical describes the activity triggered by the user to process a WorkItem. E.g. "submit", "approve" "close". The event name can be used by applications to display event based workflow actions. 

<center><img src="../images/modelling/bpmn_screen_30.png" width="200px" /></center>


###Documentation

The documentation of the Event element can be used to provide the user with additional information about the event. Typically the documentation provides information how the event will change the current status of the process instance. The documentation can be used  by applications to display event based information. 
 
 
 
##Workflow Properties
The Tab 'Workflow' contains Processing Information for an Event element. These information
are evaluated by the Workflow Engine while a WorkItem is processed 
 
<img src="../images/modelling/bpmn_screen_20.png" width="600px" />  
  
 
### ID 
Every Workflow Event assigned to a Workflow Task has an unambiguously ID. The Event ID is used  by the Workflow Engine to identify the Event.
 
 
### Result
The 'Result' defines application specific processing information evaluated by the Workflow Engine. These information are evaluated by plugins and applications. See the section [ResultPlugin](../engine/plugins/resultplugin.html) for further information.

 
###Visiblity
The section 'visibility' defines whether an event is visible to a workflow user or not. The visibility is separated into three levels of visibility. 

<ul>
 
<strong>Public Events:</strong> <br />
Per default each Imixs-Event is public and visible to all users. If the property 'Public Event' is set to 'No', the event will not be part of the public event list of an workitem and not be displayed by an application as a click action.<br /><br />

<strong>Restricted Events:</strong><br />
A 'Public Event' can be restricted to a specific group of users depending on the state of a workflow instance. The restriction is based on the 'Actor Properties' which are defined by the [process properties](./main_editor.html) of the workflow model. If a restriction is set the event is only visible to the workflow user if the userid is listed in one of the Actor Properties. With the restriction feature, the visibility of a workflow event can be configured on a very fine grained level.  <br />  <br />

<strong>Read Access:</strong><br />
The 'Read Access' property can be used to define a general read permission for a Imixs-Event element. The list can contain any application specific role or userid. If a 'read access' is specified the event is only visible to users with the corresponding access role or userid. 
</ul>

  
## ACL Properties
The ACL Tab is used to define the Access Control List (ACL) for a workitem which is processed by the Imixs-Workflow engine.

<img src="../images/modelling/bpmn_screen_21.png"/>  

The ACL defines the read- and write access a user will be granted for, after a WorkItem was successful  processed. This is one of the most important features of the Imixs Workflow System. Also the ownership can be defined by the ACL properties. The ACL can be set based on the 'Actor Properties' which are defined by the [process properties](./main_editor.html) of the workflow model. To activate the feature the option 'Update ACL' has to be checked.

<strong>Note:</strong> If the ACL setting is defined on Event level, the ACL settings on the Task Level are overwritten.


### Owner, Read and Write Access:
The ACL of a WorkItem is defined in three different layers.  The 'Owner' defines the users assigned to the WorkItem for the next Workflow Task. This setting typically adds the WorkItem to the users task list.  The 'Read Access' is used to restrict the read access for a WorkItem. Only users which are assigned to the Read Access of a WorkItem can access the workitem from the application. If no Users or Roles are defined in the Read Access not read restrictions are set to a WorkItem.
 
The 'Write Access' restricts the author access for a WorkItem. The Author Access depends on the 
 AccessLevel a user is granted for. If a user is assigned to the role 'org.imixs.ACCESSLEVEL.AUTHORACCESS' and is not listed in the Write Access List of a WorkItem, the user is not allowed to change or process the WorkItem. 
 
  

<ul>

<strong>Dynamic ACL:</strong><br />
The dynamic ACL settings are used to compute the access list based on the definition of 'Actors'.  Actors play an essential role in a user-centric workflow system. The actors are computed dynamically based on the properties of a WorkItem. See the section [Process Property Editor](./main_editor.html) for more  information how to define Actors in an Imixs BPMN model. <br /><br />

<strong>Static ACL:</strong><br />
It is also possible to define the ACL in a static way. The UserIDs and Role names have to match the  login name and role definitions of the workflow application. The following Imixs standard roles can be used here:<br />


 <ul><li> org.imixs.ACCESSLEVEL.READACCESS</li>
 
 <li>org.imixs.ACCESSLEVEL.AUTHORACCESS</li>
 
 <li>org.imixs.ACCESSLEVEL.EDITORACCESS</li>
 
 <li>org.imixs.ACCESSLEVEL.MANAGERACCESS</li>
 </ul>

</ul>


See also the section [security settings](../engine/acl.html) for more details.  

## History Properties
This history property defines information added by the [HistoryPlugin](../engine/plugins/historyplugin.html) during the processing phase of the imixs-workflow engine. 

<img src="../images/modelling/bpmn_screen_22.png"/>  

For each event processed by the Imixs-Workflow engine a new history entry will be added into the WorkItem. The history is a user-friendly process documentation like in the following example:

	02.10.2006 13:36:47 : Document saved by Tom.
	02.10.2006 13:46:37 : Document assigned by Mark.
	02.10.2006 13:36:47 : Document saved by Anna.

A history entry support the [Text Replacer feature](./textreplacement.html).

    Document saved by <itemvalue>namcurrenteditor</itemvalue>

## Mail Properties
The property section 'Mail' defines information for mail messaging during a process step.

<img src="../images/modelling/bpmn_screen_23.png"/>  

A mail message is defined by the mail subject and mail body. Both fields support the [Text Replacer feature](./textreplacement.html). A mail message can define different recipients in the sections 'To', 'CC' and 'BCC'. See the section [Process Property Editor](./main_editor.html) for more information how to define Actors in an Imixs BPMN model. 

A Mail message can be send by the [Imxis-Mail Plugin](../engine/plugins/mailplugin.html)


##Rule Properties

The property section 'Rule' allows the definition of business rules to be evaluated during the processing life cycle. 

<img src="../images/modelling/bpmn_screen_24.png"/>

A business rule can be defined in different script languages. See the section [Rule Plugin](../engine/plugins/ruleplugin.html) for further information.


##Report Properties

The 'Report' section describes a report definition to be executed the event.

<img src="../images/modelling/bpmn_screen_25.png"/>  

See the section [Rule Plugin](../restapi/reportservice.html) for further information about reports.


##Version Properties

The section 'Version' defines if a new version of a process instance should be created by the event. Versions are used to archive a process instance or to create a copy of a workitem.   

<img src="../images/modelling/bpmn_screen_26.png"/>

See the section [Version Plugin](../engine/plugins/versionplugin.html) for further information about reports.

##Timer Properties

Events can be triggered by user interaction or based on a timer event. The section 'timer' allows the definition of a timer event. 

<img src="../images/modelling/bpmn_screen_27.png"/>

See the section [Workflow Scheduler](../engine/scheduler.html) for further information.
  