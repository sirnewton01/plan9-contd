# Plan 9 - Continued

Plan 9 did much to bring the clunky and complex Unix environment into the modern era twenty years ago. It proved in a variety of ways that you can actually get more with less using an equal measure of simplicity and consistency. One exampl was the adoption of the file system as the common method of communication between processes, the operating system kernel and even the across networks. This helped to redefine the Plan 9 system as more than just a regular computer operating system into an operating system for an entire network of systems. Since the system is highly consistent it is easier to be a generalist because the behaviour and code of much of the system follows the same design and conventions from kernel to compiler to user applications.

Since the 1980’s a number of factors have changed or evolved since the original inception of Plan 9. In some cases the new developments are extensions of the ones that drove the original design. Computers are increasingly networked together to provide common services as a single logical entity, but increasingly those entities are consolidated into vast data centers distributed globally. Accesses to those systems is not always reliable or stable, such as over a mobile connection or even when moving between wifi hotspots with your laptop. Meanwhile, the computing power and storage has continued its steady expansion over the years.

These changes have occurred on top of an ancient software base (ie. Unix/Linux/BSD and Microsoft Windows) whose origins date back at least forty years. Those bases were so lacking and unappealing that large software stacks erupted in order to drive the current cloud revolution and build magnificent user interfaces. Instead of simple, small and consistent the picture looks much the opposite. It also means that your smartphone and smart watch have ancient teletype handling code in them waiting for something to exploit it.

If the current computing landscape was built on outdated base imagine what could be accomplished by lifting the weights off with a relatively modern alternative. Further adaptations and modernization are still be required, but certainly far less than the effort spent and continues to be spent on the legacy systems. In contrast, imagine how much more limiting and costly it would have been if we had continued to push MS-DOS and Multics forward.

Let's consider how the compuring environment has changed since Plan 9 was under active development and discuss how it can adapt to the new circumstances while retaining the core benefits of the system.

## Filesystems and unstable connections

Within the confines of corporate networks of the 1980's network connections were relatively stable. Machines were rarely disconnected as they had physical connections to the network. They rarely moved or changed IP addresses because they were expensive pieces of equipment with physical security policies limiting their movement. Since workstations were shared resources they rarely changed IP address or at least their host names were stable enough to be relied on. This allowed persistent connections to be made for the most part with little disruption.

As a result, Plan 9 doesn't handle stale network connections particularly well in its core because it assumes a level of stability. The current world of wireless access points, dynamic IP address assignment and user-serviced equipment makes things much more dynamic. The web has solved this problem through a stateless protocol, HTTP, and the use of client-side caches. Using HTTP or porting the current web plumbing, such as web browsers, API's is antithetical to the simple and consistent underpinings of Plan 9. It would add another set of incompatible tools to the base system and adds another layer of complexity for the rest of the system to work around. Existing web support in the system is often confusing to new users since it is not feature complete and also inconsistent with the rest of the system inhibiting reuse and learning.

Can the 9P protocol, which underpins the Plan 9 filesystem, be used or adapted somehow to work under these conditions? It is both stateful and relies on a persistent connection with timeouts in case of failure. One could presumably live with the problem in a number of ways. Carefully mounting and unmounting remote file systems when needed is an option, but it would be easy to forget a filesystem mount in one of your windows or even accidentally bind some other file systems or directories on top of it.

Perhaps you can leave everything on your home network and (re)boot your device every time using a network boot from an image provided by your home network. This is similar to the historical Plan 9 topology with CPU servers, file servers and user terminals that boot except that the terminal can be on a remote network. If you build your profile and update it frequently then it could bring you to workable state quickly with all of your persisted state saved remotely. One trouble would be if you have situations where some resources are accessible while others are not. This is certainly a possible scenario if you consistently use a mix of services each with varying availability. Imagine a case where your home network is available but a cloud service that you use frequently is not. What happens if you don't have a home network and rely instead only on your mobile device and cloud services?

9P itself requires a constant stateful connection to a filesystem, but what if there was a 9P proxy that can connect, reconnect and cache information to a remote filesystem? It may be possible to give the filesystem the commands to establish a connection to an unreliable remote file system so that it can manage the connections. In cases of total loss of connection it can also cache information for read and queue up modifications to replay once a connection is re-established. All that is required on the user's part is to mount any remote filesystems with this mechanism. However, depending on the type of filesystem the coherence of the cache/queueing mechanism can be a problem.

## Service discovery

## Free software and Open Source

## Networks within the machine

## Webs of text

## Graphics pipelines

## Authentication

## Limited hardware variants

## Low cost machines
