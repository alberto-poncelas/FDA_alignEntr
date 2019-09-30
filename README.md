FDA
===

Feature Decay Algorithms (FDA)

Citation:

Ergun Bicici and Deniz Yuret, “Optimizing Instance Selection for Statistical Machine Translation 
with Feature Decay Algorithms”, IEEE/ACM Transactions On Audio, Speech, and Language Processing (TASLP), 2014.


FDA is developed as part of the PhD thesis:

Ergun Biçici. The Regression Model of Machine Translation. PhD thesis, Koç University, 2011. Note: Supervisor: Deniz Yuret.

# Installation

Execute `make` command

# Usage

Usage: fda [opts] train1 test1 [train2] [test2]\n"

*  train1: first (mandatory) arg gives source language train file.
*  test1 : second (mandatory) arg gives source language test file.
*  train2: third (optional) arg gives target language train file. This is used to output target sentences as a second column.
*  test2 : fourth (optional) arg gives target language test file. This is used to calculate metrics like bigram coverage

If any of these arguments are \"-\", the data is read from stdin.
Gzip compressed files are automatically recognized and handled.
Other options (and defaults) are:
*  -v (1): verbosity level, -v0 no messages, -v2 more detail
*  -n (3): maximum ngram order for features
*  -t (0): number of training words output, -t0 means no limit
*  -T (0): number of training lines output
*  -o (null): output file, stdout is used if not specified

The rest of the options are used to calculate feature and sentence scores:
*  -i (1.0): initial feature score idf exponent
*  -l (1.0): initial feature score ngram length exponent
*  -d (0.5): final feature score decay factor
*  -c (0.0): final feature score decay exponent
*  -s (1.0): sentence score length exponent

Formulas:
* initial feature score: fscore0 = idf^i * ngram^l
* final feature score  : fscore1 = fscore0 * d^cnt * cnt^(-c)
* sentence score       : sscore  = sum_fscore1 * slen^(-s)


# Example of execution 

In order to retrieve the most relevant `$LINES` sentences for a file `$INPUT` from a parallel set `$pathFrom` (source language) and `$pathTo` (target language):
```
fda -o out -T$LINES $pathFrom $INPUT $pathTo
```
The output can be split into two files containing the source-side and target-side sentences
```
cat out | awk -F"\t" '{print $1}' > data.srclang
cat out | awk -F"\t" '{print $2}' > data.trglang
```

# Alignment Entropy

In order to provide different decay factors for each n-gram, create a file with the following format

```
first gram|0.60
another gram|0.42
unigram|0.32
```

The decay scores must be in the (0,1) range.
Execute FDA including the option `-b 10 -m /path/to/scores_file` 

## Citation

```
@article{poncelas2017applying,
  title={Applying N-gram Alignment Entropy to Improve Feature Decay Algorithms},
  author={Poncelas, Alberto and Maillette de Buy Wenniger, Gideon and Way, Andy},
  journal={The Prague Bulletin of Mathematical Linguistics},
  volume={108},
  number={1},
  pages={245--256},
  year={2017},
}
```
