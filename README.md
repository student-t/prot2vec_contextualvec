# prot2vec_contextualvec

This repository is for training prot2vec models and generating contextual vectors for S/T/Y sites in protein sequences. 

## Basic requirement

To run the scripts in this repository, you need to install 

* Python >= 2.7.x

* [Numpy](http://www.numpy.org) >= 1.3

* [SciPy](https://www.scipy.org) >= 0.7 

* [Gensim](https://radimrehurek.com/gensim/) >= 2.3.0

The detailed description for the word2vec and doc2vec model can be found in [Distributed representations of words and phrases and their compositionality](https://arxiv.org/pdf/1310.4546.pdf) and [Distributed representation of Sentences and Documents](https://cs.stanford.edu/~quocle/paragraph_vector.pdf). 

## Training your own prot2vec

To train your own prot2vec model, you will need to prepare a database of protein sequences in the format of [FASTA](https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Web&PAGE_TYPE=BlastDocs&DOC_TYPE=BlastHelp). 

If the path to your protein sequence database is ```$path_to_database```, you can train a prot2vec model by running the following script, 

```
> ./run_prot2vec.sh $PATH_TO_DATABASE 
```

This training process could take several days depending on the size of your protein sequence database. 

The trained prot2vec model will be under the directory DATA.  

## Hyper-parameters for training

The hyper-parameters for training the prot2vec models is the same with the one for training a doc2vec model. 

You can change the hyper-parameters at __line 9__ in the script ```run_prot2vec.sh```. These parameters include, 

* __size__, which indicates the size of the vectors generated. The default values is set to 100. 

* __window__, which indicate the maximum distance between the target words and its context words used for prediction. The default value is set to 25. 

* __train_alpha__, which is the initial learning rate. The default value is set to 0.005. 

* __iter__, which is the number of iteractions the training performed on the corpus. The default value is set to 400. 

* __negative__, which indicates the number of examples sampled for negative sampling. The default value is set to 5. 

* __hs__, which indicates whether hierarchical softmax is performed. The default value is set to 0. 

The above hyper-parameters are derived from the doc2vec model, for which more detailed hyper-parameter descriptions can be found [here](https://radimrehurek.com/gensim/models/doc2vec.html). 

## Obtain the contextual vectors for protein sequences

After the training of the prot2vec model is completed, you can obtain the contextual feature vectors for S/T/Y sites from your own prot2vec model. 

You should prepare the protein sequences for query in the format of FASTA. 

If the path to this query file is ```$PATH_TO_QUERY```, you can obtain the contextual vectors (of __window size = 2*3+1__) for __Y sites__ by running the following script, 

```
> get_context.sh $PATH_TO_QUERY $PATH_TO_PROT2VEC 3 Y
```

Here, the ```$PATH_TO_PROT2VEC``` refers to the path where your prot2vec model is saved. You can find it in directory ```OUTPUT``` after training. The ```$PATH_TO_PROT2VEC``` could be something like ```OUTPUT/uniprot-all_s100_w25_lr0.005_itr400_ns5_hs0```. 

The __window size__ can be adjusted to any size that you would like to test and the site type can be one of (__'S'__, __'T'__, __'Y'__ and __'*'__) where * means all three types of sites. 

By combining the generated contextual vectors with other residue-level vector of potential phosphorylation sites, you can train your own models for predicting phosphorylation sites. 

