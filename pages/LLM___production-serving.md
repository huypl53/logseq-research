## Flow
	- ### Phase 1: Architectural Strategy & Disaggregation
		- Production VLMs face a "heterogeneous computational burden": the prefill phase is compute-heavy (vision encoding), while the decode phase is memory-bandwidth-bound (text generation).
		- **Implement Disaggregated Serving:** Instead of running everything on one GPU instance, separate your infrastructure into **Prefill Nodes** and **Decode Nodes**.
			- > https://grok.com/share/bGVnYWN5LWNvcHk_fd0c5bbe-6bcd-4c70-9151-de374a3f997b
			- **Prefill Nodes:** Handle the vision encoder (e.g., SigLIP, CLIP) and the initial **prompt processing**. These require high FLOPs.
			  collapsed:: true
				- Phase one-time: process input once -> compute **KV cache for all tokens**
				- Compute heavy (many FLOPs)
				- Affected metrics: TTFT
			- **Decode Nodes:** Handle the autoregressive text generation. These require high HBM (High Bandwidth Memory).
			  collapsed:: true
				- iteratively generate new token using **previous KV cache**
				- heavy memory bandwidth
				- affected: ITL
			- **Mechanism:** Transfer the KV cache from prefill to decode nodes (e.g., using "Mooncake" architecture or SGLang's disaggregation). This prevents massive image processing tasks from stalling the token generation of other users, stabilizing tail latency (P99).
	- ### Phase 2: Model Compression & Quantization
	  collapsed:: true
		- > Visual tokens exhibit higher variance and entropy than text tokens, making them sensitive to naive quantization ("performance collapse"). You must choose modality-aware strategies.
		- **For NVIDIA H100/B200:** Use **FP8 (W8A8)**. It offers a 2x memory reduction and ~1.6x throughput increase with virtually no accuracy loss (>99.5% retention).
		- **For A100/Consumer GPUs:** Use **AWQ (Activation-Aware Weight Quantization)**. Unlike GPTQ, AWQ protects "salient" weights (often visual features) from aggressive quantization, preserving visual reasoning capabilities.
		- **Advanced Optimization:** Apply **Modality-Aware Quantization**. This balances the reconstruction loss between visual and textual tokens during calibration, preventing visual tokens from dominating the optimization process and degrading text quality.
	- ### Phase 3: Visual Token Optimization
	  collapsed:: true
		- > A single image can generate thousands of tokens, clogging the KV cache. You must reduce this input size dynamically.
		- **Dynamic Resolution:** Deploy models like **Qwen2.5-VL** or **LFM2-VL** that use dynamic resolution or "NaFlex" encoders. These process images at native aspect ratios rather than forcing fixed square inputs, avoiding wasted computation on padding.
		- **Token Reduction (Pruning):**
			- **Do Not Use:** Naive attention-based pruning (e.g., older FastV) for OCR or localization tasks, as it often discards small but critical details.
			- **Do Use:** Similarity-based reduction like **DART** (Duplication-Aware Reduction) or **DyMU**, which merges redundant tokens (e.g., sky/background) while preserving unique features. This can reduce token count by ~50% with minimal accuracy loss.
			- **LiteVLM Approach:** Implement a **Patch Selection Module** to filter irrelevant camera views (e.g., in robotics/driving) before the vision encoder processes them.
	- ### Phase 4: Selecting the Inference Engine
	  collapsed:: true
		- | Scenario | Recommended Engine | Why? |
		  | **Agentic / Multi-Turn** | **SGLang** | Uses **RadixAttention** (Radix Tree). If a user asks follow-up questions about the *same* image, SGLang reuses the cached visual prefix instantly, skipping the expensive vision encoding step entirely. |
		  | **High Throughput / Batch** | **vLLM** | Uses **PagedAttention** and Continuous Batching. Best for serving massive concurrent requests from different users where prefix sharing is minimal. It is the industry standard for stability. |
		  | **Peak Performance (NVIDIA)** | **TensorRT-LLM** | Uses kernel fusion and CUDA graph rewriting. Offers the absolute lowest latency on NVIDIA hardware but requires complex compilation and setup. |
		  | **Edge / CPU** | **LMDeploy / MLC LLM** | **LMDeploy** (TurboMind) offers highly optimized C++ kernels that rival SGLang speeds. **MLC** enables browser/mobile deployment. |
	- ### Phase 5: Acceleration & Decoding
	  collapsed:: true
		- > Once the model is running, optimize the decoding loop.
		- **Structured Output Enforcement:** For production apps requiring JSON, use **XGrammar** (with SGLang or vLLM). It pre-computes valid token masks, enabling "Jump-Forward" decoding where the engine can skip predicting static syntax tokens (like `{"key": "`), boosting throughput by up to 1.6x.
		- **Speculative Decoding:** Implement **SpecVLM** or **EagleVLM**. These use a smaller draft model to guess tokens and a larger target model to verify them.
			- *Challenge:* Standard text draft models fail on images.
			- *Solution:* SpecVLM uses an **elastic visual compressor** in the draft model to handle visual inputs efficiently, achieving 1.5–2.9x end-to-end speedups.
	- ### Summary Checklist for Production
	  collapsed:: true
		- **Engine:** Deploy **SGLang** if your app involves multi-turn conversation (visual agents); use **vLLM** for independent queries.
		- **Format:** Convert weights to **FP8** (if H100) or **AWQ** (if A100).
		- **Visuals:** Enable **RadixAttention** to cache processed image features across requests.
		- **Safety:** Integrate **XGrammar** to guarantee valid JSON outputs without latency penalties.
		- **Scaling:** If traffic is high, separate **Prefill** (Vision) and **Decode** (Text) onto different GPU pools.
- ## Strategies
	- Parellel
		- TODO `tp` (tensor parallelism)
		- TODO `pp` (pipeline parallelism)
- ## Keywords
	- TTFT: time to first token
	- ITL: inter-token-latency
		-