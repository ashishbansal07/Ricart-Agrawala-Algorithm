# SSZG526 : ASSIGNMENT (EC-1)
@Author: Ashish Bansal (2021MT12174)
# Ricart Agrawala Algorithm for Mutual Exclusion

## Overview of the Algorithm

This Algorithm enables to achieve Mutual Exclusion among Nodes which operate in a distributed environment with no shared memory. The entire communication amongst the Nodes is based on Network Messages.

Whenever a Node wants to enter the Critical Section, it sends a REQUEST message to all the other Nodes. On receiving the REPLY, the Node is entitled to enter the Critical Section.

The steps involved are briefed below:

* Node which wants to enter the Critical Section sends REQUEST to all the other Nodes. The REQUEST consists of its Node ID and a Request Number.
* The Other Nodes, on receiving decides whether it shall allow the Request Sender Node to enter the Critical Section or not based on information contained in the REQUEST i.e Request Number and the Node ID.
* There is no conflict of interest when the receiver Node does not want to enter the Critical Section. When the receiver itself wants to enter Critical Section priority based decision is taken using the Request Number and the Node ID information. Thus the request shall be deferred or replied immediately based on the decision. Deferred requests are processed after the Node exits from the Critical Section. In case of conflict of interest the decision is based on the flowchart presented in Figure-1.
* The Request Sender waits till it receives REPLY from all the other Nodes. It is not aware of the deferment of its request, if any. In case of deferment, the REPLY message is only sent when the conflict of interest is resolved. Once the REPLY messages are received from all the other Nodes, the Request Sender shall enter the Critical Section.

The Algorithm is deadlock free and starvation free. Also the same is achieved with optimum number of messages which is 2(N-1) for each Critical Section entry. The important assumption made here is the faultless Network communication.

# Platform Tested on: Macbook (MacOS 11.2.3 ), JDK 1.8
## Implementation Aspects

1. There are N Nodes in the system, numbered from zero to N-1. Each node executes on a different machine. The Nodes communicate with each other using TCP socket communication.

2. Each Node is designed with a Server and Client Component. On start up, the Node starts listening using its Server component to receive messages from other Nodes. In order to receive messages in concurrent manner, Server components are implemented in N-1 threads.

3. When a Node wants to enter its Critical Section it uses the Client component to connect to each of the other Nodes and sends the REQUEST message. Once the REPLY is received from all the other Nodes, it does the Critical Section processing. On exiting from the Critical Section the deferred requests, if any are processed.

For the sake of demonstration each Node requests for entering the Critical Section for a predefined number of times. Once a Node enters the Critical Section it exists from there after a predefined time duration.

## Assumptions during the Implementation

* The nodes do not fail

* The number of nodes are known and defined before-hand

* Network communication is faultless

* Communication channel are assumed to follow FIFO order

## Running the Project

* Unzip the project or clone project from `git@github.com:curiouslad/Ricart-Agrawala-Algorithm.git`

* Open terminal and enter project directory using `cd` command

* Define the number of nodes `N` in the `config.txt` file along with their IP and port addresses using `vi config.txt` command.

* Compile the program by typing `javac RA_Main.java'

* Set up nodes in `N` different terminals, type `java RA_Main <node_number> <number_of_times_to_enter_critical_section>` in each.

* Example
```
java RA_Main 0 2
```
```
java RA_Main 1 2
```
```
java RA_Main 2 2
```


# Evaluation of the output: 
1. Observe the terminal/console prints showing REQUEST and REPLY messages based on the algorithm's condition to enter-exit CS. 
2. Observe the terminal/console prints showing request getting deferred when the receiving site in CS or itself requesting with lesser timestamp or same timestamp but the id is less than 		   the requesting site id.
3. Observe the terminal/console prints for request deferred array (RD<site_id>) of each site whenver a request is replied or deferred.
4. Observe the terminal/console prints for the respective site entering-exiting CS along with the timestamps.
5. Observe the terminal/console prints showing how many times a respective site entered CS.


## Concluding Remarks

The Algorithm provides efficient deadlock free and starvation free mutual exclusion processing among N nodes in distributed environment. In this implementation I have demonstrated the same. Handling message losses during Network failure and dynamically adding or removing Nodes are some of the extensions which are to be addressed for real life implementations.