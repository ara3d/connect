# About Ara 3D Connect

_Ara 3D Connect_ is an ecosystem of tools and specifications designed to let you track changes to BIM projects as they happen across different authoring tools without having to share the source documents.
It is inspired by how software developers work. 

It is intended to save time in BIM coordination scenarios such as where architects and engineers have to work together. 

It makes it faster to find conflicts, resolve where they are, and reduce the possibility of them being created in the first place.    

_In summary_: Ara 3D helps users synchronize work and assure consistency of the project while work continues independently 
on different documents without disrupting workflows.  

# How it Works for a User

* **Initialization** - Users run an initialization tool. It installs plug-ins and tools, and creates a copy of a repository
* **Publishing** - They configure what data they want to publish and how often. By default it publishes whenever they save.
* **Review** - Users are informed when changes have been published, or a review has been requested.    

# How it Works Under the Hood

_Ara 3D Connect_ uses Git under the hood. There are two separate repositories that it uses:

1.	The **Coordination Repository** - An shared Git repository, that is both online, and local to everyone's computer. 
This is used to synchronize and track changes across documents. Users control what data is published here, and when. 
This Git repository houses an ECS database stored as a JSON file, and additional data payloads in various formats.  

2.	The **Working Document Repository** - This a Git repository local only to the user’s computer to track changes to design 
documents. This is so that a user can easily revert to a previous state.
 
From the user's perspective the working document repository is just a folder where the working design files are stored. 

The official current version of the design files may be in any content or document management system the user wants. When
they want to edit documents locally, they just have to copy it to this folder. 

# The Loca 

Two people may have access to the same repository because they need to be informed of each other’s changes but are working on design documents that only they are allowed to modify (e.g., an architect and engineer in different firms).  
Component Data and Payloads
Data is stored in the repository components and payloads. Components are associated with entities via an ID. Components may be one of the 
A component is a JSON record that has a:
•	Entity ID – a UUID representing the entity the component is associated with. 
•	Component Type – a URN that specifies the kind of component.
•	Component ID – a UUID that is created when the component is first created and doesn’t change even when the component data changes.
•	Component Version ID – a UUID that is updated whenever data associated with the component is changed. 
•	A Source ID – a UUID that represents the source design document.   
•	Optional: Payload – a record which has a:
o	relative path (string) to a payload file in the repository containing data, 
o	a MIME type (string) describing the type of data  
o	an MD5 hash (string) that uniquely identifies the contents 

# Terminology

