# Customizable TF-IDF

Customizable TF-IDF allows you to use different corpora for TF and IDF.

## Getting Started
The required libraries can be installed with:  

```
pip install -r Requirements.txt
```

## Usage

You can set different lists of documents for the TF corpus and the IDF corpus.

```python
from customizable_tfidf_vectorizer import CustomizableTfidfVectorizer

# Create TF-IDF vector
ctfidf = CustomizableTfidfVectorizer(tf_docs, idf_docs)
# Get TF-IDF vector
tfidf_vec = ctfidf.get_tfidf_vec()
```

You can leave the TF corpus as an empty list. You can update the TF corpus and compute TF-IDF later.
The IDF corpus can also be the path to a directory of text files, instead of a list of docuents.

```python
from customizable_tfidf_vectorizer import CustomizableTfidfVectorizer

# Precompute IDF
ctfidf = CustomizableTfidfVectorizer([], idf_dir="./idf_documents") 
# Update TF corpus and compute TF-IDF
ctfidf.update(tf_docs)
```

There are several more useful functions.

```python
#Get the document by ID
>>> ctfidf.id2doc(0)
'It is the measure of the frequency of words in a document.'
```

```python
# Get the token by ID
>>> ctfidf.id2token(0)
'words'
```

```python
# Get the vocabulary
>>> ctfidf.feature_names()
['words', 'to', 'word', 'the', 'number', 'that', 'is', 'ratio', 'in', 'of', 'it']
```

```python
# Get the number of documents
>>> ctfidf.idf_doc_num()
4
```

```python
# Get the TF-IDF vector
>>> vec = ctfidf.get_tfidf_vec()
array([[0.0330395 , 0.        , 0.        , 0.03448276, 0.        ,
        0.        , 0.0330395 , 0.        , 0.0330395 , 0.05209744,
        0.02604872],
       [0.01533033, 0.00978515, 0.01533033, 0.032     , 0.03066065,
        0.01533033, 0.01533033, 0.0120866 , 0.03066065, 0.03625981,
        0.0120866 ]])
```

```python
# Get the most common words in IDF documents
>>> ctfidf.most_common_idf()
[('the', 10), ('of', 5), ('log', 3), ('to', 3), ('we', 3), ('corpus', 2), ('idf', 2), ('documents', 2), ('it', 2), ('number', 2), ('ratio', 2), ('large', 2), ('this', 2), ('by', 2), ('a', 1), ('have', 1), ('high', 1), ('in', 1), ('occur', 1), ('rarely', 1), ('score', 1), ('that', 1), ('words', 1), ('containing', 1), ('is', 1), ('word', 1), ('because', 1), ('becomes', 1), ('can', 1), ('causing', 1), ('dampen', 1), ('effect', 1), ('explode', 1), ('get', 1), ('hence', 1), ('take', 1), ('taking', 1), ('values', 1), ('when', 1), ('will', 1), ('adding', 1), ('cannot', 1), ('denominator', 1), ('divide', 1), ('smoothen', 1), ('value', 1)]
```

You can rank vocabulary words based on TF-IDF scores and get the top N words in each document. The results are a descending order (top N highest scores) when N is a positive number and an ascending order (top N lowest scores) when N is a negative number.

```python
# Return a set of top N words in each document
>>> features = ctfidf.rank_tfidf(n=5, print_=True)
 doc_id | token | TF-IDF 
---------------------------
 0 | of         | 0.05209743530227554 
 0 | the        | 0.034482758620689655 
 0 | words      | 0.033039495377140606 
 0 | is         | 0.033039495377140606 
 0 | in         | 0.033039495377140606 
 1 | of         | 0.036259814970383775 
 1 | the        | 0.032 
 1 | number     | 0.030660651709986483 
 1 | in         | 0.030660651709986483 
 1 | words      | 0.015330325854993242 
{'words', 'the', 'number', 'is', 'in', 'of'}
```

You can also filter words by TF-IDF scores by specifying both or either of the maxinum or minimum scores. 

```python
# Return a set of words filtered by TF-IDF scores
>>> features = ctfidf.filter_tfidf(max=0.03, min=0.01, print_=True)
 doc_id | token | TF-IDF 
---------------------------
 0 | it         | 0.02604871765113777 
 1 | is         | 0.015330325854993242 
 1 | it         | 0.012086604990127927 
 1 | ratio      | 0.012086604990127927 
 1 | that       | 0.015330325854993242 
 1 | word       | 0.015330325854993242 
 1 | words      | 0.015330325854993242 
{'words', 'word', 'that', 'is', 'ratio', 'it'}
```