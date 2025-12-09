- [[LLM/DPO]]
- [[ROPE]]
- ## Pipeline
  collapsed:: true
	- Pre-training
		- Data preparation
			- crawl data
			- preprocess raw data
		- Tokenization
		  collapsed:: true
			- |      Method     |    Granularity    | Vocab Size |    Rare Words   | Complexity | Example Models |
			  |:---------------:|:-----------------:|:----------:|:---------------:|:----------:|:--------------:|
			  | [[BPE]]             | Subword           | Medium     | Good (subwords) | Low        | GPT, LLaMA     |
			  | WordPiece       | Subword           | Medium     | Good (subwords) | Medium     | BERT           |
			  | Unigram         | Subword           | Medium     | Good (flexible) | High       | T5, ALBERT     |
			  | SentencePiece   | Subword (BPE/Uni) | Medium     | Good            | Medium     | T5, XLNet      |
			  | Character-Level | Character         | Tiny       | Excellent       | Very Low   | Early models   |
			  | Word-Level      | Word              | Large      | Poor (<UNK>)    | Low        | word2vec       |
		- LLM training
			- Base model gpt-3 có hàm loss hội tụ trong khoảng từ 1.5-1.7
			- Base model llama có hàm loss hội tụ trong khoảng từ 1.8-2.0
	- Post-training
		- (SFT) supervisored Fine-tuning
			- Types of data:
			  collapsed:: true
				- | Các loại data            | Mô tả                                                                 | Ứng dụng                                      |
				  |--------------------------|----------------------------------------------------------------------|----------------------------------------------|
				  | Instruction-Answer Pairs | Đang câu hỏi – câu trả lời, có thể tư vấn giản đơn đến phức tạp.      | Tốt cho các tác vụ u hỏi đáp, truy vấn trực. |
				  | Conversational Data      | Dữ liệu dạng hội thoại giữa người dùng và trợ lý, theo kiểu luân phiên (user - assistant). | Dành cho huấn luyện mô hình dạng hội thoại ChatGPT. |
				  | Text Generation          | Dữ liệu sinh văn bản theo prompt đã vào (viết truyện, viết code, mô tả sản phẩm...). | Viết sáng tạo, sinh mô tả sản phẩm, sinh nội dung tư vấn... |
				  | Code Generation / Completion | Prompt là câu hỏi về lập trình, hỗ trợ hoàn thiện.                   | Hỗ trợ ngôn ngữ coding, có thể sinh code.    |
				  | Reasoning / Step-by-step / Chain-of-thought | Thay vì trả lời trực tiếp, mô hình được huấn luyện tra cứu theo từng bước lập luận. | Tăng khả năng suy luận, giải toán, giải thích logic. |
		- (RLHF) Reinforcement learning with human feedback
			- graph
			  collapsed:: true
				- ```mermaid
				  graph TD
				      %% Step labels as separate nodes
				      S1[Step 1: Collect demonstration data and train supervised policy]
				      S2[Step 2: Collect comparison data and train reward model]
				      S3[Step 3: Optimize policy against reward model using RL]
				      
				      %% Step 1: Collect demonstration data and train supervised policy
				      A[Prompt Dataset] --> B[Sample Prompt]
				      B --> C[Human Labeler]
				      C --> D[Demonstrates Desired Output]
				      D --> E[Supervised Fine-Tuning SFT]
				      E --> F[Base Policy Model]
				      
				      %% Step 2: Collect comparison data and train reward model
				      G[Sample Prompt] --> H[Multiple Model Outputs]
				      H --> I[Human Labeler Ranks Outputs]
				      I --> J[Best to Worst Ranking]
				      J --> K[Train Reward Model RM]
				      
				      %% Step 3: Optimize policy using reinforcement learning
				      L[New Prompt] --> M[Policy Generates Output]
				      M --> N[Reward Model Calculates Reward]
				      N --> O[Update Policy using PPO]
				      O --> P[Optimized Policy]
				      
				      %% Connect the steps
				      F -.-> G
				      K -.-> N
				      
				      %% Position step labels above each section
				      S1 --- A
				      S2 --- G
				      S3 --- L
				      
				      %% Styling
				      classDef stepLabel fill:#f8f9fa,stroke:#495057,stroke-width:2px,font-weight:bold
				      classDef modelBox fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
				      classDef humanBox fill:#fff3e0,stroke:#f57c00,stroke-width:2px
				      classDef processBox fill:#e8f5e8,stroke:#2e7d32,stroke-width:2px
				      
				      class S1,S2,S3 stepLabel
				      class F,K,P modelBox
				      class C,I humanBox
				      class E,J,O processBox
				  ```
			- Notes
				- | Bước | Tên gọi                    | Mục tiêu chính                     | Kỹ thuật         |
				  |------|-----------------------------|------------------------------------|------------------|
				  | 1    | Supervised Fine-Tuning (SFT) | Day mô hình phân hội cùng con người | Học có giám sát  |
				  | 2    | Reward Model (RM)           | Day mô hình đánh giá phân hội      | Học tư xếp hạng  |
				  | 3    | Reinforcement Learning      | Tối ưu phân hội thông qua đánh giá RM | [[LLM/PPO]] |
	- Inference
		- single-turn inference
		- multi-turn inference
		- batched inference
		- streaming inference
		-
	- QA
		- tags:: QA
			- pretraining  phase is to train LLM on which data? why there is SFT (Post training)
- ## Tools
  collapsed:: true
	- [LLM Visualization](https://bbycroft.net/llm)
	-
- ## Inference
	- Self-host:
		- [[vLLM]]
	- Cost
		- [[Prompt caching]]
- ## Performance
	- TTFT: time to first token
- ## Evaluation
	- [[ELO]] algorithm
	-