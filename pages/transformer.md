# Variants
	- [[SwinTransformer]]
- ## huggingface
	- components
		- pipeline
			- **inference** class
			- includes tasks: text generation, image segmentation, ASR
			- [review]
				- abstract for user
				- only need to specify task type, model id, device...
				- return output corresponding to task type
		- trainer
			- features
				- comprehensive trainer: **training and evaluation**
					- mixed precision
					- torch.compile
					- FlashAttention
		- model is implemented from only three main classes (configuration, model, and preprocessor)
			- files
			  collapsed:: true
				- `configuration.py`
					- with specific attributes like the **number of hidden layers, vocabulary size, activation function, and more**.
				- `modeling.py`
					- learn from `configuration.py`
					- defines the layers and mathematical operations taking place inside each layer
			- types
			  collapsed:: true
				- barebones: `AutoModel`, `LlamaModel` that outputs hidden states
				- with specific head attached `AutoModelForCausalLM` or `LlamaForCausalLM`, for performing specific tasks.
			- `PretrainedConfig`
				- specifies model attributes as the number of **attentions heads or vocab size**
			- `PreTrainedModel`
				- defined by configuration file
				- only return **hidden states**
				- for a specific task, use the appropriate model head to **convert the raw hidden states into a meaningful result** (`LlamaModel` vs `LlamaForCausalLM`)
			- `Preprocessor`
				- converts text, images, audio, multimodal -> numerical inputs -> model
			- save/load
				- be saved to shards which is default 10GB
			- **Big model**
				- Model weights are dispatched **across all available devices**, starting with
				   the fastest device (usually the GPU) and then offloading any remaining 
				  weights to slower devices (CPU and hard drive).
			- **Data type**
				- if the model in `torch.float32` by default, load it via `torch.float16` requires additional memory due to double loading times
				- setting `torch_dtype="auto"` auto load weights in the data type they are stored in
		- generate
		  collapsed:: true
			- features
				- LLMs
				- VLMs
				- streaming
				- decoding strategies
	- miscs
		- `safetensor`
			- more secure than pytorch's `pickle` weights
			- faster to load
	- ### Concepts
		- checkpoint