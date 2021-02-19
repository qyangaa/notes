### Design Twitter HashTag - trending

1. Senarios:
   1. Clarifications:
      1. Key purpose: system provide a list of topics that a popular at current moment
      2. Trending means: use a word/ phrase to represent a topic
      3. Definition of popularity: a term used in high volume (post volume) in recent tweets (time) comparing to past volume (to prevent stop words), [need to decide the scale of volume and time]
      4. Example: definition of trending topic, tending index - a term with top K recent(hours) to past (days) term volume ratio & recent volume > certain threshold
      5. DAU: 300M
      6. Requirements: instant read, large volume of data source
      7. Additional features: personalization
   2. Enumerate Features
      1. Collect post data
      2. Compute trending index
      3. Send topics with top trending indexes to user
      4. Send posts under these topics to user
   3. Select core features
      1. Collect post data
      2. Compute trending index
      3. Send topics with top trending indexes to user
      4. Remove: Send posts under these topics to user, this can be achieved by separate search / filter query
   4. Analyze:
      1. DAU => QPS: 60 reads, 1 write everyday:
         1. Reads: 200M * 60 / 86400  = 2E8 * 6E1 / 8E4 = 1E5
         2. Writes: 1E4
         3. Peak Reads: 3E5
2. Service
   1. trendingIndexService: compute index, backend with strong computational power
   2. trendingDisplayService: send trending topics to clients
3. Storage:
   1. Apart from storage of tweets and users
   2. Real time: if trending topics are constantly updating, pushing might require a lot of writes, instead use pull and store trending topics directly in Cache for fast read.
   3. In computation, the topics are stored as priority queue sorted by trending index.
   4. In cache, the topics are stored as simple array of topic name
4. Optimization
   1. Trade-offs: distributed computation are faster but merge can be slow across computers. 
   2. How to deal with celebrities: put more weights or less weights?



#### Design a database system: add tags, delete tages..



#### Designing a micro-service within the *Atlassian architecture.*

*Microservice should be able to calculate time based metrics.*







​    

### Social Media - general/ aws

![image-20210218133921394](/home/arkyyang/files/notes/notes/attachments/image-20210218133921394.png)

### Social media - Tinder microservice



![image-20210217094124379](/home/arkyyang/files/notes/notes/attachments/image-20210217094124379.png)

### Social media - Instagram NewsFeed

![image-20210217205220762](/home/arkyyang/files/notes/notes/attachments/image-20210217205220762.png)

### Ranking - Top 10 Songs

![image-20210217124324665](/home/arkyyang/files/notes/notes/attachments/image-20210217124324665.png)

### Ranking - Amazon best seller

![image-20210217153412398](/home/arkyyang/files/notes/notes/attachments/image-20210217153412398.png)

### Messaging - Whatsapp

![image-20210218104741701](/home/arkyyang/files/notes/notes/attachments/image-20210218104741701.png)

### Booking - Airbnb

![image-20210217150903874](/home/arkyyang/files/notes/notes/attachments/image-20210217150903874.png)



### Streaming - Spotify microservice

![image-20210217152112561](/home/arkyyang/files/notes/notes/attachments/image-20210217152112561.png)



### Booking - Ticket Booking microservice

![image-20210217152756556](/home/arkyyang/files/notes/notes/attachments/image-20210217152756556.png)

SAGAS:

![image-20210217152839517](/home/arkyyang/files/notes/notes/attachments/image-20210217152839517.png)



### E-commerce - QR code payment

![image-20210217165809786](/home/arkyyang/files/notes/notes/attachments/image-20210217165809786.png)

### Tiny Url

![image-20210217170327042](/home/arkyyang/files/notes/notes/attachments/image-20210217170327042.png)

### Job Scheduler

![image-20210218124543493](/home/arkyyang/files/notes/notes/attachments/image-20210218124543493.png)



### Distributed Message Queue

![image-20210217095903767](/home/arkyyang/files/notes/notes/attachments/image-20210217095903767.png)

![image-20210217095918252](/home/arkyyang/files/notes/notes/attachments/image-20210217095918252.png)

+ Front end service:
  + Request Validation: make sure required parameters are present, and data size/ type are acceptable
  + TLS termination: decrypt request, handled by TLS HTTP proxy as a process on the same host
  + Authentication/ authorization: validating/ allow access
  + Server-side encryption
  + Caching: stores metadata about most actively used queues and user identity information,
  + Rate limiting: throttling, limit requests/ unit time (leaky bucket algorithm)
  + Request dispatching: circuit breaker pattern prevents repeatedly trying to execute operation that's likely to fail
  + User data collection

![image-20210217101130012](/home/arkyyang/files/notes/notes/attachments/image-20210217101130012.png)

+ Backend service
  + Store messages: RAM + local disk
  + Replicate data: within group of hosts
  + Communication between frontend and backend: use matadata service

![image-20210217101306046](/home/arkyyang/files/notes/notes/attachments/image-20210217101306046.png)

![image-20210217101346303](/home/arkyyang/files/notes/notes/attachments/image-20210217101346303.png)



### Publish - subscribe service

![image-20210217101720318](/home/arkyyang/files/notes/notes/attachments/image-20210217101720318.png)

![image-20210217102458516](/home/arkyyang/files/notes/notes/attachments/image-20210217102458516.png)

+ A trivial semaphore is a plain variable that is changed (for example, incremented or decremented, or toggled) depending on programmer-defined conditions.

### Distributed transaction with SAGAS

![image-20210217104702260](/home/arkyyang/files/notes/notes/attachments/image-20210217104702260.png)

![image-20210217104717659](/home/arkyyang/files/notes/notes/attachments/image-20210217104717659.png)



### Message Queue in real word

![image-20210217121120886](/home/arkyyang/files/notes/notes/attachments/image-20210217121120886.png)

![image-20210217121129706](/home/arkyyang/files/notes/notes/attachments/image-20210217121129706.png)

### Distributed logging

![image-20210217121428239](/home/arkyyang/files/notes/notes/attachments/image-20210217121428239.png)



### BitBucket

![img](https://confluence.atlassian.com/enterprise/files/668468332/935393801/3/1566884320777/BitbucketDataCenter-4-node-architecture_diagram.png)

### AWS

![image-20210218125005136](/home/arkyyang/files/notes/notes/attachments/image-20210218125005136.png)