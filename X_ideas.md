#X Ideas

##1. Semantic Search

__Problem:__  Given a search query _q_ containing query terms _q1,...,qN_, try to augment the query terms by adding additional terms that are semantically similar. This broadens the search results to include documents that do not necessarily contain any terms included in the original query but do contain terms in the augmented set. 

__Potential Solutions:__  
A potential solution to this problem must include a method by which to find semantically similar terms to a given query term.

1.  __Word Vectors:__ Compute word vectors for every word in the vocabulary (via the _Word2Vec_ or _GloVe_ methods).  Compute similarity between two arbitrary words by taking the cosine similarity between their corresponding word vectors.  Can either
    * Use the pre-trained _GloVe_ word vectors from [https://github.com/stanfordnlp/GloVe](https://github.com/stanfordnlp/GloVe) (definitely the easiest way) or,
    * Train word vectors from our own corpus or,
    * Train separate word vectors for each account (this might be problematic since the training data sizes would vary uncontrollably).
2.  __Latent Semantic Analysis (LSA):__ Another way to compute word vectors - this way using singular value decomposition.  Can compute semantic similarity the same way as with word vectors.
3.  __Pointwise Mutual Information (PMI):__  A method to compute semantic similarity between two words based on individual word frequencies vs word co-occurrence frequencies.

Rather than performing a nearest neighbour (semantically) seach over the entire vocabulary for each query term, it would be much more efficient to pre-compute the k-nearest neighbours for each word in the vocabulary and store this mapping somewhere.  Then, at query time, the nearest neighbours can be looked up in constant time.  

_But how can this be integrated with Solr??????_
_How do we evaluate this????_

##2. Tag Recommendation

__Problem:__ Tagging is often used as way to organize documents as an alternative or compliment to full-text search.  Let's look at the case of emails.  Most email services provide a tagging feature (it looks like _X_ provides tagging as well).  However, in most cases, the number of emails that a user tags is _much_ smaller than the number of emails that are left un-tagged.  From the set of tagged emails, we would like to train a tag recommender system that could then be applied to the set of un-tagged emails.  This way, the organizational benefits of tagging can be extended to all emails rather than just the small set of those that the user manually tagged.   This is similar to email classification but since the set of potential tags is very large, it is more appropriate to treat it as a recommendation problem.

__Potential Solutions:__  

1.  Solve this problem with known recommender system methods based on training data of tags.
    * Simple Solution:  Build simple association rule mappings between tags and words appearing in emails (based on Pearson correlation, PMI, etc.).
2.  Don't use tag training data at all.
    * Topic Modelling:  Train an LDA model on just the email text. However in this will great more broad, non-specific tags (more like categories).
    * Tf-Idf Tags:  Tag an email based on unigrams or bigrams with the highest _tf-idf_ scores.  However this might yeild unexpected results and would only produce tags that are words appearing in the email (overly specific).

_Easy to evaluate if there is training data._
 


##3. Named Entity Recognition and Disambiguation

__Problem:__  Given a document, identify the tokens corresponding to mentions of specific named entities (e.g. names, organizations, locations etc.).  Once these mentions 

__Potential Solutions:__
There are only a few known ways of solving the NER problem


##4. Email Signature Extraction




##5. Attachment Analysis



##6. Question Answering

