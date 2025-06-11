- >Continual Model Averaging
- Weighted Ensemble Models using Fisher
- Keywords
	- > [[Model merging]], [[Fisher]]
- Methods
	- Inspired by #EWC and #Fisher
	- Store
- References
	- Pre-trained model based methods
		- Dyer, E., Lewkowycz, A., Ramasesh, V.: Effect of scale on catastrophic 
		  forgetting in neural networks. In: Int. Conf. Learn. Represent. (2022)
		- Mehta, S.V., Patil, D., Chandar, S., Strubell, E.: An empirical investigation 
		  of the role of pre-training in lifelong learning (2023) 2, 4, 13
		- Wu, T.Y., Swaminathan, G., Li, Z., Ravichandran, A., Vasconcelos, N., 
		  Bhotika, R., Soatto, S.: Class-incremental learning with strong 
		  pre-trained models. In: IEEE Conf. Comput. Vis. Pattern Recog. (2022)
	- sequential full fine-tuning of the PTM backbone results in deterioration
	  collapsed:: true
	   of the original PTM representation, alongside **significant forgetting on
	   the previously learned tasks**
		- 38. McDonnell, M.D., Gong, D., Parvaneh, A., Abbasnejad, E., van den Hengel, A.: Ranpac: Random projections and pre-trained models for continual learning. Adv. Neural Inform. Process. Syst. 36 (2024) 2, 4, 10, 11, 9
		- 47. Panos, A., Kobe, Y., Reino, D.O., Aljundi, R., Turner, R.E.: First session adaptation: A strong replay-free baseline for class-incremental learning. In: Int. Conf. Comput. Vis. (2023) 2
		  86. Zhang, G., Wang, L., Kang, G., Chen, L., Wei, Y.: Slca: Slow learner with classifier alignment for continual learning on a pre-trained model. In: Int. Conf. Comput. Vis. (2023) 2, 4, 9, 10, 11, 12, 13, 1, 5
		- 88. Zhou, D.W., Ye, H.J., Zhan, D.C., Liu, Z.: Revisiting class-incremental learning with pre-trained models: Generalizability and adaptivity are all you need (2023) 2
	- **confine the PTM fine-tuning only to the first adaptation session** [38, 47, 88] or carefully **choose a low learning rate** to finetune the backbone [86]
	  collapsed:: true
		- 38. McDonnell, M.D., Gong, D., Parvaneh, A., Abbasnejad, E., van den Hengel, A.: Ranpac: Random projections and pre-trained models for continual learning. Adv. Neural Inform. Process. Syst. 36 (2024) 2, 4, 10, 11, 9
		- 47. Panos, A., Kobe, Y., Reino, D.O., Aljundi, R., Turner, R.E.: First session adaptation: A strong replay-free baseline for class-incremental learning. In: Int. Conf. Comput. Vis. (2023) 2
		- 88. Zhou, D.W., Ye, H.J., Zhan, D.C., Liu, Z.: Revisiting class-incremental learning with pre-trained models: Generalizability and adaptivity are all you need (2023) 2
- Review
	- Bài Class incremental learning
	- Áp dụng model merging sử dụng Fisher Maxtrix dạng đường chéo -> giảm tính toán, thao tác tính fisher matrix tương đương chạy thêm 1 epoch