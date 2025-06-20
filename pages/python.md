- from library
	- functools
	  collapsed:: true
		- wraps
			- forward the source function name, metadata to wrapper
	- logging
	  id:: 67f38eb3-1f21-4518-9e86-3c629b3b6996
	  collapsed:: true
		- custom logger propagates log to `rootLogger`
			- to stop propagation, use `logger.propagate=False`
	- contextlib
	  collapsed:: true
		- Context Manager
			- defines the runtime
			- context to be established + `with` statement
			- handles entry into, exit from, the desired runtime context for execution of the block of code
			- A custom `Context Manager` object must implement `__enter__` (return [as Target]) and `__exit__` methods
		- ExitStack/AsyncExitStack
			- A context manager combines **other context managers and cleanup functions**, especially those that are optional or otherwise driven by input data **dynamically at runtime**
			- Their `__exit__` methods are called in `a reverse order` even if exceptions occur
			- Allows registering arbitrary cleanup functions, not just context managers
	- atexit
	  collapsed:: true
		- register callback to be run at python app exit
		- **not regarding app killed by signal not handled by python**
			- using `system.signal` to catch these cases
	- tempfile
	  collapsed:: true
		-
	- re
		- `re.search` search for groups matching patterns by orders
		- Flags
			- `re.DOTALL` seek **line_break** in `.*` pattern
			-
- generator
  collapsed:: true
	- $.send()
		- helps a **bidirectional communication** between caller and generator
		- caller uses $.send() to pass a value to generator
		- the passed value inside generator is defined: `sent = yield <something>`
- asyncio
  collapsed:: true
	- event loop
		- picks the most ready task from the task queue then run the picked task
		- when the task waits for another, pick the next one to process
	- awaitable object
		- an object that implement `__await__()` method, which returns an iterator (typically a generator-like object)
			- which the event loop uses to **suspend and resume** execution
		- types of awaitables
			- **Coroutines**
				- define by `async def`
				- must be awaited or scheduled (e.g via `asyncio.run()`)
				- when using `await`, coroutine yields control back to the event loop, wait for other **coroutine, task or awaitable** to complete
			- **Futures**: Low-level objects representing a **pending result** (e.g `asyncio.Future`)
			- **Tasks**: Wrapped coroutines scheduled in an event loop (e.g., created by `asyncio.create_task()`).
- helpful packages
  collapsed:: true
	- hydra
		- Features
			- **composable** configuration from multiple sources
			- run **multiple jobs** with different args
			- logging
				- hydra log itself
				- log for executed jobs
		- Config Concepts
			- packages
			  collapsed:: true
				- determines where the **content of each input config is placed in the output config**
				- is derived from `Config Group` by default
			- Config
			  collapsed:: true
				- a **yaml file** contains configurations
					- primary config: config in current file denoted by `_self_`
					- `Default List`:
				- can be select by: `config_name` in `@hydra.main`
			- Config group
			  collapsed:: true
				- **a directory** contains multiple config files
			- `defaults` list:
				- config
					- specified by the conf file name (*.yml) with[out] extension
				- `_self_`
					- determines the relative position of `this` config in the Defaults List
		- Configuration
			- can be access in `config` as well as **python runtime**
			- hydra
				- job
					- name: job name, default by python filename
					- chdir:
				- run: single run
					- dir: output directory
				- sweep: multi runs
					- dir
				- runtime: should not be overridden
					- cwd: original working dir the app was executed from
					- output_dir: dir created by hydra for saving logs and yaml config files
		-
	- uv
	  collapsed:: true
		- package and project manager
		- tags:: package, project
		- Dependencies
			- can be index, git, URL, path (folder, .whl, .pth) or `workspace` member
			- Path:
				- ```
				  $ uv add --editable ../packages/foo/
				  
				  # pyroject.tomp
				  [tool.uv.sources]
				  foo = { path = "../packages/foo/", editable = true}
				  ```
		- Workspace
			- workspace member as dependency
				- ```
				  # dependencye `foo` is fetch from workspace
				  [tool.uv.sources]
				  foo = {workspace = true}
				  
				  # add a member pakacge `foo` to `workspace`
				  [tool.uv.workspace]
				  members = ["package/foo"]
				  ```
	- hatch
	  collapsed:: true
		- extensible project manager
		- tags:: project
		-
		-