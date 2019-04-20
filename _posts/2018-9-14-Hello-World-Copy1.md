---
layout: post
title: Natural Language Processing.
---

Have you ever thought how Gmail decides which mails of yours should go to your Primary inbox , which ones to Social inbox  and what goes in Promotions inbox ? It is like someone sorting your emails on your behalf by checking the content of it . This key feature of "Email Classification" is achieved by Natural Language Processing . It is an area in AI which is concerned with the interactions between computers and human languages and the ability of machines to understand, analyze and derive information from  natural human language . Sounds surprising !? Then you would love to know that you use NLP in your everyday life more often than you can imagine. Remember the auto completion of statements when you type in your smart phone , or the predictive typing which suggests the next word when you are typing in your search engine! That’s Natural Language Processing working for you. 

This is what makes NLP so much important and robust . It is used for simple purposes and also for much complex usage like Google assistant , ALEXA , Siri.   

### How does Natural Language Processing works ? – The Data Scientist’s view.

Text is one of the most unstructured form of input . So the first task is to clean the text and make it structured for the algorithm .
The process is called Text Processing. 
 
```
High level flow for text processing:

Raw text ---> Stopwords, URL ---> Lemmatisation, Stemming ---> Cleaned Text
             (Noise Removal)     (Lexicon Normalisation)               

```
<ins>**Noise Removal**</ins> : To remove words which does not impact the context , like I, am , is , the , of  etc. 
                               Known as Stopwords. Also to remove URLs or links . 
                               
<ins>**Lexicon Normalizaton**</ins>: To normalize repeated words in different forms, like ‘copies’ , copying’ ‘copied’ . 
                                     All these are from word ‘copy’ .
* Stemming : Is removing the ‘ing’ ,’ies’,’ed’ suffixes from words .
* Lemmatization : Finding out the root form of the word , like , finding word ‘copy’ from ‘ copies’/’copying’.

After structuring the text , it needs to get analysed by words , phrases , topics to understand the context.
  
<ins>**Text to Features**</ins> : It is the procedure of finding keywords which impacts the context and converting them into features . 
                                  Like , dividing the words into parts of speech , noun , verbs etc. 
                                  To draw the relationship between words in a sentence the dependency of each word with others has to be                                   charted by analysisng the word's arrangement and grammer. It is widely know as Syntactic Parsing.

```
Example : "She submitted her thesis on NLP ."

| Labels  |  Words   |
----------|----------|
| Root    | Submitted|
| Subject | She      |
| Object  | Thesis   |
| Object  | NLP      |
| Modifier| On       |

Explanation : Here "Submitted" is the main verb , so this is the Root . 
              Now , who submits ? "She". So it is Subject . What is she submitting - "Thesis" and "NLP"
              so they are objects. How is the object related , by "On" , so it is the modifier. 
```
Also by giving weight to a word on how many times it has been used in sentence .
```
Example : Text -- I love dancing , so I danced in rain.
          Tokens/words – (“I”,2),(“love”,1]),(“Dance”,2),(“in”,1),(“rain”,1).
          
Explanation : The frequency of using same words helps to understand the importance of the context .
```         
#### Example : Text preprocessing code snippet :    

```
import spacy
# Load English model for SpaCy
nlp = spacy.load("en_core_web_sm")

def preprocess(text, 
               min_token_len = 2 , 
               irrelevant_pos = ['ADV','PRON','CCONJ','PUNCT','PART','DET','ADP','SPACE']): 
    """
    Given text, min_token_len, and irrelevant_pos carry out preprocessing of the text 
    and return a preprocessed string. 
    
    Keyword arguments:
    text -- (str) the text to be preprocessed
    min_token_len -- (int) min_token_length required
    irrelevant_pos -- (list) a list of irrelevant pos tags
    
    Returns: (str) the preprocessed text
    """
    # Cleaning text and making it lower case
    text = re.sub(r'\S*@\S*\s?', '', text)
    text = text.replace('In article (', '')
    text = text.replace(') says:', '')
    text = text.lower()
    
    # Replace multiple spaces with a single space. 
    text = re.sub("\s\s+", " ", text)
    
    #3. Remove numbers : 
    text=re.sub('[0-9]+', '', text)

    doc = nlp(text) 
    
    #Removing stop words , punctuation and irrelevant part of speech and getting lemmatized.
    tokens=[]
    token_len=[]
    lemma=[]
    for token in doc: 
        if not token.is_stop | token.is_punct | token.is_space:
            if (token.pos_ not in irrelevant_pos): 
                lemma.append(token.lemma_)
    for i in lemma:
        if len(i) > min_token_len:
            token_len.append(i)       
    return token_len
    
df['clean_text'] = df['text'][0:6].apply(preprocess)
df.head()
```
* Result : 

