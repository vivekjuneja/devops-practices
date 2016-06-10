# devops-practices
A list of practices that aligns with Devops methodology

The following cookbook will go into details of how DevOps expectations can be implemented. The explanation here is independent of any toolchain.

1. Can perform continuous integration for changes, and integrate changes often in a week. We do not use long running feature branches. 

Aspects
1.a) A reasonable Integration schedule is arrived by evaluating the Velocity, Volume and Variety of source code changes that span over a particular time period like for eg: a week.
1.b) Assume a reasonable integration schedule for the team. Let's say its every Thursday. Identify how much time is spent on integration of changes across all developers in team
1.c) Each Developer uses a short-lived branch for their changes. This short-lived branch is visible to all team members. The Developer could use non-visible branches that are also short-lived, but is only used for temporary purpose
1.c) A threshold for maximum merge time has to be set, against which the time for merge is measured. If the merge time is more than the threshold, then the integration / merge should happen more frequently
1.d) Volume of changes indicate the amount of changes that go in a particular span of time. Variety of changes indicate the type of changes, including CSR's of different complexities. The Variety is decided based on the number of High Complex, Medium Complex, Less Complex CSRs that are taken up in a given span of time. Velocity indicates how many changes have to go in a particular span of time. The Velocity is measured in terms of number of changes that are planned to go in a time period.
1.e) A common Branching strategy needs to be created that is followed by all developers in the team. This Branching strategy includes the guidelines when to create a branch, and when not to create a branch. It also contains the indicators when a Branch is needed, and how long it is to be maintained.
1.f) The final goal for this particular DevOps expectation is to make integration or merging a Non-event, meaning that this step is not a particular complex or time-consuming activity
1.g) Every merge must trigger a deployment to the Dev-Integration Environment. A Dev-Integration deployment is usually a single instance of the environment, meaning there will be no more than One Dev-Integration environment instance running at a time. If a repeated merge happens, then that merge overrides the existing Dev-Integration deployment.

2. Can perform continuous deployment for their changes

Aspects
A continuous deployment of a change in code or infrastructure leads to continuous provisioning of the virtual environment (dev, test or staging). An Environment refers to the runtime specific for the application including We application Server, Database, Network topology. The physical infrastructure or VMs can remain mutable if the cost of provisioning them is expensive. A Change does not travel to the environment via any means except creation of the entire environment running that change. An environment does not exist without a running code, including Production environment. If a Test environment is not needed, it does not exist. 
The process of creation of new Environment like Dev, Test, Staging and Prod must take minimal time to setup and configuration. Post the creation of the environment, it must accessible to the end user via friendly URLs or IP Addresses through easy to use interfaces. 
Developer can deploy Dev Env. that runs their specific committed changes via Branch deployment or Trunk Deployment. This Deployment however does not create any asset that is carried over to the further environments like Dev-Integration, Test etc.
Code Builds are generated twice, once during the Dev Environment provisioning, and other during the creation of the Dev-Integration Environment.
Build generated from Dev-Integration Environment is promoted to Test, Staging and eventually to Production environment
The Deployment process of a change that includes provisioning of environment is repeatable and consistent across different flavors of environment like Dev, Prod, Test etc. The Deployment process is version controlled, and can be bundled as a template with the application.
Dev Environments are short-lived and expire after a configurable amount of time. The expiration period is configurable and is known to the developer.
Dev-Integration deployments will be On-demand or triggered based on Schedule (like Nightly). This schedule can be changed at any point of time, and the schedule is known to all developers in the team. The Dev-Integration environments can be short-lived as well to allow for effective use of infrastructure. The expiration period for Dev-Integration is configurable
The Deployment process will acknowledge the variation amongst the environments like number of running instances, different integration points for upstream and downstream systems etc. This variation is captured as configuration templates, whose values are captured from the end user or predefined values before deployment.
A Deployment strategy needs to be created that will contain the details on what gets deployed, when does it get deployed and how often deployment happens

