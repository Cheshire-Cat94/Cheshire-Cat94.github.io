# Assessing the communicated severity of COVID-19 with real-life severity

Authors: Alexander Elg, Lea Dornacher and Sina Wahby

**The code was stolen from Armin Pournaki and the Sciences Po Medialab**

## Introduction

### Background

It is scarcely an overstatement to suggest that the COVID-19 pandemic has had enormous ramifications on the way we live, work, and relate to each other and the world around us. While sequestered and isolated in their poorly organised home offices, academics the world over have revelled at the opportunity to analyse every single facet of the nature of the disease; the management of the response to it; how close we should be to each other; whether mask mandates work. The list could be infinitely long. A curious common denominator among these lines of inquiry, however, has been the focus on **narrative shaping behaviour**. Not only has the response to the pandemic been scrutinised, but the manner in which that response has been detailed and delivered to the public, namely the **crisis communication**, has been seemingly almost as important.

### Crisis Communication

**Crisis communication** may be defined as the strategic management of a specific event, aimed at framing the public perception of the event itself, and decreasing the level of harm experienced by the state, the stakeholders, and the general public (Schwarz, Seeger, Auer, Rogers & Pearce, 2016). The  crisis communication strategy is highly important, impacting risk perception, affecting both short- (vaccination, evacuation of homes, conforming to restrictions) and long-term behaviour (physical and mental health outcomes). It has been shown that when threat appraisals are high, the public is more likely to respond to public health messages (Schwarz et al., 2016). Nevertheless, the public response itself is difficult to predict, not always being a protective one, e.g. the unnecessarily high use of antibiotics in reaction to the anthrax attacks in the US. The direction and amplitude of the response are influenced vastly by individual beliefs about the content of the public health message, as well as the perceptions of the individual/organisation who is communicating the message (the messenger).

### The United Kindom

Our country of study for this experiment is the United Kingdom. Why? To limit the scope of the study, we needed to situate ourselves in a context that fulfilled a certain number of criteria:

1. *Easily accessible information*. Since an important goal of this project is to put into practice that which we have learned during the course of this semester, and given the fact that we only had a certain number of weeks to complete it, we needed information that was accessible within the remit of our technical capabilities. mention data scraping, the guardian api, tweets, etc.
2. *That information needed to be available in a language we (and the models we use) speak*. While the team is made up by nothing but the finest of polyglots, the most common word embedding models are not. Moreover, choosing a country in which all information is available in English increases the number of usable data sources.
3. The specificities ofthe UK. The prevailing story surrounding the British government's management of the pandemic has been labeled a "communications crisis", charactarised by a declining use and trust in news, and a significant decline in the government as a source of information about the pandemic (Kleis Nielsen, Fletcher, Kalogeropoulos & Simon, 2020). 10 Downing Street has been criticised for everything from its flip-flopping stance on proper virus handling (Mason, 2022), to prematurely (The Lancet Microbe, 2021) and irresponsibly  abandoning (Fetzer, 2022) restrictions, to awarding millions of pounds worth of testing contracts to companies of doubtful competence because of personal connections in government. In July 2020, just after the first wave, Oxford University published the results of a major study about how people thing about the coronavirus crisis and the corresponding institutional response (Fletcher, Kalogeropoulos & Kleis Nielsen, 2020):
   - More than half of the interviewees (57%) feel that their government's response was worse than in other developed countries;
   - And nearly half of the persons interviewed (44%) found the governments response focused too much on protecting the economy.

Further qualitative studies have shown that the clashing of narratives, sometimes even from the same source, has decreased citizen trust in government, and increased their vaccine hesitancy (Lockyer et al., 2021). This setting should therefore lend itself to an antagonistic and productive narrative landscape, optimal for our research.

### Framing our Research

#### Main Research Question

**1. How can we use NLP to show a difference in communicated urgency over the course of a crisis?**

     H1: The proportion of urgent words is lower in TP2 than in TP1 and TP3

     H1: The proportion of urgent words is lower in TP3 than in TP1

The Coronavirus having been such a hot topic over the past two years, guaranteed both timely and sufficient data, contributing to our choice of topic. Additionally, the observable, large, and quantifiable difference in "real urgency" (ICU bed occupancy) between wave peaks and lows, sets optimal conditions for an analysis. This further makes it reasonable to assume that indeed there should be a change in communication over time, allowing us to test if simple NLP can detect such differences. Lastly, if indeed simple NLP does detect such differences, the application of complex NLPs to similar research questions is rendered highly interesting.

