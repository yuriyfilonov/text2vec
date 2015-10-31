# text2vec
Text vector model is supposed to train text vector representations 
described in https://cs.stanford.edu/~quocle/paragraph_vector.pdf
It runs [theano](http://deeplearning.net/software/theano/) underhood and 
can be launched on both CPU and GPU.

All functionality is splitted into controllers, infrustructure and learning.

##Controllers
implement different stages of data processing. They can be launched
either from command line or from python. When launced from a command line
each controller acts more like a standalone tool with it's own arguments
that will save work results to HDD. But these controllers also provide a bunch
of tools to process data in-memory.

##Infrustructure
Contains primitives for accessing data stored on disk, I/O routines,
logging wrappers etc.

##Learning
Finally, learning module provides access to text vector training functionality.

##Data processing stages
Generally data goes through the following pipeline:
Injection → Processing → Training

###Injection
On this stage data may be collected from a number of datasets using dataset connectors.
Each connector is a generator that provides access to the name/text pairs stored in a
dataset. No text transformations are applied on this stage except only dataset cpecific ones.
Thus, for instance, when injecting Wikipedia dataset all gzipped wiki dump is unpacked into
a set of name/text pairs with all wiki markup specific noise removed.
Currently, the connectors for the following datasets are available:
- [Wikipedia](http://kopiwiki.dsd.sztaki.hu/);
- [Rotten Tomatos](https://www.kaggle.com/c/sentiment-analysis-on-movie-reviews);
- [Large Movie Review Dataset v1.0](http://ai.stanford.edu/~amaas/data/sentiment/).

###Processing
The main goal of processing is to subsample the most frequent words, prune least unique ones,
build word contexts and append them with negative samples.

###Training
Text vector training. May be launched in two modes - with weights fixed or not. When weights are 
fixed training module acts as a text vector inferer. Training module does not train word vector
representations. Instead it relies on those word vectors that are trained by word2vec tool.