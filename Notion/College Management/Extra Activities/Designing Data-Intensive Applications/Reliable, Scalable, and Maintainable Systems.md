  

  

# Reliability

  

> [!important] Reliability roughly means “continuing to work correctly, even things go wrong.”

  

The things that can go wrong are called **faults**, and systems that anticipate faults and cope with them are called **fault-tolerant or resilient.**

  

A fault is usually defined as one component of the system deviating from its spec, whereas a failure is when the system as a whole stops providing the required service to the user.

- it is impossible to reduce the probability of a fault to zero. so it is best to design fault tolerance mechanisms that prevents faults from causing failures.

  

Many critical bugs are due to poor error handling; by deliberately inducing faults, you ensure that the fault-tolerance machinery is continually exercised and tested, which is why testing is important.

## Hardware Faults

- hardware faults can quickly come into mind. HDDs crash, RAM becomes faulty.
- first response is to add redundancy to individual hardware components to reduce the failure rate.
- until recently, redundancy has been sufficient, but now as data volumes and applications’ computing demands increase, more applications are using more machines, which proportionally increases the rate of hardware faults.
- hence, there is a move towards systems that can tolerate the loss of entire machines by using software fault-tolerance techniques in prefrence to hardware redundancy.

## Software Errors

- systematic error within the system. such faults are harder to anticipate, and because they are correlated across nodes, they tend to cause more system failures than uncorrelated hardware faults.
- the bugs that cause these kinds of software faults often lie dormant for a long time until they are triggered by an unusual set of circumstances. in those circumstances, usually software is making some kind of assumption about its environment—and while that assumption is usually true, it eventually stops being true for some reason.
- there is no quick solution to the problem of systematic faults.
    - carefully think about assumptions and interactions in the sytstem
    - thorough testing
    - process isolation
    - allowing processes to crash and restart
    - measuring
    - monitoring and analyzing system behavior in production.

## Human Errors

- humans design and build software systems, and the operators who keep the system running are also human. even when they have the best intentions, humans are known to be unreliable.
- the best systems combine several approaches
    - design systems that minimizes opportunities for errors
    - decouple the places where people make the most mistakes from the places where they can cause failures.
    - test thoroughly at all levels from unit tests to whole-system integration tests and manual tests. automated testing is widely used
    - allow quick and easy recovery from human errors to minimize the impact in the cause of failure
    - set up detailed and clear monitoring such as performance metrics and error rates.
    - good management practices and training

  

  

# Scalability

  

Even if a system is working reliably today, does not mean that it will work in the future. One common reason for degradation is increased load.

  

> [!important] Scalability is the term usually used to describe a system’s ability to cope with increased load.

- discussing scalability means to discuss the question: if the system grows in a particular way, what are our options for coping with the growth.

  

## Describing Load

- we need to describe load before discussing growth questions. Load can be described with a few numbers which we call load parameters.
    - it can be requests per second; ratio of reads to writes; number of simultaneously active users in a chat room, hit rate.

  

## Describing Performance

- you can now investigate what happens when the load increases
    - when you increase a load parameter, and keep the system resources unchanged, how is performance of your system affected?
    - when you increase a load parameter, how much do you need to increase the resources if you want to keep performance unchanged.
- performance metrics can be throughput, response times and stuff
- High percentiles become especially important in backend services that are called multiple times as part of serving a single end-user req. even if you make the calls in parallel, the end user req still needs to wait for the slowest of the parallel calls to complete.

  

  

## Approaches for coping with load

- how do we maintain good performance, even when our load parameters increase by some amount.

  

An architecture that is appropriate for one level of load is unlikely to cope with ten times that load.

  

People often talk of a dichotomy between scaling up (vertical scaling, moving to a more powerful machine) and scaling out (horizontal scaling, distributing the load across multiple smaller machines)

- distributing load across multiple machines is also known as a shared nothing architecture. Scaling out refers to adding more independent machines (nodes) to distribute the workload instead of upgrading a single machine (scaling up). This is commonly known as a **"share-nothing" architecture** because each node operates independently and does not share memory or disk storage with other nodes. There is no shared state between nodes and if they need to, they do so using external means such as message queues and databases.

  

In reality, good architectures usually involve a pragmatic mixture of approaches.

  

Some systems are elastic, meaning that they automatically add computing resources when they detect a load increase, whereas other systems are scaled manually (human analyses)

- an elastic system can be useful if load is highly unpredictable but manually scaled systems are simpler and may have fewer operational surprises.

  

The architecture of systems that operate at large scale is usually highly specific to the application—there is no such thing as a generic, one-size-fits-all scalable architecture.

an architecture that scales well for a particular application is built around assumptions of which operations will be common and which will be rare—the load parameters.

In an early-stage startup, it’s usually more important to be able to iterate quickly on product features that to scale some hypothetical future load.

Scalable architecture are nevertheless usually built from general-purpose building blocks.

  

# Maintainability

  
  

> [!important] Majority of software costs is not in development, but in maintenance

- this includes fixing bugs, keeping systems operational, investigating failures, adaptation, modification for new reqs, repaying technical debt, and adding new features.

  

Many people working on software systems dislike maintenance of so-called legacy systems. We can and should design software in such a way that it will hopefully minimize pain during maintenance, and thus avoid creating legacy software ourselves.

  

## Three Design Principles for Software Systems

  

### Operability

- Make it easy for operations teams to keep the system running smoothly
- good operability means making routine tasks easy.
    - provide visibility into the runtime behavior and internals of the system , with good monitoring
    - good support for automation and integration with standard tools
    - avoid dependency on individual machines
    - good documentation and an easy-to-understand operational model
    - good default behavior, but also giving admins the freedom to override defaults when needed
    - self-healing when appropriate, but also giving admins manual control over the system when needed
    - predictable behavior, minimizing surprises

### Simplicity

- make it easy for new engineers to understand the system by removing as much complexity as possible from the system
- a software project mired in complexity is sometimes called a big ball of mud.

  

### Evolvability

- make it easy for engineers to make changes to the system and adapt it. aka extensibility, modifiability, plasticity