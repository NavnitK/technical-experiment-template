# **Assignment A1: Architectural Drivers and Quality Attributes**
## **Question 1**

**Section 3.1 of \[Wojcik06] describes the three different inputs of the ADD method, which are in fact the three different types of requirements that may influence the software architecture.
Imagine you are the architect of a GPS navigation native mobile app (such as Naver or Waze). Give an example of one requirement of each type that would make sense for this application.**

### **Answer 1**
The three types of requirements are:
1. **Functional Requirement**
   + The system shall allow users to receive real-time traffic updates 

2. **Design Constraint**
   + The system shall use a community-sourced mapping platform, allowing users to contribute and edit map data in real time 

3. **Quality Attribute Requirement**
   + User-reported traffic incidents shall be updated to all nearby users within 5 seconds to support real-time navigation

## **Question 2**
**For each of the design ideas below, indicate what tactic/pattern we discussed in class it corresponds to or is most directly related to.**
For each of the design ideas below, indicate what tactic/pattern we discussed in class it corresponds to or is most directly related to.
1. Configure two Docker containers that **interact frequently** to be deployed to the same node.
2. Reduce the number of http endpoints that are visible in the DMZ.
3. Replace a remote call (over the network) with a local call (in-process or in-vm).
4. Require a token that identifies a valid user in all http requests.
5. Break a 1000-LOC program into five separate programs.
6. Have a synchronized replica of your data store configured so that primary and replica stores
are always in sync.
7. Use multiple threads to process different steps of a batch program in parallel.
8. Make the sensors in a building alarm system periodically send a message to a central
monitoring station.
9. Add memory and increase the bandwidth in the supporting infrastructure.
10. Use SHA-256 to generate a hash of message payloads.
11. Follow PoLP when granting a group of users the rights to execute an operation

### **Answer 2**
1. Performance: Reduce computational overhead
2. Security: Limit exposure
3. Modifiability: Redistribute Responsibilities. Performance: Co-locate communicating resources
4. Security: Brokered authentication pattern
5. Modifiability: Split module
6. Availability: Active redundancy (hot spare)
7. Performance: Manage Resources: Introduce concurrency
8. Availability: Heartbeat
9. Performance scalability: Vertical scalability 
10. Security tactics: Verify message integrity
11. Security tactics: Resist Attacks: Limit access


## **Question 3**
**Among the tactics studied in the reading assignment, which performance tactic can be used to implement the Cancel usability tactic? Justify your answer.**


### **Answer 3**
Main context of **Cancel** usability tactics to support user initiative is as follows:
- Independent command handler which listens to user action
   - We can use **Introduce Concurrency** tactics in which we implement **Spawn threads** strategy to have independent listener
   - We can use **Prioritize events** to provide cancel command to have higher priority over other commands
- Terminate ongoing activity
   - **Spawn thread** will invoke terminate operation
   - We can use **Maintain multiple copies of computations** which will redirect new user request to one of available duplicate service
- Free aquired resources
   - Service that handles cancel command will gracefully terminate 
   - We can use **Periodic cleaning** to cleanup of resources when graceful termination failed
- Inform collaborating components
   - **Spawned thread** will **prioritize** cancel event to multicast to all collaborating components


## **Question 4**
**Rewrite the quality attribute requirement that is part of your answer to question 1 in the form of a six-part quality attribute scenario. Identify each of the six parts.**

### **Answer 4**
**Quality Attribute Requirement**
   + User-reported traffic incidents shall be updated to all nearby users within 5 seconds to support real-time navigation

This Quality attribute requirement focus on:
   + Performance: New traffic incident shall be updated to all nearby users within 5 seconds latency

Performance Quality attribute scenarios:
+ Source: Waze Application Mobile User
+ Stimulus: Traffic incident
+ Artifict: Incident handler service
+ Environment: Normal mode
+ Response: Traffic incident event notification to all nearby users
+ Response measure: Traffic incident event received within 5 seconds by user within 5 km range radius
