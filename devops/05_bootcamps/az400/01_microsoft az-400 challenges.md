
==link: https://learn.microsoft.com/en-us/collections/mormao0n8jo3r7?sharingId=81BD9974A8BFFD25

# Introduction to technical debt

Completed 100 XP

- 5 minutes

**Technical debt** is a term that describes the future cost that will be incurred by choosing an easy solution today instead of using better practices because they would take longer to complete.

The term-technical debt was chosen for its comparison to financial debt. It's common for people in financial debt to make decisions that seem appropriate or the only option at the time, but in so doing, interest increases.

The more interest that accumulates, the harder it is for them in the future and the more minor options available to them later. With financial debt, soon, interest increases on interest, creating a snowball effect. Similarly, technical debt can build up to the point where developers spend almost all their time sorting out problems and doing rework, either planned or unplanned, rather than adding value.

So, how does it happen?

The most common excuse is tight deadlines. When developers are forced to create code quickly, they'll often take shortcuts. For example, instead of refactoring a method to include new functionality, let us copy to create a new version. Then I only test my new code and can avoid the level of testing required if I change the original method because other parts of the code use it.

Now I have two copies of the same code that I need to modify in the future instead of one, and I run the risk of the logic diverging. There are many causes. For example, there might be a lack of technical skills and maturity among the developers or no clear product ownership or direction.

The organization might not have coding standards at all. So, the developers didn't even know what they should be producing. The developers might not have precise requirements to target. Well, they might be subject to last-minute requirement changes.

Necessary-refactoring work might be delayed. There might not be any code quality testing, manual or automated. In the end, it just makes it harder and harder to deliver value to customers in a reasonable time frame and at a reasonable cost.

Technical debt is one of the main reasons that projects fail to meet their deadlines.

Over time, it increases in much the same way that monetary debt does. Common sources of technical debt are:

- Lack of coding style and standards.
- Lack of or poor design of unit test cases.
- Ignoring or not-understanding object orient design principles.
- Monolithic classes and code libraries.
- Poorly envisioned the use of technology, architecture, and approach. (Forgetting that all system attributes, affecting maintenance, user experience, scalability, and others, need to be considered).
- Over-engineering code (adding or creating code that isn't required, adding custom code when existing libraries are sufficient, or creating layers or components that aren't needed).
- Insufficient comments and documentation.
- Not writing self-documenting code (including class, method, and variable names that are descriptive or indicate intent).
- Taking shortcuts to meet deadlines.
- Leaving dead code in place.  
    

Note

Over time, the technical debt must be paid back. Otherwise, the team's ability to fix issues and implement new features and enhancements will take longer and eventually become cost-prohibitive.  

We have seen that technical debt adds a set of problems during development and makes it much more difficult to add extra customer value.

Having technical debt in a project saps productivity, frustrates development teams, makes code both hard to understand and fragile, increases the time to make changes and validate those changes. Unplanned work frequently gets in the way of planned work.

Longer-term, it also saps the organization's strength. Technical debt tends to creep up on an organization. It starts small and grows over time. Every time a quick hack is made or testing is circumvented because changes need to be rushed through, the problem grows worse and worse. Support costs get higher and higher, and invariably, a serious issue arises.

Eventually, the organization can't respond to its customers' needs in a timely and cost-efficient way.

## Automated measurement for monitoring

One key way to minimize the constant acquisition of technical debt is to use automated testing and assessment.

In the demos that follow, we'll look at one of the common tools used to assess the debt: SonarCloud. (The original on-premises version was SonarQube).

There are other tools available, and we'll discuss a few of them.

Later, in the next hands-on lab, you'll see how to configure your Azure Pipelines to use SonarCloud, understand the analysis results, and finally how to configure quality profiles to control the rule sets that are used by SonarCloud when analyzing your projects.

