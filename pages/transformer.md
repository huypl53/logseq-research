# Variants
	- [[SwinTransformer]]
- ## huggingface
	- components
		- **pipeline**
			- **inference** class that **infers model** for **task**
			- includes **tasks: text generation, image segmentation, ASR**
			- [review]
			  collapsed:: true
				- abstract for user
				- only need to specify task type, model id, device...
				- return output corresponding to task type
		- **trainer**
			- features
				- comprehensive trainer: **training and evaluation**
					- mixed precision
					- torch.compile
					- FlashAttention
		- **pretrained-model** is implemented from only three main classes (**configuration, model, and pre-processor**)
			- by files
			  collapsed:: true
				- `PretrainedConfig`/`configuration.py`
					- with specific attributes like the **number of hidden layers, number of attention heads, vocabulary size, activation function, and more**.
				- `PreTrainedModel`/`modeling.py`
					- learn from `configuration.py`
					- defines the layers and mathematical operations taking place inside each layer
					- only return **hidden states**
					- for a specific task, use the appropriate model head to **convert the raw hidden states into a meaningful result** (`LlamaModel` vs `LlamaForCausalLM`)
				- `Preprocessor`
					- converts text, images, audio, multimodal -> numerical inputs -> model
			- types
			  collapsed:: true
				- barebones: `AutoModel`, `LlamaModel` that outputs hidden states
				- with specific head attached `AutoModelForCausalLM` or `LlamaForCausalLM`, for performing specific tasks.
			- save/load
			  collapsed:: true
				- be saved to shards which is default 10GB
			- **Big model**
			  collapsed:: true
				- Model weights are dispatched **across all available devices**, starting with
				   the fastest device (usually the GPU) and then offloading any remaining 
				  weights to slower devices (CPU and hard drive).
			- **Data type**
			  collapsed:: true
				- if the model in `torch.float32` by default, load it via `torch.float16` requires additional memory due to double loading times
				- setting `torch_dtype="auto"` auto load weights in the data type they are stored in
			- `[AutoClass](https://huggingface.co/docs/transformers/en/model_doc/auto)` is recommended because it load both **model and preprocessor** for relevant task
				- Use [from_pretrained()](https://huggingface.co/docs/transformers/v4.53.0/en/main_classes/model#transformers.PreTrainedModel.from_pretrained) to load the weights and configuration file from the Hub into the model and preprocessor class.
		- generate
		  collapsed:: true
			- features
				- LLMs
				- VLMs
				- streaming
				- decoding strategies
	- miscs
	  collapsed:: true
		- `safetensor`
			- more secure than pytorch's `pickle` weights
			- faster to load
	- ### Concepts
		- checkpoint
	- ## Learn
		- Intro
			- hf auto loads **pretrained** model and **pre-processor** into model via `AutoModel`
			- then create a pipeline for **specified task** and its model, tokenizer
		- Review
			- ? using `pipeline` of **text-generation**, conversational messages support
				- use `mode.generate()` directly, prompt/response require **stop token**. E.g Phi-3-mini-4k-instruct requires `<|assistant|>`
-