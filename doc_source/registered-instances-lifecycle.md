# Registered Instance Life Cycle<a name="registered-instances-lifecycle"></a>

**Note**  
This feature is supported only for Linux stacks\.

The registered instance lifecycle starts after the agent is installed and running\. At that point, it directs AWS OpsWorks Stacks to register the instance with the stack\. The following state diagram summarizes the key lifecycle elements\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opsworks/latest/userguide/images/on-prem-state.png)

Each state corresponds to an instance status\. The edges represent one of the following AWS OpsWorks Stacks commands\. The details are discussed in the following sections\.
+ **Setup** – This command corresponds to the Setup [lifecycle event](workingcookbook-events.md) and runs the instance's Setup recipes\.
+ **Configure** – This command corresponds to the Configure lifecycle event\.

  AWS OpsWorks Stacks triggers this event on every instance in the stack when an instance enters or leaves the online state\. The instances run their Configure recipes, which make any changes that are required to accommodate the new instance\.
+ **Shutdown** – This command corresponds to the Shutdown lifecycle event, which runs the instance's Shutdown recipes\.

  These recipes perform tasks such as shutting down services, but they do not stop the instance\.
+ **Deregister** – This command deregisters an instance and does not correspond to a lifecycle event\.

**Note**  
For simplicity the diagram does not show the Deregistering and Deleted states\. You can deregister an instance from any of the states in the diagram, which sends a Deregister command to the instance and moves it to the Deregistering state\.  
If you deregister an online instance, AWS OpsWorks Stacks sends a Configure command to the remaining instances in the stack to notify them that the instance is going offline\.
After the Deregister command is acknowledged, the instance is still running, but it is in the Deleted state and no longer part of the stack\. If you want to incorporate the instance into the stack again, you must re\-register it\.

