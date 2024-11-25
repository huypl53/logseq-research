Methods

	- knowledge distillation #card
	  card-last-interval:: 4
	  card-repeats:: 1
	  card-ease-factor:: 2.36
	  card-next-schedule:: 2024-11-25T08:58:19.616Z
	  card-last-reviewed:: 2024-11-21T08:58:19.617Z
	  card-last-score:: 3
	  collapsed:: true
		- {{cloze A technique which transfers knowledge from a complex model (the teacher) to a simpler model (the student) while minimizing the forgetting of previously learned information. E.g: Mixture of Experts exploit **frozen CLIP as teacher model** and then use its output features as input for a smaller model, the small one is a student model }}
	- SSF (Scale and Shift Factors) #card
	  collapsed:: true
		- tags:: #PEFT
		- modifies pre-trained models to **align the feature distribution** to downstream tasks
		- **insert layers** into transformer operation within the pre-trained model. These layers use scale $\gamma$ and shift $\beta$ to adjust the output of each module
		-
	- Visual Prompt Tuning (VPT) #card
	  collapsed:: true
		- tags:: PEFT
		- introduces trainable parameters called **prompts** into the input space after the embedding layers. The key ideas is to **update these prompts during fine-tuning**, leaving the pretrained model untouched
		- Cons: when the **same prompt params are used across different learning sessions**, VPT can also suffer from catastrophic forgetting
- Papers
	- #SSIAT
	- #MagMax
	- #OPE
-
- Ideas for new paper
	- benchmark on contrastive loss
	- different location for OPE loss: after adapter, after local classifier
	-
	- experiment on contrastive loss for: 1. current session features, 2. replayed data
-