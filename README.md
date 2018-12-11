# Plan 9 - Continued

Plan 9 did much to bring the clunky and complex Unix environment into the modern era twenty years ago. It proved in a variety of ways that you can actually get more with less using an equal measure of simplicity and consistency. One example was the adoption of the file system as the common method of communication between processes, the operating system kernel and even the across networks. This helped to redefine the Plan 9 system as more than just a regular computer operating system into an operating system for an entire network of systems. Since the system is highly consistent it is easier to be a generalist because the behaviour and code of much of the system follows the same design and conventions from kernel to compiler to user applications.

Since the 1980’s a number of factors have changed or evolved since the original inception of Plan 9. In some cases the new developments are extensions of the ones that drove the original design. Computers are increasingly networked together to provide common services as a single logical entity, but increasingly those entities are consolidated into vast data centers distributed globally. Accesses to those systems is not always reliable or stable, such as over a mobile connection or even when moving between wifi hotspots with your laptop. Meanwhile, the computing power and storage has continued its steady expansion over the years.

These changes have occurred on top of an ancient software base (ie. Unix/Linux/BSD and Microsoft Windows) whose origins date back at least forty years. Those bases were so lacking and unappealing that large software stacks erupted in order to drive the current cloud revolution and build magnificent user interfaces. Instead of simple, small and consistent the picture looks much the opposite. It also means that your smart phone and smart watch have relic teletype handling code in them waiting for something to exploit it.

If the current computing landscape was built on outdated base imagine what could be accomplished by lifting the weights off with a relatively modern alternative. Further adaptations and modernization are still be required, but certainly far less than the effort spent and continues to be spent on the legacy systems. In contrast, imagine how much more limiting and costly it would have been if we had continued to push MS-DOS and Multics forward.

Let's consider how the computing environment has changed since Plan 9 was under active development and discuss how it can adapt to the new circumstances while retaining the core benefits of the system.

## Filesystems and unstable connections

Within the confines of corporate networks of the 1980's network connections were relatively stable. Machines were rarely disconnected as they had physical connections to the network. They rarely moved or changed IP addresses because they were expensive pieces of equipment with physical security policies limiting their movement. Since workstations were shared resources they rarely changed IP address or at least their host names were stable enough to be relied on. This allowed persistent connections to be made for the most part with little disruption.

As a result, Plan 9 doesn't handle stale network connections particularly well in its core because it assumes a level of stability. The current world of wireless access points, dynamic IP address assignment and user-serviced equipment makes things much more dynamic. The web has solved this problem through a stateless protocol, HTTP, and the use of client-side caches. Using HTTP or porting the current web plumbing, such as web browsers, API's is antithetical to the simple and consistent underpinnings of Plan 9. It would add another set of incompatible tools to the base system and adds another layer of complexity for the rest of the system to work around. Existing web support in the system is often confusing to new users since it is not feature complete and also inconsistent with the rest of the system inhibiting reuse and learning.

Can the 9P protocol, which underpins the Plan 9 filesystem, be used or adapted somehow to work under these conditions? It is both stateful and relies on a persistent connection with timeouts in case of failure. One could presumably live with the problem in a number of ways. Carefully mounting and unmounting remote file systems when needed is an option, but it would be easy to forget a filesystem mount in one of your windows or even accidentally bind some other file systems or directories on top of it.

Perhaps you can leave everything on your home network and (re)boot your device every time using a network boot from an image provided by your home network. This is similar to the historical Plan 9 topology with CPU servers, file servers and user terminals that boot except that the terminal can be on a remote network. If you build your profile and update it frequently then it could bring you to workable state quickly with all of your persisted state saved remotely. One trouble would be if you have situations where some resources are accessible while others are not. This is certainly a possible scenario if you consistently use a mix of services each with varying availability. Imagine a case where your home network is available but a cloud service that you use frequently is not. What happens if you don't have a home network and rely instead only on your mobile device and cloud services?

9P itself requires a constant stateful connection to a filesystem, but what if there was a 9P proxy that can connect, reconnect and cache information to a remote filesystem? It may be possible to give the filesystem the commands to establish a connection to an unreliable remote file system so that it can manage the connections. In cases of total loss of connection it can also cache information for read and queue up modifications to replay once a connection is re-established. All that is required on the user's part is to mount any remote filesystems with this mechanism. However, depending on the type of filesystem the coherence of the cache/queueing mechanism can be a problem.

## Service discovery