**Topics**
+ [Registering](#registered-instances-lifecycle-registering)
+ [Running Setup](#registered-instances-lifecycle-running-setup)
+ [Registered](#registered-instances-lifecycle-registered)
+ [Assigning](#registered-instances-lifecycle-assigning)
+ [Online](#registered-instances-lifecycle-online)
+ [Setup Failed](#registered-instances-lifecycle-setup-failed)
+ [Unassigning](#registered-instances-lifecycle-unassigning)
+ [Initial Setup Configuration Changes](#registered-instances-lifecycle-setup-config)

## Registering<a name="registered-instances-lifecycle-registering"></a>

After the agent sends a registration request, AWS OpsWorks Stacks starts the instance lifecycle by sending a Setup command to the instance, putting it in the Registering state\. After the instance acknowledges the Setup command, it moves to the [Running Setup](#registered-instances-lifecycle-running-setup) state\.

## Running Setup<a name="registered-instances-lifecycle-running-setup"></a>

The Running Setup state runs the instance's Setup recipes\. Setup works depending on the preceding state\.

**Note**  
If you unassign the instance while it is in the Running Setup state, AWS OpsWorks Stacks sends a Shutdown command, which runs the instance's Shutdown recipes but does not stop the instance\. The instance moves to the [Unassigning](#registered-instances-lifecycle-unassigning) state\.

**Topics**
+ [Registering](#registered-instances-lifecycle-running-setup-registering)
+ [Assigning](#registered-instances-lifecycle-running-setup-assigning)
+ [Setup Failed](#registered-instances-lifecycle-running-setup-failed)

### Registering<a name="registered-instances-lifecycle-running-setup-registering"></a>

During the Registering process, setup creates an AWS OpsWorks Stacks instance to represent the registered instance in the stack, and runs a set of core Setup recipes on the instance\.

One key change performed by initial setup is overwriting the instance's hosts file\. By registering the instance, you have handed user management over to AWS OpsWorks Stacks, which must have its own hosts file to control SSH login permissions\. Initial setup also creates or modifies a number of files and, on Ubuntu systems, modifies package sources and installs a set of packages\. For details, see [Initial Setup Configuration Changes](#registered-instances-lifecycle-setup-config)\.

During registering, the process calls the IAM `AttachUserPolicy` that is part of the permissions attached to the IAM user that you create as a prerequisite\. If `AttachUserPolicy` does not exist \(most likely because you are running an older release of the AWS CLI\), the process falls back to calling `PutUserPolicy`\.

**Note**  
For consistency, AWS OpsWorks Stacks runs every core Setup recipe\. However, some of them perform some or all of their tasks only if an instance has been assigned to at least one layer, so they do not necessarily affect initial setup\.
+ If setup is successful, the instance moves to the [Registered](#registered-instances-lifecycle-registered) state\.
+ If setup is unsuccessful, the instance moves to the [Setup Failed](#registered-instances-lifecycle-setup-failed) state\.

### Assigning<a name="registered-instances-lifecycle-running-setup-assigning"></a>

The instance has at least one assigned layer\. AWS OpsWorks Stacks runs each layer's Setup recipes, including any custom recipes that you have [assigned to the layers' Setup event](workingcookbook-executing.md)\.
+ If setup is successful, the instance moves to the Online state and AWS OpsWorks Stacks triggers a Configure lifecycle event on every instance in the stack to notify them of the new instance\.
+ If setup is unsuccessful, the instance moves to the Setup Failed state\.

**Note**  
This setup process runs the core recipes a second time\. However, Chef recipes are idempotent, so they do not repeat any tasks that have already been performed\.

### Setup Failed<a name="registered-instances-lifecycle-running-setup-failed"></a>

If a setup process for an instance in the [Assigning](#registered-instances-lifecycle-assigning) state fails, you can try again by using the [Setup stack command](workingstacks-commands.md) to manually rerun the instance's Setup recipes\.
+ If setup is successful, the assigned instance moves to the [Online](#registered-instances-lifecycle-online) state and AWS OpsWorks Stacks triggers a Configure lifecycle event on every instance in the stack to notify them of the new instance\.
+ If the setup attempt is unsuccessful, the instance moves back to the Setup Failed state\.

## Registered<a name="registered-instances-lifecycle-registered"></a>

Instances in the Registered state are part of the stack and are managed by AWS OpsWorks Stacks but are not assigned to a layer\. They can remain in this state indefinitely\.

If you assign the instance to one or more layers, AWS OpsWorks Stacks sends a Setup command to the instance and it moves to the [Assigning](#registered-instances-lifecycle-assigning) state\.

## Assigning<a name="registered-instances-lifecycle-assigning"></a>

After the instance acknowledges the Setup command, it moves to the [Running Setup](#registered-instances-lifecycle-running-setup) state\.

If you unassign the instance while it is in the Assigning state, AWS OpsWorks Stacks terminates the setup process and sends a Shutdown command\. The instance moves to the [Unassigning](#registered-instances-lifecycle-unassigning) state\.

## Online<a name="registered-instances-lifecycle-online"></a>

The instance is now a member of at least one layer and is treated like a regular AWS OpsWorks Stacks instance\. It can remain in this state indefinitely\.

If you unassign the instance while it is in the Online state, AWS OpsWorks Stacks sends a Shutdown command to the instance and a Configure command to the rest of the stack's instances\. The instance moves to the [Unassigning](#registered-instances-lifecycle-unassigning) state\.

## Setup Failed<a name="registered-instances-lifecycle-setup-failed"></a>

The Setup command has failed\.
+ You can try again by running the [Setup stack command](workingstacks-commands.md)\.

  The instance returns to the [Running Setup](#registered-instances-lifecycle-running-setup) state\.
+ If you unassign the instance, AWS OpsWorks Stacks sends a Shutdown command to the instance\.

  The instance moves to the [Unassigning](#registered-instances-lifecycle-unassigning) state\.

## Unassigning<a name="registered-instances-lifecycle-unassigning"></a>

After the Shutdown command finishes, the instance is no longer assigned to any layers and returns to the [Registered](#registered-instances-lifecycle-registered) state\.

**Note**  
If an instance is assigned to multiple layers, unassignment applies to every layer; you cannot unassign a subset of the assigned layers\. If you want a different set of assigned layers, unassign the instance and then reassign the desired layers\.

## Initial Setup Configuration Changes<a name="registered-instances-lifecycle-setup-config"></a>

Initial setup creates or modifies the following files and directories on all registered instances\.

**Created Files**  

```
/etc/apt/apt.conf.d/99-no-pipelining
/etc/aws/
/etc/init.d/opsworks-agent
/etc/motd
/etc/motd.opsworks-static
/etc/sudoers.d/opsworks
/etc/sudoers.d/opsworks-agent
/etc/sysctl.d/70-opsworks-defaults.conf
/opt/aws/opsworks/
/usr/sbin/opsworks-agent-cli
/var/lib/aws/
/var/log/aws/
/vol/
```

**Modified Files**  

```
/etc/apt/apt.conf.d/99-no-pipelining
/etc/crontab
/etc/default/monit
/etc/group
/etc/gshadow
/etc/monit/monitrc
/etc/passwd
/etc/security/limits.conf (removing limits only for EC2 micro instances)
/etc/shadow
/etc/sudoers
```

Initial setup also creates a swap file on Amazon EC2 micro instances\.

Initial setup makes the following changes to Ubuntu systems\.

Package Sources  
Initial setup changes package sources to the following\.  
+ `deb http://archive.ubuntu.com/ubuntu/ ${code_name} main universe`

  To: `deb-src http://archive.ubuntu.com/ubuntu/ ${code_name} main universe`
+ `deb http://archive.ubuntu.com/ubuntu/ ${code_name}-updates main universe`

  To: `deb-src http://archive.ubuntu.com/ubuntu/ ${code_name}-updates main universe`
+ `deb http://archive.ubuntu.com/ubuntu ${code_name}-security main universe`

  To: `deb-src http://archive.ubuntu.com/ubuntu ${code_name}-security main universe`
+ `deb http://archive.ubuntu.com/ubuntu/ ${code_name}-updates multiverse`

  To: `deb-src http://archive.ubuntu.com/ubuntu/ ${code_name}-updates multiverse`
+ `deb http://archive.ubuntu.com/ubuntu ${code_name}-security multiverse`

  To: `deb-src http://archive.ubuntu.com/ubuntu ${code_name}-security multiverse`
+ `deb http://archive.ubuntu.com/ubuntu/ ${code_name} multiverse`

  To: `deb-src http://archive.ubuntu.com/ubuntu/ ${code_name} multiverse`
+ `deb http://security.ubuntu.com/ubuntu ${code_name}-security multiverse`

  To: `deb-src http://security.ubuntu.com/ubuntu ${code_name}-security multiverse`

Packages  
Initial setup uninstalls `landscape` and installs the following packages\.      
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/opsworks/latest/userguide/registered-instances-lifecycle.html)