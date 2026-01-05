## Quantization backend
	- **FP16 / BF16**
	- **INT8 (AWQ)**
	- **INT4 (AWQ)**
	- **GPTQ** (limited but improving)
	- Many HF “quantized” models use **bitsandbytes**:
- ## Notes
	- vLLM un-compatible:
		- load_in_4bit=True
			- This relies on:
			- PyTorch custom ops
			- runtime dequantization
			- 🚫 vLLM does not use PyTorch forward()
			  🚫 vLLM cannot intercept bitsandbytes ops