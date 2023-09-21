# Introduction

Threat Modeling is the process of identifying risks to a system. This includes defining potential threats, issues that could arise from these threats, and mitigation strategies.

Starting the threat modeling process early in the software development lifecycle can save money and time in the long run by mitigating issues and modifying architecture before any software is written. Threat modeling is also a great way to have multiple team members collaborate to understand a system which is important to building a strong DevSecOps culture.

There are four steps in our Threat Modeling methodology:

* Define the system
* Enumerate the threats
* Evaluate the threats
* Reassess the model


You can fill out the spreadsheet to the right as part of the threat modeling process.

# Step 1: Define the System

The first step is to define the system your team wants to create.

When defining the system, it is important to:

* Define the boundaries of the system. Which components are and are not part of this system?
* Understand the users of the system. What are the types of users and their different levels of access to the system?
* Understand the data stored on the system. Which data is sensitive and has special access restrictions?
* Understand how the system will be used.

For this lesson, we will define an fictitious application to use for building an example threat model.

# Ecommerce Application

The application is a simple ecommerce website that is hosted in Amazon Web Services (AWS) and accepts credit card payments. The system includes the virtual machine on AWS, the networking components, the functionality of authentication and payment processing. It will have shoppers, admins to administer on AWS, and admins to add products, remove products, and view analytics. The system will store credit card information for follow on purchases as well as name and address information. It will also store inventory and product information. Lastly, authentication information will need to be stored for user and admin accounts. There will be one web interface to the system.

# Step 2: Enumerate the Threats

After defining the system, you should have a good understanding of all the components. The next step is to brainstorm threats that could potentially happen and how they could happen. To prepare for this brainstorming session, it is important to have a group of people who understand the system and to set a time limit for the whole process (1-2 hours).

Many frameworks have been created to aid in enumerating threats. In this lesson we will use the most widely used framework, STRIDE. STRIDE is a mnemonic that lists different threat categories:

* Spoofing
* Tampering
* Repudiation
* Information Disclosure
* Denial of Service
* Elevation of Privileges.

As we go through each threat category with its definition and examples, you will add to the spreadsheet on the right as part of the process. You can fill out the spreadsheet here or you can click `Download` in the spreadsheet application and fill it out locally on your computer. Either way, make sure you save a copy of the spreadsheet and download it before finishing the lesson to keep as an example.

# Spoofing

Spoofing is pretending to be someone or something else.

#### Security Property: Authentication

Examples:

* Logging into a system with someone else’s credentials
* Changing the IP address of a request to bypass network ACLs
* Calling IT and claiming you are an employee when you are not

In our ecommerce application, one example of spoofing would be a user finding a way to purchase something as another user and using their payment information. Fill out another example of spoofing in the spreadsheet.

# Tampering

Tampering means modifying a piece of data through unauthorized channels.

#### Security Property: Integrity

Examples:

Changing the amount of money in your account
Manipulating packets in network traffic
In the ecommerce application, one example would be changing the price of a product. Fill out another example of tampering in the spreadsheet.

# Repudiation

Repudiation is being able to claim that you did not do something.

#### Security Property: Non-repudiation

Example:

* Confirming that you did not send an email that your coworker received from your email address
  
In the ecommerce application, one example would be making a purchase and sending it to someone without having an account. Please fill out another example of Repudiation in the spreadsheet.

# Information Disclosure

Definition: Exposing information to an entity that is not authorized to view it

#### Security Property: Confidentiality

Example:

* Storing sensitive information in the “hidden” fields of html
  
In the ecommerce application, one example would be viewing all of the users of the system. Please fill out another example of Information Disclosure in the spreadsheet.

# Denial of Service

Denial of Service is using more resources on a service resulting in the in the unavailability of the service.

#### Security Property: Availability

Example:

* Flooding the network traffic with bogus packets

In the ecommerce application, one example would be stopping all users from making a purchase by flooding the credit card processor with too many requests. Please fill out another example of Denial of Service in the spreadsheet.

# Elevation of Privileges

Elevation of Privileges gives someone or something the ability to do something they should not be allowed to do.

#### Security Property: Authorization

Example:

* Normal running administrative functions
  
In the ecommerce application, one example would be a user having the ability to change the quantity of an item through an API call. Please fill out another example of Elevation of Privileges in the spreadsheet.

Step 3: Evaluate the Threats
After enumerating the possible threats, the next step is to prioritize them. This step is subjective based on the specific organization and system. While your team works through each threat one at a time, evaluate each threat based on their risk.

To help calculate the risk, we will use another useful mnemonic device called DREAD. Answer each question in the DREAD mnemonic with a rating of 1-5, assuming that the threat has occurred.

* Damage: How much damage will be caused?
* Reproducibility: How easy is the threat to reproduce?
* Exploitability: What resources are needed to exploit this threat?
* Affected Users: How many users will be affected?
* Discoverability: How easily can this threat be discovered again?
Looking at our spoofing example: "a user finding a way to purchase something as another user and using their payment information", we will walk through the scoring:

* D – 5. Credit card companies would refund the money and cancel the transactions and if merchandise has already been sent it could cost the company a lot of money.
* R – 4. If this is a vulnerability with one of the APIs it could be easy to reproduce.
* E – 5. An API vulnerability could be very simple to exploit using a tool such as BurpSuite
* A – 5. This would affect every user of the system.
* D – 4. This may take some work to find, but would not be too complex.
  
The total would be 23.

Go through this exercise in the spreadsheet and fill out a score in each of the columns corresponding to DREAD.

Prioritizing and Risk Strategy
After entering in all the scores for each threat, you will see the sum of each threat listed in the TOTAL column of the spreadsheet. Based on these scores, we can rank the threats and give a priority to them.

After prioritizing each threat, we can address them based on risk and decide how to best manage them. For each risk, your team can choose to do one of the four following actions:

Mitigate the threat - Reduce the likelihood of the threat (e.g. add an application firewall).
Eliminate the threat - Completely remove the likelihood that the threat can occur (e.g. do not allow account deletions).
Transfer the threat - Transfer the risk to a third-party (e.g. use payment processor such as Stripe for credit card transactions).
Accept the risk - Do not act on this threat as you are willing to accept the consequences.
For our example with a risk score of 23 out of 25, ideally we would want to eliminate the treat, but since this could be very difficult in our use case we would try to mitigate the threat. One way to do this would be to use a payment processor that can do very well at detecting and stopping fraud. Another thing we could do is use penetration testing to try to find any potential vulnerabilities with this type of spoofing.

In the final two columns of the spreadsheet, you can choose one of the above actions and define a rationale for each, just as we did with this example.

Step 4: Reassess the Threat Model
When your team reaches this step, you have a tangible document with a definition of your system, an enumeration of possible threats to your system, and a ranking and risk strategy for each threat. Our next step is to take a step back and ask if this model really makes sense.

Some important questions to ask the team are:

* Has the model really covered everything? Is there anything missing?
* Does everybody on the team agree on the inputs to the threat model?
* Does the risk management align with the prioritization of the protection of our system?
* Are the attacks an actual threat? Will they actually occur?
If these questions need to be readdressed, then the team should go back and modify the threat model as needed. This step is a "gut check" and you can move forward when you feel comfortable with your modeling.

Now your team is finally finished and can relax because all threats to your system are taken care of. Just kidding!

The threat landscape is constantly changing. Going forward, it is important to stay aware of the changing landscape of attacks and how that can affect your system. Some important considerations to ask your team are:

* How are you going to monitor the threat landscape on an ongoing basis?
* When the threat landscape changes, who will help reassess and modify the threat model?

# Conclusion

The goal of threat modeling is to understand the risks before developing a system. If the team understands the importance behind the threats, then the team will understand the areas of focus during development.

Every system and application are unique. They deal with different threats based on data collected, usage by industry, etc. Therefore, there is not a one-size-fits-all methodology to defining all threats.

Threat Modeling is a good way to start the conversation and keep the correct focus during development, but it is also important to continually think about new, emerging threats and strategies to overcome these threats.

(Remember to save the spreadsheet before completing the lesson)

## Attachments

[Threat Model.xlsx](Threat%20Model.xlsx)