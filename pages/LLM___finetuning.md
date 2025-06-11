## Types
	- Full fine-tuning (FFT)
	  collapsed:: true
		- 16-bit precision
		- high VRAM usage
	- LoRA
	  collapsed:: true
		- 16-bit precision
		- $$h=W_0x + \Delta Wx = W_0x + BAx$$ $$  B \in \mathbb{R}^{d\times r},   A \in \mathbb{R}^{r\times k}$$
			- only $$B,A$$ are trained
		- LoRA is only applied to **attention layer**
		-
	- QLoRA
	  collapsed:: true
		- quantize weights: 4-bit precision
		- 4-bit NormalFloat: đề xuất 1 loại dữ liệu tối ưu, kết quả thực nghiệm cho thấy độ
		  chính xác tốt hơn các loại dữ liệu như 4-bit Int và 4-bit Float.
		- Double Quantization:
			- Bước 1: quantize trọng số mô hình từ FP16/FP32 -> 4-bit [[NF4]]
			- Bước 2: Các hằng số scale dùng để giải mã 4-bit ở bước 1, thay vì để dạng
			  float32, sẽ tiếp tục được nén xuống 8-bit
			  => phương pháp làm giảm 0.37 bits cho mỗi tham số(xấp xỉ 3 GB cho model 65B)
		- Paged Optimizers: Kỹ thuật giúp huấn luyện LLM mà không cần nhiều VRAM nhờ
		  cơ chế luân chuyển dữ liệu giữa CPU và GPU một cách thông minh
		-
-
- ## Hyper params
	- Learning rate: Dựa nhiều vào kinh nghiệm và thực nghiệm, LR thường được dùng
	  trong các mô hình ngôn ngữ lớn là 1e-5 hoặc 2e-5.
	- Number of Epochs: Phụ thuộc vào learning rate và số lượng tokens:
	- Dataset lớn khoảng hơn 2 triệu mẫu = 1-2 epochs
	- Dataset nhỏ khoảng dưới 1 triệu mẫu = 3-4 epochs
	- Sequence length: Khoảng 25-50% so với độ dài context window của Base Model,
	  phụ thuộc vào batch size và VRAM mà bạn có
	- Batch size: 8 là con số hiệu quả tuy nhiên có thể lớn hơn phụ thuộc vào VRAM
	- LoRA rank: 16-64 tốt trong hầu hết các bộ dữ liệu
- ## Parameters for finetuning
	- r: rank
	- target_modules: which modules to be finetuned
	- lora_alpha: We suggest this to equal to the rank `r`, or double it.
		- tags:: LoRA
	- lora_dropout:
	- bias:
- ## Dataset
  collapsed:: true
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