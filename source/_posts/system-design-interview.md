---
title: System Design Interview
date: 2023-01-10 09:00:00
---

<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style type="text/css">
#tg  {border-collapse:collapse;border-spacing:0;border-color:#aabcda;}
#tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#aabcda;color:#000;background-color:#e8edfd;}
#tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#aabcfe;color:#039;background-color:#b9c9dd;}
#tg #tg-hmp3{background-color:#D2E4FC;text-align:left;vertical-align:top}
#tg #tg-baqh{text-align:center;vertical-align:top}
#tg #tg-mb3i{background-color:#D2E4FC;text-align:right;vertical-align:top}
#tg #tg-lqy6{text-align:right;vertical-align:top}
#tg #tg-0lax{text-align:left;vertical-align:top}

h1, div {
  padding-left: 5rem;
}


#mySidenav a {
  position: fixed;
  left: -80px;
  transition: 0.3s;
  padding: 15px;
  width: 100px;
  text-decoration: none;
  font-size: 15px;
  color: white;
  border-radius: 0 5px 5px 0;
}

#mySidenav a:hover {
  left: 0;
}

#beginning {
  top: 20px;
  background-color: #04AA6D;
}


</style>

<div>

<!-- Side navigation -->
<div id="mySidenav" class="sidenav">
  <a href="#main-content" id="beginning">To beginning</a>

  <!-- <a href="#Scale-from-zero-to-million-users" id="topic-1">Scale from zero to million users</a>
  <a href="#Back-of-the-envelope-estimation" id="topic-2">Back-of-the-envelope estimation</a>
  <a href="#A-framework-for-system-design-interviews" id="topic-3">A framework for system design interviews</a>
  <a href="#Design-a-rate-limiter" id="topic-4">Design a rate limiter</a>
  <a href="#Design-consistent-hashing" id="topic-5">Design consistent hashing</a>
  <a href="#Design-a-key-value-store" id="topic-6">Design a key-value store</a> -->

</div>


<br>

<div>

This is just main notes captured from the book System Design Interview. 
I strongly encourage to read this book before reading notes as there are a lot of moments described with details in the book and this website provides just some notes that can be useful to review.

</div>

<br>
<br>

<a name="main-content"></a>

<div class="main-content-block">

### Content (Chapters):

