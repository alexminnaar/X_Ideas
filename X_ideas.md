#X Ideas

##1. Semantic Search

###Problem:  
Given a search query _q_ containing query terms _q1,...,qN_, try to augment the query terms by adding additional terms that are semantically similar. This broadens the search results to include documents that do not necessarily contain any terms included in the original query but do contain terms in the augmented set. 

###Potential Solutions:  
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

###Problem:
Tagging is often used as way to organize documents as an alternative or compliment to full-text search.  Let's look at the case of emails.  Most email services provide a tagging feature (it looks like _X_ provides tagging as well).  However, in most cases, the number of emails that a user tags is _much_ smaller than the number of emails that are left un-tagged.  From the set of tagged emails, we would like to train a tag recommender system that could then be applied to the set of un-tagged emails.  This way, the organizational benefits of tagging can be extended to all emails rather than just the small set of those that the user manually tagged.   This is similar to email classification but since the set of potential tags is very large, it is more appropriate to treat it as a recommendation problem.

###Potential Solutions:  

1.  Solve this problem with known recommender system methods based on training data of tags.
    * Simple Solution:  Build simple association rule mappings between tags and words appearing in emails (based on Pearson correlation, PMI, etc.).
2.  Don't use tag training data at all.
    * Topic Modelling:  Train an LDA model on just the email text. However in this will great more broad, non-specific tags (more like categories).
    * Tf-Idf Tags:  Tag an email based on unigrams or bigrams with the highest _tf-idf_ scores.  However this might yeild unexpected results and would only produce tags that are words appearing in the email (overly specific).

_Easy to evaluate if there is training data.  How much training data do we have access to?  X-specific tags? Can we get gmail/outlook tags also?????_
 


##3. Named Entity Recognition and Disambiguation

###Problem:  
Given a document, identify the tokens corresponding to mentions of specific named entities (e.g. names, organizations, locations etc.).  Once these mentions have been identified we want to link them to canonical entries in a knowledgebase (disambiguation).

###Potential Solutions:
There are only a few known ways of solving the NER problem

1. Heuristics: Dictionary matching, regex etc.  Generally low performance.
2. Model Based:  Train a probabilistic model on a training dataset (Crowdflower, Mechanical Turk, etc.).  Also useful for the heurstic approach for evaluation purposes.


Once the mention has been identified, it must be disambiguated wrt a knowledgebase of canonical entity names.  Obviously this first requires us to build a knowledgebase of canonical entity names.  The knowledgebase depends on the entities we are interested in.  Some public knowledgebases are Freebase, YAGO, etc. however the entities we are interested in are likely too specific to be included in these.  

Here are some possible ways to build our knowledgebase.

* __Person Entities:__  Canonical person names can be extracted from gmail/outlook contacts, Slack contacts, Salesforce contacts.
* __Company Entities:__  Canonical company names can be extracted from Salesforce (I assume...). 

The next step is to link the named entity mention to the most likely entry in the knowledgebase.  This is a straight-forward _learning to rank_ problem.  The ranking of knowledgebase entries for a given named entity mention should be dependent upon

* String similarity between mention and canonical name or any known aliases of the canonical name.
* Context of mention and how it relates to known facts about the candidate knowledgebase entry.


##4. Email Signature Extraction

###Problem:
Most professional emails contain a signature at the end which usually contains information about the sender (i.e. name, phone number, address, company, etc.).  Extracting this information could be very beneficial in knowledgebase building.  Since we know that the signature contains information about the sender, and the sender's name is already in the knowledgebase (because it is a gmail contact), we can use this information to augment this person's knowledgebase entry.  Having a more detailed knowledgebase has many benefits.  For example, it can improve disambiguation of entity mentions of this type.  Signatures can also provide aliases which we can record in a knowledgebase (For example, the signature "regards, Al" for an emal sent by "Alex Minnaar" tells us that "Al" is an alias for the canonical name "Alex Minnaar").

###Potential Solutions:
Email signature extraction is very similar to the named entity recognition problem, however there is no disambiguation part because we know that all of the extracted information is associated with the sender.


##5. Attachment Analysis



##6. Question Answering

