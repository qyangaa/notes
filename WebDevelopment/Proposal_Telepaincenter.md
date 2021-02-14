# Project Proposal - Tele Pain Center

Arky Yang

## Objective

Tele Pain Center is a telemedicine website designed as a centralized medical management and communication platform for chronic pain patients. This project is aimed at introducing a patient-centered medical experience, which brings doctors of different related backgrounds to patients, rather than making patients running between multiple doctor's offices. This approach will largely improve the convenience of regular doctor visits of patients, make medical records more accessible and understandable to patients, and transform chronic pain management to a more wholistic process.



## Background



Over 20% of adults in the US suffer from chronic pain, with economic impact of back pain along contributes to a fifth of one country's total health expenditure, representing three-times the total cost of all types of cancer. Regular doctor visits and cost associated with traveling to doctor's office and skipping work for the visits becomes a large burden on chronic pain patients. Seeking referrals to and appointments with specialists and therapists can be a lengthy process, and feedback can be extremely slow. All these problems within the current model of pain management worsen the life quality of chronic pain patients. Therefore, a centralized and responsive platform as described in this project is highly desired.

[Reference: [cdc.gov,](https://www.cdc.gov/nchs/products/databriefs/db390.htm) [C. Phillips (2006)](https://pubmed.ncbi.nlm.nih.gov/20528505/#:~:text=The%20impact%20of%20pain%20on,of%20all%20types%20of%20cancer.)]

## Status Quo

Telemedicine has seen increasing interest these years, especially during the period of COVID-19 pandemic and large-scale lock-down. Platforms for general care includes Teladoc, MDLIVE, and American Well are among the most successful examples. And platforms focused at remote therapy such as Betterhelp and Talkspace demonstrates how a well targeted platform can provide affordable and accessible service to a diverse large population. Remote platforms for chronic pain management is under developed with only early stage proposals and small scale projects ([Reference: '[The Pain Mobile](https://www.sbir.gov/node/820215)']).



## Features

The Tele Pain Center platform should provide the following features:

1. User login and profile management
   ![image-20210211170039212](/home/arkyyang/files/notes/notes/attachments/image-20210211170039212.png)
2. Listing of health care professionals for patients to search and filter through, and make appointments with.
   ![image-20210211170113182](/home/arkyyang/files/notes/notes/attachments/image-20210211170113182.png)
3. Live text messaging between patients and doctors.!
   <img src="/home/arkyyang/files/notes/notes/attachments/image-20210211170140788.png" alt="image-20210211170140788" style="zoom:67%;" />
4. Video meeting room for virtual doctor visits.
5. Patients can view annotated medical records and share them across different health professionals.
6. Patients can send the records and statistics from their external pain tracking app to the doctors.



## Implementation design details

1. Problem: how to manage state of current user
   1. Option 1: Put most functionalities to back-end
      1. Pros: simple front-end logic
      2. Cons: can be slow, requires customizable back-end setup
   2. Option 2: Use Redux to manage front-end data
      1. Pros: more flexible with back-end setup
      2. Cons: complicated front-end logic can also reduce performance
2. Problem: how to approach data management for users, live text messaging, and video chat.
   1. Option 1: Google Firebase as cloud database and use Firebase APIs to retrieve data
      1. Pros: Easy set-up and integration; no need for back-end development, established security features
      2. Cons: May become expensive in the future, lacking flexibility for customization, not well suited for live messaging and video chat applications.
   2. Option 2: MongoDB with Node.js back-end
      1. Pros: Fully customizable; allows us to bring complex logic from front-end to back-end
      2. Cons: Takes time to build; improving security can be challenging
   3. Other available APIs:
      1. Twilio Video
      2. Twilio SMS API
      3. MessageBird
      4. WebRTC
      5. Socket.io
3. Problem: should we separate between doctor's platform and user's platform
   1. Option 1: Combine doctor and patients in the same platform and database
      1. Pros: Enables smooth communication and data sharing, simple setup
   2. Option 2: Separate doctor and patients to different platform and database.
      1. Pros: Can be more scalable, each components will be better focused
4. Problem: how should data be shared (medical records, pain-tracker records) between patients and doctors during chat
   1. Option 1: Through links to pages where the files are displayed.
      1. Pros: simpler layout in chat screen, less delay when messaging
   2. Option 2: Embed in chat messages directly
      1. Pros: no need to redirect to other pages

## Others

1. Testing and evaluation plan
2. Impact Hypothesis
3. Logging
4. Deployment Plan



In site search:

elastic search

yest