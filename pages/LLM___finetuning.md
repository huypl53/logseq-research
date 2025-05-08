## Parameters for finetuning
	- r: rank
	- target_modules: which modules to be finetuned
	- lora_alpha: We suggest this to equal to the rank `r`, or double it.
		- tags:: LoRA
	- lora_dropout:
	- bias:
- ## Dataset
	- Multiple cols dataset
		- Use a template with placeholders for each column -> then convert to a single prompt
	- Multi turn conversations
		- Dataset usually contains a single conversation like: instruction/output
		- Select random single conversations then combine them into one
	- Examples
		- ```
		  {
		  	"instruction":...,
		      "input":...,
		      "output":...
		  }
		  ```
		- Convert 3 fields above to a single prompt consisting of 3 corresponding parts
	- ### Chat format
		- chatGPT
			- ```
			  """
			  {SYSTEM}
			  USER: {INPUT}
			  ASSISTANT: {OUTPUT}
			  """
			  ```
		- ChatML
			- ```
			  <|im_start|>system{SYSTEM}<|im_end|>
			  <|im_start|>user{INPUT}<|im_end|>
			  <|im_start|>assistant{OUTPUT}<|im_end|>
			  ```
		- Llama-3 template