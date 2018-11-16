---
title: "ChatBot Menggunakan Python"
description: "Membuat program chatbot sederhana dengan menggunakan python"
<!-- repo: "#" # delete this line if you want blog-like posts for projects -->
tags: ["machine learning", "artifisial intelegence", "python"]
weight: 2
draft: false
---

## Intro

Machine Learning adalah teknologi yang sedang booming akhir-akhir ini. Seiring dengan perkembangan sistem informasi, data pun menjadi hal yang sangat berharga. Dengan data perusahaan yang tidak punya aset fisik sekali pun dapat menjual perusahaannya dengan harga yang fantastik. Inilah yang kerap terjadi pada perusahaan start-up. Ini akan menjadi hal yang sangat menarik sekaligus ancaman bagi bisnis-bisnis konvensional yang lengah dengan perkembangan teknologi. 

## Konfigurasi 
1. python 3.6
2. jupyter notebook
3. library nltk
4. data set nltk

## Program Sederhana
```
import nltk
import numpy as np
import random
import string # to process standard python strings
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics.pairwise import cosine_similarity

f=open('chatbot.txt','r', errors = 'ignore')
raw=f.read()
raw=raw.lower()# converts to lowercase
# nltk.download('punkt') # first-time use only
# nltk.download('wordnet') # first-time use only
sent_tokens = nltk.sent_tokenize(raw)# converts to list of sentences 
word_tokens = nltk.word_tokenize(raw)# converts to list of words

# print(sent_tokens[:1])
# print(word_tokens[:2])

# ### pre-processing the raw text

# In[2]:
lemmer = nltk.stem.WordNetLemmatizer()
#WordNet is a semantically-oriented dictionary of English included in NLTK.
def LemTokens(tokens):
    return [lemmer.lemmatize(token) for token in tokens]
remove_punct_dict = dict((ord(punct), None) for punct in string.punctuation)
def LemNormalize(text):
    return LemTokens(nltk.word_tokenize(text.lower().translate(remove_punct_dict)))


# ### keyword matching

# In[3]:


GREETING_INPUTS = ("hello", "hi", "greetings", "sup", "what's up","hey",)
GREETING_RESPONSES = ["hi", "hey", "*nods*", "hi there", "hello", "I am glad! You are talking to me"]
def greeting(sentence):
 
    for word in sentence.split():
        if word.lower() in GREETING_INPUTS:
            return random.choice(GREETING_RESPONSES)


# ### generating response

# In[4]:


def response(user_response):
    robo_response=''
    TfidfVec = TfidfVectorizer(tokenizer=LemNormalize, stop_words='english')
    tfidf = TfidfVec.fit_transform(sent_tokens)
    vals = cosine_similarity(tfidf[-1], tfidf)
    idx=vals.argsort()[0][-2]
    flat = vals.flatten()
    flat.sort()
    req_tfidf = flat[-2]
    if(req_tfidf==0):
        robo_response=robo_response+"I am sorry! I don't understand you"
        return robo_response
    else:
        robo_response = robo_response+sent_tokens[idx]
        return robo_response


# ### ready

# In[5]:


flag=True
print("ROBO: My name is Robo. I will answer your queries about Chatbots. If you want to exit, type Bye!")
while(flag==True):
    user_response = input()
    user_response=user_response.lower()
    if(user_response!='bye'):
        if(user_response=='thanks' or user_response=='thank you' ):
            flag=False
            print("ROBO: You are welcome..")
        else:
            if(greeting(user_response)!=None):
                print("ROBO: "+greeting(user_response))
            else:
                sent_tokens.append(user_response)
                word_tokens=word_tokens+nltk.word_tokenize(user_response)
                final_words=list(set(word_tokens))
                print("ROBO: ",end="")
                print(response(user_response))
                sent_tokens.remove(user_response)
    else:
        flag=False
        print("ROBO: Bye! take care..")


# In[ ]:


```


## 
### thank you!