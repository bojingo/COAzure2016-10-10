# COAzure 2016-10-10 Meeting
### Architecture Design Case Study

The goal of this meeting’s activity will be to learn more about the various Azure services, and how to use them together to architect a solution in Azure.  Work as teams to research various Azure services and decide how to use those to propose an architecture for the system described in the below case study and application/system requirements.

Each team will have a little over an hour to come up with its solution architecture, drawing them up on large sheets of paper.  Experienced Azure architects are available to answer questions and provide guidance.  Research and discuss the many available services with your team.  We’ll end the meeting with each team sharing its solution with the whole group.

## Current architecture:
Our client provides a point-of-sale (POS) solution to its customers, including branded credit cards and a reward points program.  The POS devices currently send each transaction’s data to an on-premise local server at each customer site (sales location), which accumulates the transaction data.  The on-premise server executes a nightly job to upload transactions to a central server, which stores data for all customers.  The central server updates everyone’s account balances, reward points, etc., and then sends those updates back to the on-premise servers.  The POS devices pull the updated information from the on-premise servers the next morning.  

The POS devices run Windows and have a local SQL Server Express database.  The on-premise servers and the central servers run Windows Server and SQL Server.  

In addition, there is a central website that allows customer end-users to view their account balances and reward points, pay their credit card bills, etc.

## Problem Statement

The current solution presents a number of problems for our client.

1. They would rather not have to mess with installing, configuring and maintaining the on-premise servers; they would like to cut out the “middle man.”  
2. Nightly data synchronization is too infrequent, but the current architecture cannot handle more frequent updates.  Because of the delayed updates, our client has issues with its customers’ account balances and reward points being out of date if they make multiple transactions in a single day.  In other words, a customer might purchase something at one area of a store and max out their account balance, but still be able to purchase something at another area in the store because the second POS device has an old account balance.
3. The client would like to move to the cloud so they can avoid having to maintain their own central servers.
4. Because of issues with scaling the current architecture, our client has been unable to sell their solution to larger sales locations.  The new solution must scale by orders of magnitude to support much larger customers.
5. The on-premise servers record some data that is not currently synchronized to the central server.

## Solution Requirements
1. Replace the current solution with a cloud-based architecture.
2. POS devices should post transactions in near real-time (at least once a minute).
3. The process of updating account balances and reward points should be near real-time as well.  I.e. the POS devices should always have customer data that is no more than 1-minute old.
4. There needs to be a process for migrating customers off of the on-premise servers to the new solution.  Our client would like to move that data over a period of time, perhaps a month or two, before they update the POS devices with the new solution.  
5. They need some proof/verification that the new solution will scale to meet their needs.  Their current solution processes about 1.25 million transactions per hour.  They would like the new solution to immediately handle 10X that volume, with room to scale well beyond that.

## Resources

![Azure Periodic Table](http://i0.wp.com/photos3.meetupstatic.com/photos/event/4/e/4/0/600_454820032.jpeg?w=640)
