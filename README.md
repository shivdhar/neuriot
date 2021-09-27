# Sentisum

## Author: Shiv Dhar


# Setup

Please ensure `conda` is installed and available on your path

Run:
```bash
conda env create -f=environment.yml
jupyter lab
```
# Current Approach

- We treat this as a multi-label classification problem. 
- We use a pretrained DistilBert model and finetune it on the sentisum data outputting zero or more of 59 available classes.
- We clean the data, correcting the labels of three misspelled classes.

# Scores on Validation Set

Data split : (7599, 2533) | (Train, Val)
|                                          |   macro_precision |   macro_recall |   macro_f1 |   micro_precision |   micro_recall |   micro_f1 |
|:-----------------------------------------|------------------:|---------------:|-----------:|------------------:|---------------:|-----------:|
| models/distilbert-base/checkpoint-500    |          0.059345 |      0.0396144 |   0.041802 |          0.856022 |       0.429093 |   0.571642 |
| models/distilbert-base/checkpoint-25500  |          0.415192 |      0.36272   |   0.369645 |          0.764186 |       0.752598 |   0.758347 |
| models/distilbert-base/checkpoint-50500  |          0.409581 |      0.314709  |   0.329796 |          0.772327 |       0.738276 |   0.754917 |
| **models/distilbert-base/checkpoint-75500**  |          0.388809 |      0.324525  |   0.335822 |          0.753079 |       0.738276 |   0.745604 |
| models/distilbert-base/checkpoint-100500 |          0.391917 |      0.284568  |   0.311856 |          0.767338 |       0.727043 |   0.746647 |

## Observations:

Top 5 classes:

|                               Class  |          %  |
|:-------------------------------------|------------:|
| value for money positive             | 32.9655     |
| garage service positive              | 14.0069     |
| ease of booking positive             |  8.18621    |
| location positive                    |  7.33103    |
| length of fitting positive           |  4.53103    |


- The dataset is heavily imbalanced.
- This limits accuracy for classed with fewer labels.

# Alternative Approaches

## Direct aspect labeling of data

- By labelling the data differently, we could extract aspects directly from the text, and calculate aspect polarity separately.
- This would turn it into an NER(aspect) + classification(polarity) problem.
- Since the model would directly what caused a particular aspect and its polarity, accuracy may be expected to increase.
## Hierarchical classification

- Use hierarchical classification which identifies a particular aspect and then classifies its polarity.

## RNN-based classifier
- If inference time is an issue, drop the pretrained transformer part and use an RNN (GRU/LSTM) for generating review embeddings. 
- We would probably lose some performance but gain very fast predictions on CPU.


# Improvements

## Hyperparameter Tuning
- Systematically tune hyperparameters of model, optimizer, etc.
- Add further dropout. 
- Not implemented here due to lack of time.
