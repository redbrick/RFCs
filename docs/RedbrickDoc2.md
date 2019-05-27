# Redbrick Documentation 2.0
Status: proposal | **accepted** | rejected | deprecated | superseded
Author: Tom Doyle (greenday)
Date: 2019-05-03

## Context 
Documentation is a vital part of learning, maintaining and handing over of all infrastructure that Redbrick runs. It is a way of teaching and providing knowledge transfer that is agreed upon. This is vital in the context of Redbrick because of the fast handover that happens within Redbrick. This is because an admin can only hold their position for two years and spend only and average of three to four years around as a primary part of the team. For this reason knowledge transfer is vital. 

## Current Configuration 
* The current documentation is held in a central Github repository that is independent of any other repository
* This repository is then put through a Circleci pipeline where the documentation is built into a docker container using the html-proofer image. This will build the docs with `mkdocs`. All internal links are then checked
* The docs site is then deployed every hour on the server currently under the name halfpint. The deployment method is a cron job that runs `git pull && mkdocs build`

## Requirements/Definition of Done
* Provide easy to access documentation for services, hardware and configurations
* Provide redundant access to documentation should all of Redbrick machines be off or unavailable
* Provide a method for collaboration on documents by any person not just members of the society or administrators
* Provide access to documents by anyone not just members of the society or administrators
* Provide the most simple possible deployment solution yet robust
* Provide a simple method to add and edit documentation as to not deter people from writing it 
* Add an incentive to create documentation as changes are being made
* Provide a halt system to stop changes being made without documenting these changes
* Provide redundancy so as no one point of failure can delete all the docs

## Thoughts & Research
There is a solution currently deployed at Redbrick. In all my years studying and training to become an admin it has always been the case that the documentation has been yards behind what has been the case in production. To keep documentation up to date it would often mean making a pull request to a repository, going through the hassle of getting it approved and merged, then starting the process of writing documentation to go with that, putting it in a new repository and going through the hassle of getting that reviewed, approved and added to the repository. There were often markdown linters on this documentation repository also making the pull request process somewhat tiresome.

While this was a good solution at the time, I think that we can make improvements on this system. One problem is that the documentation for a service is very far away from the system, a completely different repository. This leads to duplication of documentation in the best case scenario with README, and docs in the repository and in the main repository. This leads to the duplication of work and the duplication of documentation. Following on it is often likely that one will get updated and not the other causing chaos as to which is correct. 

Another problem with the current set up is the fact that changes are often made without a second thought for documentation, often forgotten in the heat of the fix. If we could make one of the approval requirements documentation for you change we could ensure that the documentation is always in the most up to date and correct state.

With new changes that are being made in the industry and talk of Redbrick following suit towards the infrastructure as code movement there will be more code than ever for deployments and services. This provides a great opportunity for Redbrick to adopt a documentation per repository approach. This means that changing a services and deploying it would mean making the change in the repository but also the documentation for the change also. One code review would then suffice for the documentation change, in one place, that is close to the service. It is a firm belief of this author that the closer the documentation to the code the more correct it is likely to be.

With all the documentation folders being automatically centralized it brings in unwanted and unwarranted complexity, until you realize that one of the requirements we laid out above was the redundant need to have not a single point of failure. The would normally involve moving documentation to a back up location or away from this repository. Having the central repository allows all the documentation to sit in the on place but also be distributed to every project it describes at the same time. I also feel it is a great point to be able to supply people that are using and working with the repository a local manual. 

The drawback to this does seem to be at the current draft there is no software to centralize all these files into a useful location. It is however not a hard problem to solve, and the required custom solution would not have to be a complex or huge system. But with adding custom code to the stack that Redbrick runs it does add a layer of complexity and the add responsibility of maintenance. I do believe at this point that the benefits of this solution out weight the draw backs 

### Solution Diagram
![](https://i.imgur.com/qiKOCdl.png)

## Pros and Cons of the Options

#### Appeals of Solution
* Documentation is closer to source
* Documentation is only written once
* Documentation can be enforced where needed
* Documentation is in redundant locations
* Documentation is easy to access
* Documentation is easy to contribute to
* Provide platform to share knowledge with members, netsocs and the world


#### Limitations of Solution
* Adds complexity to system 
* Involves writing and maintaining scripts
* Adds a learning aspect on how to maintain documentation
* Makes documentation another service to handle
* Limits speed of pull request being made


## Deployment Plan
* Currently the projects are stored in a git repository this would mean that the documentation would have to reside with it for this solution to be effective.
* The custom part of this solution is to write a central aggregator, to take all the documentation folders in all the repositories of an organisation and merge them into one repository, automatically and regularly. This can be achieved with webhooks
* It will be then part of that pipeline to deploy these documents to a docker container running as a Redbrick service.

*Note*: It is very well understood that this is a high level approach. There can also be a push or pull design implemented here. Collisions with two repositories may mean that a pull option may be a better solution or a push may work just fine. These details are not blockers but up to the designer to make a choice. 
