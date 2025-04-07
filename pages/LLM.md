- [[LLM/DPO]]
- [[ROPE]]
- Tokenizer
	- |      Method     |    Granularity    | Vocab Size |    Rare Words   | Complexity | Example Models |
	  |:---------------:|:-----------------:|:----------:|:---------------:|:----------:|:--------------:|
	  | [[BPE]]             | Subword           | Medium     | Good (subwords) | Low        | GPT, LLaMA     |
	  | WordPiece       | Subword           | Medium     | Good (subwords) | Medium     | BERT           |
	  | Unigram         | Subword           | Medium     | Good (flexible) | High       | T5, ALBERT     |
	  | SentencePiece   | Subword (BPE/Uni) | Medium     | Good            | Medium     | T5, XLNet      |
	  | Character-Level | Character         | Tiny       | Excellent       | Very Low   | Early models   |
	  | Word-Level      | Word              | Large      | Poor (<UNK>)    | Low        | word2vec       |