![data](/images/NLP/img1.PNG)


<ins>**Entity as Features**</ins> : To understand the noun phrases , verb phrases. 
                                    This is rule based model , where algorithms are used to understand consumer insights,
                                    or used in chatbots.

```    
Example  : Mr Roy , the CEO of XYZ Company is talking with his family over phone while travelling to NY office. 
Entities : (“person” – Mr. Roy) , (“Location : NY) ,("Organisation : XYZ)

Explanation : The location , organisation of the person will help understand his needs ,
              also help give suggestion for food joints, shops in that area. 
```
    
<ins>**Topic Modelling**</ins> : It is to determine from the text or paragraph which topic the context is referring to . 
```
Example : Words like “medicine”, “doctor” refers to Healthcare .

Explanation : Understanding the topic will help understand what other elements under 
              this topic might be needed and accordingly suggestions can be proposed.
```

#### Example of Topic Modeling : 

Using LDA model :   
Created a dictionary using gensim's corpora.Dictionary method.  
Created the document-term co-occurrence matrix using corpora.Dictionary's doc2bow method.

```
corpus = [doc.split() for doc in df['clean_text'].tolist()]

dictionary = corpora.Dictionary(corpus)
doc_term_matrix = [dictionary.doc2bow(doc) for doc in corpus]

lda = models.LdaModel(corpus=doc_term_matrix, 
                      id2word=dictionary, 
                      num_topics=6, 
                      passes=8)

#Building the dictionary with topic label :
topic_map={0:"Travel",
           1:"Issue",
            2:"Electronics",
            3:"Politics-War",
            4:"Entertainment",
            5:"Service feedback"}
            
```

Getting the topics of a test data :   

```
def get_topic_label_prob(unseen_document, model = lda, dct=dictionary):
    """
    Given an unseen_document, and a trained LDA model, this function
    assigns a topic and returns a string containig your manually assigned 
    label of the best topic and its probability. 
    
    Keyword arguments:
    unseen_document -- (str) the document to be labeled with a topic
    model -- (gensim ldamodel) the trained LDA model
    
    Returns: (ste) a string of the form your manually assigned 
    label of the topic:probability of the topic.
    
    """
    final=[]

    clean_text=preprocess(unseen_document).split()
    bow_vector = [dct.doc2bow(i) for i in [clean_text]]
    
    for i in lda[bow_vector]: 
        topic_assign = sorted(i,key=lambda x:x[1],reverse=True)
        for key, value in topic_map.items():
            if str(topic_assign[0][0]) == str(key):
                final.append("".join([value,":",str(topic_assign[0][1])]))
    return final

test_df['topic'] =test_df['text'].apply(get_topic_label_prob)

#Taking a random sample set
test_df[10:31]   
```
* Result : 

![data](/images/NLP/img2.PNG)   
*Few of them match perfectly and makes so much sense, but few of them do not match very well. However, they give a broader sense of the topic.


In nutshell , the above process of semantic analysis, breaks down the human language for computers to understand and analyze to form meaningful insights. NLP is not an easy subject to understand , it is very vast and has lot of layers to it . As NLP develops we will see more interactions between AI and human life . It gives me immense hope that NLP is capable of predicting much deeper issues like psychiatric symptoms . It can detect depression by analyzing speech , usage of words, phrases and send alert (real SMS, yes it can !) for helping the patient . Also research and experiments are in progress to predict suicidal ideation in text based mental health intervention . 




*References :*

[Introduction to Natural Language Processing - Cambridge Data Science Bootcamp](https://www.youtube.com/watch?v=8S3qHHUKqYk)

[Natural Language Processing: Crash Course Computer Science #36](https://www.youtube.com/watch?v=fOvTtapxa9c)

[Natural Language Generation at Google Research](https://www.youtube.com/watch?v=MNvT5JekDpg)

[Research Article](https://www.hindawi.com/journals/cmmm/2016/8708434/)

[The Dependency Parsing Problem](https://www.youtube.com/watch?v=o7WlsBW-ndg)


