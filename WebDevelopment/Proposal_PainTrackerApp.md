# Project Proposal - Pain Tracker App

Arky Yang

## Objective

Pain management is an experimental process of trial and error for many people, due to the diversity conditions, and complex combinations of causes from everyday life. Pain Tracker will be a cross-platform mobile and web application for chronic pain patients to do daily tracking multiple aspects of everyday life related to pain, and to establish a community for chronic pain management. This app will be minimalistic to provide the best pain-logging experience, while giving necessary feedback to patient's status and progress. Pain-managing methods discovered by users and from tracked data will be sent back to the community as tips on pain-management.



## Background

Over 20% of adults in the US suffer from chronic pain. Although a large collection of pain medicines have been developed and prescribed for daily usage, pain-free state is usually not attainable, and pain treatment and management strongly relies on lifestyle modification and discovering personalized approaches to enhance functional abilities and quality of life [ [Practice Guidelines for Chronic Pain Management](https://www.aafp.org/afp/topicModules/viewTopicModule.htm?topicModuleId=61)]. 

Many patients have experienced improvements through physical activities such as yoga and aerobic exercises [[Physiopedia](https://www.physio-pedia.com/Exercise_and_Activity_in_Pain_Management)], and psychological conditions such as depression and anxiety has also been proven strongly connected to chronic pain [[J Sheng (2017)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5494581/)]. Keeping track of pain condition, activity level, and mood can help patients discover patterns, correlations, and maybe pain-management schemes. Collective knowledge of these patterns and correlations will benefit the chronic pain community as a whole.



## Status Quo

Several pain tracker app exist on the market. With most successful examples include Pain Scale,  Chronic Pain Tracker, GeoPain etc. And some creative and artistic applications such as Year in Pixels. Possibility to share logged data with health professionals is a common feature of the successful applications. Balancing the effort spent in logging and amount of feedback is a common challenge among the applications, and experiences with logging can usually be less pleasurable or more troublesome than desired.  Therefore, we want to focus on minimizing the effort it takes to do daily logging, while provide enough feedback and recommendations.



## Features

To remove redundancy seen in most pain-tracking application, we distilled our features to three simple parts:

1. Tracking: track pain, mood, and exercises. Users can choose to log additional information such as special discoveries, but nothing is required.

1. Feedback and recommendations: a simple screen that provides statistics with graphics and tips shared by the community.
2. A simple share button that generates link that user can send the statistics to health professionals.

![Pain Tracker app](/home/arkyyang/files/notes/notes/attachments/Pain Tracker app.JPG)

## Implementation design details

1. Selection of frameworks
   1. Cross platform mobile: React Native
   2. Do we need to develop web version?
2. Problem: how to minimize number of clicks needed for logging while recording most important information
   1. Keep selection simple and intuitive, leave less common selections to input boxes
      1. example: (pain gets better/ worse/ same) vs. (pain level 1,2,3,4,5..)
   2. Memorize past selections for easier logging under little changes in conditions
   3. Single direction flow of user experience: no redundant navigation.
   4. Smart completion of input box/ generation of buttons 
3. Problem: how to further improve experience in logging
   1. Make UI more appealing and relaxing. Mediation apps are especially good examples: https://ictsolved.com/beautiful-meditation-app-ui-concepts/
   2.  Do interviews and studies on people's behavior and preferences when using such application
4. Problem: how to provide feed back
   1. Option 1: through back-end generated graphs 
      1. Pros: simplistic front-end design and less cumbersome user experience
   2. Option 2: through raw data and plotting in front-end
      1. Pros: more flexibility for user when viewing data
   3. Option 3: provides options
5. Problem: how to manage community sharing
   1. Option 1:  single push of tips after every daily log
   2. Option 2: create a blog-style community that users can explore within

## Others

1. Testing and evaluation plan
2. Impact Hypothesis
3. Logging
4. Deployment Plan