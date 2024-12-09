
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
# Scan your codebase for dependencies

There are several ways to identify the dependencies in your codebase.

These include scanning your code for patterns and reuse and analyzing how the solution is composed of individual modules and components.

- **Duplicate code** When certain pieces of code appear in several places, it's a good indication that this code can be reused. Keep in mind that code duplication isn't necessarily a bad practice. However, if the code can be made available properly, it does have benefits over copying code and must manage that. The first step to isolate these pieces of duplicate code is to centralize them in the codebase and componentize them in the appropriate way for the type of code.
- **High cohesion and low coupling** A second approach is to find code that might define components in your solution. You'll look for code elements that have high cohesion and low coupling with other parts of code. It could be a specific object model with business logic or code related to its responsibility, such as a set of helper or utility codes or perhaps a basis for other code to be built upon.
- **Individual lifecycle** Related to high cohesion, you can look for parts of the code that have a similar lifecycle and can be deployed and released individually. If such code can be maintained by a team separate from the codebase that it's currently in, it's an indication that it could be a component outside of the solution.
- **Stable parts** Some parts of your codebase might have a slow rate of change. That code is stable and isn't altered often. You can check your code repository to find the code with a low change frequency.
- **Independent code and components** Whenever code and components are independent and unrelated to other parts of the system, they can be isolated to a separate component and dependency.

You can use different kinds of tools to assist you in scanning and examining your codebase.

These range from tools that scan for duplicate code and draw solution dependency graphs to tools that compute metrics for coupling and cohesion.

___

# Implementing a version strategy

Software changes over time. The requirements for the software don't stay the same.

The functionality it offers and its use will grow, change, and adapt based on feedback.

The hosting of an application might evolve as well, with new operating systems, new frameworks, and versions thereof.

The original implementation might contain flaws and bugs. Whatever the reason for the change, it's unlikely that the software is stable and doesn't need to change.

Since the software you build takes dependencies on other components, the same holds for the components and packages you build or use while building your software.

Correct versioning becomes essential to maintaining a codebase to keep track of which piece of software is currently being used.

Versioning is also relevant for dependency management, as it relates to the versions of the packages and the components within.

Each dependency is identified by its name and version. It allows keeping track of the exact packages being used. Each of the packages has its lifecycle and rate of change.

## Immutable packages

As packages get new versions, your codebase can choose when to use a new version of the packages it consumes.

It does so by specifying the specific version of the package it requires. This implies that packages themselves should always have a new version when they change.

Whenever a package is published to a feed, it shouldn't be allowed to change anymore. If changed, it would be at the risk of introducing potential breaking changes to the code. In essence, a published package is immutable.

Replacing or updating an existing version of a package isn't allowed. Most package feeds don't allow operations that would change a current version.

Regardless of the size of the change, a package can only be updated by introducing a new version.

The new version should indicate the type of change and impact it might have.

This module explains versioning strategies for packaging, best practices for versioning, and package promotion.

# Understand versioning of artifacts

Its proper software development practice to indicate changes to code with the introduction of an increased version number.

However small or large a change, it requires a new version. A component and its package can have independent versions and versioning schemes.

The versioning scheme can differ per package type. Typically, it uses a scheme that can indicate the kind of change that is made.

Most commonly, it involves three types of changes:

- **Major change** Major indicates that the package and its contents have changed significantly. It often occurs at the introduction of a new version of the package. It can be a redesign of the component. Major changes aren't guaranteed to be compatible and usually have breaking changes from older versions. Major changes might require a large amount of work to adopt the consuming codebase to the new version.
- **Minor change** Minor indicates that the package and its contents have extensive modifications but are smaller than a major change. These changes can be backward compatible with the previous version, although they aren't guaranteed to be.
- **Patch** A patch or revision is used to indicate that a flaw, bug, or malfunctioning part of the component has been fixed. Usually, It's a backward-compatible version compared to the previous version.

How artifacts are versioned technically varies per package type. Each type has its way of indicating the version in the metadata.

The corresponding package manager can inspect the version information. The tooling can query the package feed for packages and the available versions.

Additionally, a package type might have its conventions for versioning and a particular versioning scheme.

# Explore semantic versioning

One of the predominant ways of versioning is the use of semantic versioning.

It isn't a standard but does offer a consistent way of expressing the intent and semantics of a particular version.

It describes a version for its backward compatibility with previous versions.

Semantic versioning uses a three-part version number and an extra label.

The version has the form of `Major.Minor.Patch` corresponds to the three types of changes covered in the previous section.

Examples of versions using the semantic versioning scheme are `1.0.0` and `3.7.129`. These versions don't have any labels.

For prerelease versions, it's customary to use a label after the regular version number.

A label is a textual suffix separated by a hyphen from the rest of the version number.

The label itself can be any text describing the nature of the prerelease.

Examples of these are `rc1`, `beta27,` and `alpha`, forming version numbers like `1.0.0-rc1` is a prerelease for the upcoming `1.0.0` version.

Prereleases are a common way to prepare for the release of the label-less version of the package.

Early adopters can take a dependency on a prerelease version to build using the new package.

Generally, using a prerelease version of packages and their components for released software isn't a good idea.

It's good to expect the impact of the new components by creating a separate branch in the codebase and using the prerelease version of the package.

Chances are that there will be incompatible changes from a prerelease to the final version.