-	Branch – a named commit, that is used as the ancestor of two or more parallel commit histories. 
-	Commit – a snapshot of the change history, has an ID and a message. 
-	Component – A group of related data relevant to some part of the project stored as a JSON object, with optional payload. 
-	Design document – any file that is used to specify design intent. This could be a CAD file, a BIM file, or a specification file. It could even be a CSV or DOCX file. 
-	ECS Database – the JSON file(s) containing a list of component records.
-	Entity – An abstract or physical part of a project (e.g., room, 
-	Initialization – either the process of creating an initial Git repository online, or the process of making a local copy of an online repository. 
-	Mainline – a commit history that originates from the first commit and is used as the official history of the final product 
-	Merge – the combing of one commit (repository snapshot) into another (files and change history) 
-	Merge Conflict – when two commits ca
-	Tracking tool – a
-	Repository – a collection of files and their change history.  
-	Payload – a text or binary file containing additional data not in the main ECS database.
-	Publish –  
-	Publishing tool – A plug-in or command-line tool that updates the ECS database and issues commands to Git. 
-	Pull – when the online repository state is 
-	Pull Request – a proposal to merge a set of changes from one branch into another
-	Push – when the current repository state is merged into the online repository. This can be blocked if the merge is not clean. 
-	Publishing configuration – a JSON file that specifies what is published by a publishing tool.  
•	ECS – Stands for Entity Component System and is a method of organizing data. 

Selected Data
Ara 3D only publishes the data that you select. This is configured for each publishing tool in a JSON file called a publishing configuration. Within a publishing configuration, you can choose all data, or data matching some criteria (e.g. design option, work-set, Revit schedule, phase, category, etc.). 
It is possible to have two copies of the same source file, each with different publishing configurations. 
Publishing
Every time your file is saved selected data is published. 
•	You might modify, create, or delete one or more entity.
•	Modifying an entity implies that one or more components associated with the entity has been modified. 
•	Modifying a component does not change it 
•	After creation the number of components generally does not change, unless the publishing settings change. 
Components
•	A component s

Use Cases
•	Initializing a Repository
o	The first step to using Ara 3D connect is to create (initialize) a Git repository online.
•	Cloning a Repository
o	Cloning a repository means to create a local copy using “git clone”. 
•	Adding a New Design Document
o	Every time a new design document is added to a repository a special component is created called a DesignDocumentComponent.
o	The design document is associated with a new entity ID.
o	That entity ID uniquely identifies the design document.
o	The DesignDocumentComponent holds a copy of the publishing configuration. 
o	If the design document is stored in a Git repository, it’s commit hash is stored. 
•	Publishing a Document
o	Publishing a document can happen at any time.  
o	Relevant components are added, deleted, or updated in the ECS database.
o	Publishing happens from within Revit for Revit files, but can happen either from within Rhino or a command-line for Rhino files, and from the command-line for IFC files. 
o	In the Git repository it means an “add”, “commit” and “push” happens. 
o	The user is usually prompted to publish on save and/or upload to a server.  
o	This can be changed based on a configuration options.
o	What happens when a user publishes design data that was not saved?
1.	The data published is likely to be inaccurate. 
o	The DesignDocumentComponent will be updated, even if no other component was changed. 
o	If the design document is stored in a separate Git repository, that file’s change is committed to the repository and the commit hash is stored in the repository.
•	Commits
o	A commit is a snapshot of the change history.
o	It may be tagged (assigned a name) or not. 
•	Branching
o	What happens when a user creates a branch?
1.	A branch is a named commit (a snapshot of the project) which subsequent changes use as an ancestor point. 
2.	It acts as a new timeline, and allows work to be tracked separately in parallel, and merged at a later point.  
•	Recommended Branching Strategy
o	Every user should work in their own branch and merge to the main branch only upon approved review. 
•	Reviewing
o	Reviewing is the process of comparing two snapshots of the project, adding comments, and approving, reviewing, or blocking.
o	Reviews happen on GitHub – it is a cloud service and not part of Git itself.  
o	Reviews are usually conducted before a branch is merged into the main line. 
•	Rejected reviews - 
o	Approved reviews 
o	
•	Diffing
o	A “diff” is a comparison of two commits. 
o	It is possible to diff any two different commits.
o	You will see what was added, removed, and changed.
o	A diff requires that another temporary copy of the repo is created.  
o	The JSON files are compared, as are the associated tracked payloads. 
•	Storing Design Documents in the Repository
o	Is it possible for a user store design documents in the repository? 
1.	Yes, but it is not the most common use case.
2.	Usually this would be incompatible with the data management policies of the firm. 
o	Is it possible for a user to store design documents in a different Git repository?
1.	Yes, in fact it is recommended to use one locally. 
2.	The user must leverage Git LFS. 
•	Multiple Repositories
o	Can different repositories be created for the same project? 
o	This is not yet explored, but it could be desirable to control who has access to the repository. 
•	Restoring a Previous State (Reverting / Checking Out)
o	A restore tool would have to be used.  
1.	A restore tool would be responsible for determining whether the required design documents can be accessed.
2.	It would next have to restore those documents to the user’s computer. 
3.	This works best if these documents are stored in a separate Git repo on the user’s computer. 
o	Going to a different state in the repo is called “checking out”. 
o	If the state is in the history, it is called “reverting”. 
•	What If the Design Documents are not Restored? 
o	If checking out happens and the design documents are not restored then the next publish step will 
•	Saving a Design Document  
o	Every time the design document is saved, the DesignDocumentComponent has a version ID which is updated. 
o	This happens regardless of whether any of the published components are changed. 
•	Deleting files 
o	If a source file is deleted, then the next time a publish happens, the user will be prompted. Is the file deleted, or just not found?
o	If deleted, then the associated components are deleted.
o	If not, then the user is asked whether they want to continue to track it or stop tracking. 
•	File Tracking 
o	Not every user has every file. 
o	A user who creates or opens a file, starts tracking it. 
o	If the file goes missing, while a user is tracking it, is assumed 
•	Published Design Document Duplication 
o	What happens when a published design document (call it A1) is duplicated (call it A2)?
1.	Nothing happens, until A2 is published. 
o	What if A2 is published with the same configuration? 
1.	A new set of components is created that is identical to the previous one. 
o	What if the old file was deleted but not published? 
1.	The system assumes that the old file exists until it is informed otherwise (through publishing)
•	Duplicated Components 
o	What happens when two components exist on the same entity?
1.	If the data is the same but source is different.  
•	Copying a Repository 
o	What happens when a repository is copied? 
•	Deleting, Moving, or Renaming a Design Document 
o	What happens when a published design document is deleted? 
1.	When a source file is deleted from a compute, the next publishing action from that computer will inform the repository and will delete all of the components associated with that file. 
2.	Data is assumed to exist, until publishing happens. 
o	When moved? 
1.	The components are updated. 
o	When renamed?
1.	Renaming is the same as moving. 
•	Deleting an Entity 
o	An entity ceases to exist when there are no more components associated with it. 
•	Changing an Entities Data
o	This is not possible. 
o	Entities have no data, they are only an ID. 
o	Only components have data. 
•	Changing an Entity ID
o	This is not possible. 
o	Entity IDs exist only as component references.
o	If you change the entity ID of a component.  
•	Creating an Entity
•	Moving a File between Users
o	There are three possibilities.
1.	one is a copy of the other, for references sake (no publishing)  
2.	one is going to be edited for other purposes (e.g., structural engineering model based on an architectural model). 
3.	the file ownership has moved.  
•	Passive tracking 
o	Sometimes a user has a copy of a file and is using it to model something. 
o	However, the original of the file is updated, and the user using the copy should be informed. 
o	They never publish the data of their new model, but they want 
o	

Tracking
Tracking changes is done via polling. Your system will poll at a set interval, for changes. It will then show you the incoming changes. 
Question: are the changes   

Use Cases
Revit from Scratch 
1.	Initialize project. 
2.	Start authoring Revit file. 
Every time the Revit file is saved, the selected data is published. 
Revit from Sample
1.	We have the Revit sample.
2.	We want to link the models. 
IFC from Sample
1.	ABC.

Use Case 1 
Two IFC files exist and need to be linked – One is based on the other, and has a new object inserted at the beginning. 
Use Case 2
Two Rhino files exist and need to be linked. No property information, just geometry. 
Use Case 3
Starting from a Revit.  
Use Case 4
A Revit file and a Rhino file. 
Use 
Predefined Component Types
•	Geolocation
•	Project Origin
•	Design Document
•	Project Information
•	Organization  
•	2D Bounding Box
•	3D Bounding Box 
•	Geometry 
•	Convex Hull 
•	Building
•	Level 
•	Phase
•	Category
•	Property Set
•	Family
•	Family Instance
//==
IfcRoot
IfcObject
IfcProduct
IfcGroup – 
https://standards.buildingsmart.org/IFC/DEV/IFC4_2/FINAL/HTML/schema/ifcproductextension/lexical/ifcpositioningelement.htm
IfcAnnotation - https://standards.buildingsmart.org/IFC/DEV/IFC4_2/FINAL/HTML/schema/ifcproductextension/lexical/ifcannotation.htm 
IfcElement - https://standards.buildingsmart.org/IFC/DEV/IFC4_2/FINAL/HTML/schema/ifcproductextension/lexical/ifcelement.htm
IfcPort - https://standards.buildingsmart.org/IFC/DEV/IFC4_2/FINAL/HTML/schema/ifcproductextension/lexical/ifcport.htm
StructuralItem -https://standards.buildingsmart.org/IFC/DEV/IFC4_2/FINAL/HTML/schema/ifcstructuralanalysisdomain/lexical/ifcstructuralitem.htm

//==

To-do:
https://git-scm.com/docs/git-diff
//==