Can request for on-demand Test Environments (Performance, Functional, Security etc.)
Aspects
An environment differs from the other only in terms of the values for configuration templates like "number of instances", "integration points". The deployment system will tag each instance of an environment with a set of labels that will help the user to differentiate between such environments. This means, there is no limitation to the number and type of on-demand Environments that could be created, each serving the purpose of a specific need. The reason to have multiple instances of diverse environment types is to help isolate the respective behaviors like Performance, Functional, Security, Availability etc. under test conditions.
Tester can request to deploy Test environments via on-demand mechanism or through scheduled triggers.
Build generated from Dev-Integration Environment is promoted to Test, Staging and eventually to Production environment. The Tester can view the candidate Builds available for her to deploy as a new Test Environment.
An Environment is a running instance of a specification that contains how the application is deployed and run. This specification is version controlled, and is shared across all environments
Test Environments are short-lived and expire after a configurable amount of time. The expiration period is visible to the Tester.
Deployment to Test environments will be On-demand, and through a trigger based on Schedule. The On-demand deployment will require the tester to pick up the candidate Builds that are have been through the Dev-Integration phase. The Scheduled test deployments will pick up the next available Build that has been flagged by the Developers as a candidate for Test.
The Test phase may include running Test automation on the Deployed application. The Test automation can be integrated in the Deployment Pipeline.
The Deployment process for Test enviromments will acknowledge the variation amongst the environments like number of running instances, different integration points for upstream and downstream systems etc. This variation is captured as configuration templates, whose values are captured from the end user or predefined values before deployment.

Can perform release with minimal or no manual intervention
Aspects
A Developer can move her / his changes till Production environment without approval. Post Deployment to Production, Release process is kicked off. 
Release process requires approval process. The Approval process will require a person of authority to approve the changes to be released. 
The Release process essentially involves modifying the route for incoming traffic to the running instances. The release process will also ensure the management and supporting functions like Monitoring, Logging and Notifications system play well with the scenario where the old instances are removed.
A Canary style release is recommended, where-in new instances remain side by side with the old LIVE instances. The traffic is then progressively increased to the new instances, and take away from the old instances, until the new instance has the entire traffic flowing to it. If there is a problem that gets identified, then the Traffic can be shifted back to the old instance. In this case, Old instance do not get removed from the Production environment instantly. Rather, they are removed after a grace period, where the chances of reverting to the particular Old build is limited. 
The new instances keep running in the Canary, and based on the approval for the Release process, is brought in in a Rolling upgrade manner. This means, the old instances are asked to shutdown, and be removed from the traffic serving router. 
The Release process is unattended, except the process of Approval. In some cases, low impact changes which are tested, and is in no shape to impact the stability of the system are auto-approved. The Scope of auto-approval usually involves changes which are not visible to end customer. 
The goal for automated release is to ensure that most Releases are non-events. 

Can make breaking changes, but are in a position to revert back to any stable version with minimal or no manual intervention

Aspects
A roll back process requires access to old stable releases. The Old stable releases must be indicated with various attributes that provides enough information to identify the right version to roll back to. 
In cases where rollback impacts the upstream or downstream system, a rollback decision also requires approval process
Each build that gets deployed and then released is tagged with the appropriate build identification. This identification is used to associate the artifacts with the actual deployed and released instances. In some cases, we can use the Build ID that gets generated on each Build event. Together with Build Id, attributes such as Commit messages,


Developers can make themselves available for support if something breaks in Production 
Aspects
Allow automated creation of new Environments that take minimal time
Developer can deploy Dev Env. that runs their specific committed changes via Branch deployment or Trunk Deployment
Build generated from Dev-Integration Environment is promoted to Test, Staging and eventually to Production environment
An Environment is a running instance of a specification that contains how the application is deployed and run. This specification is version controlled, and is shared across all environments
Dev Environments are short-lived and expire after a configurable amount of time.
Dev-Integration deployments will be On-demand or triggered based on Schedule (like Nightly).
Deployment to Test environments will be On-demand

Development, Testing and Production environments are similar to each other, except in the number of instances / or CPU and Memory
Aspects
Allow automated creation of new Environments that take minimal time
Developer can deploy Dev Env. that runs their specific committed changes via Branch deployment or Trunk Deployment
Build generated from Dev-Integration Environment is promoted to Test, Staging and eventually to Production environment
An Environment is a running instance of a specification that contains how the application is deployed and run. This specification is version controlled, and is shared across all environments
Dev Environments are short-lived and expire after a configurable amount of time.
Dev-Integration deployments will be On-demand or triggered based on Schedule (like Nightly).
Deployment to Test environments will be On-demand