Plan 9 was conceived in a more closed computing environment than today. Machines were often physically connected with statically assigned IP addresses on a stable network. The group were aware of the names of the CPU servers and file servers. Standard filesystem mounts were shared amongst the group. There was little need for discovery services to detect available computers on the network and query for their capabilities.

Computer networks are often much more ad-hoc, even within a large data centre. Home networks have computers that startup/shutdown/sleep at a variety of times depending on their type and usage patterns. Data centres are constantly swapping in and out computing equipment to increase their capacity and manage the lifecycle of the machines. In both cases, there is a need for more automated discovery of services to perform a task, whether it is storage, CPU, network or specific data. As devices become smaller, more numerous and more specialized this trend will only increase over time.

There are a variety of approaches to service discovery in the current computing environment ranging from simple DNS to services such as etcd or kubernetes. In Plan 9 there should be something equivalent to match the current and future need, except done in the usual pragmatic, simple and composable way.

One problem in service discovery is simply communicating with other devices on the network without knowing their host name or IP addresses. Probing IP address ranges to find other machines simply isn't practical since the address spaces can be prohibitively large, especially with IPv6and scanning can take significant time whenever the user wants to run a task. DNS is appealing here, but it requires oversight and careful design to make it work. Data centres sometimes make use of this approach, but it is unclear how well it can work in ad-hoc home environments.

The IPv6 specification now requires that each interface have a link-local IP address and be capable of handling multicast traffic. There are different pre-allocated addresses devoted to all nodes on the network, routers or other specific types of services. Plan 9 can make use of these to automatically discover other nodes on the local network. A command-line tool or filesystem that provides the addresses of local nodes would help to discover other nodes. If those nodes are also Plan 9 systems then in theory 9P connections can be made with them enabling further discovery of their capabilities based on the available files provided that an authentication scheme can be found to support it.

## Free software and Open Source

Linux proved that opening up the source code is the best way to gain widespread adoption. Open source code means that people can take full control of their own systems. It eliminates the need for all patches and fixes to come from a single provider allowing for more collaboration. Debugging problems becomes much easier when you have the original source code. Also, from a software engineering perspective it helps to improve confidence in the system when you can reason about how the system works by directly consulting the code. Any ugliness or complexity is quite transparent.

While the source code was likely available to Bell Labs engineers the business disallowed anyone outside from having access to it in the early days. It took many years for the system to be open sourced. Luckily, all source code to the system is now available so that everyone can benefit from it.

## Networks within the machine

There is a growing need to carve up idle computing resources and run software with them. Sometimes that software was originally written assuming that it has full access to a computer. Other times the software can't be fully trusted to access the host system because it is written by a third party or the source code is not available. Current systems have developed a variety of tools to handle these cases such as virtual machines, containers and jails/sandboxes. The net effect is that a program appears to run in isolation within a virtual network of one physical machine. Multiple CPU core, hardware hyperthreading and vast RAM have helped to sustain the growth.

In the virtualization space things are quite complicated. There is little consistency between virtual machines, containers and jails in terms of how you deploy them. Also, consider that each major operating system (and some of the less common ones) have their own ways of handling each with their own caveats. The picture is large requiring a great deal of specialization and specialists in order to maintain systems.

Plan 9 was able to take a more general approach due to its simple and consistent design. Each process has a "namespace," which represents its view of the filesystem. The vast majority of interactions any process makes with the system is done through accessing files, including network connections. Plan 9 allows each process's namespace to be customized. Each process can also mount filesystems into their namespace visible only to it and its child processes. As a result, it is straight forward to mock or virtualize network connections and other inter process communication. Namespaces can also be constrained using pruning or permissions to restrict the areas of a system that a process can access. Finally, there is a special flag on rfork that allows you to prevent a process tree from breaking out of its namespace making it a complete jail within the constraints of the hardware and OS bugs.

While the Plan 9 approach covers what is currently considered containers and jails it has only recently begun to support virtual machines. One reason for this could be that the underlying system already supports a wide variety of process isolation and customization options as described above alleviating the need for full machine virtualization. Another could be that the idea of running truly untrusted or legacy programs on your machine is something that should be avoided. Recent hardware exploits have demonstrated the extreme difficulty in maintaining absolute isolation between processes. In the end it could be that legacy or untrusted applications should be physically isolated as much as possible making virtual machines unnecessary.


## Webs of text

## Graphics pipelines

## Authentication

## Hardware specialization and limited configurations

## Low cost machines

## Inseparability of computer system, business and society

Do you have any issues with this document? If so, you can raise them here [https://github.com/sirnewton01/plan9-contd/issues](https://github.com/sirnewton01/plan9-contd/issues)