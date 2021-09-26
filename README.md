# Sentisum

## Author: Shiv Dhar

# Current Approach

We treat this as a multi-label classification problem. We use a pretrained DistilBert model and finetune it on the sentisum data outputting zero or more of 59 available classes.

# Alternative Approaches

- By labelling the data differently, we could extract aspects directly from the text, and calculate aspect polarity separately.
- This would turn it into an NER+classification problem.

# Improvements and Future Work

- Systematically tune hyperparameters of model, optimizer, etc. for better accuracy.
- Try a hierarchical classifier which identifies a particular aspect and then classifies its polarity.
- If inference time is an issue, drop the pretrained transformer part and use an RNN(GRU/LSTM) for generating review embeddings. We would probably lose some performance but gain very fast predictions on CPU.
