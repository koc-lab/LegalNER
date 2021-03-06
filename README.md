# LEGALNER - Named Entity Recognition in Turkish Legal Text

## Data Format

Follow the [`data/train_and_test`]

1. For `name` in `{train, test}`, create files `{name}.words.txt` and `{name}.tags.txt` that contain one sentence per line, each
word / tag separated by space using IOBES tagging scheme. 
2. Create files `vocab.words.txt`, `vocab.tags.txt` and `vocab.chars.txt` that contain one token per line. This can be automized using the corresponding function in [`src/preprocessing.py`] and altering the fields __DATASET_DIR__ to point to the locations of the file in Step 1 where __DATA_DIR__ pointing to the output directory.

4. Create a `glove.*.npz` file containing one array `embeddings` of shape `(size_vocab_words, 300)` using [GloVe 840B vectors](https://nlp.stanford.edu/projects/glove/). This can be built by using the corresponding function in [`src/preprocessing.py`] after completing Step 2 and altering the field __VECTOR_DIR__ to point desired output directory. 

## Get Started

Tensorflow 1.15 should be used, other versions are untested. Remaining packages should be operational wihout a specified version.

Once produced all the required data files, use the `main.py` with specifying correct parameters. These are:

1. __model (-m):__ Three base architectures; `lc`for LSTM-CRF, `llc` for LSTM-LSTM-CRF and `lcc` for LSTM-CRF-CRF.
2. __embeddings (-e):__ Three embeddings to pair with a base architecture; `glove` for GloVe, `m2v` for Morph2Vec and `hybrid` for their combination. Make sure that correct `.npz` files are present in the `data` folder and call preprocessing each time a different embedding is used. 
3. __preprocessing (-p):__ Flag to use preprocessing scripts, non-mandatory. Must be called in between new embedding selections. 
4. __mode (-a):__ Four modes, directories are used as stated in [`src/data.json`]; `train` to train a model from scratch, `k_fold` to perform cross-validation (default=5), `test` for testing and validating a specific input (default=None) and `use` for generating an output from a specified file using a trained model (default=None).

```
main.py -m <model> -e <embed> -p (preprocess flag) -a <mode>
```

If multiple tests are aimed to be performed with slight changes on the parameters, check out [`src/multiple_run.sh`] script.