# Examine release views

When building packages from a pipeline, the package needs to have a version before being consumed and tested.

Only after testing is the quality of the package known.

Since package versions can't and shouldn't be changed, it becomes challenging to choose a specific version beforehand.

Azure Artifacts recognizes the quality level of packages in its feeds and the difference between prerelease and release versions.

It offers different views on the list of packages and their versions, separating these based on their quality level.

It fits well with the use of semantic versioning of the packages for predictability of the intent of a particular version.

Still, its extra metadata from the Azure Artifacts feed is called a `descriptor`.

Feeds in Azure Artifacts have three different views by default. These views are added when a new feed is created. The three views are:

- **Local.** The `@Local` view contains all release and prerelease packages and the packages downloaded from upstream sources.
- **Prerelease.** The `@Prerelease` view contains all packages that have a label in their version number.
- **Release.** The `@Release` view contains all packages that are considered official releases.

## Using views

You can use views to offer help consumers of a package feed filter between released and unreleased versions of packages.

Essentially, it allows a consumer to make a conscious decision to choose from released packages or opt-in to prereleases of a certain quality level.

By default, the `@Local` view is used to offer the list of available packages. The format for this URI is:

URL

```
https://pkgs.dev.azure.com/{yourteamproject}/_packaging/{feedname}/nuget/v3/index.json

```

When consuming a package feed by its URI endpoint, the address can have the requested view included. For a specific view, the URI includes the name of the view, which changes to be:

URL

```
https://pkgs.dev.azure.com/{yourteamproject}/_packaging/{feedname}@{Viewname}/nuget/v3/index.json

```

The tooling will show and use the packages from the specified view automatically.

Tooling may offer an option to select prerelease versions, such as shown in this Visual Studio 2017 NuGet dialog. It doesn't relate or refer to the `@Prerelease` view of a feed. Instead, it relies on the presence of prerelease labels of semantic versioning to include or exclude packages in the search results.

# Promote packages

Azure Artifacts has the notion of promoting packages to views to indicate that a version is of a certain quality level.

By selectively promoting packages, you can plan when packages have a certain quality and are ready to be released and supported by the consumers.

You can promote packages to one of the available views as the quality indicator.

Release and Prerelease's two views might be sufficient, but you can create more views when you want finer-grained quality levels if necessary, such as `alpha` and `beta`.

**Packages** will always show in the Local view, but only in a particular view after being promoted.

Depending on the URL used to connect to the feed, the available packages will be listed.

**Upstream** sources will only be evaluated when using the @Local view of the feed.

After they've been downloaded and cached in the @Local view, you can see and resolve the packages in other views after being promoted to it.

It's up to you to decide how and when to promote packages to a specific view.

This process can be automated by using an Azure Pipelines task as part of the build pipeline.

Packages that have been promoted to a view won't be deleted based on the retention policies.

# Explore best practices for versioning

There are several best practices to effectively use versions, views, and promotion flow for your versioning strategy.

Here's a couple of suggestions:

- Have a documented versioning strategy.
- Adopt SemVer 2.0 for your versioning scheme.
- Each repository should only reference one feed.
- On package creation, automatically publish packages back to the feed.

It's good to adopt a best practice yourself and share these with your development teams.

It can be made part of the Definition of Done for work items related to publishing and consuming packages from feeds.

# Introduction to GitHub Packages

This module introduces you to GitHub Packages. It explores ways to control permissions and visibility, publish, install, delete and restore packages using GitHub.

GitHub Packages is a software package hosting service that allows you to host your packages, containers, and other dependencies. It's a central place to provide integrated permissions management and billing for software development on GitHub.

GitHub Packages can host:

- npm.
- RubyGems.
- Apache Maven.
- Gradle.
- Docker.
- NuGet.
- GitHub's Container registry is optimized for containers and supports Docker and OCI images.


### Support for package registries

|**Language**|**Package format**|**Package client**|
|---|---|---|
|JavaScript|package.json|npm|
|Ruby|Gemfile|gem|
|Java|pom.xml|mvn|
|Java|build.gradle or build.gradle.kts|gradle|
|.NET|nupkg|dotnet CLI|
|N/A|Dockerfile|Docker|

When creating a package, you can provide a description, installation and usage instructions, and other details on the package page. It helps people consuming the package understand how to use it and its purposes.

If a new package version fixes a security vulnerability, you can publish a security advisory to your repository.

___

# Develop monitor and status dashboards

GitHub's offers native monitoring of project-related activities through its insights, including code changes and collaboration metrics. By enabling the monitoring functionality, you can track repository traffic, assess contributor activity, and identify trends over time. This enhances visibility, fosters collaboration, and helps making data-driven decisions. By default, charts are available to anyone that can view the project.

With insights for Projects, you can use the default or create custom charts. Insights support two types of charts: current and historical.

## Current charts

Current charts provide the ability to visualize project items. The default Status chart shows the current total number of items in the project. You can create custom charts to show how many work items are assigned to each team member or how many issues are raised in each iteration. You also can modify the way data is presented through filtering, such as displaying the volume of remaining work, but limiting the results to particular labels or assignees.

## Historical charts