For more information, see [SonarCloud](https://sonarcloud.io/about).

## To review:

Azure DevOps can be integrated with a wide range of existing tooling used to check code quality during builds.

Which code quality tools do you currently use (if any)?

What do you like or don't you like about the tools?

---


# Plan effective code reviews

Completed 100 XP

- 2 minutes

Most developers would agree that code reviews can improve the quality of the applications they produce, but only if the process for the code reviews is effective. It is essential, upfront, to agree that everyone is trying to achieve better code quality.

Achieving code quality can seem to challenge because there's no single best way to write any piece of code, at least code with any complexity. Everyone wants to do good work and to be proud of what they create.

It means that it's easy for developers to become over-protective of their code. The organizational culture must let all involved feel that the code reviews are more like mentoring sessions where ideas about improving code are shared than interrogation sessions where the aim is to identify problems and blame the author.

The knowledge-sharing that can occur in mentoring-style sessions can be one of the most important outcomes of the code review process. It often happens best in small groups (even two people) rather than in large team meetings. And it's important to highlight what has been done well, not just what needs improvement.

Developers will often learn more in effective code review sessions than they'll in any formal training. Reviewing code should be an opportunity for all involved to learn, not just as a chore that must be completed as part of a formal process.

It's easy to see two or more people working on a problem and think that one person could have completed the task by themselves. That is a superficial view of the longer-term outcomes.

Team management needs to understand that improving the code quality reduces the cost of code, not increases it. Team leaders need to establish and foster an appropriate culture across their teams.

## Use CI and CD for your project

Continuous integration is used to automate tests and builds for your project. CI helps to catch bugs or issues early in the development cycle when they're easier and faster to fix. Items known as artifacts are produced from CI systems. The continuous delivery release pipelines use them to drive automatic deployments.

Continuous delivery is used to automatically deploy and test code in multiple stages to help drive quality. Continuous integration systems produce deployable artifacts, which include infrastructure and apps. Automated release pipelines consume these artifacts to release new versions and fixes to the target of your choice.

|**Continuous integration (CI)**|**Continuous delivery (CD)**|
|---|---|
|Increase code coverage.|Automatically deploy code to production.|
|Build faster by splitting test and build runs.|Ensure deployment targets have the latest code.|
|Automatically ensure you don't ship broken code.|Use tested code from the CI process.|
|Run tests continually.||

## Continuous delivery

Continuous delivery (CD) (also known as Continuous Deployment) is a process by which code is built, tested, and deployed to one or more test and production stages. Deploying and testing in multiple stages helps drive quality. Continuous integration systems produce deployable artifacts, which include infrastructure and apps. Automated release pipelines consume these artifacts to release new versions and fix existing systems. Monitoring and alerting systems constantly run to drive visibility into the entire CD process. This process ensures that errors are caught often and early.

## Continuous integration

Continuous integration (CI) is the practice used by development teams to simplify the testing and building of code. CI helps to catch bugs or problems early in the development cycle, making them more accessible and faster to fix. Automated tests and builds are run as part of the CI process. The process can run on a schedule, whenever code is pushed, or both. Items known as artifacts are produced from CI systems. The continuous delivery release pipelines use them to drive automatic deployments.

## Deployment target

A deployment target is a virtual machine, container, web app, or any service used to host the developed application. A pipeline might deploy the app to one or more deployment targets after the build is completed and tests are run.

## Job

A build contains one or more jobs. Most jobs run on an agent. A job represents an execution boundary of a set of steps. All the steps run together on the same agent.

For example, you might build two configurations - x86 and x64. In this case, you have one build and two jobs.

## Pipeline

A pipeline defines the continuous integration and deployment process for your app. It's made up of steps called tasks.

It can be thought of as a script that describes how your test, build, and deployment steps are run.

## Release

When you use the visual designer, you can create a release or a build pipeline. A release is a term used to describe one execution of a release pipeline. It's made up of deployments to multiple stages.

## Stage

Stages are the primary divisions in a pipeline: "build the app," "run integration tests," and "deploy to user acceptance testing" are good examples of stages.

## Task

A task is the building block of a pipeline. For example, a build pipeline might consist of build and test tasks. A release pipeline consists of deployment tasks. Each task runs a specific job in the pipeline.

## Trigger

A trigger is set up to tell the pipeline when to run. You can configure a pipeline to run upon a push to a repository at scheduled times or upon completing another build. All these actions are known as triggers.

---

# What is blue-green deployment?

Completed 100 XP

- 3 minutes

Blue-green deployment is a technique that reduces risk and downtime by running two identical environments. These environments are called blue and green.

Only one of the environments is live, with the live environment serving all production traffic.

![[Pasted image 20241208064851.png]]

for this example, blue is currently live, and green is idle.

As you prepare a new version of your software, the deployment and final testing stage occur in an environment that isn't live: in this example, green. Once you've deployed and thoroughly tested the software in green, switch the router or load balancer so all incoming requests go to green instead of blue.

Green is now live, and blue is idle.

This technique can eliminate downtime because of app deployment. Besides, blue-green deployment reduces risk: if something unexpected happens with your new version on the green, you can immediately roll back to the last version by switching back to blue.

When it involves database schema changes, this process isn't straightforward. You probably can't swap your application. In that case, your application and architecture should be built to handle both the old and the new database schema.

When using a cloud platform like Azure, doing blue-green deployments is relatively easy. You don't need to write your code or set up infrastructure. You can use an out-of-the-box feature called deployment slots when using web apps.

Deployment slots are a feature of Azure App Service. They're live apps with their hostnames. You can create different slots for your application (for example, Dev, Test, or Stage). The production slot is the slot where your live app stays. You can validate app changes in staging with deployment slots before swapping them with your production slot.

You can use a deployment slot to set up a new version of your application, and when ready, swap the production environment with the new staging environment. It's done by an internal swapping of the IP addresses of both slots.

## Swap

The swap eliminates downtime when you deploy your app with seamless traffic redirection, and no requests are dropped because of swap operations.

# Introduction to feature toggles

Feature Flags allow you to change how our system works without making significant changes to the code. Only a small configuration change is required. In many cases, it will also only be for a few users.

Feature Flags offer a solution to the need to push new code into the trunk and deploy it, but it isn't functional yet.

They're commonly implemented as the value of variables used to control conditional logic.

Imagine that your team is all working in the main trunk branch of a banking application.

You've decided it's worth trying to have all the work done in the main branch to avoid messy operations of merge later.

Still, you need to ensure that significant changes to the interest calculations can happen, and people depend on that code every day.

Worse, the changes will take you weeks to complete. You can't leave the main code broken for that period.

A Feature Flag could help you get around it.

You can change the code so that other users who don't have the Feature Flag set will use the original interest calculation code.

The members of your team who are working on the new interest calculations and set to see the Feature Flag will have the new interest calculation code.

It's an example of a business feature flag used to determine business logic.

The other type of Feature Flag is a Release Flag. Now, imagine that after you complete the work on the interest calculation code, you're nervous about publishing a new code out to all users at once.

You have a group of users who are better at dealing with new code and issues if they arise, and these people are often called Canaries.

The name is based on the old use of the Canaries in coal mines.

You change the configuration so that the Canary users also have the Feature Flag set, and they'll start to test the new code as well. If problems occur, you can quickly disable the flag for them again.

Another release flag might be used for AB testing. Perhaps you want to find out if a new feature makes it faster for users to complete a task.

You could have half the users working with the original version of the code and the other half working with the new code version.

You can then directly compare the outcome and decide if the feature is worth keeping. Feature Flags are sometimes called Feature Toggles instead.

By exposing new features by just "flipping a switch" at runtime, we can deploy new software without exposing any new or changed functionality to the end-user.

The question is, what strategy do you want to use in releasing a feature to an end-user.

- Reveal the feature to a segment of users, so you can see how the new feature is received and used.
- Reveal the feature to a randomly selected percentage of users.
- Reveal the feature to all users at the same time.

The business owner plays a vital role in the process, and you need to work closely with them to choose the right strategy.

Just as in all the other deployment patterns mentioned in the introduction, the most crucial part is always looking at how the system behaves.

The idea of separating feature deployment from Feature exposure is compelling and something we want to incorporate in our Continuous Delivery practice.

It helps us with more stable releases and better ways to roll back when we run into issues when we have a new feature that produces problems.

We switch it off again and then create a hotfix. By separating deployments from revealing a feature, you make the opportunity to release a day anytime since the new software won't affect the system that already works.

## What are feature toggles?

Feature toggles are also known as feature flippers, feature flags, feature switches, conditional features, and so on.

Besides the power they give you on the business side, they also provide an advantage on the development side.

Feature toggles are a great alternative to branching as well. Branching is what we do in our version control system.

To keep features isolated, we maintain a separate branch.

When we want the software to be in production, we merge it with the release branch and deploy it.

With feature toggles, you build new features behind a toggle. Your feature is "off" when a release occurs and shouldn't be exposed to or impact the production software.

## How to implement a feature toggle

In the purest form, a feature toggle is an IF statement.

When the switch is off, it executes the code in the IF, otherwise the ELSE.

You can make it much more intelligent, controlling the feature toggles from a dashboard or building capabilities for roles, users, and so on.

If you want to implement feature toggles, many different frameworks are available commercially as Open Source.

A feature toggle is just code. And to be more specific, conditional code. It adds complexity to the code and increases the technical debt.

Be aware of that when you write them, and clean up when you don't need them anymore.

While feature flags can be helpful, they can also introduce many issues of their own.

The idea of a toggle is that it's short-lived and only stays in the software when it's necessary to release it to the customers.

You can classify the different types of toggles based on two dimensions as described by Martin Fowler.

He states that you can look at the dimension of how long a toggle should be in your codebase and, on the other side how dynamic the toggle needs to be.

he most important thing is to remember that you need to remove the toggles from the software.

If you don't do that, they'll become a form of technical debt if you keep them around for too long.

As soon as you introduce a feature flag, you've added to your overall technical debt.

Like other technical debt, they're easy to add, but the longer they're part of your code, the bigger the technical debt becomes because you've added scaffolding logic needed for the branching within the code.

The cyclomatic complexity of your code keeps increasing as you add more feature flags, as the number of possible paths through the code increases.

Using feature flags can make your code less solid and can also add these issues:

- The code is harder to test effectively as the number of logical combinations increases.
- The code is harder to maintain because it's more complex.
- The code might even be less secure.
- It can be harder to duplicate problems when they're found.

A plan for managing the lifecycle of feature flags is critical. As soon as you add a flag, you need to plan for when it will be removed.

Feature flags shouldn't be repurposed. There have been high-profile failures because teams decided to reuse an old flag that they thought was no longer part of the code for a new purpose.

## Tooling for release flag management

The amount of effort required to manage feature flags shouldn't be underestimated. It's essential to consider using tooling that tracks:

- Which flags exist.
- Which flags are enabled in which environments, situations, or target customer categories.
- The plan for when the flags will be used in production.
- The plan for when the flags will be removed.

Using a feature flag management system lets you get the benefits of feature flags while minimizing the risk of increasing your technical debt too high.

# Implement canary releases and dark launching

The term canary release comes from the days that miners took a canary with them into the coal mines.

The purpose of the canary was to identify the existence of toxic gasses.

The canary would die much sooner than the miner, giving them enough time to escape the potentially lethal environment.

A canary release is a way to identify potential problems without exposing all your end users to the issue at once.

The idea is that you tell a new feature only to a minimal subset of users.

By closely monitoring what happens when you enable the feature, you can get relevant information from this set of users and either continue or rollback (disable the feature).

If the canary release shows potential performance or scalability problems, you can build a fix for that and apply that in the canary environment.

After the canary release has proven to be stable, you can move the canary release to the actual production environment.

![[Pasted image 20241208070435.png]]

Canary releases can be implemented using a combination of feature toggles, traffic routing, and deployment slots.

- You can route a percentage of traffic to a deployment slot with the new feature enabled.
- You can target a specific user segment by using feature toggles.

Traffic Manager is resilient to failure, including the breakdown of an entire Azure region.

While the available options can change over time, the Traffic Manager currently provides six options to distribute traffic:

- **Priority**: Select Priority when you want to use a primary service endpoint for all traffic and provide backups if the primary or the backup endpoints are unavailable.
- **Weighted**: Select Weighted when you want to distribute traffic across a set of endpoints, either evenly or according to weights, which you define.
- **Performance**: Select Performance when you have endpoints in different geographic locations, and you want end users to use the "closest" endpoint for the lowest network latency.
- **Geographic**: Select Geographic so that users are directed to specific endpoints (Azure, External, or Nested) based on which geographic location their DNS query originates from. It empowers Traffic Manager customers to enable scenarios where knowing a user's geographic region and routing them based on that is necessary. Examples include following data sovereignty mandates, localization of content & user experience, and measuring traffic from different regions.
- **Multivalue**: Select MultiValue for Traffic Manager profiles that can only have IPv4/IPv6 addresses as endpoints. When a query is received for this profile, all healthy endpoints are returned.
- **Subnet**: Select the Subnet traffic-routing method to map sets of end-user IP address ranges to a specific endpoint within a Traffic Manager profile. The endpoint returned will be mapped for that request's source IP address when a request is received.
## Controlling your canary release

Using a combination of feature toggles, deployment slots, and Traffic Manager, you can achieve complete control over the traffic flow and enable your canary release.

You deploy the new feature to the new deployment slot or a new instance of an application, and you enable the feature after verifying the deployment was successful.

Next, you set the traffic to be distributed to a small percentage of the users.

You carefully watch the application's behavior, for example, by using application insights to monitor the performance and stability of the application.

# Understand dark launching

Dark launching is in many ways like canary releases.

However, the difference here's that you're looking to assess users' responses to new features in your frontend rather than testing the performance of the backend.

The idea is that rather than launch a new feature for all users, you instead release it to a small set of users.

Usually, these users aren't aware they're being used as test users for the new feature, and often you don't even highlight the new feature to them, as such the term "Dark" launching.

Another example of dark launching is launching a new feature and using it on the backend to get metrics.

Let me illustrate with a real-world "launch" example.

As Elon Musk describes in his biography, they apply all kinds of Agile development principles in SpaceX.

SpaceX builds and launches rockets to launch satellites. SpaceX also uses dark launching.

When they have a new version of a sensor, they install it alongside the old one.

All data is measured and gathered both by the old and the new sensor.

Afterward, they compare the outcomes of both sensors.

Only when the new one has the same or improved results the old sensor is replaced.

The same concept can be applied to software. You run all data and calculations through your new feature, but it isn't "exposed" yet.

# Implement A/B testing and progressive exposure deployment

A/B testing (also known as split testing or bucket testing) compares two versions of a web page or app against each other to determine which one does better.

A/B testing is mainly an experiment where two or more page variants are shown to users at random.

Also, statistical analysis is used to determine which variation works better for a given conversion goal.

A/B testing isn't part of continuous delivery or a pre-requisite for continuous delivery. It's more the other way around.

Continuous delivery allows you to deliver MVPs to a production environment and your end-users quickly.

Common aims are to experiment with new features, often to see if they improve conversion rates.

Experiments are continuous, and the impact of change is measured.

A/B testing is out of scope for this course.

But because it's a powerful concept that is enabled by implementing continuous delivery, it's mentioned here to dive into further.

# Explore CI-CD with deployment rings

Progressive exposure deployment, also called ring-based deployment, was first discussed in Jez Humble's Continuous Delivery book.

They support the production-first DevOps mindset and limit the impact on end users while gradually deploying and validating changes in production.

Impact (also called blast radius) is evaluated through observation, testing, analysis of telemetry, and user feedback.

In DevOps, rings are typically modeled as stages.

Rings are, in essence, an extension of the canary stage. The canary releases to a stage to measure impact. Adding another ring is essentially the same thing.

![[Pasted image 20241208070931.png]]

With a ring-based deployment, you first deploy your changes to risk-tolerant customers and progressively roll out to a more extensive set of customers.

The Microsoft Windows team, for example, uses these rings.

![[Pasted image 20241208070954.png]]

When you have identified multiple groups of users and see value in investing in a ring-based deployment, you need to define your setup.

Some organizations that use canary releasing have multiple deployment slots set up as rings.

The first release of the feature to ring 0 targets a well-known set of users, mostly their internal organization.

After things have been proven stable in ring 0, they propagate the release to the next ring. It's with a limited set of users outside their organization.

And finally, the feature is released to everyone. It is often done by flipping the switch on the feature toggles in the software.

As in the other deployment patterns, monitoring and health checks are essential.

By using post-deployment release gates that check a ring for health, you can define an automatic propagation to the next ring after everything is stable.

When a ring isn't healthy, you can halt the deployment to the following rings to reduce the impact.

# Explore infrastructure as code and configuration management

Infrastructure as code (IaC) doesn't quite trip off the tongue, and its meaning isn't always straightforward.

But IaC has been with us since the beginning of DevOps—and some experts say DevOps wouldn't be possible without it.

As the name suggests, infrastructure as code is the concept of managing your operations environment like you do applications or other code for general release.

Rather than manually making configuration changes or using one-off scripts to make infrastructure changes, the operations infrastructure is managed instead using the same rules and strictures that govern code development—particularly when new server instances are spun up.

That means that the core best practices of DevOps—like version control, virtualized tests, and continuous monitoring—are applied to the underlying code that governs the creation and management of your infrastructure.

In other words, your infrastructure is treated the same way that any other code would be.

The elasticity of the cloud paradigm and the disposability of cloud machines can only be used by applying the principles of Infrastructure as Code to all your infrastructure.

This module describes key concepts of infrastructure as code and environment deployment creation and configuration. Also, understand the imperative, declarative, and idempotent configuration and how it applies to your company.

Suppose you've ever received a middle-of-the-night emergency support call because of a crashed server.

In that case, you know the pain of searching through multiple spreadsheets or even your memory—to access the manual steps of setting up a new machine.

Then there's also the difficulty of keeping the development and production environments consistent.

An easier way to remove the possibility of human error when initializing machines is to use Infrastructure as Code.

## Manual deployment versus infrastructure as code

A common analogy for understanding the differences between manual deployment and infrastructure as code is the distinction between owning pets and owning cattle.

When you have pets, you name each one and regard them as individuals; if something terrible happens to one of your pets, you're inclined to care a lot.

If you have a herd of cattle, you might still name them, but you consider their herd.

In infrastructure terms, there might be severe implications with a manual deployment approach if a single machine crashes and you need to replace it (pets).

If you adopt infrastructure as a code approach, you can more easily provision another machine without adversely impacting your entire infrastructure (cattle) if a single machine goes down.

## Implementing infrastructure as code

With infrastructure as code, you capture your environment (or environments) in a text file (script or definition).

Your file might include any networks, servers, and other computing resources.

You can check the script or definition file into version control and then use it as the source for updating existing environments or creating new ones.

For example, you can add a new server by editing the text file and running the release pipeline rather than remoting it into the environment and manually provisioning a new server.

The following table lists the significant differences between manual deployment and infrastructure as code.

|**Manual deployment**|**Infrastructure as code**|
|---|---|
|Snowflake servers.|A consistent server between environments.|
|Deployment steps vary by environment.|Environments are created or scaled easily.|
|More verification steps and more elaborate manual processes.|Fully automated creation of environment Updates.|
|Increased documentation to account for differences.|Transition to immutable infrastructure.|
|Deployment on weekends to allow time to recover from errors.|Use blue/green deployments.|
|Slower release cadence to minimize pain and long weekends.|Treat servers as cattle, not pets.|

## Benefits of infrastructure as code

The following list is benefits of infrastructure as code:

- Promotes auditing by making it easier to trace what was deployed, when, and how. (In other words, it improves traceability).
- Provides consistent environments from release to release.
- Greater consistency across development, test, and production environments.
- Automates scale-up and scale-out processes.
- Allows configurations to be version-controlled.
- Provides code review and unit-testing capabilities to help manage infrastructure changes.
- Uses _immutable_ service processes, meaning if a change is needed to an environment, a new service is deployed, and the old one was taken down, it isn't updated.
- Allows _blue/green_ deployments. This release methodology minimizes downtime where two identical environments exist—one is live, and the other isn't. Updates are applied to the server that isn't live. When testing is verified and complete, it's swapped with the different live servers. It becomes the new live environment, and the previous live environment is no longer the live.
- Treats infrastructure as a flexible resource that can be provisioned, de-provisioned, and reprovisioned as and when needed.

# Examine environment configuration

**Configuration management** refers to automated configuration management, typically in version-controlled scripts, for an application and all the environments needed to support it.

Configuration management means lighter-weight, executable configurations that allow us to have configuration and environments as code.

For example, adding a new port to a Firewall could be done by editing a text file and running the release pipeline, not by remoting into the environment and manually adding the port.

Note

The term _configuration as code_ can also be used to mean configuration management. However, it isn't used as widely, and in some cases, infrastructure as code is used to describe both provisioning and configuring machines. The term _infrastructure as code_ is also sometimes used to include _configuration as code_, but not vice versa.

## Manual configuration versus configuration as code

Manually managing the configuration of a single application and environment can be challenging.

The challenges are even more significant for managing multiple applications and environments across multiple servers.

Automated configuration, or treating configuration as code, can help with some of the manual configuration difficulties.

The following table lists the significant differences between manual configuration and configuration as code.

|**Manual configuration**|**Configuration as code**|
|---|---|
|Configuration bugs are challenging to identify.|Bugs are easily reproducible.|
|Error-prone.|Consistent configuration.|
|More verification steps and more elaborate manual processes.|Increase deployment cadence to reduce the amount of incremental change.|
|Increased documentation.|Treat environment and configuration as executable documentation.|
|Deployment on weekends to allow time to recover from errors.||
|Slower release cadence to minimize the requirement for long weekends.||

## Benefits of configuration management

The following list is benefits of configuration management:

- Bugs are more easily reproduced, audit help, and improve traceability.
- Provides consistency across environments such as dev, test, and release.
- It increased deployment cadence.
- Less documentation is needed and needs to be maintained as all configuration is available in scripts.
- Enables automated scale-up and scale out.
- Allows version-controlled configuration.
- Helps detect and correct configuration drift.
- Provides code-review and unit-testing capabilities to help manage infrastructure changes.
- Treats infrastructure as a flexible resource.
- Promotes automation.

# Understand imperative versus declarative configuration

There are a few different approaches that you can adopt to implement Infrastructure as Code and Configuration as Code.

Two of the main methods of approach are:

- **Declarative** (functional). The declarative approach states _what_ the final state should be. When run, the script or definition will initialize or configure the machine to have the finished state declared without defining _how_ that final state should be achieved.

- **Imperative** (procedural). In the imperative approach, the script states the _how_ for the final state of the machine by executing the steps to get to the finished state. It defines what the final state needs to be but also includes how to achieve that final state. It also can consist of coding concepts such as _for_, _if-then_, _loops_, and matrices.

## Best practices

The **declarative** approach abstracts away the methodology of how a state is achieved. As such, it can be easier to read and understand what is being done.

It also makes it easier to write and define. Declarative approaches also separate the final desired state and the coding required to achieve that state.

So, it doesn't force you to use a particular approach, allowing for optimization.

A **declarative** approach would generally be the preferred option where ease of use is the primary goal. Azure Resource Manager template files are an example of a declarative automation approach.

An **imperative** approach may have some advantages in complex scenarios where changes in the environment occur relatively frequently, which need to be accounted for in your code.

There's no absolute on which is the best approach to take, and individual tools may be used in either _declarative_ or _imperative_ forms. The best approach for you to take will depend on your needs.

# Understand idempotent configuration

**Idempotence** is a mathematical term that can be used in Infrastructure as Code and Configuration as Code. It can apply one or more operations against a resource, resulting in the same outcome.

For example, running a script on a system should have the same outcome despite the number of times you execute the script. It shouldn't error out or do the same actions irrespective of the environment's starting state.

In essence, if you apply a deployment to a set of resources 1,000 times, you should end up with the same result after each application of the script or template.

You can achieve idempotency by:

- Automatically configuring and reconfiguring an existing set of resources.  
    
- Discarding the existing resources and recreating a new environment.

When defining Infrastructure as Code and Configuration as Code, as a best practice, build the scripts and templates in such a way as to embrace idempotency.

It's essential when working with cloud services because resources and applications can be scaled in and out regularly. New instances of services can be started up to provide service elasticity.

---
