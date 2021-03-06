# (RE-)PACRR Neural IR models 

This is a Keras (TensorFlow) implementation of the model described in:

Kai Hui, Andrew Yates, Klaus Berberich, Gerard de Melo.
[PACRR: A Position-Aware Neural IR Model for Relevance Matching](https://arxiv.org/pdf/1704.03940.pdf).
*In EMNLP, 2017.*

Kai Hui, Andrew Yates, Klaus Berberich, Gerard de Melo.
[RE-PACRR: A Context and Density-Aware Neural Information Retrieval Model](https://arxiv.org/pdf/1706.10192.pdf).
*In Neu-IR workshop, 2017.*


## Contact
***Code author:*** Kai Hui and Andrew Yates

***Pull requests and issues:*** @khui @andrewyates

## Contents
* [Model Overview](#model-overview)
* [Getting Started](#getting-started)
    * [Install Required Packages](#install-required-packages)

## Model overview

The *(RE-)PACRR* models the interaction between a query-document pair, evaluating the
relevance of the document for the query. The input is the similarity matrix between 
a query and a document, and the output is a scaler. The full pipeline of the model will be 
published soon. 

## Getting Started

### Install Required Packages

First install Anaconda (Recommended).

* **Anaconda** ([instructions](https://www.continuum.io/downloads))

Thereafter install tensorflow and keras on Anaconda.

* **TensorFlow** ([instructions](https://www.tensorflow.org/install/))
* **Keras** ([instructions](https://keras.io/#installation))
* **numpy** and **scipy** ([instructions](https://www.scipy.org/install.html))
* **sacred** ([instruction](http://sacred.readthedocs.io/en/latest/quickstart.html#installation))


## Preparation: word2vec and similarity matrices

The preprocessing is to generate a simmilarity matrix for individual query-document pairs. 

There are two phases:

1) Extract the text from warc;

2) Prepare the word embedding for individual terms in the query and docs, by using a pre-trained word2vec corpus.One needs to make up the missing vectors for the terms that are not included in the pre-trained word2vec. In the preprocess/wordembedding directory, the train_w2v.py makes up the missing ones by keep training the word2vec meanwhile fixing the vectors that are presented already;

3) the computation of the similarity matrices for individual query-doc pairs (not included in the code)

At this moment, we include the pre-computed simmilarity matrices in the followings. One could download and unpack the [similarity matrices](https://drive.google.com/file/d/0B3FrsWe6Y5YqdEtfSjI4N0h1LXM/view?usp=sharing) 
for clueweb as described in PACRR and RE-PACRR. 

run the following:

       tar xvf simmat.tar.gz

## Usage: train, predict and evaluation

Configure the $parentdir in *.sh as the root directory for all outputs.

Configure the sim_dir in utils/config.py, holding the similarity matrices.

### Train

    python -m train_model with expname=$expname train_years=$train_years {param_name=param_val}

or use the script

    bash bin/train_model.sh

Configure different parameters in train.sh or utils/config.py

### Predict

    python -m pred_per_epoch with expname=$expname train_years=$train_years test_year=$test_year {param_name=param_val}

or use the script

    bash bin/pred_per_epoch.sh

Configure different parameters in pred_per_epoch.sh or utils/config.py


### Evaluation

Evaluate the prediction over the three benchmarks as described in our RE-PACRR paper. Note that 
for Rerank-ALL benchmark one needs to dump [all trec-runs](http://trec.nist.gov/results/) 
and their corresponding evaluation results
under data/trec_runs and data/eval_trec_runs respectively.

    python -m evals.docpairs with expname=$expname train_years=$train_years {param_name=param_val}

    python -m evals.rerank with expname=$expname train_years=$train_years {param_name=param_val}

or use the script

    bash bin/evals.sh

Configure different parameters in evals.sh or utils/config.py



