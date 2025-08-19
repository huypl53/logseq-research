- Miscs
  collapsed:: true
	- collaboration mechanisms
		- actors
		- types (cooperation, competition, coopetition), structures, strategies, and coordination protocols
	- MASs (Multi-Agent System)
		- domains:
			- 5G/6G networks
			- natural network generation
		- Artificial collective intelligence
	- Elements
		- Role-playing
		- Focus
			- **long-context, lot of tools, unclear target** can result in hallunication
			- -> **break big task** into **smaller ones** and **assign them for expertise agents**
		- Tools
		- Cooperation
		- Guardrails
		- Memory
- Keywords
	- [[human-in-the-loop]]
- ## Frameworks
  collapsed:: true
	- [[langgraph]]
	- [[swarm]]
	- [[crewAI]]
	- [[openAI]]
	- [[autogen]]
	  collapsed:: true
- ## Characteristics
  collapsed:: true
	- Deterministic
		- the output or behavior is entirely **predictable based on the input** and **the rules** governing the system
		- A deterministic AI agent will always produce the same output when given the same input, with **no randomness or variability involved**.
		- Usecases:
			- consistency and predictability are critical
			- rule-based systems
	- ### Control vs freedom
		- When designing AI applications, you face a fundamental trade-off between **control** and **freedom**:
			- **Freedom** gives your LLM more room to be creative and tackle unexpected problems. i.e: [[smolagent]]
			- **Control** allows you to ensure predictable behavior and maintain guardrails. i.e: [[langgraph]]
-
- ## Workflows
  collapsed:: true
	- Prompt chaining
		- Prompt chaining **decomposes a task** into a sequence of steps
		- Each LLM call processes the output of the previous
		- **Optional checks** on intermediate steps to ensure the process is still on track
		- Usecases:
			- generate outline of document -> check if the outline meets certain criteria -> write the document based on the outline
	- Routing
		- **Classifies an input and directs it** to specialized followup task
		- Usecases:
			- Routing easy/common questions to smaller models like Claude 3.5 Haiku 
			  and hard/unusual questions to more capable models like Claude 3.5 Sonnet
			   to optimize cost and speed.
	- Parallelization
		- LLMs can **work simultaneously** on a task and have their **outputs aggregated** programmatically.
		- Variations:
			- **Sectioning**: Breaking a task into independent subtasks run in parallel.
			- **Voting:** Running the same task multiple times to get diverse outputs.
	- Orchestrator-workers
		- central LLM **dynamically breaks down tasks**, **delegates them to worker LLMs**, and **synthesizes** their results.
		- Usecases:
			- you can't predict the **subtasks needed**, which is determined by the orchestrator based on the specific input
			- Coding products that make complex changes to multiple files each time.
	- Evaluator-optimizer
		- one LLM call generates a response while another provides evaluation and feedback in a loop.
		- Usecases:
			- have clear evaluation criteria, and when iterative refinement provides measurable value
- ## Patterns
	- **Reflection**:
	  collapsed:: true
		- a multi-agent design pattern where a **critic agent evaluates the responses of a primary agent**
- ## MAS design tips
	- **Clear Agent Roles**: Each agent will have a **distinct, non-overlapping responsibility** to avoid redundancy and ensure smooth handoffs.
	- **Improved Workflow**: A more **linear and adaptive workflow** to handle various user intents (e.g., product inquiries, order placement, personalized recommendations, and sentiment handling).
	- ## Strategy
	  collapsed:: true
		- Chọn đúng mô hình LLM
			- Ưu tiên mô hình có khả năng suy luận và phản hồi ổn định. Llama, Claude Opus, Mistral là những lựa chọn mã nguồn mở tốt.
		- Thiết kế logic hoạt động cho agent
			- Nó có cần suy nghĩ trước khi trả lời? Có nên lập kế hoạch? Khi nào thì dùng công cụ hỗ trợ?
		- Viết hướng dẫn vận hành rõ ràng
		  Định nghĩa:
		  • Cách phản hồi
		  • Khi nào dùng công cụ
		  • Hành vi trong từng tình huống cụ thể
		- Thêm khả năng ghi nhớ hiệu quả
			- LLM dễ quên – **dùng cửa sổ trượt, tóm tắt hội thoại**, lưu trữ sở thích dài hạn (công cụ như **MemO, ZepAI** có thể hỗ trợ).
		- Kết nối công cụ & API
			- Cho phép truy vấn dữ liệu, gọi CRM, tra cứu giá cổ phiếu...
		- Giao cho nó một công việc cụ thể
			- “Phân tích phản hồi & gợi ý cải tiến” thì ổn. “Hãy hữu ích” thì quá mơ hồ.
		- Mở rộng bằng hệ thống multi-agent
			- Một agent thu thập, một phân tích, một trình bày. Mỗi agent có vai trò rõ ràng.
- ## Diary
	- Control vs freedom
		- Agents can be **free** to ReAct, call tools, plan... like [[smolagent]]
		- or can be **controlled explicitly** like [[langgraph]]
		- ### [[crewAI]] Focus on tasks over agents
			- writing clear **task instructions**
			- define **detailed input and expected outputs**
			- add **examples and context to guide execution**
			- dedicated the remaining time to agent **role, goal and back story**
- ## Prompting
	- [leaked-prompts](https://github.com/jujumilk3/leaked-system-prompts): learn system prompts
	-
- ## Practices*
	- [ ] [philoagents](https://github.com/neural-maze/philoagents-course): Learn how to build an AI-powered game simulation engine to impersonate popular philosophers.
	  collapsed:: true
		- Create AI agents that authentically embody historical philosophers
		- Master building agentic applications
		- Architect and implement a production-ready RAG, LLM and LLMOps system from scratch
		  collapsed:: true
			-
			-
			-
	-
-