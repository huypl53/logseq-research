## Keywords
	- cyclic graphs
	- persistent
	- human-in-the-loop
- ## Components
  collapsed:: true
	- Nodes: agents or function
	- edges: connect nodes
	- Conditional edges: decisions
- ## Concepts
	- Data/state: be tracked over time
		- Agent state is **accessible** to all parts of the graph
		- Be **local** to the graph
		- Can be stored in a persistence layer: can **resume/update** that state anytime
- ## Reviews
  collapsed:: true
	- ![image.png](../assets/image_1737301254789_0.png)
	- An AI agent can be represented as a graph. The core component of LangChain for this task is **GraphState**. When user sends messages, the messages are **forwarded to LLM**. LLM might need helps to look up something. LLM returns query to Graph, graph, if condition edge passes, route the query to **tool** by **take_action**
	- e.g: Agentic search tools
		- Init an agent consisting a graph of nodes which are **LLM and action**. LLM can make **decision to call the initiated tool or not.** Note that the tools are agent's attributes, LLM only **control the tool invocation**. In the action node, based on the decision of the LLM node, the graph invoke the tool then return the control back to LLM
		- ![image.png](../assets/image_1738597478708_0.png)
- ## Miscs
  collapsed:: true
	- ### Persistance
		- Check pointer
			- >is the **state** after and between every node. Normally when we refresh the code, we lose the context history. Now we can save the **graph state into a database** like sql, postgre, redis
			-
	- ### Streaming
		- Stream the messages that AI agent receives and the observation messages which are results
		- Token streaming: "So for each token of the LLM call we might want to stream the output". The streaming tokens make the output look like human writting
- ## Practices
	- Create `Agent` for persistance
		- Define custom `AgentState` type
		- create graph
			- graph adds nodes: LLM, action(e.g: call external tools tavily search); then assign handler to nodes
			- graph adds [conditional] edges: LLM -> action, action -> LLM
			- compile graphs
		- save tools
		- configure `thread_id` for identically access state of the graph's thread while invoking `graph.stream`