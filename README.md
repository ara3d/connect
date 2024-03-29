# Ara 3D Connect 🌿

_Connect your BIM data, don't convert it!_ 

## Tracking, Reviewing, and Versioning Tools for BIM built on Git 

**Ara 3D Connect** is an open-source set of tools and specifications for working with BIM data that can work with Git and GitHub.

## Benefits

Ara 3D Connect allows you to track changes to entities across all documents in a BIM project. It does this independently of the
source design data, so it works seamlessly with your current workflows and data management processes. 

## How it Works

Ara 3D Connect by default stores component and entity identification data for a project in JSON files in a Git repository. 

You **do not** have to be familiar with Git to use Ara 3D Connect. The tools automate and streamline the process so 
you never have to leave your BIM authoring environment.  

> ### What is a Repository?
> 
> A repository is a set of files which are stored locally and on a server.
> Different people can have their own local copy of the repository.
> Contributors make and store changes to their own local copy.
> They regularly _push_ their local changes to the server, and
> regularly _pull_ the most up to data changes from the server to their local copy.
> If a _conflict_ happens then conflicts must be resolved and changes are merged.

# Getting Started

To get started with Ara 3D connect: 

1. initialize a repository using the _Initialize_ tool.    
1. install one or more _Publishing_ tools (e.g., a plug-in or command line tool) to publish selected data from your design documents
1. configure the publishing tool to declare which data you want to share, and when to publish (e.g., automatic or manual) 
1. start authoring and publishing your design documents
1. _Optionally:_ install and configure a _Reviewing_ tool, to track and review changes across the project.

## Functionality  

Ara 3D Connect includes a suite of open-source tools that are being built for Revit, Rhino, IFC, and more. 
Some work as plug-ins, and others as command-line tools.

There are three types of tools:

* **Initialization** - creates repositories locally and remotely, and installs or updates other tools.
* **Publishing** - push changes to repositories automatically or manually 
* **Reviewing** - track changes and conflicts, provides mechanisms for approving, rejecting, and resolving conflicts 

## Identity Conflicts 

One of the key contributions of Ara 3D Connect is that it solves the _identity problem_. This means it provides 
mechanisms to know when two identifiers in different tools or documents are refering to the same entity. 

## An ECS Data Architecture

Ara 3D connect uses an Entity Component System (ECS) architecture inspired by [Strange Matter](https://github.com/gschleusner1972/strange_matter)
and the [Unity](https://unity.com/ecs) game engine.

## Relationship to Speckle 

At the top-level, Speckle is a replacement for Git, and Ara 3D Connect works with Git. 

Speckle _converts_ design data into an object-representation that it stores in a server. Only data on the server is versioned. Ara 3D on the other
hand creates a reference to the design document data.      

Speckle and Ara 3D Connect can both co-exist and provide value together.

## Acknowledgements 

We wish to acknowledge the contributions of the following people and organizations:

* Valentin Noves of [E-Verse](https://e-verse.com/) who first shared with me a vision of using Git for version control of BIM data in 2020.     
* [Members of team "Identity"](https://github.com/ara3d/aec-hackathon-identity) who won **Best Overall Project** at the Zurich AEC Hackathon in February 2024.
* Philippe Block, Edyta Augustynowicz and Yashar Moradi who adjudicated the Zurich AEC Hackathon.
* Greg Schleusner whose work on [Strange Matter](https://github.com/gschleusner1972/strange_matter) was very influential.
* Maximilian Vomhof, [Opensource Construction](https://www.opensource.construction/en), and [Zhaw University](https://www.zhaw.ch/en/university/) who hosted and organized the AEC Hackathon in Zurich 2024.
* Damon Hernandez and the other founders and organizers of the [AEC Hackathon organization](https://hackaec.com/).
