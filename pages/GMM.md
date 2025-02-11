- > Gaussian Mixture Models
- Procedures (using [[EM]] method)
	- Initialization
	  logseq.order-list-type:: number
		- Randomly initialize the parameters (mean, variance and mixing coefficients) of the 2 Gaussian distributions
		  logseq.order-list-type:: number
	- E-step:
	  logseq.order-list-type:: number
		- For each data point, estimate the **probability** that it belongs to the each Gaussian distribution (responsibility) using current parameters
		  logseq.order-list-type:: number
	- M-step:
	  logseq.order-list-type:: number
		- Update the parameters (mean, variance and mixing coefficients) of each Gaussian distribution using the **responsibilities** (probability that it belong to a distribution) computed in E-step
		  logseq.order-list-type:: number
	- Repeat:
	  logseq.order-list-type:: number
		- Iterate between E-step and M-step util the parameters converge
		  logseq.order-list-type:: number