Can test in Production without affecting the behavior of upstream and downstream systems
Aspects
Allow automated creation of new Environments that take minimal time
Developer can deploy Dev Env. that runs their specific committed changes via Branch deployment or Trunk Deployment
Build generated from Dev-Integration Environment is promoted to Test, Staging and eventually to Production environment
An Environment is a running instance of a specification that contains how the application is deployed and run. This specification is version controlled, and is shared across all environments
Dev Environments are short-lived and expire after a configurable amount of time.
Dev-Integration deployments will be On-demand or triggered based on Schedule (like Nightly).
Deployment to Test environments will be On-demand

Can ramp-up or ramp-down in the limits of the raw hardware capacity allocated to them, without the involvement of Operations staff
Aspects
Allow automated creation of new Environments that take minimal time
Developer can deploy Dev Env. that runs their specific committed changes via Branch deployment or Trunk Deployment
Build generated from Dev-Integration Environment is promoted to Test, Staging and eventually to Production environment
An Environment is a running instance of a specification that contains how the application is deployed and run. This specification is version controlled, and is shared across all environments
Dev Environments are short-lived and expire after a configurable amount of time.
Dev-Integration deployments will be On-demand or triggered based on Schedule (like Nightly).
Deployment to Test environments will be On-demand

Can test their application in isolation
Aspects
Allow automated creation of new Environments that take minimal time
Developer can deploy Dev Env. that runs their specific committed changes via Branch deployment or Trunk Deployment
Build generated from Dev-Integration Environment is promoted to Test, Staging and eventually to Production environment
An Environment is a running instance of a specification that contains how the application is deployed and run. This specification is version controlled, and is shared across all environments
Dev Environments are short-lived and expire after a configurable amount of time.
Dev-Integration deployments will be On-demand or triggered based on Schedule (like Nightly).
Deployment to Test environments will be On-demand

No need to wait to release changes to Production environment, without affecting the stability of upstream and downstream systems
Aspects
Allow automated creation of new Environments that take minimal time
Developer can deploy Dev Env. that runs their specific committed changes via Branch deployment or Trunk Deployment
Build generated from Dev-Integration Environment is promoted to Test, Staging and eventually to Production environment
An Environment is a running instance of a specification that contains how the application is deployed and run. This specification is version controlled, and is shared across all environments
Dev Environments are short-lived and expire after a configurable amount of time.
Dev-Integration deployments will be On-demand or triggered based on Schedule (like Nightly).
Deployment to Test environments will be On-demand

Has access to all required tools to monitor, debug and diagnose their applications at all time
Aspects
Allow automated creation of new Environments that take minimal time
Developer can deploy Dev Env. that runs their specific committed changes via Branch deployment or Trunk Deployment
Build generated from Dev-Integration Environment is promoted to Test, Staging and eventually to Production environment
An Environment is a running instance of a specification that contains how the application is deployed and run. This specification is version controlled, and is shared across all environments
Dev Environments are short-lived and expire after a configurable amount of time.
Dev-Integration deployments will be On-demand or triggered based on Schedule (like Nightly).
Deployment to Test environments will be On-demand

Can measure productivity in terms of usable data, rather than subjective explanation.
Aspects
Allow automated creation of new Environments that take minimal time
Developer can deploy Dev Env. that runs their specific committed changes via Branch deployment or Trunk Deployment
Build generated from Dev-Integration Environment is promoted to Test, Staging and eventually to Production environment
An Environment is a running instance of a specification that contains how the application is deployed and run. This specification is version controlled, and is shared across all environments
Dev Environments are short-lived and expire after a configurable amount of time.
Dev-Integration deployments will be On-demand or triggered based on Schedule (like Nightly).
Deployment to Test environments will be On-demand

No need to wait for other developer to test her / his changes 
Aspects
Allow automated creation of new Environments that take minimal time
Developer can deploy Dev Env. that runs their specific committed changes via Branch deployment or Trunk Deployment
Build generated from Dev-Integration Environment is promoted to Test, Staging and eventually to Production environment
An Environment is a running instance of a specification that contains how the application is deployed and run. This specification is version controlled, and is shared across all environments
Dev Environments are short-lived and expire after a configurable amount of time.
Dev-Integration deployments will be On-demand or triggered based on Schedule (like Nightly).
Deployment to Test environments will be On-demand