#### Supplementary research questions

We posit several different things, all of which we will test and explain below. 
First, that narratives around the **severity** of the pandemic have shifted over time, as we have learned how to ""live with it"". This is to say that we would expect a difference between the communicated urgency of the pandemic at time T+n compared to at time T. 
Second, we think that there might exist a **discrepancy** between how severe in nature the pandemic is said to be and how severe it actually was. In other words, did governments (or a government, specifically the British one in our study) under- or overdeliver compared to the situation on the ground?
Third, and finally, there should be a difference **between information sources** in just how severe they think the pandemic is/was. It would be congruent with our intuition if say a conservative newspaper downplayed the urgency of the situation compared to a more liberal one.
In line with this, we formulated two further research questions, which focus more on the contextual, rather than methodological side:

**2. As the pandemic wears on, does the communication about the urgency of COVID become less urgent, even though the real urgency of the situation speaks to the contrary?**

**3. Is there a difference in urgency between the data sources?**

## Methods

### Temporal Scope

Timing is important. Given the relative simplicity of our model (elaborated on in the next section), it had to be tested on time periods during which we could assume that there should be a clear difference in how urgent the pandemic was described relative to any other time period. This led us to limit our scope to three distinct time periods (from April 09 to April 15 2020 (TP1); from August 25 to August 31 2020 (TP2); from January 21 to January 27 2021 (TP3). How did we choose these particular time periods? Our reasoning went as follows.

1. Communication around the pandemic should (roughly) follow its spread through society.
2. Disease spread can be measured through absolute overall caseload, ICU occupancy, or death rate, to name a few indicators.
3. There is significant discrepancy between the peaks in ICU occupancy and overall cases.
4. ICU occupancy better reflects the severity (in terms of its impact on society's capacity to manage the disease) than does overall case load.
5. Therefore, we chose ICU occupancy as our proxy for societal severity.

![Peak ICU first wave](https://raw.githubusercontent.com/Cheshire-Cat94/Cheshire-Cat94.github.io/main/peak%20icu%20w1.png)

![overall ICU peaks](https://raw.githubusercontent.com/Cheshire-Cat94/Cheshire-Cat94.github.io/main/peak%20icu%20overall.png)

![incidence](https://raw.githubusercontent.com/Cheshire-Cat94/Cheshire-Cat94.github.io/main/new%20cases.png)

Comparing the figures above, it is clear that the relative peaks in ICU occupancy and overall case load only faintly correspond to each other. The time periods for which we gathered data thus correspond to a week surrounding the peak in ICU occupancy during the first Covid wave in the UK (TP1); a week surrounding the absolute lowest rate of ICU occupancy for the dataset as a whole (which fortunately enough also is in between the two peaks) (TP2); and a week surrounding the absolute peak in ICU occupancy (TP3).

### Data Collection

This section will present the code used to collect the data used to answer the research questions. 

```
import pandas as pd
import json
import spacy
import requests 
import random
```

#### Official Communication

Only official UK government communications published on the governmental website were considered under this category (https://www.gov.uk/search/news-and-communications). The tool "Webscraper" was used to download the RSS- feed per time period. The tool automatically generated csv files, which were then imported. The csv files may be found in the repository.

```
%pip install 'openpyxl'
import openpyxl
officialTP1 = pd.read_excel("./govtp11.xlsx")
officialTP2 = pd.read_excel("./govtp2.xlsx")
officialTP3 = pd.read_excel("./govtp3_1.xlsx")
```

A function was then defined to convert extract the information from the csv files, placing them into dictionaries.

```
def dictget(df, key, value):
    titlel = df[key].tolist()
    textl = df[value].tolist()
    results = {}
    for i in range(len(titlel)):
        results[titlel[i]]=textl[i]
    return results
```

A dictionary per time period (TP) was then defined.

```
of1_dic = dictget(officialTP1, 'arttitle', 'combinedarttex')
of2_dic = dictget(officialTP2, 'arttitle', 'combinedarttex')
of3_dic = dictget(officialTP3, 'arttitle', 'combinedarttex')
```

#### The Guardian

**Cher Alexander,**
*Est-ce que tu pourrais écrire une mot sur le processus?*

```
import json
import spacy
import requests 
import numpy as np
import pandas as pd
import warnings

def searchguard(api_key,tag,q,begin_date,end_date, pages):
    
    qs = q.split()
    """Returns a list of articles for a given query
    
    Parameters:
    api_key (str): your Guardian api key
    keyword (str): a keyword by which you want to filter the articles
    begin_date (str): a begin date in the format "YYYYMMDD"
    end_date (str): a begin date in the format "YYYYMMDD"
    pages (int): how many pages to run through (you get 10 articles per page)
    tag (str): the specific tag you're searching for (structured like {main topic}/{subtopic})
    
    Returns:
    docs (list): a list of articles as dictionaries
    """
    st = ""
    docs = []
    if len(qs) > 1:
        for w in qs:
            st += w + "%20OR%20"
    else:
        st = qs[0]
    for i in range(pages):
        #pagination starts at 1 hence
        i = i+1
        api = f"https://content.guardianapis.com/search?page={i}&q={st}&tag={tag}&from-date={begin_date}&to-date={end_date}&show-fields=bodyText&api-key={api_key}"   
        response = requests.get(api)
        data = response.json()
        docs.append(data)  
    return docs

def results(l):
    #every list index contains a dictionary. iterate through them
    #and initialise dictionary to return   
    results={}
    for q in l:
        #this layer contains a list of dictionaries. iterate through them
        for w in q.values():
            #this **should** create a list with -- you know it -- more dictionaries
            res = w['results']
            for k in res:
                #iterate through values in this final nested dictionary
                for v in k.keys():
                 #   print(k['webTitle'])
                    #if i print v, i get 30 headlines, which is what I would expect
                    #but only 11 get put in the dictionary
                    #and create a new dictionary into which to put the articles
                    results[k['webTitle']]=k['fields']['bodyText']             
    return results

api_key = "55f57dcd-e04e-4905-8734-e598ff187eaf"
begin_date_TP1 = "2020-04-09"
end_date_TP1 = "2020-04-15"
begin_date_TP2 = "2020-08-25"
end_date_TP2 = "2020-08-31"
begin_date_TP3 = "2021-01-21"
end_date_TP3 = "2021-01-27"
tag = "politics/health"
q = 'covid'
```

The retreived articles were then extracted into dictionaries (again per time period), with headlines as keys and body text as values.

```
p = 3
gtp1 = (results(searchguard(api_key, tag, q, begin_date_TP1, end_date_TP1, p)))

p = 2
gtp2 = (results(searchguard(api_key, tag, q, begin_date_TP2, end_date_TP2, p)))

p = 3
gtp3 = (results(searchguard(api_key, tag, q, begin_date_TP3, end_date_TP3, p)))
```

#### The Telegraph

As there is no public API for The Telegraph, the Mediacloud API was used instead. 

```
!pip install python-dotenv
!pip install mediacloud

from dotenv import load_dotenv
load_dotenv()  # load config from .env file

import os, mediacloud.api
# Read your personal API key from that .env file
MC_API_KEY='a514f7d36dcb83bd196d4c2786500c8ef158f76af306e464afad27749de31d29'
my_mc_api_key = os.getenv(MC_API_KEY)
# A convention we use is to name your api client `mc`
mc = mediacloud.api.MediaCloud(MC_API_KEY)
mediacloud.__version__

# make sure your connection and API key work by asking for the high-level system statistics
mc.stats()

# Telegraph media_id: '1750'
matching_sources = mc.mediaList(name_like='telegraph', sort='num_stories')
```

This method, however, did not allow to download the entire text of each article. Thus, a dictionary with the articles' Urls was created, the articles then scraped and "sorted" into dictionaries based on time periods.

```
# !pip install newspaper3k
from tqdm import tqdm
from newspaper import Article
import datetime

def url_to_text(url):
    article = Article(url)
    article.download()
    article.parse()
    text = article.text
    return text

# since everything else is provided as a dictionary, let's do it here too
def teldic (l):
    result = {}
    for i in range(len(l)):
        result[i]=l[i]
    return result

query = "((UK OR England) AND (quarantine OR lockdown OR covid19 OR coronavirus OR corona OR pandemic))"

# generic function to retrieve articles from the telegraph
def searchtel(begindate, enddate, query):
    fetch_size = 100000
    stories = []
    last_processed_stories_id = 0
    while len(stories) < 2000:
        fetched_stories = mc.storyList(solr_query= f"{query} AND media_id:1750", # Telegraph media_id=1750
                                   solr_filter=mc.dates_as_query_clause(begindate, enddate),
                                   last_processed_stories_id=last_processed_stories_id, rows= fetch_size)
        stories.extend(fetched_stories)
        if len( fetched_stories) < fetch_size:
            break
        last_processed_stories_id = stories[-1]['processed_stories_TP1_id']

    print(f'Number articles: {len(stories)}')

    has_no_text=0
    for story in stories:
        if story['word_count'] is None:
            has_no_text+=1

    url_list=list()
    for story in stories:
        url_list.append(story['url'])
    
    url2text = dict.fromkeys(url_list)
    for url in tqdm(url_list):
        try:
            text = url_to_text(url)
            url2text[url] = text
        except :
            url2text[url] = 'Error'

    telegraph=[x for x in list(url2text.values()) if x!='Error']

    toreturn = teldic(telegraph)

    return toreturn

begin_TP1 = datetime.date(2020,4,9)
end_TP1=datetime.date(2020,4,15)
telegraph_TP1_dic = searchtel(begin_TP1, end_TP1, query)

begin_TP2 = datetime.date(2020,8,25)
end_TP2=datetime.date(2020,8,31)
telegraph_TP2_dic = searchtel(begin_TP2, end_TP2, query)

begin_TP3=datetime.date(2021,1,21)
end_TP3=datetime.date(2021,1,27)
telegraph_TP3_dic = searchtel(begin_TP3, end_TP3, query)     
```

#### Twitter

Since the standard Twitter API only allows to retrieve of tweets from the past week, we may recur to scraping in order to collect tweets that were published earlier than this. The downside of this method is that it is a bit slower and that it does not allow us to retrieve any retweet information. Using minet, we can scrape tweets from the public API using an advanced search query.

```
!pip install minet
!minet tw scrape --help

# Time Period 1
!minet tw scrape tweets "((UK OR England) AND (quarantine OR lockdown OR covid19 OR coronavirus OR corona OR pandemic)) lang:en until:2020-04-15 since:2020-04-09" --limit 1000 > tweets_TP1.csv

df_TP1_Twitter = pd.read_csv("./tweets_TP1.csv")

df_TP1_Twitter = df_TP1_Twitter[df_TP1_Twitter['text'].notna()]

c1indl = df_TP1_Twitter.index
c12l = df_TP1_Twitter['text'].tolist()
df_1_dic = {}
for i in range(len(c1indl)):
    df_1_dic[c1indl[i]]=c12l[i]
    
# Time Period 2
!minet tw scrape tweets "((UK OR England) AND (quarantine OR lockdown OR covid19 OR coronavirus OR corona OR pandemic)) lang:en until:2020-08-31 since:2020-08-25" --limit 1000 > tweets_TP2.csv

df_TP2_Twitter = pd.read_csv("./tweets_TP2.csv")

df_TP2_Twitter = df_TP2_Twitter[df_TP2_Twitter['text'].notna()]

c2indl = df_TP2_Twitter.index
c22l = df_TP2_Twitter['text'].tolist()
df_2_dic = {}
for i in range(len(c2indl)):
    df_2_dic[c2indl[i]]=c22l[i]
    
# Time Period 3

!minet tw scrape tweets "((UK OR England) AND (quarantine OR lockdown OR covid19 OR coronavirus OR corona OR pandemic)) lang:en until:2021-01-27 since:2021-01-21" --limit 1000 > tweets_TP3.csv

df_TP3_Twitter = pd.read_csv("./tweets_TP3.csv")

df_TP3_Twitter = df_TP3_Twitter[df_TP3_Twitter['text'].notna()]

c3indl = df_TP3_Twitter.index
c32l = df_TP3_Twitter['text'].tolist()
df_3_dic = {}
for i in range(len(c3indl)):
    df_3_dic[c3indl[i]]=c32l[i]
```

Dictionaries per time period, including index and text per tweet, were created from the csv files that were returned.

### Data Analysis

In our model, we aim to measure the degree of urgency expressed in media and official communications.

To do so, we start from the assumption that certain words are used to express that a situation is urgent ("urgency signifiers"), and we can identify such words in the texts extracted. On the other hand, one might intuitively assume that there are other words used to describe situations that are not of immediate urgency. However, such words are much harder to identify clearly. Therefore, in our model we assume that the absence or lower occurrence  of “urgency signifiers” implies that a situation is being conveyed as less urgent.

Thus, in order to measure the level of occurrence of urgency signifiers in our sources, we first identify list of words that we intuitively would assume to signify urgency. 
> critical, troubling, problematic, essential, troublesome, pivotal, serious, worrisome, extreme, major, significant, dangerous, vital, crucial, important, critical, urgent, risky, severe, worrying, stringent, unprecedented

This list can be iteratively improved by using the most.similar function.
Subsequently, we find the average vector of the words in our list. To check if this average vector is accurately capturing urgent words, we can apply the most.similar function to the vector and verify that the resulting words are, indeed, words used to convey urgency.

Next, we create an identification function that compares the cosine similarity between a given word and the average vector with a set threshold value. If the cosine similarity exceeds the threshold, then the word fed to the function is classified as an "urgency signifier". If the cosine similarity falls below the threshold, then the word is not classified as an "urgency signifier". 
To identify an appropriate threshold value, we applied the function to all adjectives within a given text and manually checked which value produced results that came closest to our human judgement of which words may be "urgency signifiers". With this approach, we simultaneously tested the general performance of our function in identifying appropriate "urgency signifiers".

Finally, a the identification function is embedded in a function producing the overall proportion of "urgency signifiers" within the entire body of adjectives used in a text. **(NOTE: Should we explain why we use proportion of adjectives rather than proportion of all words?)**
The average proportion of "urgency signifiers" of texts can then be compared across time periods, and also across different sources.
**(NOTE: maybe still need to be more specific about which glove etc we are using....)**

```
#!pip install gensim
#!pip install pandas

import gensim
import numpy as np
import gensim.downloader as api
import matplotlib.pyplot as plt
from numpy.linalg import norm
from collections import Counter

nlp = spacy.load("en_core_web_sm")
from statistics import mean

# Choose pre-trained model and load it
model = api.load("word2vec-google-news-300")
```

## Results

## Discussion

### Limitations

## References

Fetzer, T. (2022). Subsidising the spread of COVID-19: Evidence from the UK’S Eat-Out-to-Help-Out Scheme*, The Economic Journal, 132(643), Pages 1200–1217. https://doi.org/10.1093/ej/ueab074

Fletcher, R., Kalogeropoulos, A., Kleis Nielsen, R. (2020). Majority think UK government COVID-19 response worse than other developed countries, almost half say response too focused on protecting the economy. The UK COVID-19 news and information project. https://reutersinstitute.politics.ox.ac.uk/majority-think-uk-government-covid-19-response-worse-other-developed-countries-almost-half-say

Kleis Nielsen, R., Fletcher, R., Kalogeropoulos, A., Simon, F. (2020). Communications in the coronavirus crisis: lessons for the second wave. The UK COVID-19 news and information project. https://reutersinstitute.politics.ox.ac.uk/communications-coronavirus-crisis-lessons-second-wave

Lockyer B, Islam S, Rahman A, Dickerson J,  Pickett K, Sheldon T, Wright J, McEachan R, Sheard L; Bradford Institute  for Health Research Covid-19 Scientific Advisory Group. Understanding  COVID-19 misinformation and vaccine hesitancy in context: Findings from a  qualitative study involving citizens in Bradford, UK. Health Expect.  2021 Aug;24(4):1158-1167. doi: 10.1111/hex.13240. Epub 2021 May 4. PMID:  33942948; PMCID: PMC8239544.

Mason, R. (2022). UK government has abandoned its own Covid health advice, leak reveals. The Guardian. https://www.theguardian.com/world/2022/feb/25/government-has-abandoned-its-own-covid-health-advice-leak-reveals

Schwarz, A., Seeger, M. W., Auer, C., Brooke Rogers, M., & Pearce, J. M. (2016). Chapter 4. The Psychology of Crisis Communication. In The Handbook of International Crisis Communication Research. Essay, John Wiley & Sons.  

The Lancet Microbe (Ed.). (2021). UK government's COVID-19 Gamble. The Lancet Microbe, 2(8). https://doi.org/10.1016/s2666-5247(21)00186-5
