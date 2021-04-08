# System Design Note

[1.Introduction](./README.md#section#introduction)


# 1. Introduction(Steps)

Thousand feet overview of system design: 



#### 1.1 Requirements clarifications
It is always a good idea to ask questions about the exact scope of the problem we are trying to solve. We should clarify what parts of the system we will be focusing on.

- Will users of our service be able to post tweets and follow other people?
- Should we also design to create and display the user’s timeline?
- Will tweets contain photos and videos?
- Are we focusing on the backend only, or are we developing the front-end too?
- Will users be able to search tweets?
- Do we need to display hot trending topics?
- Will there be any push notification for new (or important) tweets?
All such questions will determine how our end design will look like.

#### 1.2 Back-of-the-envelope estimation
It is always a good idea to estimate the scale of the system we’re going to design. This will also help later when we focus on scaling, partitioning, 
load balancing, and caching.

Let’s expand this with an actual example of designing a Twitter-like service. Here are some questions for designing Twitter that should be answered before moving on to the next steps:
- What scale is expected from the system (e.g., number of new tweets, number of tweet views, number of timeline generations per sec., etc.)?
- How much storage will we need? We will have different storage requirements if users can have photos and videos in their tweets.
- What network bandwidth usage are we expecting? This will be crucial in deciding how we will manage traffic and balance load between servers.

#### 1.3 System interface definition
Define what APIs are expected from the system. This will establish the exact contract expected from the system and ensure if we haven’t gotten any requirements wrong. 

#### 1.4 Defining data model
Defining the data model in the early part of system design, it will clarify how data will flow between different system components. Later, it will guide for data partitioning and management. You should identify various entities of the system, how they will interact with each other, and different aspects of data management like storage, transportation, encryption, etc. Which database system should we use? Will NoSQL like Cassandra best fit our needs, or should we use a MySQL-like solution? What kind of block storage should we use to store photos and videos?

#### 1.5 High level desing
Draw a block diagram with 5-6 boxes representing the core components of our system. We should identify enough components that are needed to solve the actual problem from end-to-end.

For Twitter, at a high-level, we will need multiple application servers to serve all the read/write requests with load balancers in front of them for traffic distributions. If we’re assuming that we will have a lot more read traffic (compared to write), we can decide to have separate servers to handle these scenarios. On the back-end, we need an efficient database that can store all the tweets and support a huge number of reads. We will also need a distributed file storage system for storing photos and videos.

![overview image](https://github.com/khabib97/system-design-note/blob/main/images/overview%20diagram.png)

#### 1.6 Detailed design
We should think different approaches, their pros and cons, and explain why we will prefer one approach over the other. There is no single way; the only important thing is to consider tradeoffs between different options while keeping system constraints in mind.

Since we will be storing a massive amount of data, how should we partition our data to distribute it to multiple databases? Should we try to store all the data of a user on the same database? What issue could it cause?
How will we handle hot users who tweet a lot or follow lots of people?
- Since users’ timeline will contain the most recent (and relevant) tweets, should we try to store our data so that it is optimized for scanning the latest tweets?
- How much and at which layer should we introduce cache to speed things up?
- What components need better load balancing?

#### 1.7 Identifying and resolving bottlenecks
Explore different approaches to mitigate them.

- Is there any single point of failure in our system? What are we doing to mitigate it?
- Do we have enough replicas of the data so that we can still serve our users if we lose a few servers?
- Similarly, do we have enough copies of different services running such that a few failures will not cause a total system shutdown?
- How are we monitoring the performance of our service? Do we get alerts whenever critical components fail or their performance degrades?




