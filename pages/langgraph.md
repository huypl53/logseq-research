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
	- **Cognitive architecture**
		-
- ## Reviews
  collapsed:: true
	- ![image.png](../assets/image_1737301254789_0.png)
	- An AI agent can be represented as a graph. The core component of LangChain for this task is **GraphState**. When user sends messages, the messages are **forwarded to LLM**. LLM might need helps to look up something. LLM returns query to Graph, graph, if condition edge passes, route the query to **tool** by **take_action**
	- e.g: Agentic search tools
		- Init an agent consisting a graph of nodes which are **LLM and action**. LLM can make **decision to call the initiated tool or not.** Note that the tools are agent's attributes, LLM only **control the tool invocation**. In the action node, based on the decision of the LLM node, the graph invoke the tool then return the control back to LLM
		- ![image.png](../assets/image_1738597478708_0.png)
- ## Miscs
- ## Practices
  collapsed:: true
	- Create `Agent` for persistance
		- Define custom `AgentState` type
		- create graph
			- graph adds nodes: LLM, action(e.g: call external tools tavily search); then assign handler to nodes
			- graph adds [conditional] edges: LLM -> action, action -> LLM
			- compile graphs
		- save tools
		- configure `thread_id` for identically access state of the graph's thread while invoking `graph.stream`
- [[human-in-the-loop]]
	- https://langchain-ai.github.io/langgraph/concepts/human_in_the_loop
	- `human node` can return message to next node/ return Command for routing
	- Approve or Reject
		- > Depending on the human's approval or rejection, the graph can **proceed with the action** or **take an alternative path**.
		- ```python
		  def human_approval(state: State) -> Command[Literal["some_node", "another_node"]]:
		    if is_approved:
		      return Command(goto="some_node")
		    else:
		      return Command(goto="another_node")
		  ```
	- Review & Edit State
	  collapsed:: true
		- ```python
		  def human_editing(state: State):
		      ...
		      result = interrupt(
		          # Interrupt information to surface to the client.
		          # Can be any JSON serializable value.
		          {
		              "task": "Review the output from the LLM and make any necessary edits.",
		              "llm_generated_summary": state["llm_generated_summary"]
		          }
		      )
		  
		      # Update the state with the edited text
		      return {
		          "llm_generated_summary": result["edited_text"] 
		      }
		  ```
	- Review Tool Calls
		- >A human can review and edit the output from the LLM
		- routing can be done as in previous examples
	- Multi-turn conversation
		- > A **multi-turn conversation architecture** where an **agent and human node** cycle back and forth until the agent decides to hand off the conversation to another agent or another part of the system. **One or more agents** may need to carry out multi-turn conversations with a human