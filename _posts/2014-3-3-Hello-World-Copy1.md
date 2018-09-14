---
layout: post
title: Natural Language Processing.
---

Have you ever thought how Gmail decides which mails of yours should go to your Primary inbox , which ones to Social inbox  and what goes in Promotions inbox ? It is like someone sorting your emails on your behalf by checking the content of it . This key feature of "Email Classification" is achieved by Natural Language Processing . It is an area in AI which is concerned with the interactions between computers and human languages and the ability of machines to understand, analyze and derive information from  natural human language . Sounds surprising !? Then you would love to know that you use NLP in your everyday life more often than you can imagine. Remember the auto completion of statements when you type in your smart phone , or the predictive typing which suggests the next word when you are typing in your search engine! That’s Natural Language Processing in action . 

This is what makes NLP so much important and robust . It is used for simple purposes which we have taken for granted for many years and also for much complex usage like Google assistant , ALEXA , Siri.

### What is Natural Language Processing in natural words ?
_“It’s only words and words are all I have to take your heart away “ – Ronan Keating._

So truly said (read sang) . There are thousands of words used by every human being every day . These words are interpreted and actions are taken on them . Though we all know words spoken are not always meant , they have inner meanings as well. However , it is not always the words which are used to communicate , humans use body language , tone too to understand the context. Similarly , NLP works with contextual patterns but not intonations. It figures out the pattern from repeatedly used words to form an insight. It also understands human speech and simulate answers in natural language. Best example is Google Assistant booking appointments. In business world , it is used to find which words customers use while searching a product . Then it derives a pattern and understands the need of the customer which helps the business to offer products or deals via advertisements to target groups. 

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

In nutshell , the above process of semantic analysis, breaks down the human language for computers to understand and analyze to form meaningful insights. NLP is not an easy subject to understand , it is very vast and has lot of layers to it . As NLP develops we will see more interactions between AI and human life . It gives me immense hope that NLP is capable of predicting much deeper issues like psychiatric symptoms . It can detect depression by analyzing speech , usage of words, phrases and send alert (real SMS, yes it can !) for helping the patient . Also research and experiments are in progress to predict suicidal ideation in text based mental health intervention . 

This is how Natural Language Processing will make the future of AI-Human interaction more powerful and useful.



*References :*

[Introduction to Natural Language Processing - Cambridge Data Science Bootcamp](https://www.youtube.com/watch?v=8S3qHHUKqYk)

[Natural Language Processing: Crash Course Computer Science #36](https://www.youtube.com/watch?v=fOvTtapxa9c)

[Natural Language Generation at Google Research](https://www.youtube.com/watch?v=MNvT5JekDpg)

[Research Article](https://www.hindawi.com/journals/cmmm/2016/8708434/)

[The Dependency Parsing Problem](https://www.youtube.com/watch?v=o7WlsBW-ndg)