All team members have complete knowledge about the constituents of the application environment across Dev, Test and Prod. 
Aspects
Allow automated creation of new Environments that take minimal time
Developer can deploy Dev Env. that runs their specific committed changes via Branch deployment or Trunk Deployment
Build generated from Dev-Integration Environment is promoted to Test, Staging and eventually to Production environment
An Environment is a running instance of a specification that contains how the application is deployed and run. This specification is version controlled, and is shared across all environments
Dev Environments are short-lived and expire after a configurable amount of time.
Dev-Integration deployments will be On-demand or triggered based on Schedule (like Nightly).
Deployment to Test environments will be On-demand

Can deploy small changes often to the Production environment, instead of large number of big changes. 
Aspects
Allow automated creation of new Environments that take minimal time
Developer can deploy Dev Env. that runs their specific committed changes via Branch deployment or Trunk Deployment
Build generated from Dev-Integration Environment is promoted to Test, Staging and eventually to Production environment
An Environment is a running instance of a specification that contains how the application is deployed and run. This specification is version controlled, and is shared across all environments
Dev Environments are short-lived and expire after a configurable amount of time.
Dev-Integration deployments will be On-demand or triggered based on Schedule (like Nightly).
Deployment to Test environments will be On-demand

Always use CI and CD Pipeline to deploy changes to Production only. No direct changes are made to Production
Aspects
Allow automated creation of new Environments that take minimal time
Developer can deploy Dev Env. that runs their specific committed changes via Branch deployment or Trunk Deployment
Build generated from Dev-Integration Environment is promoted to Test, Staging and eventually to Production environment
An Environment is a running instance of a specification that contains how the application is deployed and run. This specification is version controlled, and is shared across all environments
Dev Environments are short-lived and expire after a configurable amount of time.
Dev-Integration deployments will be On-demand or triggered based on Schedule (like Nightly).
Deployment to Test environments will be On-demand

A Change in Infrastructure Topology also uses CI and CD to deploy it across all the environments till Production. No Infrastructure change is made outside the Pipeline
Aspects
Allow automated creation of new Environments that take minimal time
Developer can deploy Dev Env. that runs their specific committed changes via Branch deployment or Trunk Deployment
Build generated from Dev-Integration Environment is promoted to Test, Staging and eventually to Production environment
An Environment is a running instance of a specification that contains how the application is deployed and run. This specification is version controlled, and is shared across all environments
Dev Environments are short-lived and expire after a configurable amount of time.
Dev-Integration deployments will be On-demand or triggered based on Schedule (like Nightly).
Deployment to Test environments will be On-demand

On Commit, a build is generated. If the Build fails, then the developer is notified of the change. Build status of the Trunk / Master should be Green, that is Ready to be released.
Aspects
Allow automated creation of new Environments that take minimal time
Developer can deploy Dev Env. that runs their specific committed changes via Branch deployment or Trunk Deployment
Build generated from Dev-Integration Environment is promoted to Test, Staging and eventually to Production environment
An Environment is a running instance of a specification that contains how the application is deployed and run. This specification is version controlled, and is shared across all environments
Dev Environments are short-lived and expire after a configurable amount of time.
Dev-Integration deployments will be On-demand or triggered based on Schedule (like Nightly).
Deployment to Test environments will be On-demand

Automated Unit and Acceptance Tests exist for the code, and are integrated in the Build and Deployment Pipeline. 
Aspects
Allow automated creation of new Environments that take minimal time
Developer can deploy Dev Env. that runs their specific committed changes via Branch deployment or Trunk Deployment
Build generated from Dev-Integration Environment is promoted to Test, Staging and eventually to Production environment
An Environment is a running instance of a specification that contains how the application is deployed and run. This specification is version controlled, and is shared across all environments
Dev Environments are short-lived and expire after a configurable amount of time.
Dev-Integration deployments will be On-demand or triggered based on Schedule (like Nightly).
Deployment to Test environments will be On-demand

There are clear Performance and Availability metrics available for the Code.
Aspects
Allow automated creation of new Environments that take minimal time
Developer can deploy Dev Env. that runs their specific committed changes via Branch deployment or Trunk Deployment
Build generated from Dev-Integration Environment is promoted to Test, Staging and eventually to Production environment
An Environment is a running instance of a specification that contains how the application is deployed and run. This specification is version controlled, and is shared across all environments
Dev Environments are short-lived and expire after a configurable amount of time.
Dev-Integration deployments will be On-demand or triggered based on Schedule (like Nightly).
Deployment to Test environments will be On-demand










