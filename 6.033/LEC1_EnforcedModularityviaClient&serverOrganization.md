笔记栏：
*记录（Record）
//带着框架和目的去读，尝试回答问题
4.1 Client/Service Organization
	- modules interact only by sending messages (request | errors | penetrate)
	- *procedure : reusable code fragment that performs a specific task
		entry point : name of the procedure ( address )
		zero or more formal parameters
		local variables ( only exists while procedure is executing )
		allocated ( invoked )
		deallocated ( return )
		return to the caller when finished
	- Soft modularity
		a.  create modularity in a large program by divide it up into named procedures that call one another
		b.  errors can propagate between caller to callee and not just through their specified interfaces
		c.  beside specified interfaces:
			§ infinite loop in a called procedure, callee will never receive control again
			§ caller and callee are in the same address space and use the same stack, either one can accidentally store something in a space allocated to the other
			§ callee returns somewhere else
			§ callee stores the return value somewhere else
			§ callee may have changed the content of the temporary registers
			§ fate sharing : disasters in the callee can have side effects in the caller
				□ callee divides by zero
				□ callee terminates
			§ corrupt shared global variable
		d. usually attained through specifications
		e. nothing forces the interactions among modules to their defined interfaces
		F. *programming languages that doesn’t enforce modularity:
			§ C
			§ C++
			§ processor instructions
		G. *strongly typed languages that program cannot avoid the type system
			§ JAVA
			§ C#
		H. marshaling
			§ client must convert arguments into a canonical representation so that the service can interpret the arguments
			§ notation {a, b}, ­marshaled message that contains the fields a and b
	- enforced modularity / hard boundaries : client and service organization
		A. sweeping simplification ( example of )
		B. errors can propagate only with messages
		C. clients can check for certain errors by just considering the messages
		D. advantages:
			§ client and service don’t rely on shared state other than the messages
			§ client can protect itself even against a service that fails to return because the client can put an upper limit on the time it waits for a response
			§ explicit, well-defined interfaces
		E. trade-off:
			§ between ease of accessing the data that a module needs and ease of error propagation within a module
			§ deciding which data and procedures to group into a coherent unit with the data that they manipulate
			§ that coherent unit then becomes a separate service, and errors are contained within the unit
		F. Multiple Clients and Services
			§ One service can work for multiple clients
			§ One client can use several services
			§ A single module can take on the roles of both client and service
		G. trusted intermediary
			§ a service that functions as the trusted third party among multiple, perhaps mutually suspicious, clients
			§ control shared resources in a careful manner
			§ “thin-client computing”
				□ only the trusted intermediary must run on a powerful computer / distributed system
				□ clients don’t run complex functions
				□ can become a choke point when many clients ask for the service at the same time
			§ downsides:
				□ may be vulnerable to failures
				□ attacks that knock out the service
		H. *untrusted intermediary
			§ used to buffer
			§ deliver messages to multiple recipients
		I. decentralized search services
		J. *Peer-to-peer: Computing without Trusted Intermediaries
			§ decentralized
			§ ex:
				□ the Internet e-mail system
				□ the Internet news bulletin service
				□ Internet service providers to route Internet packets
				□ IBM’s Systems Network Architecture
			§ every computer participating in the application is a peer and is equal in function (but perhaps not in capacity) to any other computer
			§ failure of one node leads at most to a performance degradation rather than to a complete failure
			§ the peers locate resources by querying other peers
			§ accurately and quickly finding information in a large network of peers without a trusted intermediary is a difficult problem
			§ A distributed algorithm is necessary to find a resource
				□ A simple algorithm is to send a query for a song to all neighbor peers
				□ if they don’t have a copy, the peers forward the query to their neighbors and so on
				□ inefficient because it sends a query to every node in the network
				□ avoid flooding : stop forwarding the query after it has been forwarded a number of times
		K. Communication between client and service
			§ remote procedure call (RPC)
				□ a stylized form of client/service interaction in which each request is followed by a response
			§ able to send messages to a recipient that is not on-line and to receive messages from a sender that is not on-line
				□ email using an intermediary
		L. Stubs
			§ A stub is a procedure that hides the marshaling and communication details from the caller and callee
			§ use stubs:
				□ when the client module invokes a remote procedure A, in the same way that it would call any other procedure
				□ A is actually just the name of a stub procedure that runs inside the client module
				□ The stub marshals the arguments of a call into a message, sends the message, and waits for a response
			§ convert more complex objects into an appropriate on-wire representation
		M. RPCs are not Identical to Procedure Calls
			§ semantics:
				□ RPCs can reduce fate sharing between caller and callee by exposing the failures of the callee to the caller so that the caller can recover
				□ RPCs introduce new failures that don’t appear in procedure calls
					® “service failure” signal
					® “no response” failure
			§ RPCs take more time / number of instructions than procedure calls:
				□ invoking a stub
				□ marshaling arguments
				□ sending a request over a network
				□ invoking a service stub
				□ unmarshaling arguments
				□ marshaling the response
				□ receiving the response over the network
				□ unmarshaling the response
			§ some programming language features don’t combine well with RPC ( global variables in separate address spaces/PC )
		N. RPCs handle the no-response case / implementation strategies
			§ At-least-once RPC ( the stub resends the request as many times as necessary until it receives a response from the service )
			§ At-most-once RPC ( the client stub returns an error to the caller, indicating that the service may or may not have processed the request )
				□ banking application : either zero or one transfers take place
			§ Exactly-once RPC ( impossible to guarantee )
				□ both the client and the service stubs keep careful records of each remote procedure call request and response
		O. Communicating through an Intermediary ( can not both parties be available at the same time )
			§ consider the intermediary to be part of an untrusted network and have a separate plan for securing messages
			§ e-mail intermediary:
				□ buffered communication
					® provides the send/receive abstraction
					® hold messages until the recipient comes on-line
			§ design opportunities ( publish/subscribe ):
				□ sender and receiver may make different choices of whether to push or pull messages
				□ apply the design principle decouple modules with indirection
					® having the intermediary, rather than the originator, determine to whom a message is delivered
				□ choice of when and where to duplicate messages
					® mailing list ( sends a copy of the e-mail to each member of the list )
		P. the road ahead : building computer systems
			§ enforcing modularity within a computer
				□ virtualization : create many virtual computers out of one physical computer
			§ performance : slowest service / bottlenecks
			§ networking : may lose, reorder, or duplicate messages
			§ fault tolerance : hardware and software modules fail
				□ detecting failures, containing them, and recovering from them
				□ implement reliable services out of unreliable components
			§ atomicity
				□ concurrent access and failures
			§ consistency
			§ security


	- 4.4 Case Study: The Internet Domain Name System (DNS)

		A. DNS = client/server organization + naming scheme
		B. domain name : user-friendly character-string names
		C. internet address : machine oriented binary identifiers
		D. domain : simply a set of one or more names that have the same hierarchical ancestor
		E. interface to DNS : value <--- DNS_RESOLVE(domain_name)
			§ omits the context argument ( the reference to that one context is built into dns_resolve as a configuration parameter )
		F. DNS implementation ( create & manage tables for bindings )
			i. text editor
			ii. database generator
		G. DNS name
			i. relative path names ( resolve under its default context )
			ii. absolute path names ( distinguished by the presence of a trailing dot )

	- 4.4.1 Name Resolution in DNS

	3 ways
		A. The telephone book model
		B. The central directory service model
			i. performance bottleneck
			ii. potential source of massive failure
		C. The distributed directory service model
			i. resolving some subset of domain names
			ii. a protocol for finding a server that can resolve any particular name



  线索栏：
      *框架（Survey）
      	1. Chapter 1 _Systems
      	- 1.1 Systems and complexity
      	- 1.2 Sources of complexity
      	- 1.3 Coping with complexity 1
      	- 1.4 Computer systems are the same but different
      	- 1.5 Coping with complexity 2
      	2. Chapter 4_Enforcing Modularity with Clients and Services
      	- 4.1 Client/service organization
      	- 4.2 Communication between client and service
      	- 4.3 summary and the road ahead
      	- 4.4 case study: the internet domain name system (dns)
      	- 4.5 case study: the network file system(nfs)


