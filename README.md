# shaik_OUDSA5900
Shehnaz Begum Shaik || 113476910 || Spring 2021

# Introduction	
The National Crime Information Center reports that, in 2016, there are 5,712 cases of Missing and Murdered Indigenous Women and Girls (MMIWG). However, The US Department of Justice’s federal missing persons database, NamUs, only logged 116 cases. Despite growing knowledge about violence against women in general, knowledge concerning violence against Indigenous women is limited. The Center for Disease Control and Prevention has reported “Murder” to be the third-leading cause9 of death among American Indian (AI) and Alaska Native (AN) women. 
Despite of the fact that AI/AN women/people are murdered and sexually assaulted at rates as high as 10 times average in certain counties in the US, the crimes fall between jurisdictional cracks, leaving victims and their families without recourse. There are patterns of police dismissing concerned parents and loved ones with excuses. 
In response to this epidemic of violence, the Murdered and Missing Indigenous Women/People (#MMIW, #MMIWG. #MMIP) movement is finally drawing much-needed attention from law enforcement, legislators, and the general public. The hashtags MMIW, MMIWG are widely used across social media to spread awareness and give voice to the stories of missing and murdered Indigenous women and girls. According to a Union Metrics Twitter Snapshot Report, #MMIW tweets can generate several hundred thousand impressions every 4 hours. Most of these tweets use the hashtag to seek attention to their own missing loved one, to cover the local and regional networks with time-sensitive information to share information about police and FBI response to family’s requests for help. Today, using data science tools like sentimental analysis we can analyze, classify, and generate insights with this whole data to understand the population-level emotion on MMIW.

# Project Objective
The overlapping and confusing jurisdictional issues with Native Americans, the policies and processes, leaves the families in a vulnerable state not knowing who to contact or follow up with regarding potential investigations. The objective of this project is to assess people’s emotions on MMIW surrounding their police and criminal justice interactions through their social media tweets using sentimental analysis and Topic Modeling. 

# Data
## Data Ingestion
Social Media is a great source of data for this project as there is a need to analyze public posts to better understand opinions and human interactions. On Twitter, hashtags #MMIW, #MMIWG and its counterpart #MMIWG2 stand for Missing and Murdered Indigenous women and G2 stands for girls and two spirits. These hashtags are widely used to spread awareness and to give voice to the stories that have not been heard before. In this project we seek to analyze social media posts about MMIW/P, MMIWG on Twitter, specifically due to the ease of obtaining historical data from the year 2015 to 2020. Although Twitter restricts the tweet extraction with a limit per user every 15 minutes, the “Academic Research” product access enables the users to obtain historical data as old as from the year 2009 with monthly cap of 10 Million tweets. 
It has been quiet challenging to gain the researcher access for which I had to verify my student status, share my research profile with Twitter developer community and answer several questions related to the project I was working on. With quick communication and follow ups with Twitter the access can be granted within 2 weeks of first follow up. Twitter API v2 comes with flexible filtering options to enable us in deciding the features of interest to obtain in each tweet that we extract like tweet text, id, location etc. This contributed to extracting around 173,225 “original English tweets” (excluding retweets, replies and other languages) across the world. The Fig 1 illustrates the data ingestion method used.

![image](https://user-images.githubusercontent.com/47012176/116958312-ca401500-ac5f-11eb-838f-07766dc838fa.png)

Fig 1. Overview of Data Ingestion method

## Data Exploration
The data is extracted from Twitter API v2, using the endpoint “Full-archive search”. And the data is filtered using the endpoint parameters to obtain original English language tweets without any retweets and replies. Two data frames have been generated with the data obtained from Twitter namely, tweets_frame and user_loc_frame. The tweets_frame that contains 1,73,226 records with features such as tweet id, tweet text, tweet created_date, source, geo location, retweet_count, reply_count, like_count. 
The statistics summary of the numerical columns is as below:
![image](https://user-images.githubusercontent.com/47012176/116958420-30c53300-ac60-11eb-8753-3501b3940b98.png)
Fig 2. Statistics summary of numerical variables in tweets dataframe

The above summary conveys that the average retweet, reply and like counts are 7, 0.39 and 9 respectively. Another result to notice here is maximum retweet count one of the tweets has is 12,550. And the most liked tweet has a like count of 45,688. 
Missing Values:
There was missing data only in one feature i.e., geo that contains the location of the user while tweeting. However, many users disable their location while tweeting that explains the dip in geo column of the dataframe.

![image](https://user-images.githubusercontent.com/47012176/116958447-48042080-ac60-11eb-9398-822e07e40947.png)


Twitter user can be anywhere while tweeting, so this feature is of less significance to our analysis. This is the reason to create the user_loc_frame that contains the user’s generic location they update in their profile.

User Location distribution:
The user_loc_frame contains 4618 records with features about the users such as Full_name, Country, Country code and the longitude latitude coordinates. We can say that there are 4618 unique users who tweet about the topic in English across the world.

![image](https://user-images.githubusercontent.com/47012176/116958454-518d8880-ac60-11eb-8e0f-75e058be3e63.png)

Fig 4. Geographical distribution of users and user count using Tableau
The above Fig 4 visualization from Tableau makes us think why Canada has more users tweeting about MMIW than The United States. With a thorough lookup on the latest demographics report it has been found that as of 2016, The aboriginal population of Canada is 1.67 million1 and it is expected to be between 2.5 to 6 million2 according to IWGIA. This information further leads to another question, why does Canada have more people tweeting about MMIW, with a comparatively less population than the USA.
Tweet Trends:
We are interested in the numerical features like, retweet_count, reply_count and like count to analyze the tweets that are more popular since we are excluding retweets and replies from the data. From the below visualization Fig 5, the volume of retweets and likes seem to increase from the year 2018. And to the right side of the visualization, we can see the tweet that was retweeted and liked most is the same tweet about a missing and murdered woman named Chantel Moore.

![image](https://user-images.githubusercontent.com/47012176/116958463-5e11e100-ac60-11eb-952e-eb7f15b2e835.png)

Fig 5. Retweet and Likes trend since 2015 to 2020
## Data Preparation
The raw data obtained from Twitter was in json format. Features that are needed for analysis were extracted from the json objects and written into pandas data frames.
After creating two data frames out of the raw json file, the primary feature that needed to be filtered was text. The tweet text had URLs, hashtags, usernames, unidentified short form of words etc. These were filtered out along with stop words using NLTK. 
The data had significant number of duplicates. This is mainly because many users, even though they do not retweet, might copy/paste the tweets. By further dropping the duplicate tweets the final data frame, ready to be processed has 12,7849 records.
The clean text was then processed to create additional columns tokenized_tweet and tweet_lemma, which are created by using word_tokenize() and wordNetLemmatizer() from NLTK. 


# Methodology
For obtaining insights from the tweets data, I propose three techniques namely, Sentimental Analysis, Text Summarization and Topic Modeling. Using these techniques, we can understand the users’ tweets sentiments, summarize the large tweets dataset into a simple meaningful paragraph and understand the topics they are tweeting about.

For obtaining insights from the tweets data, I propose three techniques namely, Sentimental Analysis, Text Summarization and Topic Modeling. Using these techniques, we can understand the users’ tweets sentiments, summarize the large tweets dataset into a simple meaningful paragraph and understand the topics they are tweeting about.
 ![image](https://user-images.githubusercontent.com/47012176/116958496-7aae1900-ac60-11eb-95bd-2f9efc3d26a3.png)

Fig 6. Overview of Proposed methods
## Sentimental Analysis using VADER
Valence Aware Dictionary for Sentiment Reasoner (VADER) is a rule-based model to analyze sentiments specifically expressed in social media. It is an efficient model since it is sensitive to both positive and negative polarities and the intensity with which the sentiments are expressed on social media.

Textblob which is built on NLTK is a simple interface that returns the polarity and subjectivity scores, values of ranging from -1 to 1. If the polarity score is less than 0, it is a negative sentiment and if the score is greater than 0, it is a positive sentiment. If the score is zero it is a neutral sentiment. The subjectivity output has a range of values from 0 to 1, referring to the amount of personal opinion and factual information present in the text. 

![image](https://user-images.githubusercontent.com/47012176/116958519-8ac5f880-ac60-11eb-8b6d-9fb4bd85a799.png)

From the above Fig 7, results of both models, it is evident that VADER performs better than textblob since the neutral sentiments are significantly less. Because a sentiment is classified neutral when no emotions are implied.
Word clouds: Sentiment wise, most used words in the tweets

![image](https://user-images.githubusercontent.com/47012176/116958530-944f6080-ac60-11eb-859e-effa4bb3cd70.png)


## Text Summarization using NLTK
When there is significantly large text data, which is associated with movement like MMIW, text summarization helps to identify the tweets that are concise and precise focusing on useful information from the entire text.
Firstly, the preprocessed text with tokenized tweets is used to calculate the word frequencies of entire text data. Next, the weighted frequency of occurrence of the words is calculated. Then, the weighted frequency of words is added to calculate the sentence scores, using which the summary of desired number of highly ranked sentences can be printed.
In order to focus on summary surrounding their police and criminal justice interactions. The text summary algorithm can be provided with a subset of original dataframe. This subset dataframe must consists of tweets that have keywords like, police, court, law, arrest, trial, hearing, legislation etc. 
Below is the output of a summary of 20 sentences that have useful information according to the algorithm. The highlighted parts of the sentences in below summary helped me learn several incidents that have been recorded11-14. The news relevant to the sentences in the summary is in the references section.

```
Out: 'decapitated block away police station indigenous women going missing astonishing rates one listening. lawyer inquiry resigns citing government interference. justice families requires inquiry investigate role police neil macdonald. investigate without looking police conduct steven zhou writes. got really bad cross cultural trainer accuses thunder bay police verbal assault session. racists police force rcmp chief says. cases stop root causes addressed says canadian association chiefs police cbc ca. families share themes patterns emerging st public hearings. things families missing murdered indigenous women seek public hearings approach. youth suicides inquiry police relations centre afn meeting. hearings would light lack mental health services canada north. concern confusion police approach people testified inquiry cbc news. teen held captive tortured dismissed police runaway case inquiry hears. canada federal court hears certification arguments week lawsuit. national inquiry announces dates cross country hearings. human rights watch wants inquiry include police misconduct claims. missing murdered indigenous women girls inquiry holds final hearings ottawa cbc news. cases stop root causes addressed says canadian association chiefs police. win singer susan aglukark publicly names abuser hearings. hearings certify class action canada starts monday'
```

## Topic Modeling using LDA
The last technique used to analyze the twitter data is Topic Modeling. This project falls under the category of unsupervised learning on the twitter dataset, since there is no way to train the data. Using topic modeling clusters of words belonging to set of topics i.e., each topic consisting of one cluster.
One of the most common algorithms used to perform topic modeling is Latent Dirichlet Allocation (LDA). LDA is a way of automatically discovering topics from the content of text data. To LDA, any text data is just a collection of topics where each topic has some particular probability of generating a particular word.  What is the probability that a certain topic ‘T’ generates as many instances of a word ‘W’? This can be determined by training text data as a "bag of words" pulled from a distribution selected by a Dirichlet process.

![image](https://user-images.githubusercontent.com/47012176/116958598-cbbe0d00-ac60-11eb-9f13-3f528c215ae5.png)

Data (clean_text) needs to be preprocessed by tokenizing and lemmatizing. This data is then used to create a corpus and a dictionary of lemmatized text.
Parameters passed to build the Model:
Corpus: Bag of words of lemmatized tweet tokens. 
Num_topics: Number of topics for first run choosing num_topics = 10
Id2word: Dictionary of lemmatized text of tokenized words. (tweet_lemma)
Results and Analysis

```
Out:
Topic 0,
  '0.034*"winnipeg" + 0.030*"red" + 0.017*"w" + 0.013*"dress" + 0.012*"include" + 0.011*"n" + 0.011*"consultation" + 0.011*"fed" + 0.011*"carolyn" + 0.010*"project"'
Topic 1,
  '0.053*"inquiry" + 0.031*"cbc" + 0.024*"news" + 0.023*"case" + 0.017*"via" + 0.017*"today" + 0.015*"pre" + 0.013*"report" + 0.013*"meeting" + 0.012*"video"'
Topic 2,
  '0.054*"family" + 0.030*"one" + 0.020*"found" + 0.014*"name" + 0.013*"say" + 0.012*"inuit" + 0.011*"call" + 0.011*"member" + 0.010*"loved" + 0.010*"year"'
Topic 3,
  '0.019*"story" + 0.015*"get" + 0.014*"need" + 0.011*"work" + 0.011*"u" + 0.009*"justice" + 0.008*"see" + 0.008*"make" + 0.008*"let" + 0.008*"racist"'
Topic 4,
  '0.024*"thanks" + 0.023*"federal" + 0.019*"idlenomore" + 0.019*"american" + 0.018*"latest" + 0.015*"march" + 0.013*"cdnpoli" + 0.012*"memorial" + 0.011*"could" + 0.011*"vancouver"'
Topic 5,
  '0.172*"woman" + 0.118*"indigenous" + 0.101*"missing" + 0.077*"murdered" + 0.031*"girl" + 0.017*"native" + 0.017*"canada" + 0.015*"violence" + 0.009*"day" + 0.009*"aboriginal"'
Topic 6,
  '0.105*"inquiry" + 0.040*"cdnpoli" + 0.025*"police" + 0.024*"national" + 0.022*"canada" + 0.021*"say" + 0.020*"indigenous" + 0.015*"first" + 0.014*"report" + 0.013*"chief"'
Topic 7,
  '0.024*"violence" + 0.022*"elxn" + 0.018*"cdnpoli" + 0.017*"stop" + 0.014*"c" + 0.012*"b" + 0.010*"tear" + 0.010*"afn" + 0.009*"highway" + 0.009*"issue"'
Topic 8,
  '0.030*"sister" + 0.026*"day" + 0.018*"year" + 0.017*"please" + 0.016*"today" + 0.015*"find" + 0.015*"rt" + 0.014*"mmip" + 0.014*"last" + 0.014*"old"'
Topic 9,
  '0.017*"indigenous" + 0.017*"canada" + 0.016*"people" + 0.016*"canadian" + 0.013*"right" + 0.012*"child" + 0.012*"genocide" + 0.012*"indian" + 0.011*"want" + 0.010*"act"'
```
![image](https://user-images.githubusercontent.com/47012176/116958630-df697380-ac60-11eb-8f5f-28ff8aa05b71.png)
### Terms in each Cluster/Topic:
![image](https://user-images.githubusercontent.com/47012176/116958643-eb553580-ac60-11eb-8dae-a927d2dabd17.png)

The top relevant terms for each cluster can be viewed here in Fig 11 and overall clustering can be seen in Fig 10. Zooming in is recommended to get a better view of understanding terms in each cluster. The above visualization shows the clusters evenly distributed throughout the four quadrants. There is only one visible overlap, which again poses a question is the num_topics = 10 most optimal for the model? To answer this question, coherence score can be plotted against the number of topics to learn what the optimal number of topics for this model, to perform better.

![image](https://user-images.githubusercontent.com/47012176/116958661-f740f780-ac60-11eb-9a0d-61ce17bd6862.png)


### Evaluating with Coherence score using cv:
CV measure3 is based on a sliding window, a one-set segmentation of the top words and an indirect confirmation measure that uses normalized pointwise mutual information (NPMI) and the cosine similarity.
From the graph Fig 11, optimal num topics is the one, at which the coherence score is maximum before a major drop, which is around 15 in this case.
Final Model Results with num_topics = 15,

![image](https://user-images.githubusercontent.com/47012176/116958678-032cb980-ac61-11eb-838c-738f83843007.png)
Fig 13. LDA clusters with optimal topics
From the above visualization in Fig 13, with number of topics to be 15, the clusters have very minimal overlap, which means that the words are not repetitive among the topics. The words of each topic are analyzed thoroughly and labeled with a Topic name, which is further assigned to all the original tweets to classify the tweets into topics as seen in Fig 14. Most discussed topic seems to be about MMIW in Canada, National inquiry, police consultation, violence, racism, Highway of Tears15 (A news article related to this has been found), family crisis and so on.

![image](https://user-images.githubusercontent.com/47012176/116958695-1049a880-ac61-11eb-87d6-c8a867b268a8.png)

Fig 14. Topic labeling of output
 
# Deliverables
The deliverables of this project are to use data science methods and techniques to analyze the sentiments and emotions of people around MMIW on social media.
Using unsupervised learning methods several insights have been displayed including information on dominant emotion in entire tweet text, some real incidents related to MMIW that occurred in the past through summarization and Most discussed topics among the users tweeting about MMIW. These insights can be used by general public, law enforcement, sociologists and any MMIW/P, MMIWG activists.

# References
1.	https://www.indexmundi.com/factbook/compare/canada.united-states/demographics
2.	https://www.statcan.gc.ca/eng/dai/smr08/2018/smr08_225_2018
3.	https://towardsdatascience.com/evaluate-topic-model-in-python-latent-dirichlet-allocation-lda-7d57484bb5d0
4.	https://www.uihi.org/wp-content/uploads/2018/11/Missing-and-Murdered-Indigenous-Women-and-Girls-Report.pdf
5.	https://www.analyticsvidhya.com/blog/2018/02/natural-language-processing-for-beginners-using-textblob/
6.	https://stackabuse.com/text-summarization-with-nltk-in-python/
7.	https://t-redactyl.io/blog/2017/04/using-vader-to-handle-sentiment-analysis-with-social-media-text.html
8.	https://www.ncai.org/policy-research-center/research-data/prc-publications/VAWA_Data_Brief__FINAL_2_1_2018.pdf
9.	https://www.apa.org/pi/oema/resources/communique/2018/11/standing-sisters
10.	https://www.culturalsurvival.org/news/addressing-epidemic-missing-murdered-indigenous-women-and-girls
News articles related to highlighted text in the output of text summary:
11.	https://www.indexmundi.com/factbook/compare/canada.united-states/demographics -
12.	https://www.statcan.gc.ca/eng/dai/smr08/2018/smr08_225_2018 
13.	https://www.cbc.ca/news/canada/thunder-bay/thunder-bay-police-mmiw-training-1.3758791?__vfz=medium%3Dsharebar     
14.	https://www.cbc.ca/news/canada/north/susan-aglukark-mmiwg-inquiry-1.4547921?__vfz=medium%3Dsharebar    
15.	https://www.globalindigenouscouncil.com/highway-of-tears-innitiative


