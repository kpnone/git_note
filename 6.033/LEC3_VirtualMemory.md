线索栏：
    *框架（Survey）
    Reading: Book sections 5.1, 5.3, and 5.4
    • Chapter 5 . Enforcing Modularity with Virtualization
    • 5.1 Client/Server Organization within a Computer Using Virtualization
    • 5.1.1 Abstractions for Virtualizing Computers
    • 5.1.1.1 Threads
    • 5.1.1.2 Virtual Memory
    • 5.1.1.3 Bounded Buffer
    • 5.1.1.4 Operating System Interface
    • 5.1.2 Emulation and Virtual Machines
    • 5.1.3 Roadmap: Step-by-Step Virtualization
    • 5.2 Virtual Links Using send, receive, and a Bounded Buffer
    • 5.3 Enforcing Modularity in Memory
    • 5.4 Virtualizing Memory
    • 5.5 Virtualizing Processors Using Threads
    • 5.6 Thread Primitives for Sequence Coordination
    • 5.7 Case Study: Evolution of Enforced Modularity in the Intel x86
    • 5.8 Application: Enforcing Modularity Using Virtual Machines

Quick review :
1. Client & service module communicate with message （via wire）
- Limit interactions
- Error and attack ( Check messages )
2. Soft modularity
- Caller and callee affect each other
3. Client and service communicate through RPC
• Stubs
• Marshaling
• Inetrmediary
• Different from procedure call
• ------------------------------------------------------------------------------------
• How to deal with complexity
	○ Modularity
		§ Modules interact using reference (by name)
		§ Systems manipulate/pass objects either by value or by name
		§ Enforcing modularity with virtualization
			□ Goal : preserve an existing interface
			□ virtualized versions of the main abstractions
			□ Client/server organization within a computer using  virtualization
				□ Virtual link : SEND, RECEIVE, with a bounded buffer
				□ Virtual memory : virtual adress space
				□ Virtual processor : thread
	○ Abstraction (modularity + )
		§ less propagation of effects from one module to another
		§ interface (specifications, no details)
		§ Robutness
		§ 3 fundamnetal abstractions
			□ Memory
			□ Interpreters / processor
			□ Communication links
	○ Layering
	○ Hierarchy
	○ Names ( make connections )
		§ Benefits of using names
			§ - Retrieval
			§ - Sharing
			§ - User-friendliness
			§ - Addressing
			§ - Hiding (+access control)
			§ - Indirection
		§ naming schemes
			§ Namespace
			§ set of values
			§ look-up algorithm
	○ Iteration
	○ Keep it simple


*问题（Question）
• What is this chapter about?
• What question is this chapter trying to answer?
• How does this information help me?

	1. What is virtual memory?
	2. How does it works



*简化（Reduce）
*背诵（Recite）
//就像要跟别人讲述（主观点、问题的答案）
	1.




  笔记栏：
  *记录（Record）
  //带着框架和目的去读，尝试回答问题
  - 5.1  Client/Server Organization within a Computer Using Virtualization


  - creates many virtual objects by multiplexing one physical instance
  - provide one large virtual object by aggregating many physical instances
  - implement a virtual object from a different kind of physical object using emulation

  	1. Client/service organization within a computer (5 modules):
  		a. Text editor
  		b. E-mail reader
  		c. Keyboard manager
  		d. Window service
  		e. File service
  	- Goal : module failures don't propagate from one to another.
  	- Char : each module with its own virtual computer

  - 5.1.1 Abstractions for Virtualizing Computers
  - 5.1.1.1 Threads / thread of executions
  - An abstraction that encapsulates the eexecution state of an active computation
  - State :  variables internal to the interpreter
  - Program counter
  - Environment
  	- Stack
  	- Heap
  	- Other current objects
