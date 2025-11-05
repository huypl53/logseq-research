## Async task/Thread/Process scope
	- ### 1. FastAPI's Concurrency Model
	  
	  FastAPI primarily relies on **Asynchronous I/O (Asyncio)** for high concurrency.
	  
	  | Execution Unit | When Used | Shared Memory Impact |
	  | :--- | :--- | :--- |
	  | **Asyncio Task (Coroutine)** | For `async def` handlers (I/O-bound code). | **Shares memory** with the event loop and all other tasks in the process. |
	  | **Thread** | For synchronous `def` handlers (blocking code). | **Shares memory** with all other threads and tasks in the same process. |
	  | **Process (Worker)** | Used for production scaling (e.g., Gunicorn workers). | **Isolates memory.** Each worker process has its own dedicated memory space. |
	- ### 2. Static Members and Cloning
	  
	  The core learning revolves around when a static member (`Foo.static_member`) is duplicated versus shared.
	  
	  | Scenario | Is the Static Member Cloned? | Implication |
	  | :--- | :--- | :--- |
	  | **New Asyncio Task** | **NO.** (Runs in the same thread/memory.) | **RACE HAZARD:** Changes made by one task are immediately visible to others. Must use locks for safety. |
	  | **New Thread** | **NO.** (Runs in the same process/memory.) | **RACE HAZARD:** Changes made by one thread are immediately visible to others. Must use locks for safety. |
	  | **New Process** | **YES.** (Each worker gets a copy.) | **ISOLATION:** Changes made in Process A are **invisible** to Process B. Cannot be used for global application state. |
	  
	  ---
	- ## Practical Examples
	- ### Example 1: Shared Mutation (The Danger Zone)
	  
	  If you use a static member to count requests, the count will only be accurate *within* one worker process, and it will suffer from race conditions.
	  
	  ```python
	  class RequestCounter:
	    count = 0  # Static member
	  
	  @app.get("/count")
	  async def handle_request():
	    # Scenario: Handled by multiple Asyncio Tasks in one process
	    # DANGER: Two tasks try to read/write 'count' at the same time
	    
	    current_count = RequestCounter.count
	    # Simulate I/O pause
	    await asyncio.sleep(0.001) 
	    RequestCounter.count = current_count + 1
	    
	    return {"count": RequestCounter.count}
	  
	  # Implication:
	  # 1. If two requests hit this route at the same time in Process A, the final count might only increase by 1 (due to race condition).
	  # 2. Requests hitting Process B will update Process B's separate copy of `count`. The total application count will be wrong.
	  ```
	- ### Example 2: Process Isolation
	  
	  If you have two Uvicorn workers running:
	  
	  | Worker Process A | Worker Process B |
	  | :--- | :--- |
	  | `Foo.static_member` starts at `0`. | `Foo.static_member` starts at `0`. |
	  | Request 1 updates the member to `5`. | Request 3 updates the member to `2`. |
	  | **Result:** `Foo.static_member` in Process A is now `5`. | **Result:** `Foo.static_member` in Process B is now `2`. |
	  
	  The workers are completely unaware of each other's changes.
	- ### Key Takeaway
	  
	  For mutable state in FastAPI:
	  
	  * **Single-Process Safety:** If the state must be shared and modified within a single worker (tasks/threads), use **Asyncio Locks** or **Threading Locks**.
	  * **Multi-Process Safety (Global State):** If the state must be shared across all worker processes, **do not use static members**. Use an external, centralized system like **Redis** or a **Database**.