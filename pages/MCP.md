-
- Architecture
	- MCP host
		- LLM Apps like: Claude desktop, IDE, AI tools -> initiate connections
	- MCP Client
		- Application connects 1:1 to MCP Server
		- e.g: custom Chat Bot that needs MCP resources
	- MCP server
		- Provides **context, tools, and prompts** to clients
		- Concepts
			- lifespan
				- **initialize resources** when the **server starts and clean them up** when it stops
				- **access to** the initialized resources
				- type-safe context passing between lifespan and request handlers
	- Local Data Sources
		- File, database, local services that can be queried safely
	- Remote Services
		- Connects to MCP Server
- Core
	- Protocol layer
		- handles **message framing, request/response linking,** and high-level communication patterns.
	- Transport layer
		- Stdio: Uses **standard input/output** for communication
		- HTTP with SSE
			- Uses Server-Sent Events for server-to-client messages
			- HTTP POST for client-to-server messages
	- Message Types: Requests, Results, Errors, Notifications
	- **Resources**
		- can be various kinds of data: file, database, screen, API responses
		- types:
			- text: Source code, Configuration files, Log files, JSON/XML data, Plain text
			- binary: Images, PDFs, Audio files, Video files, Other non-text formats
		- are identified using URIs that follow this format:
		  collapsed:: true
			- ```
			  [protocol]://[host]/[path]
			  ---
			      file:///home/user/documents/report.pdf
			      postgres://database/customers/schema
			      screen://localhost/display1
			  ---
			  ```
		- access: client can access to server resources via `resources/list`, `resources/read` endpoint
		- updates: client can **subscribe** to resource updates in server
	- **Prompts**
		- > deterministic, scalability, workflow auto
		- reusable **Interaction patterns/reusable templates** and workflow for users <-> LLMs
		- **client flow**: get user question -> fetch server's `prompts` -> fill up the `prompts` -> send to its LLM
		- e.g:
		  collapsed:: true
			- ```json
			  // Request
			  {
			    method: "prompts/get",
			    params: {
			      name: "analyze-code",
			      arguments: {
			        language: "python"
			      }
			    }
			  }
			  
			  // Response
			  {
			    description: "Analyze Python code for potential improvements",
			    messages: [
			      {
			        role: "user",
			        content: {
			          type: "text",
			          text: "Please analyze the following Python code for potential improvements:\n\n```python\ndef calculate_sum(numbers):\n    total = 0\n    for num in numbers:\n        total = total + num\n    return total\n\nresult = calculate_sum([1, 2, 3, 4, 5])\nprint(result)\n```"
			        }
			      }
			    ]
			  }
			  ```
	- **Tools**
		- **Usage**: client <-> server <-> tools <-> resources, computations, action taking
		- **Discovery**: Clients can list available tools through the `tools/list` endpoint
		- **Invocation**: Tools are called using the `tools/call` endpoint
		- **Flexibility**: Tools can range from simple calculations to complex API interactions
	- **Sampling**
		- > human-in-the-loop: user review/modify `prompt`, LLM completion
		- **Usage:** servers request completions from LLMs
		- **Flow:** server sends sampling request -> client review/modify request -> client samples from LLM -> client review LLM completion -> client return result to server
		- e.g: https://github.com/jlowin/fastmcp?tab=readme-ov-file#llm-sampling
	- **Roots**
		- Usage: client informs servers **about relevant resource and location**
		- e.g:
			- ```
			  file:///home/user/projects/myapp
			  https://api.example.com/v1
			  ```
- Connection lifecycle
	- Initialization
		- client -> initialize request -> server
		  id:: 680843a5-b37f-4f53-a933-1f0fb3b8ee43
		- client <- initialize response <- server
		- client -> initialized notification -> server
		  id:: 680843a5-8bf3-4043-8c8a-26095586f27c
	- Normal message exchange begins
	- Either party can terminate the connection
		- Clean shutdown via `close()`
		- Transport disconnection
		- Error conditions
- ## Examples
	- Server
	  collapsed:: true
		- ```python
		  import asyncio
		  import mcp.types as types
		  from mcp.server import Server
		  from mcp.server.stdio import stdio_server
		  
		  app = Server("example-server")
		  
		  @app.list_resources()
		  async def list_resources() -> list[types.Resource]:
		      return [
		          types.Resource(
		              uri="example://resource",
		              name="Example Resource"
		          )
		      ]
		  
		  async def main():
		      async with stdio_server() as streams:
		          await app.run(
		              streams[0],
		              streams[1],
		              app.create_initialization_options()
		          )
		  
		  if __name__ == "__main__":
		      asyncio.run(main())
		  ```
	-
- ## Self-review
	- **Server as human-in-the-loop:**
		- `Sampling` and other features of MCP are designed to be used as part of a HITL system, where user can `reject or modify` how LLMs are used by MCP servers
-