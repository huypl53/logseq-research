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
	  collapsed:: true
		- `re.search` search for groups matching patterns by orders
		- Flags
			- `re.DOTALL` seek **line_break** in `.*` pattern
			-
	- threading
	  collapsed:: true
		- a program can have multiple threads
		- each thread run independently
		- thread.Lock can be used to **sync shared state between thread**
	- asyncio
		- event loop
			- picks the most ready task from the task queue then run the picked task
			- when the **task waits for another**, pick the next one to process
			- run in **a specific thread**
		- awaitable object
		  collapsed:: true
			- an object that implement `__await__()` method, which returns an iterator (typically a generator-like object)
				- which the event loop uses to **suspend and resume** execution
			- types of awaitables
				- **Coroutines**
					- define by `async def`
					- must be awaited or scheduled (e.g via `asyncio.run()`)
					- when using `await`, coroutine yields control back to the event loop, wait for other **coroutine, task or awaitable** to complete
				- **Futures**: Low-level objects representing a **pending result** (e.g `asyncio.Future`)
				- **Tasks**: Wrapped coroutines scheduled in an event loop (e.g., created by `asyncio.create_task()`).
		- tasks
		- lock
			- asyncio has its own lock via asyncio.Lock
				- tasks **sharing same Lock will wait for lock**
				- tasks in different threads are non-blocking
				- task **not waiting for Lock can be processed independently** by event loop
	- multiprocessing
	  collapsed:: true
		- `Process` class
			- .start()
			- .join()
		- start methods
			- `spawn`
				- starts a fresh Python interpreter process
				- child only inherit **necessary resources**, excluding file descriptors and handles from parent
			- `fork`
				- child is identical to parent process
				  id:: 6875bb38-7b3f-4322-aacf-dea3215337ac
				- Note that **safely forking a multithreaded** process is problematic.
			- `forkserer`
		- Object exchanging
			- `Queue`
			- `Pipe`
		- Synchronization
			- `Lock`
		- Pool of workers
			-
		-
	- contextvars
		-
- generator
  collapsed:: true
	- $.send()
		- helps a **bidirectional communication** between caller and generator
		- caller uses $.send() to pass a value to generator
		- the passed value inside generator is defined: `sent = yield <something>`
- patterns
  collapsed:: true
	- [python-patterns.guide]
		- The Composition Over Inheritance Principle
			- ## the subclass explosion
				- [Problem]: you want to custom a log class, then inherited to FileLog, SocketLog then PatternFilterLog. Do we need a new FilePatternFilterLog??
				- [Solve]
					- Adapter approaches
					  logseq.order-list-type:: number
						- tags:: duct type
						- Have FileLog -> FileLikeSyslog which implement `write()` method of file then we can pass it to PatternFilterLog
						- `FileLikeSyslog` have `write()` method like file class's one ( #[[duck type]] here)
- convention
  collapsed:: true
	- `duck typing`
		- type system concept where the **validity of an object for a particular operation** is determined by the p**resence of necessary methods and attributes**, rather than by i**ts explicit type or class inheritance**
	-
- helpful packages
	- hydra
	  collapsed:: true
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
		  collapsed:: true
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
	- [[python/pydantic]]
	-
	-
- Miscs
  collapsed:: true
	- force writing file to disk
		- ```python
		  csvfile.flush()
		  os.fsync(csvfile.fileno())
		  ```
		- `csvfile.flush()`
		  collapsed:: true
			- This flushes Python's internal buffer to the operating system.
			  **What happens:** When you write to a file in Python, the data doesn't immediately go to the file on disk. Instead, Python keeps it in an internal memory buffer for performance reasons. The `flush()` method forces Python to send all buffered data to the operating system.
		- `os.fsync(csvfile.fileno())`
		  collapsed:: true
			- This forces the operating system to write the data from its buffer to the actual disk.
			  
			  **What happens:** Even after `flush()`, the OS might keep the data in its own buffer for performance. The `fsync()` system call forces the OS to actually write the data to the physical storage device.
		- `csvfile.fileno()` gets the file descriptor (a number that identifies the file to the OS)
		- `os.fsync()` tells the OS: "write everything for this file to disk RIGHT NOW"
- patterns
  collapsed:: true
	- [python-patterns.guide]
		- [Problems] Subclass explosion
			- We have a `Logger` class
			- We implement children for log destination: `SocketLogger`, `SyslogLogger`
			- Next we want `FilteredLogger` to log over specific patterns
			- But now should we have `FilterSocketLogger`?? We still respect the fact that each class must serve a concrete function but the children are exploded
		- `The adapter pattern`
			- As far as we defined, `Logger` has a `file` stream to write the log
			- Why don't we just implement destination-specific **log writer** as `file` -> adaptation here: switch from `SocketLogger` -> `FileLikeSocket` and `SyslogLogger` -> `FileLiekSyslogLogger`: mimic file behavior
			- tags:: [[duck type]]
			- Then we pass it to `Logger` and `FilteredLogger`
			- **Summary:**
				- leverage #[[duck type]] to create an object that mimic existed functionality from another type
		- `The bridge pattern`
			- mainly have 2 components
				- outer **abstraction**: object that caller sees
				- inner **implementation**: be wrapped inside **abstraction**
			- `Logger` now is **abstraction** and seen by caller
			- `FileHandler`, `SocketHandler` are hidden **implementation** inside. They now have their own `emit()` methods instead mimicking file's `write()`
			- **Summary:**
				- Previously in `adapter pattern`, we have to replicate **file**'s behaviors: `write()`, `flush()` to create new adapters. But actually we don't need `flush()`
					- Now we have new classes with `emit()`
				- The **filtering** logic added to **logger** is exposed via **abstraction** `FilteredLogger`
				- The destination-specific logs are now ruled in **implementation**
- ## Concepts
	- ## [[duck type]]
		- if a cat can **quack**, it's a duck
		-
-