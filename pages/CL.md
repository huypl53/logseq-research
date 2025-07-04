- [[CL/Methods]]
	- [[knowledge distillation]] #card
	  card-last-score:: 3
	  card-repeats:: 2
	  card-next-schedule:: 2024-11-30T06:03:08.287Z
	  card-last-interval:: 4
	  card-ease-factor:: 2.22
	  card-last-reviewed:: 2024-11-26T06:03:08.287Z
	  id:: 679f2357-a923-4847-a1c7-4e0cddc203de
	  collapsed:: true
		- tags:: #PEFT, #SSIAT
		- cloze A technique which transfers knowledge from a complex model (the teacher) to a simpler model (the student) while minimizing the forgetting of previously learned information. E.g: Mixture of Experts exploit **frozen CLIP as teacher model** and then use its output features as input for a smaller model, the small one is a student model
	- [[SSF ]](Scale and Shift Factors) #card
	  card-last-score:: 3
	  card-repeats:: 1
	  card-next-schedule:: 2024-11-30T06:02:59.553Z
	  card-last-interval:: 4
	  card-ease-factor:: 2.36
	  card-last-reviewed:: 2024-11-26T06:02:59.564Z
	  id:: 679f2357-aaca-4f14-bc3c-6e6424d5b487
	  collapsed:: true
		- tags:: #PEFT, #SSIAT
		- modifies pre-trained models to **align the feature distribution** to downstream tasks
		- **insert layers** into transformer operation within the pre-trained model. These layers use scale $\gamma$ and shift $\beta$ to adjust the output of each module. Suppose $x_i$ is the pretrained module's output, SSF is: $y=\gamma \odot x_i + \beta$
	- [[VPT]] (Visual Prompt Tuning) #card
	  card-last-score:: 3
	  card-repeats:: 1
	  card-next-schedule:: 2024-11-30T06:03:02.615Z
	  card-last-interval:: 4
	  card-ease-factor:: 2.36
	  card-last-reviewed:: 2024-11-26T06:03:02.616Z
	  id:: 679f2357-5472-47dc-a5b4-2943c05148b0
	  collapsed:: true
		- tags:: PEFT, #SSIAT
		- introduces trainable parameters called **prompts** into the input space after the embedding layers. The key ideas is to **update these prompts during fine-tuning**, leaving the pretrained model untouched
		- Cons: when the **same prompt params are used across different learning sessions**, VPT can also **suffer from catastrophic forgetting**
	- [[Contrastive Learning]] #card
	  id:: 679f2357-3bc8-4b9e-ba96-ce1ea3434b48
	  collapsed:: true
		- tags:: #OPE
		- Pros: train the model to **minimize the distance between similar** pairs (positive sample) while **maximizing the distance between dissimilar** pairs (negative samples). This helps prevent model from forgetting learned information
	- [[Prototypes]] #card
	  id:: 679f2357-3cca-44d1-9f0f-30995943ea8e
	  collapsed:: true
		- tags:: OPE
		- Prototypes are the compact feature representations derived from intermediate layers of the model.
		- Pros: resource-efficient
		- Cons: may lose some fine-grained information -> sequentially update prototypes in each session, update by a small rate of new knowledge
	- [[APF]] (Adaptive Prototypical Feedback)
	  collapsed:: true
		- tags:: #OPE, #CL/Sampling
		- Select replay data based on rates of misclassification
	- Model merging
	  id:: 679f2357-ba13-4a2f-886c-d059646b246d
		- tags:: #MagMax
	- [[mixture-of-expert]]
	-
- Papers
  collapsed:: true
	- #SSIAT
	- #MagMax
	- #OPE
	- #CoFiMA
	- #EWC
	- #Fisher
	-
- ## Keywords
	- plasticity: learning on the new task
	- stability: performance on the previously learned concepts
	- #OOD
- ## Problems
	- [[shortcut learning]]
-
-
- Ideas for new paper
	- benchmark on contrastive loss
	- different location for OPE loss: after adapter, after local classifier
	-
	- experiment on contrastive loss for: 1. current session features, 2. replayed data
-