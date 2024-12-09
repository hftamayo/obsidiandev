
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

___

# Implement pipeline security

It's fundamental to protect your code protecting credentials, and secrets. Phishing is becoming ever more sophisticated. The following list is several operational practices that a team ought to apply to protect itself:

- Authentication and authorization. Use multifactor authentication (MFA), even across internal domains, and just-in-time administration tools such as Azure PowerShell [Just Enough Administration (JEA)](https://aka.ms/jea), to protect against privilege escalations. Using different passwords for different user accounts will limit the damage if a set of access credentials is stolen.
- The CI/CD Release Pipeline. If the release pipeline and cadence are damaged, use this pipeline to rebuild infrastructure. Manage Infrastructure as Code (IaC) with Azure Resource Manager or use the Azure platform as a service (PaaS) or a similar service. Your pipeline will automatically create new instances and then destroy them. It limits the places where attackers can hide malicious code inside your infrastructure. Azure DevOps will encrypt the secrets in your pipeline. As a best practice, rotate the passwords just as you would with other credentials.
- Permissions management. You can manage permissions to secure the pipeline with role-based access control (RBAC), just as you would for your source code. It keeps you in control of editing the build and releases definitions that you use for production.
- Dynamic scanning. It's the process of testing the running application with known attack patterns. You could implement penetration testing as part of your release. You also could keep up to date on security projects such as the Open Web Application Security Project ([OWASP](https://www.owasp.org)) Foundation, then adopt these projects into your processes.
- Production monitoring. It's a critical DevOps practice. The specialized services for detecting anomalies related to intrusion are known as _Security Information and Event Management_. [Microsoft Defender for Cloud](https://azure.microsoft.com/services/defender-for-cloud) focuses on the security incidents related to the Azure cloud.

# Configure GitHub Advanced Security for GitHub and Azure DevOps

GitHub Advanced Security is a suite of security features and capabilities offered by GitHub to help organizations identify and mitigate security vulnerabilities, secure their code, and protect their software supply chain. It consists of the following key components:

- **Code Scanning** automatically scans code in repositories for security vulnerabilities and coding errors by using static analysis techniques provided by CodeQL or third party tools. It identifies potential security vulnerabilities, including those related to out-of-date dependencies and weak ciphers.
- **Secret Scanning** detects and helps remediate the presence of secrets, such as API tokens and cryptographic keys in repositories and commits. It automatically scans the content of repositories and generates alerts based on its discoveries.
- **Dependency reviews** assists with identifying and managing dependencies in software projects, based on direct and transitive dependencies retrieved from package manifests and other configuration files. They allow you to assess the full impact of changes to dependencies including details of any vulnerable versions before merging a pull request.
- **Custom auto-triage rules** help you manage Dependabot alerts at scale. With custom auto-triage rules you control which alerts could be ignored and which require applying a security update.
- **Security advisories** provides curated security advisories and alerts about vulnerabilities discovered in open-source dependencies.

GitHub Advanced Security integrates natively with both GitHub and Azure DevOps.

## GitHub

GitHub makes its Advanced Security features available in private repositories based on the Advanced Security licensing. Once you purchased GitHub Advanced Security licensing for your organization, you can enable and disable these features at the organization or repository level. These features are also permanently enabled in public repositories on GitHub.com without any licensing prerequisites and can only be disabled if you change the project visibility.

To configure GitHub Advanced Security for your organization, in the upper-right corner of GitHub.com, select your profile icon and then select Your organizations. Next, select Settings and, in the Security section of the sidebar, select Code security and analysis. This will display the page that allows you to enable or disable all security and analysis features for the repositories in your organization.

The impact of configuration changes is determined by the visibility of repositories in your organization:

- Private vulnerability reporting - public repositories only.
- Dependency graph - only private repositories because the feature is always enabled for public repositories.
- Dependabot alerts - all repositories.
- Dependabot security updates - all repositories.
- GitHub Advanced Security - only private repositories because GitHub Advanced Security and the related features are always enabled for public repositories.
- Secret scanning - public and private repositories where GitHub Advanced Security is enabled. This option controls whether or not secret scanning alerts for users are enabled.
- Code scanning - public and private repositories where GitHub Advanced Security is enabled.

You can also manage the security and analysis features for individual private repositories. To do so, from GitHub.com, navigate to the main page of the repository and select Settings. In the Security section of the sidebar, select Code security and analysis. In the Code security and analysis pane, disable or enable individual features. The control for GitHub Advanced Security is disabled if your enterprise has not purchased required licenses.

Note that if you disable GitHub Advanced Security, dependency review, secret scanning alerts for users and code scanning are effectively disabled. As the result, any workflows that include code scanning will fail.

Once enabled, the security features are integrated directly into the GitHub platform, providing continuous security monitoring and alerts directly within the GitHub interface. Repository administrators and developers can access security insights, recommendations, and actionable steps to address identified security vulnerabilities and strengthen the overall security posture of their software projects. Additionally, organizations can customize security policies, configure automated workflows, and integrate GitHub Advanced Security with other security tools and services to meet their specific security requirements and compliance needs.

## Azure DevOps

GitHub Advanced Security for Azure DevOps target Azure Repos and includes:

- **Secret Scanning push protection** checks if code pushes include commits that expose secrets.
- **Secret Scanning repo scanning** searches repositories for exposed secrets.
- **Dependency Scanning** identifies direct and transitive vulnerabilities in open source dependencies.
- **Code Scanning** uses CodeQL static analysis to identify code-level application vulnerabilities such as SQL injection and authentication bypass.

Advanced Security can be turned on the organization, project, or repository level. This automatically enables secret scanning push protection and repository scanning. Effectively, any future pushes containing secrets are automatically blocked while secret scanning runs in the background.

Dependency scanning is a pipeline-based scanning tool. Results are aggregated per repository. It's recommended that you add the dependency scanning task to all the pipelines you'd like to be scanned. For the most accurate scanning results, be sure to add the dependency scanning task following the build steps of a pipeline that builds the code you wish to scan. You can add the Advanced Security Dependency Scanning task (AdvancedSecurity-Dependency-Scanning@1) directly to your YAML pipeline file or select it from the task assistant.

Code scanning is also a pipeline-based scanning tool where results are aggregated per repository. It tends to be a time-consuming build task, so consider adding the code scanning task to a separate, cloned pipeline of your main production pipeline or create a new pipeline. Within the pipeline, add the tasks in the following order:

- Advanced Security Initialize CodeQL (AdvancedSecurity-Codeql-Init@1)
- Your custom build steps
- Advanced Security Perform CodeQL Analysis (AdvancedSecurity-Codeql-Analyze@1)

Additionally, you'll need to include a comma separated list of the languages you're analyzing by using the Advanced Security Initialize CodeQL task. The supported languages include csharp, cpp, go, java, JavaScript, python, ruby, and swift.

# Explore hybrid management

The Hybrid Runbook Worker feature of Azure Automation allows you to run runbooks that manage local resources in your private data center on machines located in your data center.

Azure Automation stores and manages the runbooks and then delivers them to one or more on-premises machines.

The Hybrid Runbook Worker functionality is presented in the following graphic:

![[Pasted image 20241209083733.png]]

## Hybrid Runbook Worker workflow and characteristics

The following list is characteristics of the Hybrid Runbook Worker workflow:

- You can select one or more computers in your data center to act as a Hybrid Runbook Worker and then run runbooks from Azure Automation.
- Each Hybrid Runbook Worker is a member of a Hybrid Runbook Worker group, which you specify when you install the agent.
- A group can include a single agent, but you can install multiple agents in a group for high availability.
- There are no inbound firewall requirements to support Hybrid Runbook Workers, only Transmission Control Protocol (TCP) 443 is required for outbound internet access.
- The agent on the local computer starts all communication with Azure Automation in the cloud.
- When a runbook is started, Azure Automation creates an instruction that the agent retrieves. The agent then pulls down the runbook and any parameters before running it.

To configure your on-premises servers that support the Hybrid Runbook Worker role with DSC, you must add them as DSC nodes.

For more information about onboarding them for management with DSC, see [Onboarding machines for management by Azure Automation State Configuration](https://learn.microsoft.com/en-us/azure/automation/automation-dsc-onboarding).

For more information on installing and removing Hybrid Runbook Workers and groups, see:

- [Automate resources in your datacenter or cloud by using Hybrid Runbook Worker.](https://learn.microsoft.com/en-us/azure/automation/automation-hybrid-runbook-worker#installing-hybrid-runbook-worker)
- [Hybrid Management in Azure Automation](https://azure.microsoft.com/blog/hybrid-management-in-azure-automation/)

___

# Describe best practices for creating actions

It's essential to follow best practices when creating actions:

- Create chainable actions. Don't create large monolithic actions. Instead, create smaller functional actions that can be chained together.
- Version your actions like other code. Others might take dependencies on various versions of your actions. Allow them to specify versions.
- Provide the **latest** label. If others are happy to use the latest version of your action, make sure you provide the **latest** label that they can specify to get it.
- Add appropriate documentation. As with other codes, documentation helps others use your actions and can help avoid surprises about how they function.
- Add details **action.yml** metadata. At the root of your action, you'll have an **action.yml** file. Ensure it has been populated with author, icon, expected inputs, and outputs.
- Consider contributing to the marketplace. It's easier for us to work with actions when we all contribute to the marketplace. Help to avoid people needing to relearn the same issues endlessly.
# Mark releases with Git tags

Releases are software iterations that can be packed for release.

In Git, releases are based on Git tags. These tags mark a point in the history of the repository. Tags are commonly assigned as releases are created.

![Screenshot of Git Release Tag creation page.](https://learn.microsoft.com/en-us/training/wwl-azure/learn-continuous-integration-github-actions/media/git-release-tag-28ff3ca4.png)

Often these tags will contain version numbers, but they can have other values.

Tags can then be viewed in the history of a repository.

![Screenshot of the Git release tag page showing the tag information.](https://learn.microsoft.com/en-us/training/wwl-azure/learn-continuous-integration-github-actions/media/tag-history-ed9cbbd4.png)

For more information on tags and releases, see: [About releases](https://docs.github.com/repositories/releasing-projects-on-github/about-releases)

# Create encrypted secrets

Actions often can use secrets within pipelines. Common examples are passwords or keys.

In GitHub actions, It's called **Secrets**.

## Secrets

Secrets are similar to environment variables but encrypted. They can be created at two levels:

- Repository
- Organization

If secrets are created at the organization level, access policies can limit the repositories that can use them.

## Creating secrets for a repository

To create secrets for a repository, you must be the repository's owner. From the repository **Settings**, choose **Secrets**, then **New Secret**.

![Screenshot of new secret creation from settings.](https://learn.microsoft.com/en-us/training/wwl-azure/learn-continuous-integration-github-actions/media/new-secret-5ba1255e.png)

For more information on creating secrets, see [Encrypted secrets](https://docs.github.com/actions/security-guides/encrypted-secrets).

# Examine multi-stage dockerfiles

What are multi-stage Dockerfiles? Multi-stage builds give the benefits of the builder pattern without the hassle of maintaining three separate files.

Let us look at a multi-stage Dockerfile.

docker

```
FROM mcr.microsoft.com/dotnet/core/aspnetcore:3.1 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build
WORKDIR /src
COPY ["WebApplication1.csproj", ""]
RUN dotnet restore "./WebApplication1.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "WebApplication1.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WebApplication1.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApplication1.dll"]

```

At first, it simply looks like several dockerfiles stitched together. Multi-stage Dockerfiles can be layered or inherited.

When you look closer, there are a couple of key things to realize.

Notice the third stage.

`FROM build AS publish`

build isn't an image pulled from a registry. It's the image we defined in stage 2, where we named the result of our-build (SDK) image "builder." `Docker build` will create a named image we can later reference.

We can also copy the output from one image to another. It's the real power to compile our code with one base SDK image (`mcr.microsoft.com/dotnet/core/sdk:3.1`) while creating a production image based on an optimized runtime image (`mcr.microsoft.com/dotnet/core/aspnet:3.1`). Notice the line.

`COPY --from=publish /app/publish .`

It takes the /app/publish directory from the published image and copies it to the working directory of the production image.

## Breakdown of stages

The first stage provides the base of our optimized runtime image. Notice it derives from `mcr.microsoft.com/dotnet/core/aspnet:3.1`.

We would specify extra production configurations, such as registry configurations, MSIexec of other components. You would hand off any of those environment configurations to your ops folks to prepare the VM.

The second stage is our build environment. `mcr.microsoft.com/dotnet/core/sdk:3.1` This includes everything we need to compile our code. From here, we have compiled binaries we can publish or test—more on testing in a moment.

The third stage derives from our build stage. It takes the compiled output and "publishes" them in .NET terms.

Publish means taking all the output required to deploy your "app/publish/service/component" and placing it in a single directory. It would include your compiled binaries, graphics (images), JavaScript, and so on.

The fourth stage takes the published output and places it in the optimized image we defined in the first stage.

## Why is publish separate from the build?

You'll likely want to run unit tests to verify your compiled code. Or the aggregate of the compiled code from multiple developers being merged continues to function as expected.

You could place the following stage between builder and publish to run unit tests.

docker

```
FROM build AS test
WORKDIR /src/Web.test
RUN dotnet test

```

If your tests fail, the build will stop to continue.

## Why is base first?

You could argue it's simply the logical flow. We first define the base runtime image. Get the compiled output ready, and place it in the base image.

However, it's more practical. While debugging your applications under Visual Studio Container Tools, VS will debug your code directly in the base image.

When you hit F5, Visual Studio will compile the code on your dev machine. The first stage then volume mounts the output to the built runtime image.

You can test any configurations you have made to your production image, such as registry configurations or otherwise.

When the `docker build --target base` is executed, docker starts processing the dockerfile from the beginning through the stage (target) defined.

Since the base is the first stage, we take the shortest path, making the F5 experience as fast as possible.

If the base were after compilation (builder), you would have to wait for all the next steps to complete.

One of the perf optimizations we make with VS Container Tools is to take advantage of the Visual Studio compilations on your dev machine.

# Examine considerations for multiple stage builds

## Adopt container modularity

Try to avoid creating overly complex container images that couple together several applications.

Instead, use multiple containers and try to keep each container to a single purpose.

The website and the database for a web application should likely be in separate containers.

There are always exceptions to any rule but breaking up application components into separate containers increases the chances of reusing containers.

It also makes it more likely that you could scale the application.

For example, in the web application mentioned, you might want to add replicas of the website container but not for the database container.

## Avoid unnecessary packages

To help minimize image sizes, it's also essential to avoid including packages that you suspect might be needed but aren't yet sure if they're required.

Only include them when they're required.

## Choose an appropriate base

While optimizing the contents of your Dockerfiles is essential, it's also crucial to choose the appropriate parent (base) image. Start with an image that only contains packages that are required.

## Avoid including application data

While application data can be stored in the container, it will make your images more prominent.

It would be best to consider using **docker volume** support to maintain the isolation of your application and its data. Volumes are persistent storage mechanisms that exist outside the lifespan of a container.

For more information, see [Use multiple-stage builds](https://docs.docker.com/develop/develop-images/multistage-build/).

# Explore Azure container-related services

Azure provides a wide range of services that help you work with containers.

Here are the essential services that are involved:

[Azure Container Instances (ACI)](https://azure.microsoft.com/services/container-instances/)

Running your workloads in Azure Container Instances (ACI) allows you to create your applications rather than provisioning and managing the infrastructure that will run the applications.

ACIs are simple and fast to deploy, and when you're using them, you gain the security of hypervisor isolation for each container group. It ensures that your containers aren't sharing an operating system kernel with other containers.

[Azure Kubernetes Service (AKS)](https://azure.microsoft.com/services/kubernetes-service/)

Kubernetes has quickly become the de-facto standard for container orchestration. This service lets you quickly deploy and manage Kubernetes, to scale and run applications while maintaining overall solid security.

This service started life as Azure Container Services (ACS) and supported Docker Swarm and Mesos/Mesosphere DC/OS at release to manage orchestrations. These original ACS workloads are still supported in Azure, but Kubernetes support was added.

It quickly became so popular that Microsoft changed the acronym for Azure Container Services to AKS and later changed the name of the service to Azure Kubernetes Service (also AKS).

[Azure Container Registry (ACR)](https://azure.microsoft.com/services/container-registry/)

This service lets you store and manage container images in a central registry. It provides you with a Docker private registry as a first-class Azure resource.

All container deployments, including DC/OS, Docker Swarm, and Kubernetes, are supported. The registry is integrated with other Azure services such as the App Service, Batch, Service Fabric, and others.

Importantly, it allows your DevOps team to manage the configuration of apps without being tied to the configuration of the target-hosting environment.

[Azure Container Apps](https://azure.microsoft.com/services/container-apps/)

Azure Container Apps allows you to build and deploy modern apps and microservices using serverless containers. It deploys containerized apps without managing complex infrastructure.

You can write code using your preferred programming language or framework and build microservices with full support for [Distributed Application Runtime (Dapr)](https://dapr.io/). Scale dynamically based on HTTP traffic or events powered by [Kubernetes Event-Driven Autoscaling (KEDA)](https://keda.sh/).

[Azure App Service](https://azure.microsoft.com/services/app-service/)

Azure Web Apps provides a managed service for both Windows and Linux-based web applications and provides the ability to deploy and run containerized applications for both platforms. It provides autoscaling and load balancing options and is easy to integrate with Azure DevOps.

# Create a release pipeline

Azure DevOps has extended support for pipelines as code (also called YAML pipelines) for continuous deployment and started introducing various release management capabilities into pipelines as code.

The existing UI-based release management solution in Azure DevOps is referred to as classic release.

You'll find a list of capabilities and availability in YAML pipelines vs. classic build and release pipelines in the following table.

![[Pasted image 20241209130705.png]]

Let us quickly walk through all the components step by step.

The first component in a release pipeline is an artifact:

- Artifacts can come from different sources.
- The most common source is a package from a build pipeline.
- Another commonly seen artifact source is, for example, source control.

Furthermore, a release pipeline has a trigger: the mechanism that starts a new release.

A trigger can be:

- A manual trigger, where people start to release by hand.
- A scheduled trigger, where a release is triggered based on a specific time.
- A continuous deployment trigger, where another event triggers a release. For example, a completed build.

Another vital component of a release pipeline is stages or sometimes called environments. It's where the artifact will be eventually installed. For example, the artifact contains the compiled website installed on the webserver or somewhere in the cloud. You can have many stages (environments); part of the release strategy is finding the appropriate combination of stages.

Another component of a release pipeline is approval.

People often want to sign a release before installing it in the environment.

In more mature organizations, this manual approval process can be replaced by an automatic process that checks the quality before the components move on to the next stage.

Finally, we have the tasks within the various stages. The tasks are the steps that need to be executed to install, configure, and validate the installed artifact.

In this part of the module, we'll detail all the release pipeline components and talk about what to consider for each element.

The components that make up the release pipeline or process are used to create a release. There's a difference between a release and the release pipeline or process. The release pipeline is the blueprint through which releases are done. We'll cover more of it when discussing the quality of releases and releases processes.

See also [Release pipelines](https://learn.microsoft.com/en-us/azure/devops/pipelines/release).

What is an artifact? An artifact is a deployable component of your application. These components can then be deployed to one or more environments.

In general, the idea about build and release pipelines and Continuous Delivery is to build once and deploy many times.

It means that an artifact will be deployed to multiple environments. The artifact should be a stable package if you want to achieve it.

The configuration is the only thing you want to change when deploying an artifact to a new environment.

The contents of the package should never change. It's what we call [immutability](https://learn.microsoft.com/en-us/azure/devops/artifacts/artifacts-key-concepts). We should be 100% sure that the package that we build, the artifact, remains unchanged.

How do we get an artifact? There are different ways to create and retrieve artifacts, and not every method is appropriate for every situation.

# Examine considerations for deployment to stages

When you have a clear view of the different stages you'll deploy, you need to think about when you want to deploy to these stages.

As we mentioned in the introduction, Continuous Delivery is about deploying multiple times a day and can deploy on-demand.

When we define our cadence, questions that we should ask ourselves are:

- Do we want to deploy our application?
- Do we want to deploy multiple times a day?
- Can we deploy to a stage? Is it used?

For example, a tester testing an application during the day might not want to deploy a new version of the app during the test phase.

Another example is when your application incurs downtime, you don't want to deploy when users use the application.

The frequency of deployment, or cadence, differs from stage to stage.

A typical scenario we often see is continuous deployment during the development stage.

Every new change ends up there once it's completed and builds.

Deploying to the next phase doesn't always occur multiple times but only at night.

When designing your release strategy, choose your triggers carefully and consider the required release cadence.

Some things we need to take into consideration are:

- What is your target environment?
- Does one team use it, or do multiple teams use it?
    - If a single team uses it, you can deploy it frequently. Otherwise, it would be best if you were a bit more careful.
- Who are the users? Do they want a new version multiple times a day?
- How long does it take to deploy?
- Is there downtime? What happens to performance? Are users affected?

Instead of using out-of-the-box tasks, a command line, or a shell script, you can also use your custom build and release task.

By creating your tasks, the tasks are available publicly or privately to everyone you share them with.

Creating your task has significant advantages.

- You get access to variables that are otherwise not accessible.
- You can use and reuse a secure endpoint to a target server.
- You can safely and efficiently distribute across your whole organization.
- Users don't see implementation details.

# Explore release jobs

You can organize your build or release pipeline into jobs. Every build or deployment pipeline has at least one job.

A job is a series of tasks that run sequentially on the same target. It can be a Windows server, a Linux server, a container, or a deployment group.

A release job is executed by a build/release agent. This agent can only run one job at the same time.

You specify a series of tasks you want to run on the same agent during your job design.

When the build or release pipeline is triggered at runtime, each job is dispatched to its target as one or more.

A scenario that speaks to the imagination, where Jobs plays an essential role, is the following.

Assume that you built an application with a backend in .NET, a front end in Angular, and a native IOS mobile App. It might be developed in three different source control repositories triggering three other builds and delivering three other artifacts.

The release pipeline brings the artifacts together and wants to deploy the backend, frontend, and Mobile App all together as part of one release.

The deployment needs to take place on different agents.

If an IOS app needs to be built and distributed from a Mac, the angular app is hosted on Linux, so best deployed from a Linux machine.

The backend might be deployed from a Windows machine.

Because you want all three deployments to be part of one pipeline, you can define multiple Release Jobs targeting the different agents, servers, or deployment groups.

By default, jobs run on the host machine where the agent is installed.

It's convenient and typically well suited for projects just beginning to adopt continuous integration (CI).

Over time, you may want more control over the stage where your tasks run.

# Understand database deployment task

Integrating database deployment tasks into CI/CD Azure Pipelines and GitHub Actions workflows is meant to automate provisioning and updating of databases alongside applications which data they host. This integration facilitates seamless coordination between application and database lifecycles, reducing the risk of errors and inconsistencies that may result from manual deployments. Automating database provisioning facilitates faster release cycles, reduces the possibility of human errors, and improves collaboration between development, operations, and database teams.

## Considerations

Including data components in automated software deployment introduces additional considerations regarding maintaining data integrity and preventing potential data loss. To address and mitigate these risks, take into account the following provisions:

- Data separation: Maintain a clear separation between database schema changes and data manipulation operations. Database schema changes, such as table modifications or schema updates, should be managed separately from data manipulation operations, such as seeding initial data or importing and exporting datasets.
- Data preservation: Implement strategies to preserve existing data during pipeline or workflow redeployment. This may involve backing up critical data before deploying changes or using data migration scripts to transfer data between environments without overwriting existing data.
- Idempotent operations: Ensure that database deployment operations are idempotent, which means that they can be safely rerun multiple times resulting always in the same outcome. Idempotency should eliminate the possibility of creating duplicate schema objects and or records.
- Versioning and rollback: Maintain version control over database schema changes, data migrations, and deployment scripts to track changes and facilitate rollback procedures. Use source control repositories to manage database scripts, apply version tags or labels to database changes, and implement rollback strategies that revert database changes.
- Testing and validation: Perform thorough testing and validation of database changes in a staging or testing environment before deploying to production. Use automated testing frameworks, database unit tests, and integration tests to verify the database deployments and ensure compatibility with existing data.
- Monitoring and alerting: Implement monitoring and alerting mechanisms to detect anomalies, errors, or performance issues during database deployments. Monitor database health metrics, track audit logs, and configure alerts triggered by any unexpected behavior or failures.

The fundamental concepts and activities are similar between Azure Pipelines and GitHub Actions workflows. At a high level, incorporating deployment of databases into Azure Pipelines involves several implementation tasks:

- Creating database deployment scripts to define the database schema, seed data, and apply any additional configurations. The scripts should reside in the source control repository to facilitate change tracking and versioning.
- Creating database connection strings and storing them securely by as secret variables or Azure key vault secrets. This also requires granting pipelines access to the secrets.
- Using Azure DevOps tasks or third-party extensions to execute database deployment scripts as part of the pipeline.
- Configuring the pipeline to deploy any necessary dependencies, such as SQL Server instances, along with required tools and utilities.
- Including database testing tasks in the pipeline to validate database changes and ensure that deployments are successful. This typically involves running database unit tests, integration tests, or data validation checks.
- Implementing rollback and recovery mechanisms in the pipeline to handle deployment failures or unexpected errors. This may include creating database snapshots, backups, or transactional rollback scripts to revert changes in case of issues.

The specific of these implementation tasks vary considerably depending on the target database technology. The following section examines these specifics in the context of deploying Azure SQL services.

# Explore release approvals

As we've described in the introduction, Continuous Delivery is all about delivering on-demand.

But, as we discussed in the differences between release and deployment, delivery, or deployment, it's only the technical part of the Continuous Delivery process.

It's all about how you can technically install the software on an environment, but it doesn't say anything about the process that needs to be in place for a release.

Release approvals don't control _how_ but control _if_ you want to deliver multiple times a day.

Manual approvals also suit a significant need. Organizations that start with Continuous Delivery often lack a certain amount of trust.

They don't dare to release without manual approval. After a while, when they find that the approval doesn't add value and the release always succeeds, the manual approval is often replaced by an automatic check.

Things to consider when you're setting up a release approval are:

- What do we want to achieve with the approval? Is it an approval that we need for compliance reasons? For example, we need to adhere to the four-eyes principle to get out SOX compliance. Or Is it an approval that we need to manage our dependencies? Or is it an approval that needs to be in place purely because we need a sign-out from an authority like Security Officers or Product Owners.
- Who needs to approve? We need to know who needs to approve the release. Is it a product owner, Security officer, or just someone that isn't the one that wrote the code? It's essential because the approver is part of the process. They're the ones that can delay the process if not available. So be aware of it.
- When do you want to approve? Another essential thing to consider is when to approve. It's a direct relationship with what happens after approval. Can you continue without approval? Or is everything on hold until approval is given. By using scheduled deployments, you can separate approval from deployment.

Although manual approval is a great mechanism to control the release, it isn't always helpful.

On many occasions, the check can be done at an earlier stage.

For example, it's approving a change that has been made in Source Control.

Scheduled deployments have already solved the dependency issue.

You don't have to wait for a person in the middle of the night. But there's still a manual action involved.

If you want to eliminate manual activities but still want control, you start talking about automatic approvals or release gates.

# Explore release gates

Release gates in Azure DevOps provide enhanced control over the initiation and completion of deployment pipelines, incorporating aspects of security and governance into the process. They designate conditions which must be satisfied in order for the deployment to either continue (pre-deployment gates) or to be considered successful (post-deployment gates).

One of the important benefits of release gates is streamlining processes such as, for example, deploying a new version of an API, which would traditionally require manual intervention or dependency meetings. This way, instead of convening meetings, the gate mechanism allows stakeholders to indicate their approval to proceed with the click of a button, considerably reducing time and effort.

In addition, gates can leverage scripts and APIs to automate the approval process and provide objective, data-driven assessments, extending beyond manual approvals. These automatic approvals facilitate a wide range of other scenarios, including:

- **Incident and issues management**: Gate mechanisms ensure that deployment proceeds only if the required status for work items, incidents, and issues is met. For instance, deployment may be contingent on the absence of software bugs.
- **Approval integration with collaboration systems**: Integration with platforms like Microsoft Teams or Slack promotes communication with stakeholders for deployment approval, awaiting their response before proceeding.
- **Quality validation**: Gates can query metrics from tests on build artifacts, such as pass rate or code coverage, and deploy within specified thresholds to maintain quality standards.
- **Security scan on artifacts**: Gate mechanisms verify completion of security scans, such as anti-virus checks, code signing, and policy validation for build artifacts, ensuring compliance with security requirements before deployment.
- **User experience monitoring**: Leveraging product telemetry, gates validate that the user experience remains consistent with baseline standards, preventing deployment if regression is detected.
- **Change management integration**: Gates wait for change management procedures in systems like ServiceNow to conclude before proceeding with deployment.
- **Infrastructure health checks**: Post-deployment, gates execute monitoring processes and validate infrastructure compliance against predefined rules, ensuring resource utilization and security standards are met.

Effectively, approvals and gates offer granular control over deployment pipelines, accommodating both manual and automated verification processes. Automation expedites deployments while, at the same time, helps incorporate security measures, governance protocols, and stakeholder approvals into software delivery processes. Support for manual approvals accommodates pausing deployments in environments that require additional scrutiny before resuming or rejecting the deployment process.

# Use release gates to protect quality

A quality gate is the best way to enforce a quality policy in your organization. It's there to answer one question: can I deliver my application to production or not?

A quality gate is located before a stage that is dependent on the outcome of a previous stage. A quality gate was typically something that a QA department monitored in the past.

They had several documents or guidelines, and they verified if the software was of a good enough quality to move on to the next stage.

When we think about Continuous Delivery, all manual processes are a potential bottleneck.

We need to reconsider the notion of quality gates and see how we can automate these checks as part of our release pipeline.

By using automatic approval with a release gate, you can automate the approval and validate your company's policy before moving on.

Many quality gates can be considered.

- No new blocker issues.
- Code coverage on new code greater than 80%.
- No license violations.
- No vulnerabilities in dependencies.
- No further technical debt was introduced.
- Is the performance not affected after a new release?
- Compliance checks
    - Are there work items linked to the release?
    - Is the release started by someone else as the one who commits the code?

Defining quality gates improves the release process, and you should always consider adding them

# Explore GitOps release strategy and recommendations

GitOps is a modern software delivery approach that leverages Git repositories as the single source of truth for defining and managing infrastructure, configuration, and application code. In a GitOps release strategy, all changes to the system, including infrastructure provisioning, configuration updates, and application deployments, are made through Git commits and pull requests. The core principles of GitOps include declarative configuration, version control, automation, and continuous delivery.

## Defining a comprehensive GitOps release strategy

When defining a comprehensive GitOps strategy, you should take into account the following considerations:

- **Declarative configuration**: Express the desired system state in a declarative manner using configuration files by using tools such as PowerShell Desired State Configuration (DSC) or Ansible. Configuration files define how the system should behave, including environment variables, application settings, and service configurations.
- **Infrastructure as Code (IaC)**: Use the declarative configuration approach to define infrastructure components as code by using tools such as Bicep and Azure Resource Manager templates.  
    Version control: Store all configuration files, infrastructure definitions, and application code, in Git repositories, enabling versioning, change tracking, and collaboration among team members. Use branching strategies and pull requests for managing changes and enforcing code review processes.
- **Continuous Deployment (CD)**: Implement continuous deployment pipelines that automatically deploy changes to the system based on Git commits and pull requests. CI/CD services such as Azure Pipelines or GitHub Actions trigger pipeline executions in response to Git events, such as code merges or branch updates.
- **Automated synchronization**: Deployments are triggered automatically whenever changes are pushed to the Git repository. GitOps tools such as Flux or Argo CD continuously monitor Git repositories for changes and synchronize the desired state of the system with the configuration stored in Git.
- **Immutable infrastructure**: Adopt an immutable infrastructure approach where infrastructure components and application containers are treated as disposable artifacts. Each deployment creates a new immutable instance of the system, reducing configuration drift and ensuring consistency across environments.
- **Rollback and recovery**: Use Git-based workflows to manage rollbacks and recovery procedures in case of deployment failures or unexpected issues. As a result, reverting changes in Git repositories should trigger automatic rollback actions in the CI/CD pipeline, restoring the system to a previous known good state.
- **Observability and Monitoring**: Implement monitoring, logging, and observability practices to track system health, performance metrics, and deployment status. Integrate monitoring tools such as Azure Monitor, Prometheus, or Grafana with GitOps workflows to gain visibility into system behavior and detect anomalies in real-time.

# Provision and test environments

The release pipeline deploys software to a target environment. But it isn't only the software that will be deployed with the release pipeline.

If you focus on Continuous Delivery, Infrastructure as Code and spinning up Infrastructure as part of your release pipeline is essential.

When we focus on the deployment of the Infrastructure, we should first consider the differences between the target environments that we can deploy to:

- On-Premises servers.
- Cloud servers or Infrastructure as a Service (IaaS). For example, Virtual machines or networks.
- Platform as a Service (PaaS) and Functions as a Service (FaaS). For example, Azure SQL Database in both PaaS and serverless options.
- Clusters.
- Service Connections.

Let us dive a bit further into these different target environments and connections.

## On-premises servers

In most cases, when you deploy to an on-premises server, the hardware and the operating system are already in place. The server is already there and ready.

In some cases, empty, but most of the time not. In this case, the release pipeline can only focus on deploying the application.

You might want to start or stop a virtual machine (Hyper-V or VMware).

The scripts you use to start or stop the on-premises servers should be part of your source control and delivered to your release pipeline as a build artifact.

Using a task in the release pipeline, you can run the script that starts or stops the servers.

To take it one step further and configure the server, you should look at technologies like PowerShell Desired State Configuration(DSC).

The product will maintain your server and keep it in a particular state. When the server changes its state, you can recover the changed configuration to the original configuration.

Integrating a tool like PowerShell DSC into the release pipeline is no different from any other task you add.

## Infrastructure as a service

When you use the cloud as your target environment, things change slightly. Some organizations lift and shift from their on-premises servers to cloud servers.

Then your deployment works the same as an on-premises server. But when you use the cloud to provide you with Infrastructure as a Service (IaaS), you can use the power of the cloud to start and create servers when needed.

It's where Infrastructure as Code (IaC) starts playing a significant role.

Creating a script or template can make a server or other infrastructural components like a SQL server, a network, or an IP address.

By defining a template or using a command line and saving it in a script file, you can use that file in your release pipeline tasks to execute it on your target cloud.

The server (or another component) will be created as part of your pipeline. After that, you can run the steps to deploy the software.

Technologies like Azure Resource Manager are great for creating Infrastructure on demand.

## Platform as a Service

When you move from Infrastructure as a Service (IaaS) to Platform as a Service (PaaS), you'll get the Infrastructure from the cloud you're running on.

For example: In Azure, you can create a Web application. The cloud arranges the server, the hardware, the network, the public IP address, the storage account, and even the web server.

The user only needs to take care of the web application on this Platform.

You only need to provide the templates instructing the cloud to create a WebApp. Functions as a Service(FaaS) or Serverless technologies are the same.

In Azure, it's called Azure Functions. You only deploy your application, and the cloud takes care of the rest. However, you must instruct the Platform (the cloud) to create a placeholder where your application can be hosted.

You can define this template in Azure Resource Manager. You can use the Azure CLI or command-line tools.

In all cases, the Infrastructure is defined in a script file and lives alongside the application code in source control.

## Clusters

Finally, you can deploy your software to a cluster. A cluster is a group of servers that host high-scale applications.

When you run an Infrastructure as a Service cluster, you must create and maintain the cluster. It means that you need to provide the templates to create a cluster.

You must also ensure you roll out updates, bug fixes, and patches to your cluster. It's comparable with Infrastructure as a Service.

When you use a hosted cluster, you should consider it a Platform as a Service. You instruct the cloud to create the cluster, and you deploy your software to the cluster.

When you run a container cluster, you can use the container cluster technologies like AKS.

## Service connections

When a pipeline needs resource access, you must often create service connections.

## Summary

Whatever the technology you choose to host your application, your Infrastructure's creation or configuration should be part of your release pipeline and source control repository.

Infrastructure as Code is a fundamental part of Continuous Delivery, allowing you to create servers and environments on demand.

# Configure automated integration and functional test automation

The first thing that comes to mind about Continuous Delivery is that everything needs to be automated.

Otherwise, you can't deploy multiple times a day. But how to deal with testing, then?

Many companies still have a broad suite of manual tests to be run before delivering to production. Somehow these tests need to run every time a new release is created.

Instead of automating all your manual tests into automated UI tests, you need to rethink your testing strategy.

Lisa Crispin describes in her book Agile Testing that you can divide your tests into multiple categories.

![[Pasted image 20241209153754.png]]

We can make four quadrants where each side of the square defines our targets with our tests.

- Business facing - the tests are more functional and often executed by end users of the system or by specialized testers that know the problem domain well.
- Supporting the Team - it helps a development team get constant feedback on the product to find bugs quickly and deliver a quality build-in product.
- Technology facing - the tests are rather technical and non-meaningful to business people. They're typical tests written and executed by the developers in a development team.
- Critique Product - tests that validate a product's workings on its functional and non-functional requirements.

Now we can place different test types we see in the other quadrants. For example, we can put Unit tests, Component tests, and System or integration tests in the first quadrant.

We can place functional tests, Story tests, prototypes, and simulations in quadrant two. These tests support the team in delivering the correct functionality and are business-facing since they're more functional.

In quadrant three, we can place tests like exploratory, usability, acceptance, etc.

We place performance, load, security, and other non-functional requirements tests in quadrant four.

Looking at these quadrants, specific tests are easy to automate or automated by nature. These tests are in quadrants 1 and 4. Tests that are automatable but mostly not automated by nature are the tests in quadrant 2. Tests that are the hardest to automate are in quadrant 3.

We also see that the tests that can't be automated or are hard to automate are tests that can be executed in an earlier phase and not after release.

We call shift-left, where we move the testing process towards the development cycle.

We need to automate as many tests as possible and test them.

A few of the principles we can use are:

- Tests should be written at the lowest level possible.
- Write once, and run anywhere, including the production system.
- The product is designed for testability.
- Test code is product code; only reliable tests survive.
- Test ownership follows product ownership.

By testing at the lowest level possible, you'll find many tests that don't require infrastructure or applications to be deployed.

We can use the pipeline to execute the tests that need an app or infrastructure. We can run scripts or use specific test tools to perform tests within the pipeline.

On many occasions, you execute these external tools from the pipeline, like Owasp ZAP, SpecFlow, or Selenium.

You can use test functionality from a platform like Azure on other occasions. For example, Availability or Load Tests executed from within the cloud platform.

When you want to write your automated tests, choose the language that resembles the language from your code.

In most cases, the application developers should also write the test, so it makes sense to use the same language. For example, write tests for your .NET application in .NET and your Angular application in Angular.

The build and release agent can handle it to execute Unit Tests or other low-level tests that don't need a deployed application or infrastructure.

When you need to do tests with a UI or other specialized functionality, you need a Test agent to run the test and report the results. Installation of the test agent then needs to be done upfront or as part of the execution of your pipeline.

---

# Understand Shift-left

The goal for shifting left is to move quality upstream by performing tests early in the pipeline. It represents the phrase "fail fast, fail often" combining test and process improvements reduces the time it takes for tests to be run and the impact of failures later on.

The idea is to ensure that most of the testing is complete before merging a change into the main branch.

![Screenshot of the shift-left representation image showing Unit Tests and Functional Tests during the pipeline lifecycle.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-provision-environments/media/shift-left-cba6e08e.png)

Many teams find their test takes too long to run during the development lifecycle.

As projects scale, the number and nature of tests will grow substantially, taking hours or days to run the complete test.

They get pushed further until they're run at the last possible moment, and the benefits intended to be gained from building those tests aren't realized until long after the code has been committed.

There are several essential principles that DevOps teams should adhere to in implementing any quality vision.

![Screenshot of the quality vision principles current and future test portfolio.](https://learn.microsoft.com/en-us/training/wwl-azure/configure-provision-environments/media/shift-left-quality-vision-fe308ed2.png)

Other important characteristics to take into consideration:

- **Unit tests:** These tests need to be fast and reliable.
    - One team at Microsoft runs over 60,000 unit tests in parallel in less than 6 minutes, intending to get down to less than a minute.
- **Functional tests:** Must be independent.
- **Defining a test taxonomy** is an essential aspect of DevOps. The developers should understand the suitable types of tests in different scenarios.
    - **L0** tests are a broad class of fast in-memory unit tests. It's a test that depends on code in the assembly under test and nothing else.
    - **L1** tests might require assembly plus SQL or the file system.
    - **L2** tests are functional tests run against testable service deployments. It's a functional test category requiring a service deployment but may have critical service dependencies stubbed out.
    - **L3** tests are a restricted class of integration tests that run against production. They require a complete product deployment.

# Set up and run availability tests

After you've deployed your web app or website to any server, you can set up tests to monitor its availability and responsiveness.

It's helpful to check if your application is still running and gives a healthy response.

Some applications have specific Health endpoints that an automated process can check. The Health endpoint can be an HTTP status or a complex computation that uses and consumes crucial parts of your application.

For example, you can create a Health endpoint that queries the database. This way, you can check that your application is still accessible, but also the database connection is verified.

You can create your framework to create availability tests (ping test) or use a platform that can do it for you.

Azure has the functionality to develop Availability tests. You can use these tests in the pipeline and as release gates.

In Azure, you can set up availability tests for any HTTP or HTTPS endpoint accessible from the public internet.

You don't have to add anything to the website you're testing. It doesn't even have to be your site: you could try a REST API service you depend on.

There are two types of availability tests:

- URL ping test: a simple test that you can create in the Azure portal. You can check the URL and check the response and status code of the response.
- Multi-step web test: Several HTTP calls that are executed in sequence.

# Explore Azure Load Testing

Azure Load Testing Preview is a fully managed load-testing service that enables you to generate a high-scale load.

The service simulates your applications' traffic, helping you optimize application performance, scalability, or capacity.

You can create a load test using existing test scripts based on Apache JMeter. Azure Load Testing abstracts the infrastructure to run your JMeter script and load test your application.

Azure Load Testing collects detailed resource metrics for Azure-based applications to help you [identify performance bottlenecks](https://learn.microsoft.com/en-us/azure/load-testing/overview-what-is-azure-load-testing) across your Azure application components.

You can [automate regression testing](https://learn.microsoft.com/en-us/azure/load-testing/overview-what-is-azure-load-testing) by running load tests as part of your continuous integration and continuous deployment (CI/CD) workflow.

![[Pasted image 20241209153940.png]]

# Understand package management

## What is a package?

A package is a formalized way of creating a distributable unit of software artifacts that can be consumed from another software solution.

The package describes the content it contains and usually provides extra metadata, and the information uniquely identifies the individual packages and is self-descriptive.

It helps to better store packages in centralized locations and consume the contents of the package predictably.

Also, it enables tooling to manage the packages in the software solution.

## Types of packages

Packages can be used for different kinds of components.

The type of components you want to use in your codebase differ for the different parts and layers of the solution you're creating.

The range from frontend components, such as JavaScript code files, to backend components like .NET assemblies or Java components, complete self-contained solutions, or reusable files in general.

Over the past years, the packaging formats have changed and evolved. Now there are a couple of de facto standard formats for packages.

- **NuGet** packages (pronounced "new get") are a standard used for .NET code artifacts. It includes .NET assemblies and related files, tooling, and sometimes only metadata. NuGet defines the way packages are created, stored, and consumed. A NuGet package is essentially a compressed folder structure with files in ZIP format and has the `.nupkg` extension. See also [An introduction to NuGet](https://learn.microsoft.com/en-us/nuget/what-is-nuget).
- **npm** package is used for JavaScript development. It originates from node.js development, where it's the default packaging format. An npm package is a file or folder containing JavaScript files and a `package.json` file describing the package's metadata. For node.js, the package usually includes one or more modules that can be loaded once the package is consumed. See also [About packages and modules](https://docs.npmjs.com/about-packages-and-modules).
- **Maven** is used for Java-based projects. Each package has a Project Object Model file describing the project's metadata and is the basic unit for defining a package and working with it.
- **PyPi** The Python Package Index, abbreviated as PyPI and known as the Cheese Shop, is the official third-party software repository for Python.
- **Docker** packages are called images and contain complete and self-contained deployments of components. A Docker image commonly represents a software component that can be hosted and executed by itself without any dependencies on other images. Docker images are layered and might be dependent on other images as their basis. Such images are referred to as base images.


# Introduction to continuous monitoring

![[Pasted image 20241209154722.png]]

Continuous monitoring refers to the process and technology required to incorporate monitoring across each DevOps and IT operations lifecycles phase.

It helps to continuously ensure your application's health, performance, reliability, and infrastructure as it moves from development to production.

Continuous monitoring builds on the concepts of Continuous Integration and Continuous Deployment (CI/CD), which help you develop and deliver software faster and more reliably to provide continuous value to your users.

[Azure Monitor](https://learn.microsoft.com/en-us/azure/azure-monitor/overview) is the unified monitoring solution in Azure that provides full-stack observability across applications and infrastructure in the cloud and on-premises.

It works seamlessly with [Visual Studio and Visual Studio Code](https://visualstudio.microsoft.com/) during development and testing and integrates with [Azure DevOps](https://learn.microsoft.com/en-us/azure/devops/user-guide/index) for release management and work item management during deployment and operations.

It even integrates across your ITSM and SIEM tools to help track issues and incidents within your existing IT processes.

This article describes specific steps for using Azure Monitor to enable continuous monitoring throughout your workflows.

It includes links to other documentation that provides details on implementing different features.

## Enable monitoring for all your applications

To gain observability across your entire environment, you need to enable monitoring on all your web applications and services.

It will allow you to visualize end-to-end transactions and connections across all the components easily.

- [Azure DevOps Projects gives you a simplified experience with your existing code and Git repository or choose](https://learn.microsoft.com/en-us/azure/devops-project/overview) from one of the sample applications to create a Continuous Integration (CI) and Continuous Delivery (CD) pipeline to Azure.
- [Continuous monitoring in your DevOps release pipeline](https://learn.microsoft.com/en-us/azure/application-insights/app-insights-vsts-continuous-monitoring) allows you to gate or roll back your deployment based on monitoring data.
- [Status Monitor](https://learn.microsoft.com/en-us/azure/application-insights/app-insights-monitor-performance-live-website-now) allows you to instrument a live .NET app on Windows with Azure Application Insights without modifying or redeploying your code.
- If you have access to the code for your application, then enable complete monitoring with [Application Insights](https://learn.microsoft.com/en-us/azure/application-insights/app-insights-overview) by installing the Azure Monitor Application Insights SDK for [.NET](https://learn.microsoft.com/en-us/azure/application-insights/quick-monitor-portal), [Java](https://learn.microsoft.com/en-us/azure/application-insights/app-insights-java-quick-start), [Node.js](https://learn.microsoft.com/en-us/azure/application-insights/app-insights-nodejs-quick-start), or [any other programming language](https://learn.microsoft.com/en-us/azure/application-insights/app-insights-platforms). It lets you specify custom events, metrics, or page views relevant to your application and business.

## Enable monitoring for your entire infrastructure

Applications are only as reliable as their underlying infrastructure.

Monitoring enabled across your entire infrastructure will help you achieve full observability and make discovering a potential root cause easier when something fails.

Azure Monitor helps you track the health and performance of your entire hybrid infrastructure, including resources such as VMs, containers, storage, and network.

- You automatically get [platform metrics, activity logs, and diagnostics logs](https://learn.microsoft.com/en-us/azure/azure-monitor/data-sources) from most of your Azure resources with no configuration.
- Enable deeper monitoring for VMs with [Azure Monitor](https://learn.microsoft.com/en-us/azure/azure-monitor/insights/vminsights-overview).
- Enable deeper monitoring for AKS clusters with [Azure Monitor for containers](https://learn.microsoft.com/en-us/azure/azure-monitor/insights/container-insights-overview).
- Add [monitoring solutions](https://learn.microsoft.com/en-us/azure/azure-monitor/insights/solutions-inventory) for different applications and services in your environment.

[Infrastructure as code](https://learn.microsoft.com/en-us/azure/devops/learn/what-is-infrastructure-as-code) manages infrastructure in a descriptive model, using the same versioning as DevOps teams use for source code.

It adds reliability and scalability to your environment and allows you to apply similar processes to manage your applications.

- Use [Resource Manager templates](https://learn.microsoft.com/en-us/azure/azure-monitor/platform/template-workspace-configuration) to enable monitoring and configure alerts over a large set of resources.
- Use [Azure Policy](https://learn.microsoft.com/en-us/azure/governance/policy/overview) to enforce different rules over your resources. It ensures those resources comply with your corporate standards and service level agreements.

## Combine resources in Azure Resource Groups

Today, a typical application on Azure includes multiple resources such as VMs and App Services or microservices hosted on Cloud Services, AKS clusters, or Service Fabric.

These applications frequently use dependencies like Event Hubs, Storage, SQL, and Service Bus.

- Combine resources in Azure Resource Groups to get complete visibility of all the resources that make up your different applications. [Azure Monitor for Resource Groups](https://learn.microsoft.com/en-us/azure/azure-monitor/insights/resource-group-insights) provides a simple way to keep track of the health and performance of your entire full-stack application and enables drilling down into respective components for any investigations or debugging.

## Ensure quality through continuous deployment

Continuous Integration / Continuous Deployment allows you to automatically integrate and deploy code changes to your application based on automated testing results.

It streamlines the deployment process and ensures the quality of any changes before they move into production.

- Use [Azure Pipelines](https://learn.microsoft.com/en-us/azure/devops/pipelines) to implement Continuous Deployment and automate your entire process from code commit to production based on your CI/CD tests.
- Use Quality Gates to integrate monitoring into your pre-deployment or post-deployment. It ensures that you meet the key health/performance metrics (KPIs) as your applications move from dev to production. Any differences in the infrastructure environment or scale aren't negatively impacting your KPIs.
- [Maintain separate monitoring instances](https://learn.microsoft.com/en-us/azure/application-insights/app-insights-separate-resources) between your different deployment environments, such as Dev, Test, Canary, and Prod. It ensures that collected data is relevant across the associated applications and infrastructure. If you need to correlate data across environments, use [multi-resource charts in Metrics Explorer](https://learn.microsoft.com/en-us/azure/azure-monitor/platform/metrics-charts) or create [cross-resource queries in Log Analytics](https://learn.microsoft.com/en-us/azure/azure-monitor/log-query/cross-workspace-query).

## Create actionable alerts with actions

A critical monitoring aspect is proactively notifying administrators of current and predicted issues.

- Create [alerts in Azure Monitor](https://learn.microsoft.com/en-us/azure/azure-monitor/platform/alerts-overview) based on logs and metrics to identify predictable failure states. It would be best if you had a goal of making all alerts actionable, meaning that they represent actual critical conditions and seek to reduce false positives. Use [dynamic thresholds](https://learn.microsoft.com/en-us/azure/azure-monitor/platform/alerts-dynamic-thresholds) to automatically calculate baselines on metric data rather than defining your static thresholds.
- Define actions for alerts to use the most effective means of notifying your administrators. Available [actions for notification](https://learn.microsoft.com/en-us/azure/azure-monitor/platform/action-groups#create-an-action-group-by-using-the-azure-portal) are SMS, e-mails, push notifications or voice calls.
- Use more advanced actions to [connect to your ITSM tool](https://learn.microsoft.com/en-us/azure/azure-monitor/platform/itsmc-overview) or other alert management systems through [webhooks](https://learn.microsoft.com/en-us/azure/azure-monitor/platform/activity-log-alerts-webhook).
- Remediate situations identified in alerts with [Azure Automation runbooks](https://learn.microsoft.com/en-us/azure/automation/automation-webhooks) or [Logic Apps](https://learn.microsoft.com/en-us/connectors/custom-connectors/create-webhook-trigger) that can be launched from an alert using webhooks.
- Use [autoscaling](https://learn.microsoft.com/en-us/azure/azure-monitor/learn/tutorial-autoscale-performance-schedule) to dynamically increase and decrease your compute resources based on collected metrics.

## Prepare dashboards and workbooks

Ensuring that your development and operations have access to the same telemetry and tools allows them to view patterns across your entire environment and minimize your Mean Time To Detect (MTTD) and Mean Time To Restore (MTTR).

- Prepare [custom dashboards](https://learn.microsoft.com/en-us/azure/application-insights/app-insights-tutorial-dashboards) based on standard metrics and logs for the different roles in your organization. Dashboards can combine data from all Azure resources.
- Prepare [Workbooks](https://learn.microsoft.com/en-us/azure/application-insights/app-insights-usage-workbooks) to ensure knowledge sharing between development and operations. It could be prepared as dynamic reports with metric charts and log queries or as troubleshooting guides designed by developers to help customer support or operations handle fundamental problems.

## Continuously optimize

Monitoring is one of the fundamental aspects of the popular Build-Measure-Learn philosophy, which recommends continuously tracking your KPIs and user behavior metrics and optimizing them through planning iterations.

Azure Monitor helps you collect metrics and logs relevant to your business and add new data points in the following deployment.

- Use tools in Application Insights to [track end-user behavior and engagement](https://learn.microsoft.com/en-us/azure/application-insights/app-insights-tutorial-users).
- Use [Impact Analysis](https://learn.microsoft.com/en-us/azure/application-insights/app-insights-usage-impact) to help you prioritize which areas to focus on to drive to important KPIs.

---

![[Pasted image 20241209154817.png]]

# Explore Application Insights

You install a small instrumentation package in your application and set up an Application Insights resource in the Microsoft Azure portal.

The instrumentation monitors your app and sends telemetry data to the portal. (The application can run anywhere - it doesn't have to be hosted in Azure.)

You can instrument the web service application, background components, and JavaScript in the web pages.

![Diagram showing the Application Insights and telemetry for alerts, Power BI, Visual Studio, Rest API and Continuous Export.](https://learn.microsoft.com/en-us/training/wwl-azure/implement-tools-track-usage-flow/media/application-insights-a2f02f4e.png)

Also, you can pull in telemetry from the host environments such as performance counters, Azure diagnostics, or Docker logs.

You can also set up web tests periodically, sending synthetic requests to your web service.

All these telemetry streams are integrated into the Azure portal, where you can apply powerful analytic and search tools to the raw data.

## What's the overhead?

The impact on your app's performance is minimal. Tracking calls are non-blocking and are batched and sent in a separate thread.

## What do Application Insights monitor?

Application Insights is aimed at the development team to help you understand how your app is doing and being used. It monitors:

- Request rates, response times, and failure rates - Find out which pages are most popular, at what times of day, and where your users are. See which pages do best. If your response times and failure rates increase with more requests, perhaps you have a resourcing problem.
- Dependency rates, response times, and failure rates - Find out whether external services are slowing you down.
- Exceptions - Analyze the aggregated statistics, pick specific instances, and drill into the stack trace and related requests. Both server and browser exceptions are reported.
- Pageviews and load performance - reported by your users' browsers.
- AJAX calls from web pages - rates, response times, and failure rates.
- User and session count.
- Performance counters from your Windows or Linux server machines include CPU, memory, and network usage.
- Host diagnostics from Docker or Azure.
- Diagnostic trace logs from your app - so you can correlate trace events with requests.
- Custom events and metrics that you write yourself in the client or server code to track business events such as items sold or games won.

## Where do I see my telemetry?

There are plenty of ways to explore your data. Check out this article for more information - [Smart detection and manual alerts](https://learn.microsoft.com/en-us/azure/application-insights/app-insights-proactive-diagnostics).

Automatic alerts adapt to your app's usual patterns of telemetry and trigger when there's something outside the usual pattern. You can also [set alerts](https://learn.microsoft.com/en-us/azure/azure-monitor/app/alerts) on levels of custom or standard metrics.

![Screenshot of set alerts showing an abnormal rise in failed request rate in the app fabrikamprod.](https://learn.microsoft.com/en-us/training/wwl-azure/implement-tools-track-usage-flow/media/set-alerts-52c42e8d.png)

## Application map

The components of your app, with key metrics and alerts.

![Screenshot of the application map with the components of the app, key metrics, and alerts.](https://learn.microsoft.com/en-us/training/wwl-azure/implement-tools-track-usage-flow/media/application-map-2e670a1b.png)

## Profiler

Inspect the execution profiles of sampled requests.

![Screenshot of the Profiler. Inspect the execution profiles of sampled requests.](https://learn.microsoft.com/en-us/training/wwl-azure/implement-tools-track-usage-flow/media/profiler-3aa6ca54.png)

## Usage analysis

Analyze user segmentation and retention.

![Screenshot of usage analysis with user segmentation and retention.](https://learn.microsoft.com/en-us/training/wwl-azure/implement-tools-track-usage-flow/media/usage-analysis-f7bcbb7e.png)

## Diagnostic search, for instance, data.

Search and filter events such as requests, exceptions, dependency calls, log traces, and page views.

![Screenshot of the search and filter events such as requests, exceptions, dependency calls, log traces, and page views.](https://learn.microsoft.com/en-us/training/wwl-azure/implement-tools-track-usage-flow/media/search-filter-7cc4a795.png)

## Metrics Explorer for aggregated data

Explore, filter, and segment aggregated data such as rates of requests, failures, exceptions, response times, and page load times.

![Screenshot of the metrics and segment aggregated data such as rates of requests, failures, and exceptions, response times, and page load times.](https://learn.microsoft.com/en-us/training/wwl-azure/implement-tools-track-usage-flow/media/metrics-24659a11.png)

## Dashboards

Mash up data from multiple resources and share it with others. Great for multi-component applications and continuous display in the team room.

![Screenshot of Dashboards from multiple resources great for multi-component applications and continuous display in the team room.](https://learn.microsoft.com/en-us/training/wwl-azure/implement-tools-track-usage-flow/media/dashboards-e8b9e1b6.png)

## Live Metrics Stream

When you deploy a new build, watch these near-real-time performance indicators to ensure everything works as expected.

![Screenshot of Live Metrics Stream with near-real-time performance indicators.](https://learn.microsoft.com/en-us/training/wwl-azure/implement-tools-track-usage-flow/media/live-metrics-stream-3e84bf0a.png)

## Analytics

Answer challenging questions about your app's performance and usage by using this powerful query language.

![Screenshot of Analytics showing the answer challenging questions about app's performance and usage by using this powerful query language.](https://learn.microsoft.com/en-us/training/wwl-azure/implement-tools-track-usage-flow/media/analytics-3ad60c81.png)

# Implement Application Insights

## Monitor

Install Application Insights in your app, set up [availability web tests](https://learn.microsoft.com/en-us/azure/application-insights/app-insights-monitor-web-app-availability), and:

- Set up a [dashboard](https://learn.microsoft.com/en-us/azure/azure-monitor/app/app-insights-dashboards) for your team room to keep an eye on load, responsiveness, and the performance of your dependencies, page loads, and AJAX calls.
- Discover which are the slowest and most-failing requests.
- Watch [Live Stream](https://learn.microsoft.com/en-us/azure/application-insights/app-insights-live-stream) when you deploy a new release to know immediately about any degradation.

## Detect, Diagnose

If you receive an alert or discover a problem:

- Assess how many users are affected.
- Correlate failures with exceptions, dependency calls, and traces.
- Examine profiler, snapshots, stack dumps, and trace logs.

## Build, Measure, Learn

[Measure the effectiveness](https://learn.microsoft.com/en-us/azure/application-insights/app-insights-usage-overview) of each new feature that you deploy.

- Plan to measure how customers use new UX or business features.
- Write custom telemetry into your code.
- Base the next development cycle on hard evidence from your telemetry.

## Get started

Application Insights is one of the many services hosted within Microsoft Azure, and telemetry is sent there for analysis and presentation.

So, before you do anything else, you'll need a subscription to [Microsoft Azure](https://azure.com/).

It's free to sign up, and if you choose the basic [pricing plan](https://azure.microsoft.com/pricing/details/application-insights/) of Application Insights, there's no charge until your application has grown to have large usage.

If your organization already has a subscription, they could add your Microsoft account to it.

There are several ways to get started. Begin with whichever works best for you. You can add the others later