- [Scale from zero to million users](#Scale-from-zero-to-million-users)
   - [Single server setup](#Single-server-setup)
   - [Load balancer and database replication](#Load-balancer-and-database-replication)
   - [Cache tier and CDN](#Cache-tier-and-CDN)
   - [Stateless web tier](#Stateless-web-tier)
   - [Data centers](#Data-centers)
   - [Message queue and logging, metrics, automation](#Message-queue-and-logging-metrics-automation)
   - [Database scaling](#Database-scaling)
      - [Vertical scaling](#Vertical-scaling)
      - [Horizontal scaling](#Horizontal-scaling)
   - [Millions of users and beyond](#Millions-of-users-and-beyond)
- [Back-of-the-envelope estimation](#Back-of-the-envelope-estimation)
   - [Power of two](#Power-of-two)
   - [Latency numbers every programmer should know](#Latency-numbers-every-programmer-should-know)
   - [Availability numbers](#Availability-numbers)
- [A framework for system design interviews](#A-framework-for-system-design-interviews)
   - [A 4-step process for effective system design interview](#A-4-step-process-for-effective-system-design-interview)
   - [Step 1 - Understand the problem and establish design scope](#Step-1-Understand-the-problem-and-establish-design-scope)
   - [Step 2 - Propose high-level design and get buy-in](#Step-2-Propose-high-level-design-and-get-buy-in)
   - [Step 3 - Design deep dive](#Step-3-Design-deep-dive)
   - [Step 4 - Wrap up](#Step-4-Wrap-up)
   - [Dos](#Dos)
   - [Donts](#Donts)
- [Design a rate limiter](#Design-a-rate-limiter)
   - [Step 1 - Understand the problem and establish design scope](#Step-1-Understand-the-problem-and-establish-design-scope)
   - [Step 2 - Propose high-level design and get buy-in](#Step-2-Propose-high-level-design-and-get-buy-in)
   - [Where to put the rate limiter?](#Where-to-put-the-rate-limiter)
   - [Algorithms for rate limiting](#Algorithms-for-rate-limiting)
      - [Token bucket algorithm](#Token-bucket-algorithm)
      - [Leaking bucket algorithm](#Leaking-bucket-algorithm)
      - [Fixed window counter algorithm](#Fixed-window-counter-algorithm)
      - [Sliding window log algorithm](#Sliding-window-log-algorithm)
      - [Sliding window counter algorithm](#Sliding-window-counter-algorithm)
   - [High-level architecture](#High-level-architecture)
   - [Step 3 - Design deep dive](#Step-3-Design-deep-dive)
   - [Rate limiting rules](#Rate-limiting-rules)
   - [Exceeding the rate limit](#Exceeding-the-rate-limit)
      - [Rate limiter headers](#Rate-limiter-headers)
   - [Detailed design](#Detailed-design)
   - [Rate limiter in a distributed environment](#Rate-limiter-in-a-distributed-environment)
      - [Race condition](#Race-condition)
      - [Synchronization issue](#Synchronization-issue)
   - [Performance optimization](#Performance-optimization)
   - [Monitoring](#Monitoring)
   - [Step 4 - Wrap up](#Step-4-Wrap-up)
- [Design consistent hashing](#Design-consistent-hashing)
   - [The rehashing problem](#The-rehashing-problem)
   - [Consistent hashing](#Consistent-hashing)
   - [Hash space and hash ring](#Hash-space-and-hash-ring)
   - [Hash servers](#Hash-servers)
   - [Hash keys](#Hash-keys)
   - [Server lookup](#Server-lookup)
   - [Add a server](#Add-a-server)
   - [Remove a server](#Remove-a-server)
   - [Two issues in the basic approach](#Two-issues-in-the-basic-approach)
   - [Virtual nodes](#Virtual-nodes)
   - [Find affected keys](#Find-affected-keys)
   - [Wrap up](#Wrap-up)
- [Design a key-value store](#Design-a-key-value-store)
   - [Understand the problem and establish design scope](#Understand-the-problem-and-establish-design-scope)
   - [Single server key-value store](#Single-server-key-value-store)
   - [Distributed key-value store](#Distributed-key-value-store)
   - [CAP theorem](#CAP-theorem)
   - [System components](#System-components)
   - [Data partition](#Data-partition)
   - [Data replication](#Data-replication)
   - [Consistency](#Consistency)
      - [Consistency models](#Consistency-models)
   - [Inconsistency resolution: versioning](#Inconsistency-resolution-versioning)
   - [Handling failures](#Handling-failures)
      - [Failure detection](#Failure-detection) 
      - [Handling temporary failures](#Handling-temporary-failures)
      - [Handling permanent failures](#Handling-permanent-failures)
      - [Handling data center outage](#Handling-data-center-outage)
  - [System architecture diagram](#System-architecture-diagram)
  - [Write path](#Write-path)
  - [Read path](#Read-path)

<br>
<br>

Quick links:<br>
[Power of two table](#power-of-two)
[Latency numbers table](#latency-numbers)

Rate limiting algorithms:
• [Token bucket](#Token-bucket-algorithm)
• [Leaking bucket](#Leaking-bucket-algorithm)
• [Fixed window counter](#Fixed-window-counter-algorithm)
• [Sliding window log](#Sliding-window-log-algorithm)
• [Sliding window counter](#Sliding-window-counter-algorithm)

<br>
<br>
<br>
<br>

### Scale from zero to million users

#### Single server setup 

Single server setup:

<img src="scale-from-zero-to-million-users/single_server.png" width="550" height="330">

<br>
<br>
<br>

#### Load balancer and database replication

<br>

Server after adding <b>load balancer</b> and <b>database replication</b>:

<br>
<br>
<br>

<img src="scale-from-zero-to-million-users/server_with_load_balancer_and_db_replication.png" width="600" height="650">

<br>
<br>
<br>

• A user gets the IP address of the load balancer from DNS.
• A user connects the load balancer with this IP address.
• The HTTP request is routed to either Server 1 or Server 2.
• A web server reads user data from a slave database.
• A web server routes any data-modifying operations to the master database. This includes write, update, and delete operations.

<br>
<br>
<br>

#### Cache tier and CDN

<br>

Improving load and response time is possible by adding a cache layer and shifting static content (JavaScript/CSS/image/video files) to the content delivery network (CDN is a network of geographically dispersed servers used to deliver static content. CDN servers cache static content like images, videos, CSS, JavaScript files, etc ). The design after the CDN and cache are added:

<br>
<br>
<br>

<img src="scale-from-zero-to-million-users/server_with_cdn_and_cache.png" width="600" height="650">

<br>
<br>
<br>

1. Static assets (JS, CSS, images, etc.,) are no longer served by web servers. They are fetched from the CDN for better performance.
2. The database load is lightened by caching data.

<br>
<br>
<br>

#### Stateless web tier

<br>

It is possible to implement scaling the web tier horizontally (increasing number of servers). For this, we need to move state (for instance user session data) out of the web tier. A good practice is to store session data in the persistent storage such as relational database or NoSQL. Each web server in the cluster can access state data from databases. This is called stateless web tier.

<br>
<br>
<br>

<img src="scale-from-zero-to-million-users/server_with_stateless_web_tier.png" width="620" height="650">

<br>
<br>
<br>

#### Data centers

<br>

Here is an example setup with two data centers. In normal operation, users are geoDNS-routed, also known as geo-routed, to the closest data center, with a split traffic of x% in US-East and (100 – x)% in US-West. geoDNS is a DNS service that allows domain names to be resolved to IP addresses based on the location of a user.

<br>
<br>
<br>

<img src="scale-from-zero-to-million-users/several_data_centers.png" width="620" height="650">

<br>
<br>
<br>

In the event of any significant data center outage, we direct all traffic to a healthy data center. For example here, data center 2 (US-West) is offline, and 100% of the traffic is routed to data center 1 (US-East):

<br>
<br>
<br>

<img src="scale-from-zero-to-million-users/data_center_not_available.png" width="620" height="650">

<br>
<br>
<br>

#### Message queue and logging, metrics, automation

<br>

1. The design includes a message queue, which helps to make the system more loosely coupled and failure resilient.
2. Logging, monitoring, metrics, and automation tools are included.

<br>
<br>
<br>

<img src="scale-from-zero-to-million-users/with_monitoring_and_logging.png" width="600" height="750">

<br>
<br>
<br>

#### Database scaling

<br>

There are two broad approaches for database scaling: vertical scaling and horizontal scaling.

<br>

##### Vertical scaling

Vertical scaling, also known as scaling up, is the scaling by adding more power (CPU, RAM, DISK, etc.) to an existing machine.
Vertical scaling comes with some serious drawbacks:
• You can add more CPU, RAM, etc. to your database server, but there are hardware limits. If you have a large user base, a single server is not enough.
• Greater risk of single point of failures.
• The overall cost of vertical scaling is high. Powerful servers are much more expensive.

<br>

##### Horizontal scaling

Horizontal scaling, also known as sharding, is the practice of adding more servers. 
Sharding separates large databases into smaller, more easily managed parts called shards. Each shard shares the same schema, though the actual data on each shard is unique to the shard.
Three complexities when adding <b>sharding</b>:
- <b>Resharding data:</b> Resharding data is needed when 1) a single shard could no longer hold more data due to rapid growth. 2) Certain shards might experience shard exhaustion faster than others due to uneven data distribution. When shard exhaustion happens, it requires updating the sharding function and moving data around. Consistent hashing is a commonly used technique to solve this problem.
- <b>Celebrity problem:</b> Excessive access to a specific shard could cause server overload (like when several celebrities are located in one shard). For social applications, that shard will be overwhelmed with read operations. To solve this problem, we may need to allocate a shard for each celebrity. Each shard might even require further partition.
- <b>Join and de-normalization:</b> Once a database has been sharded across multiple servers, it is hard to perform join operations across database shards. A common workaround is to de- normalize the database so that queries can be performed in a single table.

That's how the server looks after adding databases sharding to support rapidly increasing data traffic, At the same time, some of the non-relational functionalities are moved to a NoSQL data store to reduce the database load:
<br>
<br>
<br>

<img src="scale-from-zero-to-million-users/final_server_setup.png" width="600" height="750">

<br>
<br>
<br>

#### Millions of users and beyond

<b>Short summary</b>
Scaling our system to support millions of users:
• Keep web tier stateless
• Build redundancy at every tier
• Cache data as much as you can
• Support multiple data centers
• Host static assets in CDN
• Scale your data tier by sharding
• Split tiers into individual services
• Monitor your system and use automation tools


<br>
<br>
<br>

### Back-of-the-envelope estimation

<b>Back-of-the-envelope calculations</b> are <b>estimates</b> you create using a <b>combination of thought experiments</b> and <b>common performance numbers</b> to get a good feel for which designs will meet your requirements.

The following concepts should be well understood: power of two, <b>latency numbers</b> every programmer should know, and <b>availability numbers</b>.

<br>

#### Power of two

<br>

A byte is a sequence of 8 bits. An ASCII character uses one byte of memory (8 bits). A table explaining the data volume unit: 

<a name="power-of-two"></a>
<table id="tg">
  <tr>
    <th id="tg-baqh">Power</th>
    <th id="tg-baqh">Exact value (bytes)</th>
    <th id="tg-baqh">Approximate value</th>
    <th id="tg-baqh">Full name</th>
    <th id="tg-baqh">Short name</th>
  </tr>
  <tr>
    <td id="tg-hmp3">10</td>
    <td id="tg-hmp3">1,024</td>
    <td id="tg-hmp3">1 Thousand</td>
    <td id="tg-hmp3">1 Kilobyte</td>
    <td id="tg-hmp3">1 KB</td>
  </tr>
  <tr>
    <td id="tg-hmp3">20</td>
    <td id="tg-hmp3">1,048,576</td>
    <td id="tg-hmp3">1 Million</td>
    <td id="tg-hmp3">1 Megabyte</td>
    <td id="tg-hmp3">1 MB</td>
  </tr>
  <tr>
    <td id="tg-hmp3">30</td>
    <td id="tg-hmp3">1,073,741,824</td>
    <td id="tg-hmp3">1 Billion</td>
    <td id="tg-hmp3">1 Gigabyte</td>
    <td id="tg-hmp3">1 GB</td>
  </tr>
  <tr>
    <td id="tg-hmp3">40</td>
    <td id="tg-hmp3">1,099,511,627,776</td>
    <td id="tg-hmp3">1 Trillion</td>
    <td id="tg-hmp3">1 Terabyte</td>
    <td id="tg-hmp3">1 TB</td>
  </tr>
  <tr>
    <td id="tg-hmp3">50</td>
    <td id="tg-hmp3">1,125,899,906,842,624</td>
    <td id="tg-hmp3">1 Quadrillion</td>
    <td id="tg-hmp3">1 Petabyte</td>
    <td id="tg-hmp3">1 PB</td>
  </tr>
</table>

<br>

#### Latency numbers every programmer should know

Some numbers are outdated as computers become faster and more powerful. However, those numbers should still be able to give us an idea of the fastness and slowness of different computer operations:

<a name="latency-numbers"></a>
<table id="tg">
  <tr>
    <th id="tg-baqh">Operation name</th>
    <th id="tg-baqh">Time</th>
  </tr>
  <tr>
    <td id="tg-hmp3">L1 cache reference</td>
    <td id="tg-hmp3">0.5 ns</td>
  </tr>
  <tr>
    <td id="tg-hmp3">Branch mispredict</td>
    <td id="tg-hmp3">5 ns</td>
  </tr>
  <tr>
    <td id="tg-hmp3">L2 cache reference</td>
    <td id="tg-hmp3">7 ns</td>
  </tr>
  <tr>
    <td id="tg-hmp3">Mutex lock/unlock</td>
    <td id="tg-hmp3">100 ns</td>
  </tr>
  <tr>
    <td id="tg-hmp3">Main memory reference</td>
    <td id="tg-hmp3">100 ns</td>
  </tr>
  <tr>
    <td id="tg-hmp3">Compress 100 KB bytes with Zippy</td>
    <td id="tg-hmp3">10,000 ns = 10 µs</td>
  </tr>
  <tr>
    <td id="tg-hmp3">Send 2K bytes over 1 Gbps network</td>
    <td id="tg-hmp3">20,000 ns= 20 µs</td>
  </tr>
  <tr>
    <td id="tg-hmp3">Read 1 MB sequentially from memory</td>
    <td id="tg-hmp3">250,000 ns = 250 µs</td>
  </tr>
  <tr>
    <td id="tg-hmp3">Round trip within the same datacenter</td>
    <td id="tg-hmp3">500,000 ns = 500 µs</td>
  </tr>
  <tr>
    <td id="tg-hmp3">Disk seek</td>
    <td id="tg-hmp3">10,000,000 ns = 10 ms</td>
  </tr>
  <tr>
    <td id="tg-hmp3">Read 1 MB sequentially from the network</td>
    <td id="tg-hmp3">10,000,000 ns = 10 ms</td>
  </tr>
  <tr>
    <td id="tg-hmp3">Read 1 MB sequentially from disk</td>
    <td id="tg-hmp3">30,000,000 ns = 30 ms</td>
  </tr>
  <tr>
    <td id="tg-hmp3">Send packet CA (California) -> Netherlands -> CA</td>
    <td id="tg-hmp3">150,000,000 ns = 150 ms</td>
  </tr>
</table>

<br>

<i>Notes</i>
<p>-------------</p>
ns = nanosecond, μs = microsecond, ms = millisecond 1 ns = 10^-9 seconds<br>
1 μs= 10^-6 seconds = 1,000 ns<br>
1 ms = 10^-3 seconds = 1,000 μs = 1,000,000 ns<br>


Conclusions:
• Memory is fast but the disk is slow.
• Avoid disk seeks if possible.
• Simple compression algorithms are fast.
• Compress data before sending it over the internet if possible.
• Data centers are usually in different regions, and it takes time to send data between them.

<br>

#### Availability numbers

High availability is the ability of a system to be continuously operational for a desirably long period of time. High availability is measured as a percentage, with 100% means a service that has 0 downtime. Most services fall between 99% and 100%.
A service level agreement (SLA) is a commonly used term for service providers. This is an agreement between you (the service provider) and your customer, and this agreement formally defines the level of uptime your service will deliver. Cloud providers Amazon [4], Google [5] and Microsoft [6] set their SLAs at 99.9% or above.

<a name="pookie"></a>

<table id="tg">
  <tr>
    <th id="tg-baqh">Availability %</th>
    <th id="tg-baqh">Downtime per day</th>
    <th id="tg-baqh">Downtime per year</th>
  </tr>
  <tr>
    <td id="tg-hmp3">99%</td>
    <td id="tg-hmp3">14.40 minutes</td>
    <td id="tg-hmp3">3.65 days</td>
  </tr>
  <tr>
    <td id="tg-hmp3">99.9%</td>
    <td id="tg-hmp3">1.44 minutes</td>
    <td id="tg-hmp3">8.77 hours</td>
  </tr>
  <tr>
    <td id="tg-hmp3">99.99%</td>
    <td id="tg-hmp3">8.64 seconds</td>
    <td id="tg-hmp3">52.60 minutes</td>
  </tr>
  <tr>
    <td id="tg-hmp3">99.999%</td>
    <td id="tg-hmp3">864.00 milliseconds</td>
    <td id="tg-hmp3">5.26 minutes</td>
  </tr>
  <tr>
    <td id="tg-hmp3">99.9999%</td>
    <td id="tg-hmp3">86.40 milliseconds</td>
    <td id="tg-hmp3">31.56 seconds</td>
  </tr>
</table>

<br>
<br>

### A framework for system design interviews

<br>

#### A 4-step process for effective system design interview

<br>

#### Step 1 - Understand the problem and establish design scope

One of the most important skills as an engineer is to ask the right questions, make the proper assumptions, and gather all the information needed to build a system.
What kind of questions to ask? Ask questions to understand the exact requirements. Here is a list of questions to help you get started:
• What specific features are we going to build? • How many users does the product have?
• How fast does the company anticipate to scale up? What are the anticipated scales in 3 months, 6 months, and a year?
• What is the company’s technology stack? What existing services you might leverage to simplify the design?

<br>

#### Step 2 - Propose high-level design and get buy-in

In this step, we aim to develop a high-level design and reach an agreement with the interviewer on the design. It is a great idea to collaborate with the interviewer during the process.
• Come up with an initial blueprint for the design. Ask for feedback. Treat your interviewer as a teammate and work together. Many good interviewers love to talk and get involved.
• Draw box diagrams with key components on the whiteboard or paper. This might include clients (mobile/web), APIs, web servers, data stores, cache, CDN, message queue, etc.
• Do back-of-the-envelope calculations to evaluate if your blueprint fits the scale constraints. Think out loud. Communicate with your interviewer if back-of-the-envelope is necessary before diving into it.

<br>

#### Step 3 - Design deep dive

At this step, you and your interviewer should have already achieved the following objectives: 
• Agreed on the overall goals and feature scope
• Sketched out a high-level blueprint for the overall design
• Obtained feedback from your interviewer on the high-level design
• Had some initial ideas about areas to focus on in deep dive based on her feedback
You shall work with the interviewer to identify and prioritize components in the architecture.

Time management is essential as it is easy to get carried away with minute details that do not demonstrate your abilities. Try not to get into unnecessary details.

<br>

#### Step 4 - Wrap up

In this final step, the interviewer might ask you a few follow-up questions or give you the freedom to discuss other additional points. Here are a few directions to follow:
• The interviewer might want you to identify the system bottlenecks and discuss potential improvements.
• It could be useful to give the interviewer a recap of your design.
• Error cases (server failure, network loss, etc.) are interesting to talk about.
• Operation issues are worth mentioning. How do you monitor metrics and error logs? How to roll out the system?
• How to handle the next scale curve is also an interesting topic.
• Propose other refinements you need if you had more time.

<br>

#### Dos

• Always ask for clarification. Do not assume your assumption is correct. • Understand the requirements of the problem.
• There is neither the right answer nor the best answer. A solution designed to solve the problems of a young startup is different from that of an established company with millions of users. Make sure you understand the requirements.
• Let the interviewer know what you are thinking. Communicate with your interview. • Suggest multiple approaches if possible.
• Once you agree with your interviewer on the blueprint, go into details on each component. Design the most critical components first.
• Bounce ideas off the interviewer. A good interviewer works with you as a teammate. • Never give up.

<br>

#### Donts

• Don't be unprepared for typical interview questions.
• Don’t jump into a solution without clarifying the requirements and assumptions.
• Don’t go into too much detail on a single component in the beginning. Give the high- level design first then drills down.
• If you get stuck, don't hesitate to ask for hints. • Again, communicate. Don't think in silence.
• Don’t think your interview is done once you give the design. You are not done until your interviewer says you are done. Ask for feedback early and often.

<br>

#### Time allocation on each step

Step 1 Understand the problem and establish design scope: 3 - 10 minutes Step 2 Propose high-level design and get buy-in: 10 - 15 minutes
Step 3 Design deep dive: 10 - 25 minutes
Step 4 Wrap: 3 - 5 minutes

<br>
<br>

### Design a rate limiter

In a network system, a rate limiter is used to control the rate of traffic sent by a client or a service. In the HTTP world, a rate limiter limits the number of client requests allowed to be sent over a specified period. If the API request count exceeds the threshold defined by the rate limiter, all the excess calls are blocked.

The benefits of using an API rate limiter:
• Prevent resource starvation caused by Denial of Service (DoS) attack. A rate limiter prevents DoS attacks, either intentional or unintentional, by blocking the excess calls.
• Reduce cost. Limiting excess requests means fewer servers and allocating more resources to high priority APIs.
• Prevent servers from being overloaded.

<br>

#### Step 1 - Understand the problem and establish design scope

Rate limiting can be implemented using different algorithms, each with its pros and cons. The interactions between an interviewer and a candidate help to clarify the type of rate limiters we are trying to build.

<br>

#### Step 2 - Propose high-level design and get buy-in

<br>

#### Where to put the rate limiter?

• Client-side implementation. (Client is an unreliable place to enforce rate limiting because client requests can easily be forged by malicious actors.)

• Server-side implementation.

<br>
<br>
<br>

<img src="design-a-rate-limiter/server-side-rate-limiter.png" width="600" height="120">

<br>
<br>
<br>

• Rate limiter middleware

<br>
<br>
<br>

<img src="design-a-rate-limiter/middleware-rate-limiter.png" width="600" height="200">

<br>
<br>
<br>

Assuming API allows 2 requests per second, and a client sends 3 requests to the server within a second. The first two requests are routed to API servers. However, the rate limiter middleware throttles the third request and returns a HTTP status code 429:

<br>
<br>
<br>

<img src="design-a-rate-limiter/rate-limiter-with-response.png" width="600" height="200">

<br>
<br>
<br>

Where should the rater limiter be implemented, on the server-side or in a gateway? Here are a few general guidelines:
• Evaluate your current technology stack, such as programming language, cache service, etc. Make sure your current programming language is efficient to implement rate limiting on the server-side.
• Identify the rate limiting algorithm that fits your business needs. When you implement everything on the server-side, you have full control of the algorithm. However, your choice might be limited if you use a third-party gateway.
• If you have already used microservice architecture and included an API gateway in the design to perform authentication, IP whitelisting, etc., you may add a rate limiter to the API gateway.
• Building your own rate limiting service takes time. If you do not have enough engineering resources to implement a rate limiter, a commercial API gateway is a better option.

<br>

#### Algorithms for rate limiting

<br>

##### Token bucket algorithm

The token bucket algorithm work as follows:
• A token bucket is a container that has pre-defined capacity. Tokens are put in the bucket at preset rates periodically. Once the bucket is full, no more tokens are added. Example: the token bucket capacity is 4. The refiller puts 2 tokens into the bucket every second. Once the bucket is full, extra tokens will overflow:

<br>
<br>
<br>

<img src="design-a-rate-limiter/token-bucket-example.png" width="200" height="200">

<br>
<br>
<br>

• Each request consumes one token. When a request arrives, we check if there are enough tokens in the bucket. Figure 4-5 explains how it works.
• If there are enough tokens, we take one token out for each request, and the request goes through.
• If there are not enough tokens, the request is dropped.

<br>
<br>
<br>

<img src="design-a-rate-limiter/how-token-bucket-algorithm-works.png" width="400" height="350">

<br>
<br>
<br>

<br>
<br>
<br>

<img src="design-a-rate-limiter/how-token-bucket-algorithm-works-2.png" width="400" height="400">

<br>
<br>
<br>

How many buckets do we need depends on the rate-limiting rules:
• It is usually necessary to have different buckets for different API endpoints. For instance, if a user is allowed to make 1 post per second, add 150 friends per day, and like 5 posts per second, 3 buckets are required for each user.
• If we need to throttle requests based on IP addresses, each IP address requires a bucket.
• If the system allows a maximum of 10,000 requests per second, it makes sense to have a global bucket shared by all requests.

<br>

Algorithm pros:
• The algorithm is easy to implement.
• Memory efficient.
• Token bucket allows a burst of traffic for short periods. A request can go through as long as there are tokens left.
Algorithm cons:
• Two parameters in the algorithm are bucket size and token refill rate. However, it might
be challenging to tune them properly.

<br>

##### Leaking bucket algorithm

The leaking bucket algorithm is similar to the token bucket except that requests are processed at a fixed rate. It is usually implemented with a first-in-first-out (FIFO) queue. The algorithm works as follows:
• When a request arrives, the system checks if the queue is full. If it is not full, the request is added to the queue.
• Otherwise, the request is dropped.
• Requests are pulled from the queue and processed at regular intervals.

<br>
<br>
<br>

<img src="design-a-rate-limiter/leaking-bucket.png" width="600" height="200">

<br>
<br>
<br>

Algorithm pros:
• Memory efficient given the limited queue size.
• Requests are processed at a fixed rate therefore it is suitable for use cases that a stable outflow rate is needed.
Algorithm cons:
• A burst of traffic fills up the queue with old requests, and if they are not processed in time, recent requests will be rate limited.
• There are two parameters in the algorithm. It might not be easy to tune them properly.

<br>

##### Fixed window counter algorithm

Fixed window counter algorithm works as follows:
• The algorithm divides the timeline into fix-sized time windows and assign a counter for each window.
• Each request increments the counter by one.
• Once the counter reaches the pre-defined threshold, new requests are dropped until a new time window starts.

Example with 1 second time unit and maximum of 3 requests per second:
<br>
<br>
<br>

<img src="design-a-rate-limiter/fixed-window-counter-example.png" width="450" height="250">

<br>
<br>
<br>

A major problem with this algorithm is that a burst of traffic at the edges of time windows could cause more requests than allowed quota to go through. For example, the system allows a maximum of 5 requests per minute, and the available quota resets at the human-friendly round minute. As seen, there are five requests between 2:00:00 and 2:01:00 and five more requests between 2:01:00 and 2:02:00. For the one-minute window between 2:00:30 and 2:01:30, 10 requests go through. That is twice as many as allowed requests.:

<br>
<br>
<br>

<img src="design-a-rate-limiter/fixed-window-counter-problem.png" width="450" height="200">

<br>
<br>
<br>

Algorithm pros:
• Memory efficient.
• Easy to understand.
• Resetting available quota at the end of a unit time window fits certain use cases.
Algorithm cons:
• Spike in traffic at the edges of a window could cause more requests than the allowed quota to go through.

<br>

##### Sliding window log algorithm

Sliding window log algorithm works as follows:
• The algorithm keeps track of request timestamps. Timestamp data is usually kept in cache, such as sorted sets of Redis [8].
• When a new request comes in, remove all the outdated timestamps. Outdated timestamps are defined as those older than the start of the current time window.
• Add timestamp of the new request to the log.
• If the log size is the same or lower than the allowed count, a request is accepted. Otherwise, it is rejected.

<br>
<br>
<br>

<img src="design-a-rate-limiter/sliding-window-log.png" width="500" height="400">

<br>
<br>
<br>

Algorithm pros:
• Rate limiting implemented by this algorithm is very accurate. In any rolling window, requests will not exceed the rate limit.
Algorithm cons:
• The algorithm consumes a lot of memory because even if a request is rejected, its timestamp might still be stored in memory.

<br>

##### Sliding window counter algorithm

The sliding window counter algorithm is a hybrid approach that combines the fixed window counter and sliding window log.

<br>
<br>
<br>

<img src="design-a-rate-limiter/sliding-window-counter.png" width="500" height="300">

<br>
<br>
<br>

Assume the rate limiter allows a maximum of 7 requests per minute, and there are 5 requests in the previous minute and 3 in the current minute. For a new request that arrives at a 30% position in the current minute, the number of requests in the rolling window is calculated using the following formula:
• Requests in current window + requests in the previous window * overlap percentage of the rolling window and previous window
 
• Using this formula, we get 3 + 5 * 0.7% = 6.5 request. Depending on the use case, the number can either be rounded up or down. In our example, it is rounded down to 6.
Since the rate limiter allows a maximum of 7 requests per minute, the current request can go through. However, the limit will be reached after receiving one more request.

Algorithm pros
• It smooths out spikes in traffic because the rate is based on the average rate of the previous window.
• Memory efficient.
Algorithm cons
• It only works for not-so-strict look back window. It is an approximation of the actual rate because it assumes requests in the previous window are evenly distributed.

<br>

#### High-level architecture

The basic idea of rate limiting algorithms is a counter to keep track of how many requests are sent from the same user, IP address, etc.
Where shall we store counters? Using the database is not a good idea due to slowness of disk access. In-memory cache is chosen because it is fast and supports time-based expiration strategy. (For example Redis)

<br>
<br>
<br>

<img src="design-a-rate-limiter/redis-for-rate-limiting.png" width="600" height="250">

<br>
<br>
<br>

#### Step 3 - Design deep dive

<br>

#### Rate limiting rules

Example:<br>

The system is configured to allow a maximum of 5 marketing messages per day:<br>

<code>
domain: messaging<br>
descriptors:<br>
- key: message_type<br>
&nbsp value: marketing<br>
&nbsp rate_limit:<br>
&nbsp&nbsp&nbsp unit: day<br>
&nbsp&nbsp&nbsp requests_per_unit: 5<br>
</code>

Rules are generally written in configuration files and saved on disk.

<br>

#### Exceeding the rate limit

In case a request is rate limited, APIs return a HTTP response code 429 (too many requests) to the client. Depending on the use cases, we may enqueue the rate-limited requests to be processed later.

<br>

##### Rate limiter headers

The rate limiter returns the following HTTP headers to clients:
<i>X-Ratelimit-Remaining</i>: The remaining number of allowed requests within the window.<br>
<i>X-Ratelimit-Limit</i>: It indicates how many calls the client can make per time window.<br>
<i>X-Ratelimit-Retry-After</i>: The number of seconds to wait until you can make a request again without being throttled.
When a user has sent too many requests, a 429 too many requests error and X-Ratelimit-Retry-After header are returned to the client.

<br>

#### Detailed design

<br>
<br>
<br>

<img src="design-a-rate-limiter/detailed-design-of-rate-limiter-system.png" width="600" height="500">

<br>
<br>
<br>

#### Rate limiter in a distributed environment

Scaling the system to support multiple servers and concurrent threads is a different story. There are two challenges:
• Race condition
• Synchronization issue

<br>

##### Race condition

<br>
<br>
<br>

<img src="design-a-rate-limiter/race-condition.png" width="600" height="300">

<br>
<br>
<br>

Locks is the solution for solving race condition, but locks will significantly slow down the system. Two strategies are commonly used to solve the problem: Lua script and sorted sets data structure in Redis.

<br>

##### Synchronization issue

To support millions of users, one rate limiter server might not be enough to handle the traffic. When multiple rate limiter servers are used, synchronization is required.

<br>
<br>
<br>

<img src="design-a-rate-limiter/synchronization-issue.png" width="600" height="200">

<br>
<br>
<br>

One possible solution is to use sticky sessions that allow a client to send traffic to the same rate limiter. This solution is not advisable because it is neither scalable nor flexible. A better approach is to use centralized data stores like Redis.

<br>
<br>
<br>

<img src="design-a-rate-limiter/synchronization-issue-solution.png" width="400" height="170">

<br>
<br>
<br>

#### Performance optimization

- Multi-data center setup is crucial for a rate limiter because latency is high for users located far away from the data center. Most cloud service providers build many edge server locations around the world. 

- Synchronize data with an eventual consistency model

<br>

#### Monitoring

Make sure:
• The rate limiting algorithm is effective. 
• The rate limiting rules are effective.

If rate limiting rules are too strict, many valid requests are dropped.
In another example, we notice our rate limiter becomes ineffective when there is a sudden increase in traffic like flash sales (token bucket is a good fit here).

<br>

#### Step 4 - Wrap up

Additional talking points to mention:
• Hard vs soft rate limiting.
   • Hard: The number of requests cannot exceed the threshold. 
   • Soft: Requests can exceed the threshold for a short period.
• Rate limiting at different levels (application level, rate limiting by IP addresses using Iptables)
• Avoid being rate limited. Design your client with best practices:
   • Use client cache to avoid making frequent API calls.
   • Understand the limit and do not send too many requests in a short time frame.
   • Include code to catch exceptions or errors so your client can gracefully recover from exceptions.
   • Add sufficient back off time to retry logic.

<br>

### Design consistent hashing

To achieve horizontal scaling, it is important to distribute requests/data efficiently and evenly across servers. Consistent hashing is a commonly used technique to achieve this goal.

<br>

#### The rehashing problem

If you have n cache servers, a common way to balance the load is to use the following hash method:
serverIndex = hash(key) % N, where N is the size of the server pool.

<br>
<br>
<br>

<table id="tg">
  <tr>
    <th id="tg-baqh">key</th>
    <th id="tg-baqh">hash</th>
    <th id="tg-baqh">hash % 4</th>
  </tr>
  <tr>
    <td id="tg-hmp3">key 0</td>
    <td id="tg-hmp3">18358617</td>
    <td id="tg-hmp3">1</td>
  </tr>
  <tr>
    <td id="tg-hmp3">key 1</td>
    <td id="tg-hmp3">26143584</td>
    <td id="tg-hmp3">0</td>
  </tr>
  <tr>
    <td id="tg-hmp3">key 2</td>
    <td id="tg-hmp3">18131146</td>
    <td id="tg-hmp3">2</td>
  </tr>
  <tr>
    <td id="tg-hmp3">key 3</td>
    <td id="tg-hmp3">35863496</td>
    <td id="tg-hmp3">0</td>
  </tr>
  <tr>
    <td id="tg-hmp3">key 4</td>
    <td id="tg-hmp3">34085809</td>
    <td id="tg-hmp3">1</td>
  </tr>
  <tr>
    <td id="tg-hmp3">key 5</td>
    <td id="tg-hmp3">27581703</td>
    <td id="tg-hmp3">3</td>
  </tr>
  <tr>
    <td id="tg-hmp3">key 6</td>
    <td id="tg-hmp3">38164978</td>
    <td id="tg-hmp3">2</td>
  </tr>
  <tr>
    <td id="tg-hmp3">key 7</td>
    <td id="tg-hmp3">22530351</td>
    <td id="tg-hmp3">3</td>
  </tr>
  <tr>
</table>

<br>
<br>
<br>

hash(key0) % 4 = 1 means a client must contact server 1 to fetch the cached data:

<br>
<br>
<br>

<img src="design-consistent-hashing/server-choice-depending-on-hash.png" width="400" height="200">

<br>
<br>
<br>

This approach works well when the size of the server pool is fixed, and the data distribution is even. However, problems arise when new servers are added, or existing servers are removed. For example, if server 1 goes offline, the size of the server pool becomes 3. Using the same hash function, we get the same hash value for a key. But applying modular operation gives us different server indexes because the number of servers is reduced by 1:

<br>
<br>
<br>

<img src="design-consistent-hashing/one-server-down.png" width="420" height="250">

<br>
<br>
<br>

This means that when server 1 goes offline, most cache clients will connect to the wrong servers to fetch data. This causes a storm of cache misses. Consistent hashing is an effective technique to mitigate this problem.

<br>

#### Consistent hashing

Consistent hashing is a special kind of hashing such that when a hash table is re-sized and consistent hashing is used, only k/n keys need to be remapped on average, where k is the number of keys, and n is the number of slots.

<br>

#### Hash space and hash ring

Assume SHA-1 is used as the hash function f, and the output range of the hash function is: x0, x1, x2, x3, ..., xn. In cryptography, SHA-1’s hash space goes from 0 to 2^160 - 1. That means x0 corresponds to 0, xn corresponds to 2^160 – 1, and all the other hash values in the middle fall between 0 and 2^160 - 1.

Hash space:

<br>
<br>
<br>

<img src="design-consistent-hashing/hash-space.png" width="600" height="100">

<br>
<br>
<br>

By collecting both ends, we get a hash ring:

<br>
<br>
<br>

<img src="design-consistent-hashing/hash-ring.png" width="300" height="350">

<br>
<br>
<br>

#### Hash servers

Using the same hash function f, we map servers based on server IP or name onto the ring.
Servers are mapped on the hash ring:

<br>
<br>
<br>

<img src="design-consistent-hashing/server-mapped-on-a-hash-ring.png" width="400" height="250">

<br>
<br>
<br>

#### Hash keys

Hash function used here is different from the one in “the rehashing problem,” and there is no modular operation. 4 cache keys (key0, key1, key2, and key3) are hashed onto the hash ring:

<br>
<br>
<br>

<img src="design-consistent-hashing/cache-keys-on-hash-ring.png" width="400" height="230">

<br>
<br>
<br>

#### Server lookup

To determine which server a key is stored on, we go clockwise from the key position on the ring until a server is found. Going clockwise, key0 is stored on server 0; key1 is stored on server 1; key2 is stored on server 2 and key3 is stored on server 3:

<br>
<br>
<br>

<img src="design-consistent-hashing/which-server-key-is-stored.png" width="400" height="250">

<br>
<br>
<br>

#### Add a server

Adding a new server will only require redistribution of a fraction of keys.

After a new server 4 is added, only key0 needs to be redistributed. k1, k2, and k3 remain on the same servers. Before server 4 is added, key0 is stored on server 0. Now, key0 will be stored on server 4 because server 4 is the first server it encounters by going clockwise from key0’s position on the ring. 

<br>
<br>
<br>

<img src="design-consistent-hashing/after-adding-server.png" width="400" height="280">

<br>
<br>
<br>

#### Remove a server

When a server is removed, only a small fraction of keys require redistribution with consistent hashing. When server 1 is removed, only key1 must be remapped to server 2. The rest of the keys are unaffected:

<br>
<br>
<br>

<img src="design-consistent-hashing/after-removing-server.png" width="400" height="280">

<br>
<br>
<br>

#### Two issues in the basic approach

The basic steps of consistent hashing algorithm:
• Map servers and keys on to the ring using a uniformly distributed hash function.
• To find out which server a key is mapped to, go clockwise from the key position until the first server on the ring is found.

Two problems are identified with this approach. 

First, it is impossible to keep the same size of partitions on the ring for all servers considering a server can be added or removed. It is possible that the size of the partitions on the ring assigned to each server is very small or fairly large.

<br>
<br>
<br>

<img src="design-consistent-hashing/first-problem-of-consistent-hashing-algorithm.png" width="430" height="240">

<br>
<br>
<br>

Second, it is possible to have a non-uniform key distribution on the ring. For instance, most of the keys are stored on server 2:

<br>
<br>
<br>

<img src="design-consistent-hashing/second-problem-of-consistent-hashing-algorithm.png" width="400" height="250">

<br>
<br>
<br>

A technique called virtual nodes or replicas is used to solve these problems.

<br>

#### Virtual nodes

A virtual node refers to the real node, and each server is represented by multiple virtual nodes on the ring.
Server 0 and server 1 have 3 virtual nodes:
With virtual nodes, each server is responsible for multiple partitions. Partitions (edges) with label s0 are managed by server 0, partitions with label s1 are managed by server 1.

<br>
<br>
<br>

<img src="design-consistent-hashing/virtual-nodes.png" width="410" height="270">

<br>
<br>
<br>

To find which server a key is stored on, we go clockwise from the key’s location and find the first virtual node encountered on the ring.

<br>
<br>
<br>

<img src="design-consistent-hashing/virtual-nodes-find-the-key.png" width="410" height="250">

<br>
<br>
<br>

As the number of virtual nodes increases, the distribution of keys becomes more balanced, because the standard deviation gets smaller with more virtual nodes, leading to balanced data distribution.

<br>

#### Find affected keys

Server added: the affected range starts from s4 (newly added node) and moves anticlockwise around the ring until a server is found (s3), keys located between s3 and s4 need to be redistributed to s4.

<br>
<br>
<br>

<img src="design-consistent-hashing/affected-keys-added-server.png" width="430" height="300">

<br>
<br>
<br>

Server removed: the affected range starts from s1 (removed node) and moves anticlockwise around the ring until a server is found (s0), keys located between s0 and s1 must be redistributed to s2.

<br>
<br>
<br>

<img src="design-consistent-hashing/affected-keys-removed-server.png" width="430" height="300">

<br>
<br>
<br>

#### Wrap up

The benefits of consistent hashing include:
• Minimized keys are redistributed when servers are added or removed.
• It is easy to scale horizontally because data are more evenly distributed.
• Mitigate hotspot key problem.

<br>

### Design a key-value store

A key-value store, also referred to as a key-value database, is a non-relational database. Each unique identifier is stored as a key with its associated value. This data pairing is known as a “key-value” pair.

<br>

#### Understand the problem and establish design scope

Each design achieves a specific balance regarding the tradeoffs of the read, write, and memory usage. Another tradeoff has to be made was between consistency and availability. In this chapter, we design a key-value store that comprises of the following characteristics:
• The size of a key-value pair is small: less than 10 KB.
• Ability to store big data.
• High availability: The system responds quickly, even during failures.
• High scalability: The system can be scaled to support large data set.
• Automatic scaling: The addition/deletion of servers should be automatic based on traffic. 
• Tunable consistency.
• Low latency.

<br>

#### Single server key-value store

An intuitive approach is to store key-value pairs in a hash table, which keeps everything in memory. Even though memory access is fast, fitting everything in memory may be impossible due to the space constraint. Two optimizations can be done to fit more data in a single server:
• Data compression
• Store only frequently used data in memory and the rest on disk
Even with these optimizations, a single server can reach its capacity very quickly -> A distributed key-value store is required to support big data.

<br>

#### Distributed key-value store

<br>

#### CAP theorem 

CAP (Consistency, Availability, Partition Tolerance) theorem states it is impossible for a distributed system to simultaneously provide more than two of these three guarantees: consistency, availability, and partition tolerance.

Consistency: consistency means all clients see the same data at the same time no matter which node they connect to.
Availability: availability means any client which requests data gets a response even if some of the nodes are down.
Partition Tolerance: a partition indicates a communication break between two nodes. Partition tolerance means the system continues to operate despite network partitions.

Nowadays, key-value stores are classified based on the two CAP characteristics they support:
CP (consistency and partition tolerance) systems: a CP key-value store supports consistency and partition tolerance while sacrificing availability.
AP (availability and partition tolerance) systems: an AP key-value store supports availability and partition tolerance while sacrificing consistency.
CA (consistency and availability) systems: a CA key-value store supports consistency and availability while sacrificing partition tolerance. Since network failure is unavoidable, a distributed system must tolerate network partition. Thus, a CA system cannot exist in real- world applications.

<br>

#### System components
In this section, we will discuss the following core components and techniques used to build a key-value store:
• Data partition
• Data replication
• Consistency
• Inconsistency resolution
• Handling failures
• System architecture diagram • Write path
• Read path

<br>

#### Data partition

There are two challenges while partitioning the data:
• Distribute data across multiple servers evenly.
• Minimize data movement when nodes are added or removed.

Consistent hashing discussed in previous chapter is a great technique to solve these problems.

<br>

#### Data replication

To achieve high availability and reliability, data must be replicated asynchronously over N servers, where N is a configurable parameter. These N servers are chosen using the following logic: after a key is mapped to a position on the hash ring, walk clockwise from that position and choose the first N servers on the ring to store data copies. In Figure (N = 3), key0 is replicated at s1, s2, and s3.

<br>
<br>
<br>

<img src="design-a-key-value-store/servers-replics-clockwise.png" width="400" height="400">

<br>
<br>
<br>

For better reliability, replicas are placed in distinct data centers, and data centers are connected through high-speed networks.

<br>

#### Consistency

Since data is replicated at multiple nodes, it must be synchronized across replicas.

<b>N</b> = The number of replicas
<b>W</b> = A write quorum of size W. For a write operation to be considered as successful, write operation must be acknowledged from W replicas.
<b>R</b> = A read quorum of size R. For a read operation to be considered as successful, read operation must wait for responses from at least R replicas.

<br>
<br>
<br>

<img src="design-a-key-value-store/synchronization-between-replicas.png" width="400" height="400">

<br>
<br>
<br>

W = 1 means that the coordinator must receive at least one acknowledgment before the write operation is considered as successful.

If W + R > N, strong consistency is guaranteed because there must be at least one overlapping node that has the latest data to ensure consistency.
How to configure N, W, and R to fit our use cases? Here are some of the possible setups:
If R = 1 and W = N, the system is optimized for a fast read.
If W = 1 and R = N, the system is optimized for fast write.
If W + R > N, strong consistency is guaranteed (Usually N = 3, W = R = 2).
If W + R <= N, strong consistency is not guaranteed.

<br>

##### Consistency models

Consistency model is other important factor to consider when designing a key-value store.
• Strong consistency: any read operation returns a value corresponding to the result of the most updated write data item. A client never sees out-of-date data. -> usually achieved by forcing a replica not to accept new reads/writes until every replica has agreed on current write. This approach is not ideal for highly available systems because it could block new operations.
• Weak consistency: subsequent read operations may not see the most updated value.
• Eventual consistency: this is a specific form of weak consistency. Given enough time, all updates are propagated, and all replicas are consistent.

<br>

#### Inconsistency resolution: versioning

Replication gives high availability but causes inconsistencies among replicas.

To resolve this issue, we need a versioning system that can detect conflicts and reconcile conflicts. A vector clock is a common technique to solve this problem. Let us examine how vector clocks work.

<br>
<br>
<br>

<img src="design-a-key-value-store/inconsistency-example-1.png" width="620" height="400">

<br>
<br>
<br>

<br>
<br>
<br>

<img src="design-a-key-value-store/inconsistency-example-2.png" width="720" height="400">

<br>
<br>
<br>


The solution -> <b>vector clocks</b>

1. A client writes a data item D1 to the system, and the write is handled by server Sx, which now has the vector clock D1[(Sx, 1)].
2. Another client reads the latest D1, updates it to D2, and writes it back. D2 descends from D1 so it overwrites D1. Assume the write is handled by the same server Sx, which now has vector clock D2([Sx, 2]).
3. Another client reads the latest D2, updates it to D3, and writes it back. Assume the write is handled by server Sy, which now has vector clock D3([Sx, 2], [Sy, 1])).
4. Another client reads the latest D2, updates it to D4, and writes it back. Assume the write is handled by server Sz, which now has D4([Sx, 2], [Sz, 1])).
5. When another client reads D3 and D4, it discovers a conflict, which is caused by data item D2 being modified by both Sy and Sz. The conflict is resolved by the client and updated data is sent to the server. Assume the write is handled by Sx, which now has D5([Sx, 3], [Sy, 1], [Sz, 1]). We will explain how to detect conflict shortly.

<br>
<br>
<br>

<img src="design-a-key-value-store/vector-clock.png" width="400" height="400">

<br>
<br>
<br>

#### Handling failures

<br>

##### Failure detection

Failure detection solution - gossip protocol.

Gossip protocol works as follows:
• Each node maintains a node membership list, which contains member IDs and heartbeat counters.
• Each node periodically increments its heartbeat counter.
• Each node periodically sends heartbeats to a set of random nodes, which in turn propagate to another set of nodes.
• Once nodes receive heartbeats, membership list is updated to the latest info.
• If the heartbeat has not increased for more than predefined periods, the member is considered as offline.

<br>
<br>
<br>

<img src="design-a-key-value-store/gossip-protocol.png" width="800" height="325">

<br>
<br>
<br>

##### Handling temporary failures

In the strict quorum approach -> read and write operations could be blocked

Another solution “sloppy quorum”: the system chooses the first W healthy servers for writes and first R healthy servers for reads on the hash ring. Offline servers are ignored.

<br>
<br>
<br>

<img src="design-a-key-value-store/sloppy-quorum.png" width="500" height="500">

<br>
<br>
<br>

##### Handling permanent failures

Solution: an anti-entropy protocol to keep replicas in sync, that involves comparing each piece of data on replicas and updating each replica to the newest version. A Merkle tree is used for inconsistency detection and minimizing the amount of data transferred.

Build a Merkle tree algorithm:

Step 1: Divide key space into buckets (4 in example). A bucket is used as the root level node to maintain a limited depth of the tree.

<br>
<br>
<br>

<img src="design-a-key-value-store/Merkle-tree-step-1.png" width="450" height="150">

<br>
<br>
<br>

Step 2: Once the buckets are created, hash each key in a bucket using a uniform hashing method.

<br>
<br>
<br>

<img src="design-a-key-value-store/Merkle-tree-step-2.png" width="600" height="100">

<br>
<br>
<br>

Step 3: Create a single hash node per bucket.

<br>
<br>
<br>

<img src="design-a-key-value-store/Merkle-tree-step-3.png" width="800" height="250">

<br>
<br>
<br>

Step 4: Build the tree upwards till root by calculating hashes of children.

<br>
<br>
<br>

<img src="design-a-key-value-store/Merkle-tree-step-4.png" width="900" height="500">

<br>
<br>
<br>

By traversing the tree it is possible to find which buckets are not synchronized and synchronize those buckets only.

<br>

##### Handling data center outage

To build a system capable of handling data center outage, it is important to replicate data across multiple data centers.

<br>

####  System architecture diagram

Main features of the architecture are listed as follows:
• Clients communicate with the key-value store through simple APIs: get(key) and put(key, value).
• A coordinator is a node that acts as a proxy between the client and the key-value store. 
• Nodes are distributed on a ring using consistent hashing.
• The system is completely decentralized so adding and moving nodes can be automatic. 
• Data is replicated at multiple nodes.
• There is no single point of failure as every node has the same set of responsibilities.
As the design is decentralized, each node performs many tasks

<br>
<br>
<br>

<img src="design-a-key-value-store/system-architecture-diagram.png" width="900" height="500">

<br>
<br>
<br>

#### Write path

<br>
<br>
<br>

<img src="design-a-key-value-store/write-path.png" width="800" height="400">

<br>
<br>
<br>

#### Read path

<br>
<br>
<br>

<img src="design-a-key-value-store/read-path-1.png" width="800" height="400">

<br>
<br>
<br>

<br>
<br>
<br>

<img src="design-a-key-value-store/read-path-2.png" width="800" height="400">

<br>
<br>
<br>

<table id="tg">
  <tr>
    <th id="tg-baqh">Goal/Problems</th>
    <th id="tg-baqh">Technique</th>
  </tr>
  <tr>
    <td id="tg-hmp3">Ability to store big data</td>
    <td id="tg-hmp3">Use consistent hashing to spread the load across servers</td>
  </tr>
  <tr>
    <td id="tg-hmp3">High availability reads</td>
    <td id="tg-hmp3">Data replication<br>Multi-data center setup</td>
  </tr>
  <tr>
    <td id="tg-hmp3">Highly available writes</td>
    <td id="tg-hmp3">Versioning and conflict resolution with vector clocks</td>
  </tr>
  <tr>
    <td id="tg-hmp3">Dataset partition</td>
    <td id="tg-hmp3">Consistent hashing</td>
  </tr>
  <tr>
    <td id="tg-hmp3">Incremental scalability</td>
    <td id="tg-hmp3">Consistent hashing</td>
  </tr>
  <tr>
    <td id="tg-hmp3">Heterogeneity</td>
    <td id="tg-hmp3">Consistent hashing</td>
  </tr>
  <tr>
    <td id="tg-hmp3">Tunable consistency</td>
    <td id="tg-hmp3">Quorum consensus</td>
  </tr>
  <tr>
    <td id="tg-hmp3">Handling temporary failures</td>
    <td id="tg-hmp3">Sloppy qourum and hinted handoff</td>
  </tr>
  <tr>
    <td id="tg-hmp3">Handling permanent failures</td>
    <td id="tg-hmp3">Merkle tree</td>
  </tr>
  <tr>
    <td id="tg-hmp3">Handling data center outage</td>
    <td id="tg-hmp3">Cross-data center replication</td>
  </tr>
</table>














### Common clarifying questions on designing any system


</div> <!-- main-content-block-->


</div>