Historical charts provide the ability to track and visualize changes to project items over time. The default Burn up chart visualizes the progress of project issues over time, showing how much work is completed and how much is left. You can use this chart to view progress, spot trends, and identify bottlenecks to help move the project forward. With custom charts, you can target items with open issues and pull requests, items with issues that were closed as completed or merged via pull requests, as well as items with issues that were closed as not planned. Note that Insights don't track items you have archived or deleted.

## Creating charts

Insights are available for all new projects by default although historical charts require GitHub Team and GitHub Enterprise Cloud. You can create unlimited charts in private projects with GitHub Team and GitHub Enterprise Cloud for organizations and GitHub Pro for users. Users and organizations using public projects can create an unlimited number of charts. Users and organizations using GitHub Free are limited to two charts per private project.

To create a chart, in GitHub.com navigate to the target project and, in the top-right corner of the page, select the Insights icon displaying a graph. Next, in the vertical navigation menu on the left side of the page, select New chart. Assign a meaningful name to the chart and select Configure to modify the default chart settings. In the Configure chart pane, select the values of the following properties:

- Layout: Bar, Column, Line, Stacked area, Stacked bar, or Stacked column.
- X-axis: Assignees, Labels, Milestone, Repository, Status, or Time.
- Group by (optional): None, Assignees, Labels, Milestone, or Repository.
- Y-axis; Count of items, Sum of a field, Average of a field, Minimum of a field, or Maximum of a field.

To create a historical chart, set your chart's X-axis to Time. If needed, above the chart, enter filters to scope the data used to construct the chart.

# Explore Azure Dashboards

Visualizations such as charts and graphs can help you analyze your monitoring data to drill down on issues and identify patterns.

Depending on the tool you use, you may also share visualizations with other users inside and outside of your organization.

