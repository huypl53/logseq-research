## Components
id:: 6978d9aa-a40e-4d22-838e-6bea3a874b68
	- Vision encoders (ViT):
		- use native dynamic-resolution ViT: Process images at original aspect ratio without forced cropping or padding
		  id:: 6978d9cb-2261-4893-ae8f-09dad69c2766
	- **The Projector (The "Bridge"):**
		- A lightweight MLP (Multi-Layer Perceptron) projector aligns the visual features from the ViT into the same hidden dimension as the Large Language Model
	- **LLM Backbone:**
		- A Qwen3-based transformer (available in dense and MoE variants) that treats visual tokens and text tokens as a single unified sequence.
- ## Features
	- DeepStack Integration (The "Deep" Fusion)
		- Instead of just sending the *final* output of the ViT to the LLM, DeepStack feeds features from *multiple levels/layers* of the ViT into the LLM.****
	- Multimodal Rotary Position Embedding (M-RoPE)
		- **Text tokens** receive 1D position IDs.
		- **Vision tokens** receive 3D position IDs (representing Width, Height, and Time/Frame for video)
		- **Interleaved-MRoPE** ensures that the model can attend to a specific coordinate in an image just as easily as it attends to the "next word" in a sentence
- ## Generation step
	- **Shared Attention:** The Transformer applies self-attention across the entire sequence. Text tokens can attend to visual tokens to "see" the context, and visual tokens can attend to each other to understand the scene.
	- **Causal Masking:** Just like a standard GPT model, the LLM predicts the "next token" one by one.
	- **Output Generation:** The final hidden state of the last Transformer block is passed through a language head (Linear layer + Softmax) to project it back into the vocabulary space, producing the next text token.