*问题（Question）
      • What is this chapter about?
      • What question is this chapter trying to answer?
      • How does this information help me?

      	1. Why systems so complex?
      	2. The form of complexity
      	3. How to deal with complexity?
      	4. What is client and service
      	5. How client and service communicate?
      	6. Why client and service can enforce the modularity?



*简化（Reduce）
*背诵（Recite）
      //就像要跟别人讲述（主观点、问题的答案）
      	1. Client & service module communicate with message （via wire）
      	- Limit interactions
      	- Error and attack ( Check messages )
      	2. Soft modularity
      	- Caller and callee affect each other
      	3. Client and service communicate through RPC
      	- Stubs
      	- Marshaling
      	- Inetrmediary
      	- Different from procedure call



总结栏：
        *思考（Reflet）
        *复习（Review)
        //整体观点，回答了全部的问题没有
        	- How to deal with complexity
        		○ Modularity
        			§ Modules interact using reference (by name)
        			§ Systems manipulate/pass objects either by value or by name
        			§ Benefits of using names
        				□ - Retrieval
        				□ - Sharing
        				□ - User-friendliness
        				□ - Addressing
        				□ - Hiding (+access control)
        				□ - Indirection
        			§ naming schemes
        				□ Namespace
        				□ set of values
        				□ look-up algorithm
        		○ Abstraction
        			§ Memory
        			§ Interpreters
        			§ Communication links
        		○ Layering
        		○ Hierarchy
        		○ Iteration
        		○ Keep it simple