[Azure dashboards](https://learn.microsoft.com/en-us/azure/azure-portal/azure-portal-dashboards) are the primary dashboarding technology for Azure.

They're handy in providing a single pane of glass over your Azure infrastructure and services allowing you to identify critical issues quickly.

![[Pasted image 20241208202306.png]]

## Advantages

- Deep integration into Azure. Visualizations can be pinned to dashboards from multiple Azure pages, including metrics analytics, log analytics, and Application Insights.
- Supports both metrics and logs.
- Combine data from multiple sources, including output from [Metrics explorer](https://learn.microsoft.com/en-us/azure/azure-monitor/platform/metrics-charts), [Log Analytics queries](https://learn.microsoft.com/en-us/azure/azure-monitor/log-query/log-query-overview), and [maps](https://learn.microsoft.com/en-us/azure/azure-monitor/app/app-map) and [availability](https://learn.microsoft.com/en-us/azure/azure-monitor/visualizations) in Application Insights.
- Option for personal or shared dashboards. It's integrated with [Azure role-based authentication (RBAC)](https://learn.microsoft.com/en-us/azure/role-based-access-control/overview).
- Automatic refresh. Metrics refresh depends on the time range with a minimum of five minutes. Logs refresh at one minute.
- Parametrized metrics dashboards with timestamp and custom parameters.
- Flexible layout options.
- Full-screen mode.

## Limitations

- Limited control over log visualizations with no support for data tables. The total number of data series is limited to 10, with different data series grouped under another bucket.
- No custom parameters support for log charts.
- Log charts are limited to the last 30 days.
- Log charts can only be pinned to shared dashboards.
- No interactivity with dashboard data.
- Limited contextual drill-down.


# Explore Power BI

Completed 100 XP

- 1 minute

[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) is beneficial for creating business-centric dashboards and reports analyzing long-term KPI trends.

You can [import the results of a log query](https://learn.microsoft.com/en-us/azure/log-analytics/log-analytics-powerbi) into a Power BI dataset to take advantage of its features, such as combining data from different sources and sharing reports on the web and mobile devices.

![[Pasted image 20241208202346.png]]

## Advantages

- Rich visualizations.
- Extensive interactivity, including zoom-in and cross-filtering.
- Easy to share throughout your organization.
- Integration with other data from multiple data sources.
- Better performance with results cached in a cube.

## Limitations

- It supports logs but not metrics.
- No Azure RM integration. Can't manage dashboards and models through Azure Resource Manager.
- Need to import query results need into the Power BI model to configure. Limitation on result size and refresh.

# Build your own custom application

Completed 100 XP

- 2 minutes

You can access data in log and metric data in Azure Monitor through their API using any REST client.

It allows you to build your custom websites and applications.

## Advantages

- Complete flexibility in UI, visualization, interactivity, and features.
- Combine metrics and log data with other data sources.

## Disadvantages

- Significant engineering effort is required.

# Monitor pipeline health, including failure rate, duration, and flaky tests

Completed 100 XP

- 3 minutes

Monitoring the health of Azure Pipelines is fundamental to ensuring the reliability and efficiency of software delivery processes. By tracking health metrics such as pipeline fail rates and duration in combination with assessing and remediating flaky tests, you can identify issues affecting the functionality of pipelines, optimize their performance, and enhance their resiliency. To simplify the monitoring process, you might want to leverage native Azure services such as Azure Pipelines reports and Application Insights, although you also have the option of using third party tools that integrate with Azure DevOps or are accessible via Azure DevOps service hooks.

## Monitoring and assessment targets

There are several monitoring and assessment targets that provide insights into the overall health and reliability of Azure Pipelines. Tracking and remediating their occurrences promotes continuous improvement and high-quality software releases.

### Fail Rate

The failure rate of Azure Pipelines reflects the frequency of build or deployment failures within a specified timeframe. Monitoring this metric helps identify trends and patterns in failures, pinpointing potential issues in code quality, configuration, or infrastructure. By establishing thresholds and alerts based on failure rate, you can promptly address issues, minimize downtime, and ensure the reliability of pipeline workflows.

### Duration

Tracking the duration of pipeline runs is critical for evaluating efficiency and performance of the software delivery process. Its monitoring helps identify processes that may be causing delays or inefficiencies. By analyzing duration trends over time, you can optimize resource utilization, streamline workflows, and accelerate the frequency of software releases.

### Flaky Tests

Flaky tests are automated tests that produce inconsistent results, yielding different outcomes under the same conditions. Flaky tests can cause intermittent build failures, leading to unpredictable pipeline behavior, resulting in unnecessary alerts, consuming resources for investigations, and delaying the progression of the pipeline. They also tend to lead to false positives, where passing tests incorrectly indicate the absence of defects, or false negatives, where failing tests identify non-existing defects.

While flaky tests originate from the testing phase of the pipeline, their impact extends to the overall health and reliability of the pipeline itself. Effectively, identifying flaky tests in Azure Pipelines is essential for maintaining the integrity of test suites and ensuring the reliability of test-driven development practices. By detecting and eliminating flaky tests as part of pipeline health monitoring, you can ensure the stability, accuracy, and confidence of the software delivery processes, yielding higher-quality software releases.

## Monitoring and assessment services and tools

In addition to identifying the monitoring and assessment targets, you also need to determine the most suitable service or tool to deliver the required functionality.

### Azure Pipelines reports

Azure Pipelines offer built-in pipeline run analytics. These analytics are collected and calculated over time and form the basis of the insights visualized through reports. The reports include metrics and trends that help improve pipeline efficiency.

On the reports pages you can review the status of builds and releases, as well as any pending or failed pipelines. The interface provides direct access to detailed logs and metrics for each pipeline.

For example, the _Pipeline pass rate_ report provides a granular view of the pipeline pass rate and its trend over time. You can also view which specific task failure contributes to a high number of pipelines run failures and use that insight to fix the top failing tasks.

The _Pipeline duration_ report shows detailed breakdown of how long pipelines typically take to complete successfully. This helps with identifying trends and identifying the tasks which contribute most to the pipeline duration.

The _Test failures_ report provides a granular view of the top failing tests in the pipeline, along with the failure details.

### Application Insights

Application Insights is a feature of Azure Monitor that allows you to monitor the performance of your applications, including Azure Pipelines. You can use Application Insights to monitor the latency and throughput of your pipelines, as well as any errors or exceptions that occur.

In addition, through integration of Azure Pipelines with Application Insights, it becomes possible to implement continuous monitoring of the entire software development lifecycle. With continuous monitoring, release pipelines are able leverage monitoring data from Application Insights and other Azure resources. When the release pipeline detects an Application Insights alert, the pipeline can gate or roll back the deployment until the alert is resolved.

Conversely, if all checks pass, deployments can progress automatically to production, without the need for manual intervention.

### Service hooks

Service hooks in Azure DevOps facilitate integration with a wide range of external services. They allow you to trigger notifications and actions in response to Azure DevOps events, such as pipeline runs. By incorporating service hooks into your Azure Pipelines monitoring processes, you can, for example, implement real-time notifications in response to pipeline failures and performance issues. In addition, you have the option to automatically initiate corrective actions to enhance the reliability and stability of their software delivery processes.

### Third party monitoring tools

There are also third-party monitoring tools that support integration with Azure Pipelines, including, for example, DataDog, New Relic, and AppDynamics. This provides extra flexibility, especially in scenarios that involve integrating Azure Pipeline monitoring into your existing monitoring strategy.

___
# Design processes to automate application analytics

In an Agile environment, you may typically find multiple development teams that work simultaneously. Introducing new code or code changes daily and sometimes several times a day.

In such an immediate environment, it's prevalent to find problems that have "slipped through the cracks" to find themselves in the production environment. When these issues arise, they have probably already-impacted end users, requiring a speedy resolution. It means that teams must conduct a rapid investigation to identify the root cause of the problem.

Identifying where these symptoms are coming from and then isolating the root cause is a challenging task. Symptoms can be found across various layers of a large hybrid IT environment, such as different servers/VMs, storage devices, databases, to the front-end and server-side code. Investigations that traditionally would take hours or days to complete must be completed within minutes.

Teams must examine the infrastructure and application logs as part of this investigation process. However, the massive amount of log records produced in these environments makes it impossible to do this manually.

It's much like trying to find a needle in a haystack. In most cases, these investigations are conducted using log management and analysis systems that collect and aggregate these logs (from infrastructure elements and applications), centralizing them in a single place and then-providing search capabilities to explore the data.

These solutions make it possible to conduct the investigation, but they still rely entirely on investigation skills and user knowledge. The user must know exactly what to search for and have a deep understanding of the environment to use them effectively.

It's crucial to understand that the log files of applications are far less predictable than the log files of infrastructure elements. The errors are essentially messages and error numbers that have been introduced to the code by developers in a non-consistent manner.

So, search queries yield thousands of results in most cases and do not include important ones, even when the user is skilled. That leaves the user with the same "needle in the haystack" situation.

## Assisting DevOps with augmented Search

A new breed of log management and analysis technologies has evolved to solve this challenge. These technologies facilitate the identification and investigation processes using Augmented Search.

Explicitly designed to deal with application logs' chaotic and unpredictable nature, Augmented Search considers that users don't necessarily know what to search for, especially in the chaotic application layer environment.

The analysis algorithm automatically identifies errors, risk factors, and problem indicators while analyzing their severity by combining semantic processing, statistical models, and machine learning to analyze and "understand" the events in the logs. These insights are displayed as intelligence layers on top of the search results, helping the user quickly discover the most relevant and essential information.

Although DevOps engineers may be familiar with the infrastructure and system architecture, the data is constantly changing with continuous fast-paced deployment cycles and constant code changes. It means that DevOps teams can use their intuition and knowledge to start investigating each problem, but they have blind spots that consume time because of the dynamic nature of the log data.

Combining the decisions that DevOps engineers make during their investigation with the Augmented Search engine information layers on the critical problems that occurred during the period of interest can help guide them through these blind spots quickly.

Combining the user's intellect, acquaintance with the system's architecture, and Augmented Search machine-learning capabilities on the dynamic data makes it faster and easier to focus on the most relevant data. Here's how that works in practice: One of the servers went down, and any attempt to reinitiate the server has failed. However, since the process is running, the server seems to be up. In this case, end users are complaining that an application isn't responding.

This symptom could be related to many problems in a complex environment with many servers. Focusing on the server behind this problem can be difficult, as it seems to be up. But finding the root cause of the problem requires a lengthy investigation, even when you know which server is behind this problem.

Augmented Search will display a layer that highlights critical events during the specified period instead of going over thousands of search results. These highlights provide information about the sources of the events, assisting in the triage process.

At this point, DevOps engineers can understand the impact of the problem (for example, which servers are affected by it) and then continue the investigation to find the root cause of these problems. Using Augmented Search, DevOps engineers can identify a problem and the root cause in a matter of seconds instead of examining thousands of log events or running multiple checks on the various servers.

Adding this type of visibility to log analysis and the ability to surface critical events out of tens of thousands - and often millions - of events is essential in a fast-paced environment that constantly introduces changes.

A key factor to automating feedback is telemetry. By inserting telemetric data into your production application and environment, the DevOps team can automate feedback mechanisms while monitoring applications in real time.

DevOps teams use telemetry to see and solve problems as they occur, but this data can be helpful to both technical and business users.

When properly instrumented, telemetry can also be used to see and understand how customers are engaging with the application in real time.

It could be critical information for product managers, marketing teams, and customer support. So, feedback mechanisms must share continuous intelligence with all stakeholders.

## What is telemetry, and why should I care?

In the software development world, telemetry can offer insights on which features end users use most, detect bugs and issues, and provide better visibility into the performance without asking for feedback directly from users.

In DevOps and the world of modern cloud apps, we're tracking the health and performance of an application.

That telemetry data comes from application logs, infrastructure logs, metrics, and events.

The measurements are things like memory consumption, CPU performance, and database response time.

Events can be used to measure everything else, such as when a user logged in, when an item is added to a basket, when a sale is made, and so on.

The concept of telemetry is often confused with just logging. But logging is a tool used in the development process to diagnose errors and code flows. It's focused on the internal structure of a website, app, or another development project. Logging only gives you a single dimension view. With insights into infrastructure logs, metrics, and events, you have a 360-degree view of understanding user intent and behavior.

Once a project is released, telemetry is what you are looking for to enable data collection from real-world use.

Telemetry is what makes it possible to collect all that raw data that becomes valuable, actionable analytics.

## Common sources of telemetry

When it comes to gathering telemetry, there are various sources to tap into for valuable insights. Azure offers a rich ecosystem of services that provide telemetry data from different aspects of your application and infrastructure. For instance, telemetry can be collected from Windows and Linux servers running in Azure, other cloud environments, or on-premises, containerized workloads, storage accounts, network services, and more. These sources offer a wealth of information about the health, performance, and usage of your resources.

To harness the power of this telemetry data, Azure provides specialized insight services tailored to these different sources. One such service is Application Insights, which focuses on collecting telemetry from web applications hosted on App Services, physical and virtual servers, containers, and other types of compute resources. It offers detailed monitoring and diagnostics capabilities, allowing you to track performance metrics, detect issues, and analyze user interactions.

If you want to monitor physical and virtual servers running Windows or Linux, VM Insights provides deep visibility into their performance and health metrics, enabling proactive management and troubleshooting. It offers features like performance trend analysis, anomaly detection, and recommendations for performance optimization.

Container Insights is designed to gather telemetry from containerized applications running on Azure Kubernetes Service (AKS) or Arc-enabled Kubernetes clusters. It provides insights into container performance, resource utilization, and application dependencies, helping you optimize containerized workloads and ensure their reliability and scalability.

Storage Insights focuses on telemetry data related to Azure storage accounts, providing visibility into storage performance, usage patterns, and access trends. It enables you to monitor storage latency, throughput, and capacity utilization, helping you optimize storage configurations and identify potential bottlenecks.

Lastly, Network Insights collects telemetry from Azure network services, such as Azure virtual networks, load balancers, and firewalls, to monitor network performance, connectivity, and security. It offers visibility into network traffic patterns, latency metrics, and security events, allowing you to troubleshoot network issues and optimize network configurations for better performance and reliability.

## Benefits of telemetry

The primary benefit of telemetry is the ability of an end user to monitor the state of an object or environment while physically far removed from it.

Once you've shipped a product, you can't be physically present, peering over the shoulders of thousands (or millions) of users as they engage with your product to find out what works, what is easy, and what is cumbersome.

Thanks to telemetry, those insights can be delivered directly into a dashboard for you to analyze and act.

Because telemetry provides insights into how well your product is working for your end users – as they use it – it's a unique tool for ongoing performance monitoring and management.

Plus, you can use the data you've gathered from version 1.0-to-drive improvements and prioritize updates for your release of version 2.0.

Telemetry enables you to answer questions such as:

- Are your customers using the features you expect? How are they engaging with your product?
- How frequently are users engaging with your app, and for what duration?
- What settings options do users select most? Do they prefer certain display types, input modalities, screen orientation, or other device configurations?
- What happens when crashes occur? Are crashes happening more frequently when certain features or functions are used? What is the context surrounding a crash?

The answers to these and the many other questions that can be answered with telemetry are invaluable to the development process.

It will enable you to make continuous improvements and introduce new features that, to your end users, may seem as though you have been reading their minds – which you've been, thanks to telemetry.

## Challenges of telemetry

Telemetry is a fantastic technology, but it isn't without its challenges.

The most prominent challenge – and a commonly occurring issue – isn't with telemetry itself but with your end users and their willingness to allow what some see as Big Brother-Esque spying.

In short, some users immediately turn it off when they notice it, meaning any data generated from their use of your product won't be gathered or reported.

That means the experience of those users won't be accounted for when it comes to planning your future roadmap, fixing bugs, or addressing other issues in your app.

Although it isn't necessarily a problem by itself, the issue is that users who tend to disallow these types of technologies can tend to fall into the more tech-savvy portion of your user base.

It can result in the dumbing-down of software. On the other hand, other users take no notice of telemetry happening behind the scenes or ignore it if they do.

It's a problem without a clear solution—and it doesn't negate the overall power of telemetry for driving development—but one to keep in mind as you analyze your data.

So, when designing a strategy for how you consider the feedback from application telemetry, it's necessary to account for users who don't participate in providing the telemetry.

# Examine monitoring tools and technologies

Continuous monitoring of applications in production environments is typically implemented with application performance management (APM) solutions that intelligently monitor, analyze, and manage cloud, on-premises, and hybrid applications and IT infrastructure.

These APM solutions enable you to monitor your users’ experience and improve the stability of your application infrastructure. It helps identify the root cause of issues quickly to prevent outages and keep users satisfied proactively.

With a DevOps approach, we also see more customers broaden the scope of continuous monitoring into the staging, testing, and even development environments. It's possible because development and test teams following a DevOps approach are striving to use production-like environments for testing as much as possible.

By running APM solutions earlier in the life cycle, development teams get feedback about how applications will eventually do in the production and take corrective action much earlier. Also, operations teams advising the development teams get advanced knowledge and experience to better prepare and tune the production environment, resulting in far more stable releases into production.

Applications are more business-critical than ever. They must always be up, always fast, and constantly improving. Embracing a DevOps approach will allow you to reduce your cycle times to hours instead of months, but you must keep ensuring a great user experience!

Continuous monitoring of your entire DevOps life cycle will ensure development and operations teams collaborate to optimize the user experience every step of the way, leaving more time for your next significant innovation.

When shortlisting a monitoring tool, you should seek the following advanced features:

- **Synthetic Monitoring:** Developers, testers, and operations staff all need to ensure that their internet and intranet-mobile applications and web applications are tested and operate successfully from different points of presence worldwide.
- **Alert Management:** Developers, testers, and operations staff all need to send notifications via email, voice mail, text, mobile push notifications, and Slack messages when specific situations or events occur in development, testing, or production environments, to get the right people’s attention and to manage their response.
- **Deployment Automation:** Developers, testers, and operations staff use different tools to schedule and deploy complex applications and configure them in development, testing, and production environments. We'll discuss the best practices for these teams to collaborate effectively and efficiently and avoid potential duplication and erroneous information.
- **Analytics:** Developers need to look for patterns in log messages to identify if there's a problem in the code. Operations need to do root cause analysis across multiple log files to identify the source of the problem in complex applications and systems.

# Explore IT Service Management Connector

Customers use Azure monitoring tools to identify, analyze and troubleshoot issues.

However, the work items related to an issue are typically stored in an ITSM tool.

Instead of having to go back and forth between your ITSM tool and Azure monitoring tools, customers can now get all the information they need in one place.

ITSMC will improve the troubleshooting experience and reduce the time it takes to resolve issues.

IT Service Management Connector (ITSMC) for Azure provides bi-directional integration between Azure monitoring tools and your ITSM tools – ServiceNow, Provance, Cherwell, and System Center Service Manager.

Specifically, you can use ITSMC to:

- Create or update work-items (Event, Alert, Incident) in the ITSM tools based on Azure alerts (Activity Log Alerts, Near Real-Time metric alerts, and Log Analytics alerts)
- Pull the Incident and Change Request data from ITSM tools into Azure Log Analytics.

You can set up ITSMC by following this overview:

# Introduction to Secure DevOps

While a DevOps way of working enables development teams to deploy applications faster, going faster over a cliff doesn't help!

DevOps teams have access to unprecedented infrastructure and scale thanks to the cloud. They can be approached by some of the most nefarious actors on the internet, as they risk the security of their business with every application deployment.

Perimeter-class security is no longer viable in such a distributed environment, so companies must adopt more micro-level security across applications and infrastructure and have multiple lines of defense.

How do you ensure your applications are secure and stay secure with continuous integration and delivery? How can you find and fix security issues early in the process? It begins with practices commonly referred to as DevSecOps.

DevSecOps incorporates the security team and their capabilities into your DevOps practices making security the responsibility of everyone on the team. Security needs to shift from an afterthought to being evaluated at every process step.

Securing applications is a continuous process encompassing secure infrastructure, designing architecture with layered security, continuous security validation, and monitoring attacks.

![[Pasted image 20241208204932.png]]

Security is everyone's responsibility and needs to be looked at holistically across the application life cycle.

This module introduces DevSecOps concepts, SQL injection attacks, threat modeling, and security for continuous integration.

We'll also see how continuous integration and deployment pipelines can accelerate the speed of security teams and improve collaboration with software development teams.

You'll learn the critical validation points and how to secure your pipeline.

# Describe SQL injection attack

SQL Injection is an attack that makes it possible to execute malicious SQL statements. These statements control a database server behind a web application.

Attackers can use SQL Injection vulnerabilities to bypass application security measures. They can go around authentication and authorization of a web page or web application and retrieve the content of the entire SQL database. They can also use SQL Injection to add, modify, and delete records in the database.

An SQL Injection vulnerability may affect any website or web application that uses an SQL database such as MySQL, Oracle, SQL Server, or others.

Criminals may use it to gain unauthorized access to, delete, or alter your sensitive data: customer information, personal data, trade secrets, intellectual property, and more.

SQL Injection attacks are among the oldest, most prevalent, and most dangerous web application vulnerabilities.

The OWASP organization (Open Web Application Security Project) lists injections in their OWASP Top 10 2017 document as the number one threat to web application security.

# Understand DevSecOps

While adopting cloud computing is on the rise to support business productivity, a lack of security infrastructure can inadvertently compromise data.

The 2018 Microsoft Security Intelligence Report finds that:

- Data **isn't** encrypted both at rest and in transit by:
    - 7% of software as a service (SaaS) storage apps.
    - 86% percent of SaaS collaboration apps.
- HTTP headers session protection is supported by **only**:
    - 4% of SaaS storage apps.
    - 3% of SaaS collaboration apps.

_DevOps_ is about working faster. _Security_ is about-emphasizing thoroughness. Security concerns are typically addressed at the end of the cycle. It can potentially create unplanned work right at the end of the pipeline. _Secure DevOps_ integrates DevOps with security into a set of practices designed to meet the goals of both DevOps and safety effectively.

![Diagram showing Venn Diagram with one DevOps circle and one Security circle overlapping. The overlap is labeled Secure DevOps.](https://learn.microsoft.com/en-us/training/wwl-azure/introduction-to-secure-devops/media/secure-devops-c185814f.png)

A Secure DevOps pipeline allows development teams to work fast without breaking their project by introducing unwanted security vulnerabilities.

## Security in the context of Secure DevOps

Historically, security typically operated on a slower cycle and involved traditional security methodologies, such as:

- Access control.
- Environment hardening.
- Perimeter protection.

Secure DevOps includes these traditional security methodologies and more. With Secure DevOps, security is about securing the pipeline.

Secure DevOps involves determining where to add protection to the elements that plug into your build and release pipelines.

Secure DevOps can show you how and where you can add security to your automation practices, production environments, and other pipeline elements while benefiting from the speed of DevOps.

Secure DevOps addresses broader questions, such as:

- Is my pipeline consuming third-party components, and are they secure?
- Are there known vulnerabilities within any of the third-party software we use?
- How quickly can I detect vulnerabilities (also called _time to detect_)?
- How quickly can I remediate identified vulnerabilities (also known as _time to remediate_)?

Security practices for detecting potential security anomalies must be as robust and fast as your DevOps pipeline's other parts. It also includes infrastructure automation and code development.

As previously stated, the goal of a _Secure DevOps_ Pipeline is to enable development teams to work fast without introducing unwanted vulnerabilities to their project.


![[Pasted image 20241208205209.png]]

Two essential features of Secure Azure Pipelines that aren't found in standard Azure Pipelines are:

- Package management and the approval process associated with it. The previous workflow diagram details other steps for adding software packages to the Pipeline and the approval processes that packages must go through before they're used. These steps should be enacted early in the Pipeline to identify issues sooner in the cycle.
- Source Scanner is also an extra step for scanning the source code. This step allows for security scanning and checking for vulnerabilities that aren't present in the application code. The scanning occurs _after_ the app is built _before_ release and pre-release testing. Source scanning can identify security vulnerabilities earlier in the cycle.

In the rest of this lesson, we address these two essential features of Secure Azure Pipelines, the problems they present, and some of the solutions for them.

# Explore key validation points

Completed 100 XP

- 1 minute

Continuous security validation should be added at each step from development through production to help ensure the application is always secure.

This approach aims to switch the conversation with the security team from approving each release to consenting to the CI/CD process and monitor and audit the process at any time.

The diagram below highlights the critical validation points in the CI/CD pipeline when building green field applications.

You may gradually implement the tools depending on your platform and your application's lifecycle.

Especially if your product is mature and you haven't previously run any security validation against your site or application.

![Screenshot of flowchart with IDE, and Pull, CI, Dev, and Test.](https://learn.microsoft.com/en-us/training/wwl-azure/introduction-to-secure-devops/media/flowchart-integrated-development-environment-aef25fff.png)

## IDE / pull request

Validation in the CI/CD begins before the developer commits their code.

Static code analysis tools in the IDE provide the first line of defense to help ensure that security vulnerabilities aren't introduced into the CI/CD process.

The process for committing code into a central repository should have controls to help prevent security vulnerabilities from being introduced.

Using Git source control in Azure DevOps with branch policies provides a gated commit experience that can provide this validation.

Enabling branch policies on the shared branch requires a pull request to start the merge process and ensure the execution of all defined controls.

The pull request should require a code review, the one manual but important check for identifying new issues introduced into your code.

Along with this manual check, commits should be linked to work items for auditing why the code change was made and require a continuous integration (CI) build process to succeed before the push can be completed.

# Explore continuous security validation

Today, developers don't hesitate to use components available in public package sources (such as npm or NuGet).

With faster delivery and better productivity, open-source software (OSS) components are encouraged across many organizations.

However, as the dependency on these third-party OSS components increases, the risk of security vulnerabilities or hidden license requirements also increases compliance issues.

For a business, it's critical, as issues related to compliance, liabilities, and customer personal data can cause many privacy and security concerns.

Identifying such issues early in the release cycle gives you an advanced warning and enough time to fix the problems. Notably, the cost of rectifying issues is lower the earlier the project discovers the problem.

Many tools can scan for these vulnerabilities within the build and release pipelines.

Once the merge is complete, the CI build should execute as part of the pull request (PR-CI) process.

Typically, the primary difference between the two runs is that the PR-CI process doesn't need any packaging/staging in the CI build.

These CI builds should run static code analysis tests to ensure that the code follows all rules for both maintenance and security.

Several tools can be used for it:

- SonarQube.
- Visual Studio Code Analysis and the Roslyn Security Analyzers.
- Checkmarx - A Static Application Security Testing (SAST) tool.
- BinSkim - A binary static analysis tool that provides security and correctness results for Windows portable executables and many more.

Many of the tools seamlessly integrate into the Azure Pipelines build process. Visit the Visual Studio Marketplace for more information on the integration capabilities of these tools.

Also, to verify code quality with the CI build, two other tedious or ignored validations are scanning third-party packages for vulnerabilities and OSS license usage.

The response is fear or uncertainty when we ask about third-party package vulnerabilities and licenses.

Organizations trying to manage third-party packages vulnerabilities or OSS licenses explain that their process is tedious and manual.

Fortunately, Mend Software's tools can make this identification process almost instantaneous.

In a later module, we'll discuss integrating several helpful and commonly used security and compliance tools.

# Understand threat modeling

Threat modeling is a core element of the Microsoft Security Development Lifecycle (SDL).

It's an engineering technique you can use to help you identify threats, attacks, vulnerabilities, and countermeasures that could affect your application.

You can use threat modeling to shape your application's design, meet your company's security goals, and reduce risk.

With non-security experts in mind, the tool makes threat modeling easier for all developers by providing clear guidance on creating and analyzing threat models.

![[Pasted image 20241208205404.png]]

There are five major threat modeling steps:

- Defining security requirements.
- Creating an application diagram.
- Identifying threats.
- Mitigating threats.
- Validating that threats have been mitigated.

Threat modeling should be part of your typical development lifecycle, enabling you to refine your threat model and progressively reduce risk.

## Microsoft Threat Modeling Tool

The Microsoft Threat Modeling Tool makes threat modeling easier for all developers through a standard notation for visualizing system components, data flows, and security boundaries.

It also helps threat modelers identify classes of threats they should consider based on the structure of their software design.

The tool has been designed with non-security experts in mind, making threat modeling easier for all developers by providing clear guidance on creating and analyzing threat models.

The Threat Modeling Tool enables any developer or software architect to:

- Communicate about the security design of their systems.
- Analyze those designs for potential security issues using a proven methodology.
- Suggest and manage mitigation for security issues.

# Explore CodeQL in GitHub

Developers use CodeQL to automate security checks.

CodeQL treats code like data that can be queried.

GitHub researchers and community researchers have contributed standard CodeQL queries, and you can write your own.

A CodeQL analysis consists of three phases:

- Creating a CodeQL database (based upon the code).
- Run CodeQL queries against the database.
- Interpret the results.

CodeQL is available as a command-line interpreter and an extension for Visual Studio Code.


Links relacionados:
https://learn.microsoft.com/en-us/azure/devops/pipelines/security/overview?view=azure-devops

https://learn.microsoft.com/en-us/devops/devsecops/enable-devsecops-azure-github

https://learn.microsoft.com/en-us/azure/azure-sql/database/threat-detection-overview?view=azuresql

https://azure.microsoft.com/en-us/solutions/devsecops

