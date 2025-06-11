- Methods:
	- use a lower learning rate for the early layers of the neural network, 
	  and a higher learning rate for the later layers (and especially the 
	  randomly added layers)
	- the first layers learn the very simple foundations, like edge and gradient detectors; these are likely to be useful in any task. The later layers learn much more complex concepts, which might not be useful in our desired tasks -> let the later layers fine-tune more quickly than earlier layers.