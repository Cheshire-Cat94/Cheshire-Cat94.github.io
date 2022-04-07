# Assessing the communicated severity of COVID-19 with real-life severity

Authors: Alexander Elg, Lea Dornacher and Sina Wahby

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

>H1: *The proportion of urgent words is lower in TP2 than in TP1 and TP3*

>H2: *The proportion of urgent words is lower in TP3 than in TP1*

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

![Figure 1_Peak ICU first wave](https://raw.githubusercontent.com/Cheshire-Cat94/Cheshire-Cat94.github.io/main/peak%20icu%20w1.png)

![Figure 2_overall ICU peaks](https://raw.githubusercontent.com/Cheshire-Cat94/Cheshire-Cat94.github.io/main/peak%20icu%20overall.png)

![Figure 3_incidence](https://raw.githubusercontent.com/Cheshire-Cat94/Cheshire-Cat94.github.io/main/new%20cases.png)

Comparing the figures above, it is clear that the relative peaks in ICU occupancy and overall case load only faintly correspond to each other. The time periods for which we gathered data thus correspond to a week surrounding the peak in ICU occupancy during the first Covid wave in the UK (TP1); a week surrounding the absolute lowest rate of ICU occupancy for the dataset as a whole (which fortunately enough also is in between the two peaks) (TP2); and a week surrounding the absolute peak in ICU occupancy (TP3).

### Data Collection

This section will briefly present our data collection methods per source. The respective code can be found in the respoitory under the file name *Covid severity NLP.ipynb*

#### Official Communication

Only official UK government communications published on the governmental website were considered under this category (https://www.gov.uk/search/news-and-communications). The tool "Webscraper" was used to download the RSS- feed per time period. The tool automatically generated csv files, which were then imported. The csv files may be found in the repository.

A function was then defined to convert extract the information from the csv files, placing them into dictionaries per time period.

#### The Guardian

The Guardian provides a comprehensive API that gives users access to everything except pictures and possible audio clips. We can therefore scrape every single word on specific topics during our chosen time periods.

The retreived articles were then extracted into dictionaries (again per time period), with headlines as keys and body text as values.

#### The Telegraph

As there is no accessible API for The Telegraph, the Mediacloud API was used instead. 

This method, however, did not allow to download the entire text of each article. Thus, a dictionary with the articles' Urls was created, the articles then scraped and "sorted" into dictionaries based on time periods.

#### Twitter

Since the standard Twitter API only allows to retrieve of tweets from the past week, we may recur to scraping in order to collect tweets that were published earlier than this. The downside of this method is that it is a bit slower and that it does not allow us to retrieve any retweet information. Using minet, we can scrape tweets from the public API using an advanced search query. Dictionaries per time period, including index and text per tweet, were created from the csv files that were returned.

### Data Analysis

In our model, we aim to measure the degree of urgency expressed in media and official communications.

To do so, we start from the assumption that certain words are used to express that a situation is urgent ("urgency signifiers"), and we can identify such words in the texts extracted. On the other hand, one might intuitively assume that there are other words used to describe situations that are not of immediate urgency. However, such words are much harder to identify clearly. Therefore, in our model we assume that the absence or lower occurrence  of “urgency signifiers” implies that a situation is being conveyed as less urgent.

Thus, in order to measure the level of occurrence of urgency signifiers in our sources, we first identify list of words that we intuitively would assume to signify urgency. 

> critical, troubling, problematic, essential, troublesome, pivotal, serious, worrisome, extreme, major, significant, dangerous, vital, crucial, important, critical, urgent, risky, severe, worrying, stringent, unprecedented

For the following steps, we are using the pre-trained model word2vec-google-news-300 within Gensim.

Our list can now be iteratively improved by using the most.similar function.

Subsequently, we find the average vector of the words in our list. To check if this average vector is accurately capturing urgent words, we can apply the most.similar function to the vector and verify that the resulting words are, indeed, words used to convey urgency.

Next, we create an identification function that compares the cosine similarity between a given word and the average vector with a set threshold value. If the cosine similarity exceeds the threshold, then the word fed to the function is classified as an "urgency signifier". If the cosine similarity falls below the threshold, then the word is not classified as an "urgency signifier". 

To identify an appropriate threshold value, we applied the function to all adjectives within a given text and manually checked which value produced results that came closest to our human judgement of which words may be "urgency signifiers". With this approach, we simultaneously tested the general performance of our function in identifying appropriate "urgency signifiers".

Finally, a the identification function is embedded in a function producing the overall proportion of "urgency signifiers" within the entire body of adjectives used in a text. We decided to find proportions of adjectives in a text rather than proportions of words in a text as different authors may have used different writing styles, using different proportions of adjectives overall. 

The average proportion of "urgency signifiers" of texts can then be compared across time periods, and also across different sources.

## Results
**Do our results indicate that we can use simple natural language processing methods to measure communicated urgency?**

*YES!
...and no.*

![Figure 4: Old results](https://raw.githubusercontent.com/Cheshire-Cat94/Cheshire-Cat94.github.io/main/Screenshot%202022-04-06%20at%2002.02.02.png)

Recall our hypotheses from before: 

>H1: *The proportion of urgent words is lower in TP2 than in TP1 and TP3*

>H2: *The proportion of urgent words is lower in TP3 than in TP1*

Originally, we found rather robust evidence for H1, but virtually zero evidence for H2. This is reflected in figure x below, which illustrates how urgency, as predicted, generally speaking is lower in the dull period in between covid waves. We confirmed these graphical findings by running a 2-sided t-test which proved that almost all results were statistically significant. These results were, however, not replicable. Upon testing our code again, we noticed that something likely had changed with regard to our word embedding model, resulting in ever so slightly different similarity scores. However small, those scores significantly affected the statistical robustness and significance of our findings.

![Figure 5: New results](https://raw.githubusercontent.com/Cheshire-Cat94/Cheshire-Cat94.github.io/main/Rplot04.png)

Our revised results maintain that there seems to be a difference in urgency between TP1 and TP2, but reveal significant variation and are not as statistically sound.
 
**New findings**

Comparing these new findings with what we found earlier, it is clear that while the overall trend persists, there is significant variation. This holds true in the most dramatic fashion especially for official government communication, where the variation within the 95 %-confidence interval is so marked that it is possible that there was no change in urgency at all. The relatively small sample size (max n=21) of the government communication data set might clue us as to why we should expect such significant variation. Indeed, when comparing to the exponentially larger Twitter data set (n=1000), sample size seems to provide the most plausible explanation for this volatility. 

Out of all of the sources, The Telegraph displays a rather unusual pattern. What our findings seem to indicate is that The Telegraph news desk were considerably more alarmist than its counterpart at the Guardian during the least severe period, while dropping the subject completely once we entered January of 2021. 

![Figure 6: Box plot SourcesXUrgency Proportion](https://raw.githubusercontent.com/Cheshire-Cat94/Cheshire-Cat94.github.io/main/Rplot03.png)

![Figure 7: Box plot SourcesXUrgency Proportion](https://raw.githubusercontent.com/Cheshire-Cat94/Cheshire-Cat94.github.io/main/Rplot02.png)

The two box plots above may elucidate some of the questions that might have arisen from interpreting the graph above. First of all, they confirm the U-shaped behaviour of The Guardian and Official Government Communication, and they reveal that &mdash; at least in comparison to Twitter &mdash; newspapers and governments alike displayed communicative coherence during most of the pandemic. Certain outliers are in line with surveys that find that citizens found official communication contradictory and confusing. In this regard, 10 Downing Street seems to have released some statements downplaying the severity of the pandemic during the first peak, while fear-mongering during the third [something on lockdowns here?]. 

**Twitter as pandemic tracker**

Unsurprisingly, Twitter proved to showcase the most extreme opinions in either direction. Interesting to note, however, especially in comparison to other types of communication, is that Twitter serves as a rather competent gauge of aggregate public and official opinion. During the first peak, its users did not, as with most governments, seem to have picked up on the fact that the outbreak was particularly serious, which could explain why we do not see any outliers in any direction. Things change during the absolute low. Here, there is a large number of outliers that drag the average urgency score in a negative direction, perhaps suggesting that after having spent a summer sequestered in their rooms, the Twitter user base wanted to enjoy life again. From the second to the third peak, opinion shifts massively. All of those who previously wanted to remove societal restrictions now seem to want to institute the most draconian of safety measures, indicating that, taken with a pinch of salt, Twitter could be a serviceable pandemic tracker [maybe some research on this?]

## Discussion

**At first glance**

We can glean several different things from these results. While not conclusive, they serve as an indication that given more time to explore how urgency thresholds interact with different models, and how the method itself interacts with different models, it may be possible to measure urgency (or really any other type of sentiment for which you could construct a list of representative words) by measuring the cosine distance to an ideal mean.

![Figure 4_Rplot 03](https://raw.githubusercontent.com/Cheshire-Cat94/Cheshire-Cat94.github.io/main/Rplot03.png)

**On the difference between sources**

When moving on from the confusion of the first wave, a clear difference between our liberal and conservative outlets emerges. Interestingly, this difference does not quite go in the direction that we would have expected. While the Guardian largely mirrors how the government communicated about the pandemic, the Telegraph diverged in both more- and less severe directions. Performing spot samples on the data set reveals that a plausible explanation for the unexpected shape of the Telegraph's reporting could be that it reported about other countries' -- specifically those popular among British holiday makers -- to a much larger extent during TP2, while it reported in a more critical fashion about various restrictions as more data about their efficacy had emerged by TP3. For example, an article referred to the Australian and Kiwi lockdowns as 'stop-gap measures', unlikely to keep the virus at bay in the long run. A further possible explanation for the counterintuitive development of the proportions of urgent words over time in the Telegraph dataset is that not the health situation per se, but the corresponding restrictions are framed as urgent. For example, lockdowns could have been framed by the Telegraph as dangerous infringements on individual liberties. Thus, urgent language could be temporally associated with restrictions, which could interfere with our measurement of the association between urgent language with hospitalization rates.

**Different reactions to more data**

Our second hypothesis, that we would observe a kind of fatigue stemming from persistent reporting about the pandemic for several years, was soundly rejected. Instead, most outlets, including the general public, seemed to have understood the seriousness of the virus by the third time period. The Telegraph data does not indicate that it is opposed to this view *per se*, but rather that it reacted to the knowledge we had about the pandemic differently than other sources. Shifting from treating the virus as something to be *fought* to something to be *lived* with could of course be interpreted as a move away from the urgent to the mundane, but flipped on its head the same reaction could be interpreted as treating the virus as so severe that we cannot fight it. If the same methodology were to be used on a bigger dataset, capturing the shift from *prevention* to *mitigation* or *facilitation* could be an important parameter to keep in mind as this likely would shift the surface level urgency of communication downwards while the overall picture might remain the same.

![Figure 5_Rplot 04](https://raw.githubusercontent.com/Cheshire-Cat94/Cheshire-Cat94.github.io/main/Rplot04.png)

**Patterns follow (somewhat) intuitive notions about reliability**

The shape of the data as well as the proportions confirm intuitive or perhaps classical notions about reliability. The Government exhibits both the most significant difference between time periods, and the highest urgency proportions. The Guardian follows a similar shape, suggesting that it (think about official government communication in other channels) largely follows global best available data.
- Findings indicate that it might be possible to capture urgency through simple NLP
- Clear difference between liberal and conservative outlets
- Contrary to H2, most outlets had realised the seriousness of Covid by TP3: no indication of fatigue (unless you are conservative)
- General public slower to catch up to speed
- Pattern follows (somewhat) intuitive notions about reliability

### Limitations

There are several noteworthy limitations to our model. 

Firstly, the Word2Vec model does not take into account the context of a word. This means that using our method, the following two sentences would have the same level of urgency:

> My name is urgent

> My name is not urgent

Negations are only one example of how context may change the meaning, ultimately the urgency of a sentence. Excluding direct negations would have been a way to partially adjust for this issue. However, after having read a few articles contained in our data set, we did not expect a significant share of direct negations. Thus, we did not expect our result to become more reliable by excluding simple negations from our model (e.g. not urgent, not critcal, i.e. negation directly in front of adjective).

Nevertheless, using a more complex, context sensitive model could render our results more reliable, an example being BERT. Training domain-specific models such as BERT, BioBERT or SciBERT, requires enormous amounts of text corpora and time. These domain-specific models can subsequently be fine-tuned to downstream tasks i.e named entity recognition or text classification, requiring less data and time. Relating to our research question, a "Covid-twitter-Bert", using mask language modelling, next sentence prediction, and sentence order prediction, is already in use. It has further proven effective in analyzing data on vaccine sentiment and the maternal vaccine stance in relation to the pandemic (Müller, Salathé & Kummervold, 2020). This model was trained specifically on twitter data, rendering it sub-optimal for our purposes. Keeping in mind that our study included a variety of different data sources, fine-tuning either BERT or NewsBERT (Wu et al., 2021) would in our case be more appropriate, though we actively decided against it given the scope of the assignment.

A further potential point of concern may be that the Word2Vec pre-trained Google Glove used in our analysis is somewhat outdated. A direct alternative to Word2Vec could have been FastText, which is slightly more recent. However, it seems unlikely that the use and linguistic meaning of urgency-related words have changed significantly over the last few years.

Concerning the size of our datasets, especially with respect to official communication, we see that a larger data set would have been necessary to reach conventional significance levels in testing our effect sizes. this could have been done by extending the time periods or by looking at more time periods and grouping "lows" and "highs". 

Lastly, the urgency threshold is a potential weak spot of our model, as the value ultimately was chosen based on trial and error procedure. We were not able to compare outcomes and findings for a range of threshold values chosen, which may be an interesting approach to better understand the robustness of a vector-based approach to measuring urgency. It is further worth noting that the threshold value that seems to appropriately represent human judgement on urgency of words is highly sensitive to the pre-trained model chosen.

## References

Fetzer, T. (2022). Subsidising the spread of COVID-19: Evidence from the UK’S Eat-Out-to-Help-Out Scheme*, The Economic Journal, 132(643), Pages 1200–1217. https://doi.org/10.1093/ej/ueab074

Fletcher, R., Kalogeropoulos, A., Kleis Nielsen, R. (2020). Majority think UK government COVID-19 response worse than other developed countries, almost half say response too focused on protecting the economy. The UK COVID-19 news and information project. https://reutersinstitute.politics.ox.ac.uk/majority-think-uk-government-covid-19-response-worse-other-developed-countries-almost-half-say

Kleis Nielsen, R., Fletcher, R., Kalogeropoulos, A., Simon, F. (2020). Communications in the coronavirus crisis: lessons for the second wave. The UK COVID-19 news and information project. https://reutersinstitute.politics.ox.ac.uk/communications-coronavirus-crisis-lessons-second-wave

Lockyer B, Islam S, Rahman A, Dickerson J,  Pickett K, Sheldon T, Wright J, McEachan R, Sheard L; Bradford Institute  for Health Research Covid-19 Scientific Advisory Group. Understanding  COVID-19 misinformation and vaccine hesitancy in context: Findings from a  qualitative study involving citizens in Bradford, UK. Health Expect.  2021 Aug;24(4):1158-1167. doi: 10.1111/hex.13240. Epub 2021 May 4. PMID:  33942948; PMCID: PMC8239544.

Mason, R. (2022). UK government has abandoned its own Covid health advice, leak reveals. The Guardian. https://www.theguardian.com/world/2022/feb/25/government-has-abandoned-its-own-covid-health-advice-leak-reveals

Müller, M., Salathé, M., Kummervold, P. (2020) COVID-Twitter-BERT: A Natural Language Processing Model to Analyse COVID-19 Content on Twitter. Cornell University. https://arxiv.org/abs/2005.07503

Schwarz, A., Seeger, M. W., Auer, C., Brooke Rogers, M., & Pearce, J. M. (2016). Chapter 4. The Psychology of Crisis Communication. In The Handbook of International Crisis Communication Research. Essay, John Wiley & Sons.  

The Lancet Microbe (Ed.). (2021). UK government's COVID-19 Gamble. The Lancet Microbe, 2(8). https://doi.org/10.1016/s2666-5247(21)00186-5

Wu, C., Wu, F., Yu, Y., Qi, T., Huang, Y., Liu, Q. (2021) NewsBERT: Distilling Pre-trained Language Model for Intelligent News Application. Cornell University. https://arxiv.org/pdf/2102.04887v2.pdf
