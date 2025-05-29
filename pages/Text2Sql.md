- Research
	- Keywords
		- [[Semantic parsing]]
	- Intro
	- Contributions
		- Evaluate EditSQL, IRNet
		-
	- Related works
	- Vocab
	- Conclusions
- ## Workflow
  collapsed:: true
	- Mermaid chart
		- ```mermaid
		  sequenceDiagram
		    participant Prep as Pre-processing
		    participant Gen as Candidate Generation
		    participant Post as Post-processing
		    participant Sel as Candidates Selection
		    participant Merge as Candidates Merging
		    participant Eval as Evaluation
		  
		    Note over Prep: M-Schema
		    Note over Prep: schema lininking: <br/> table selection
		    par Prep to Post
		      Prep -->> Gen: CoT Divide and Conquer
		      Prep -->> Gen: CoT Query Plan
		      Prep -->> Gen: Online synthetic <br/> Example generation
		    end
		    Note over Post: Database
		    Note over Post: Context
		    alt Selection strategy
		      Post -->> Sel: Select best SQL candidates
		    else Merge strategy
		      Post -->> Merge: Merge candidates
		    end
		    Merge -->> Eval: Evaluation steps
		  ```
	- Preprocessing
	  collapsed:: true
		- M-Schema representation | checked
			- comments for schema fields
				- use LLM gemini to generate
		- Schema linking
			- DB entity linking
			- table linking | checked
				- BASE-SQL:
					- column linking gains **insignificant accuracy; however high latency**
					- “we denote our schema linking model as flink : (q, S, Mlink) → Sq, where Sq ⊆ S represents only the schemas of the tables that are predicted to be needed for answering q” ([Gorti et al., 2025, p. 3](zotero://select/library/items/AFM3UWL7)) ([pdf](zotero://open-pdf/library/items/HUZ7PE9K?page=3))
					- https://github.com/huypl53/msc-sql/blob/0ea255f7da533f02945893639b15a249e05d3c2c/stage1_prediction.py#L17
						- return: pred table ids, user prompt, gt table ids, gt answer, gen answer, db id
					- Flow:
						- Preprocess
							- serialize DB **metadata** by: local path, foreign keys, primary keys, columns
							- **index** DB:
								- input: metadata
								- output
									- pickle raw column values
									- pickle column embedding values
						- MSC-SQL Stage 1
							- generate schema
								- [alter] use M-Schema
							-
			- column linking
			  collapsed:: true
				- XiYan-SQL
					- retrieval module
						- user questions -> keywords + entities -> top-k columns for each kws via semantic similarity
						- LSH + semantic similarity -> column + value retriever
					- column selector
						- few-shot LLM -> validate then reduce tables, columns
			-
	- Value retrieval
	  collapsed:: true
		- CHASE-SQL: user question -> extract keywords via LLM few-shot -> retrieve syntactically **similar words** (via “locality-sensitive hashing (LSH)” ([Pourreza et al., 2024, p. 3](zotero://select/library/items/9W7EVZAG)) ([pdf](zotero://open-pdf/library/items/EUBWLQ3H?page=3)))
		-
	- Candidate generation
	  collapsed:: true
		- Fintuning
			- input: query + metadata
				- metadata: column values
			- “Robust Training with Noisy Table Injection” ([Gorti et al., 2025, p. 3](zotero://select/library/items/AFM3UWL7)) ([pdf](zotero://open-pdf/library/items/HUZ7PE9K?page=3))
				- schema linking may produces **more tables than** strictly neccessary
				- **add random 1-2 tables** to finetuning SQL generator
		- Generation inference
			- Generate multiple responses, select one final response
				- “increase the next token sampling temperature,” ([Pourreza et al., 2024, p. 4](zotero://select/library/items/9W7EVZAG)) ([pdf](zotero://open-pdf/library/items/EUBWLQ3H?page=4))
				- “shuffle the order of columns and tables in the prompt” ([Pourreza et al., 2024, p. 4](zotero://select/library/items/9W7EVZAG)) ([pdf](zotero://open-pdf/library/items/EUBWLQ3H?page=4))
			- Apply CoT promting
			  collapsed:: true
				- “Divide and Conquer CoT” ([Pourreza et al., 2024, p. 4](zotero://select/library/items/9W7EVZAG)) ([pdf](zotero://open-pdf/library/items/EUBWLQ3H?page=4))
					- divide:
						- decompose task into **sub tasks**
						- gen partial **sub-queries**
					- conquer: **assemble** to final query
					- “is better suited for **decomposing complex questions**” ([Pourreza et al., 2024, p. 4](zotero://select/library/items/9W7EVZAG)) ([pdf](zotero://open-pdf/library/items/EUBWLQ3H?page=4))
				- “Query Plan CoT” ([Pourreza et al., 2024, p. 4](zotero://select/library/items/9W7EVZAG)) ([pdf](zotero://open-pdf/library/items/EUBWLQ3H?page=4))
					- mock the planning step of DB management: sql query -> query plan (low code for DB engine to execute)
					- “(1) identifying and locating the relevant **tables for the question**, (2) performing operations such as counting, filtering, or matching between tables, and (3) delivering the final result by selecting the appropriate columns to return” ([Pourreza et al., 2024, p. 4](zotero://select/library/items/9W7EVZAG)) ([pdf](zotero://open-pdf/library/items/EUBWLQ3H?page=4))
					- “excels when questions **require more reasoning over the relationships** between different parts of the question and the database schema” ([Pourreza et al., 2024, p. 4](zotero://select/library/items/9W7EVZAG)) ([pdf](zotero://open-pdf/library/items/EUBWLQ3H?page=4))
					- “explains which **tables to scan, how to match columns, and how to apply filter**” ([Pourreza et al., 2024, p. 4](zotero://select/library/items/9W7EVZAG)) ([pdf](zotero://open-pdf/library/items/EUBWLQ3H?page=4))
				- “Online Synthetic Example Generation” ([Pourreza et al., 2024, p. 5](zotero://select/library/items/9W7EVZAG)) ([pdf](zotero://open-pdf/library/items/EUBWLQ3H?page=5))
					- few-shot prompting: user input + SQL sample + DB schemas -> ask LLM to generate several examples
						- use JOIN
						- use COUNT
						- use aggregates
					- “mixing various examples for various **SQL features and database tables** with and without column filtering is observed to result in better generation quality overall” ([Pourreza et al., 2024, p. 5](zotero://select/library/items/9W7EVZAG)) ([pdf](zotero://open-pdf/library/items/EUBWLQ3H?page=5))
			- Apply few-shot prompting
				- “Contextual Retrieval through Few-Shot Examples.” ([Gorti et al., 2025, p. 3](zotero://select/library/items/AFM3UWL7)) ([pdf](zotero://open-pdf/library/items/HUZ7PE9K?page=3))
				- query sample rows
					- if `string`
						- index embedding values
						- offline search for similar values in specific `embeddings[table][col]`
					- if not string
						- query similar values directly to DB
	- Query fixer
	- Selection agent
	  collapsed:: true
		- from generated set of SQL candidates, process pair by pair to update candidate score
		- in a pair
			- if same result -> first candidate score +=1
			- if not
				- merge their input schemas
				- use binary classifier to select better candidate
				- picked candidate score += 1
	- Evaluation
		- Execution Accuracy (EX)
			- “measures the correctness of the SQL queries by **checking if their execution results match the expected outcomes**, making it a direct indicator of practical usability.” ([Gorti et al., 2025, p. 6](zotero://select/library/items/AFM3UWL7)) ([pdf](zotero://open-pdf/library/items/HUZ7PE9K?page=6))
		- Exact Match (EM)
			- assesses **syntactic precision by comparing the generated SQL query against the reference** query character by character
		- Validity and Efficiency Score (VES)
			- VES evaluates both the **correctness and computational efficiency** of generated queries, which may be important in practical real-time systems.1
- ## Challenges
-
- ## To-read
	- https://cloud.google.com/blog/products/databases/techniques-for-improving-text-to-sql
	- https://cloud.google.com/blog/products/data-analytics/nl2sql-with-bigquery-and-gemini
	-