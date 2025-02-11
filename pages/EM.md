- > Expectation-maximization (EM) algorithm is an **iterative computational method** to estimate parameters of statistical models when the data has missing or hidden variables.
- > EM is widely used in clustering, density estimation and supervised learning
- Research
	- Keywords
	- Intro
	- Contributions
		- Expectation Step (E-step)
		  logseq.order-list-type:: number
			- Estimate the missing or hidden variables using the current parameters (e.g: randomly init parameters)
			  logseq.order-list-type:: number
		- Maximization Step (M-step)
		  logseq.order-list-type:: number
			- Update the parameters to maximize the expected log-likelihood computed in the E-step
			  logseq.order-list-type:: number
			- The updated parameters are used in the next E-step
			  logseq.order-list-type:: number
		- E.g Gaussian Mixture Models ([[GMM]])
	- Related works
	- Vocab
-