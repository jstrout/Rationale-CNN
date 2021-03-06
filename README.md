# Rationale-CNN
This repository implements the model from paper: [Rationale-Augmented Convolutional Neural Networks for Text Classification](https://arxiv.org/pdf/1605.04469v2.pdf). 

Much of the code is modified from: https://github.com/yoonkim/CNN_sentence

## Preprocessing Data
You need to download [Pre-trained word2vec file](https://code.google.com/p/word2vec/), and then run
```
python process_data_doc.py path_to_word2vec
```
where path_to_word2ec is the location of the pre-trained word2vec file. This program will generate the training and test data both as 3D tensor, with size (number of documents) * (number of sentences in one document) * (number of words in one sentence + 1). The '+1' part is used to put the sentence label for rationales. If you want to train your own dataset, you should modify the process_data.doc.py, since each dataset might mark the rationale with different format. But the generated data tensor should be in the same format. 

## Train the rationale-CNN model
Run the following, and the program will perform 9-fold cross validation (CV) on movie review dataset. Originally, the dataset has 10 folds, but there is one fold without rationale labeled, so we exclude that fold. 
```
THEANO_FLAGS=mode=FAST_RUN,device=gpu,floatX=float32 python rationale_CNN.py -nonstatic -word2vec
```
This program should generate a mean accuracy around 90.11%~91%, as reported in the paper. However, note that the optimal value of dropout rate on sentences when training document-level CNN might be different on different folds, and it is worth further tuning if you want to get a even better result. 
It can also generate predicted rationales with their probabilities for some correctly classified documents. 

