- Keywords
	- [[pseudoinverse]]
- Intro
	- update model analytically
	- no gradient descent, no back propagation
- Related works
- Contributions
	- take an example of model: $$Hw=Y$$, where $$H$$ is hidden features
	- apply least-square solution: $$L(w) =  \|y - Hw\|^2$$, we need to solve $$\underset{w}{min} L(w)$$
		- $$\frac{\partial{L}}{\partial{w}} = -2(y-Hw)H = -2H^{T}(y-Hw) = 0$$
		- $$H^THw=H^Ty$$
		- $$w=(H^TH)^{-1}H^Ty$$
	- The **Moore-penrose pseudoinverse** is denoted as: $$H^{+} = (H^TH)^{-1}H^{T}$$, then we have: $$w=H^{+}y$$
- Experiment
- Review
	- Pros
		- No back propagation -> Fast training
	- Cons
		- **noise sensitivity** -> require regularization (e.g: [[Tikhonov regularization]])
		- computationally **intensive weight updates** if hidden layer is large
-