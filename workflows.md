# About Ara 3D Connect

_Ara 3D Connect_ is an ecosystem of tools and specifications designed to let you track changes to BIM projects as they happen across different authoring tools without having to share the source documents.
It is inspired by how software developers work. 

It is intended to save time in BIM coordination scenarios such as where architects and engineers have to work together. 

It makes it faster to find conflicts, resolve where they are, and reduce the possibility of them being created in the first place.    

_In summary_: Ara 3D helps users synchronize work and assure consistency of the project while work continues independently 
on different documents without disrupting workflows.  

# How it Works for a User

* **Initialization** - Users run an initialization tool. It installs plug-ins and tools, and creates a copy of a repository
* **Publish** - They configure what data they want to publish and how often. By default it publishes whenever they save. Each publish action creates a _snapshot_.
* **Requesting Review** - When a user wants to merge their current work into the main, they request a review.    
* **Review** - Reviewers and observers are informed when a review has been requested, and can comment on them, and approve or reject.  
* **Synchronize** - A special kind of review, where a person tracks difference between their version of entity data with that in the database.   
* **Compare** - Users may compare any two snapshots for geometry and data differences.  
* **Detect** - Tools are provided to detect problems such as duplicated entities, or ill-formed components.   
* **Resolving Conflict** - When a component is changed by two people separately, tools help them resolve, a merge-conflict will occur.            
* **Revert** - Users may revert their local design documents back to a previous snapshot. 

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

# What is Exactly Publishing?

When a design document is published, several things happen.

* The user enters a commit message. 
* The design document is saved.
* The design document is added to the working document repository (`git commit -a -m <message>`).
* Component records are created for all "published" data according to the publishing configuration.
* If the component records are already in the ECS JSON file they are updated, if not they are added.
* If any design documents are missing, which are "tracked" they user is given the option to "untrack", or the associated components are deleted.
* If any payloads are no longer referenced, they are deleted. 
* The coordination repository is updated (`git commit -a -m <message>`).
* The changes are pushed to the online repository.

# Merge Conflicts and Synchronization

Let's say someone (user A) is working on a Rhino file (F).
They are tracking it and publishing changes. 
They send it to someone else (user B) who also started tracking it.
The new version called F2. 
When user A makes a change, user B can go into synchronization view. 
To resolve synchronization differences user B can:

1. get an updated version of the file F (called F2) from user A to replace their copy of F1
2. make the changes themself in file F1
3. stop publishing from their file that entity, and optionally delete it.

It should not arrive under most occurences to have an actual conflict merge.
When an entity changes in different user copies of a source file, they are treated lile
different entities, and get new copies of the component. 

# Duplication Detection and Resolution

In the situaiation above when a user gets a new file F2, the system will now see
a set of changes from F1 to F2, which are actually aligned with the source file F. 
This is where duplication detection comes into play: the system looks at those
entities and determines that they are the same. If there is ambiguity then
new entities and components will be created. The user can forcefully merge them. 

# Important Git Details

Many of the following details and recommendations should be automated through tools: 

- Every publish corresponds to a "git add" and a "commit".
- Restoring to a previous commit, means that the _Working Document Repository_ is restored. 
- The _Coordination Repository_ branches should never be "reverted". It only advances.
- When restoring to a previous commit, a special commit is a 
- Every user should work in their own branch. 
- User branches are named with their username followed by a `\` followed by a branchname.
         
# Entities, Component Data and Payloads

Data is stored in as JSON records called components and external files call payloads. 
Components are associated with entities via an entity ID.
Entities are simply UUID. They are defined by the associated payload. 

A component is a JSON record that has a:

-	Entity ID – a UUID representing the entity the component is associated with. 
-	Component Type – a URN that specifies the kind of component.
-	Component ID – a UUID that is created when the component is first created and doesn’t change even when the component data changes.
-	Component Version ID – a UUID that is updated whenever data associated with the component is changed. 
-	Source ID – a UUID that represents the source design document entity. Additional information can be queried via design document components.     
-	Optional: Payload – a record which has a:
  -	relative path (string) to a payload file in the repository  
  -	a MIME type (string) describing the type of data  
  - an MD5 hash (string) that uniquely identifies the contents of the file
- Optional: Data - a record with arbitrary data.  

# Terminology

-	Branch – a named commit, that is used as the ancestor of two or more parallel commit histories. 
-	Commit – a snapshot of the change history, has an ID and a message.
-	Component – A group of related data relevant to some part of the project stored as a JSON object, with optional payload. 
-	Design document – any file that is used to specify design intent. This could be a CAD file, a BIM file, or a specification file. It could even be a CSV or DOCX file. 
-	ECS – Stands for Entity Component System and is a method of organizing data. 
-	ECS Database – the JSON file(s) containing a list of component records.
-	Entity – An abstract or physical part of a project (e.g., room, building, wall, outlet, etc.) 
-	Initialization – either the process of creating an initial Git repository online, or the process of making a local copy of an online repository. 
-	Mainline – a commit history that originates from the first commit and is used as the official history of the final product 
-	Merge – the combing of one commit (repository snapshot) into another (files and change history) 
-	Merge Conflict – when two commits overwrite each other. 
-	Payload – a text or binary file containing additional data not in the main ECS database.
-	Publish – update of the ECS database      
-	Publishing configuration – a JSON file that specifies what is published by a publishing tool.  
-	Publishing tool – A plug-in or command-line tool that updates the ECS database and issues commands to Git. 
-	Pull – when the online repository state is 
-	Pull Request – a proposal to merge a set of changes from one branch into another
-	Push – when the current repository state is merged into the online repository. This can be blocked if their is a merge conflict. 
-	Repository – a collection of files and their change history.  
- Synchronization - the act of viewing local entity data in an authoring tool compared to the current state of the ECS.  
