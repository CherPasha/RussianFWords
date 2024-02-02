# Funny Russian Words

Created by **Pavel Chernyshev** (Data analysis) and **Elizaveta Drozdovskaya** (Linguistics) in their free time out of boredom and curiosity in 2021.

------

## Introduction

### What is this project?

This project started as a joke. We were thinking of funny words and laughing about them. At a certain point, we couldn't come up with any new words for our amusement. So we created a Google Form and posted it on different social media – to get a collection of the funniest words.
But, as we (giggling) went through the results, we noticed that these words have a lot in common. So we decided to go further and see if we could decipher the humor behind these words. And maybe even predict what words would be seen as funny.
    
### Contents of this repository

This repository consists of several Jupiter notebooks that serve several purposes and data files at different stages of development.

- **RussianFWords_raw.csv** is the raw data from the experiment (see below). It contains aggregated assessments of funniness for 2250 words and assessments by group.
- **RussianFWords_processed1.csv** is the same data but preprocessed with procedures described in the notebook "GenerateFeatures.ipynb". It has such features as n-grams, phonetics, and lexical tags, and frequency measures.
- **RussianFWords_generateFeatures.ipynb** is the Jupiter notebook that takes in the "RussianFWords_raw.csv" file and gives out the "RussianFWords_processed1.csv". The preprocessing procedures are carefully described in it. Take note that some of the preprocessing, such as the interaction with the corpora takes a lot of time, so the notebook can run for up to 2 hours altogether.
- **RussianFWords_analysis.ipynb** is the Jupiter notebook that includes the analysis and visualizations connected to the data.
- 
***--> IMPORTANT: The analysis notebook is not added yet, will be very soon (it's not translated into english yet)***

### Previous research

The closest thing to what we were doing was done about English words in a series of papers:

- Engelthaler, T., & Hills, T. T. (2018). Humor norms for 4,997 English words. Behavior Research Methods, 50(3), 1116–1124. https://doi.org/10.3758/s13428-017-0930-6
- Westbury, C., & Hollis, G. (2019). Wriggly, squiffy, lummox, and boobs: What makes some words funny? Journal of Experimental Psychology: General, 148(1), 97–123. https://doi.org/10.1037/xge0000467
- Westbury, C., & Hollis, G. (2021). A pompous snack: On the unreasonable complexity of the world’s third-worst jokes. Canadian Journal of Experimental Psychology / Revue Canadienne de Psychologie Expérimentale, 75(4), 327–347. https://doi.org/10.1037/cep0000234
- Westbury, C., Shaoul, C., Moroschan, G., & Ramscar, M. (2016). Telling the world’s least funny jokes: On the quantification of humor as entropy. Journal of Memory and Language, 86, 141–156. https://doi.org/10.1016/j.jml.2015.09.001*

The most interesting to us was the analysis in Westbury & Hollis (2019), which draws on the estimations from Engelthaler & Hills (2018). However, the approach of the authors there is different. While they took a random sample of words and estimated their funnyness, we had a priviously sourced set of *very* funny words that we mixed into the sample.

There are cases to made for each approach as they rest on different assumptions. The english estimates assume that every word rests somewhere on the continuous funnyness scale. While we don't necessarily disagree with the continuous nature of the scale, we see funny words as a very particular cultural-linguistic phenomenon. This makes sense considering the origins of this project: it started when we noticed that each person we knew had a specific set of funny words that they treat not as usual words but as micro-jokes. We wanted to see the difference between these two types of words.

From a technical perspective, the English approach makes the sample more homogeneous thus facilitating the analysis. Our approach on the other hand is supposed to generate a more polorized sample. 

### Data collection
    
Data collection was undertaken in two stages.

**Gathering funny words**

On the first stage we wanted to simply collect words that people see as funny. So we created a Google Form in which we asked people in open form to tell us what they consider as 'funny words'.
The form contained two questions: "What words do you consider funny?" and "Why do you feel that they are funny?".
This form was unswered by 124 people. After cleaning the results we were left with 674 unique words that we call "Originally Funny". 72 of them were suggested by more than one individual.

**Estimating humor**

On the second stage we wanted to compare these originally funny words to other words and get a wider view on what is considered as funny. So we gathered 1575 random words (lemmatized; >4 symbols; no proper names), pulling them from the [General Internet Corpus of Russian](http://www.webcorpora.ru/) and added them to the dataset. We ended up with 2250 words (30% originally funny, 70% random).

Then we created an experiment setting using the platform [IBEX](https://spellout.net/ibexfarm). Every respondent was offered 50 individual words one-by-one in random order, and was asked to answer whether they felt the word was funny or not (binary choice). Before the bulk of the experiment every individual was offered the same 2 words for calibrating (one funny, one unfunny). The proportion of original to random words for each experiment was kept the same as in the dataset (30/70). The experiment also recorded the age, gender, education level, region of residence, and size of the settlement where the individual resides. However, as the project didn't have ANY budget, we used convinience sampling using social media, our friends and relatives. Naturally, the sample ended up pretty biased.

Due to technological problems after a week and 560 answers the survey had to be re-started, nullifying the results. After the re-launching the survey was filled in my 736 individuals. During initial analysis we've generated a number of aggregated measures for every word: count, mean, standard deviation, and also these measures by age (under/over 35) and gender (M/F).

**Pre-processing the words**

The main challenge of this project was to find what features of the words we could assume as variables. We divided all features into two groups: semantic and formal. Here are the features that we tried to extract (with different levels of success):

Formal:
- Length of the word
- Presence of specific letters and combinations of letters (n-gramms)
- Presence of specific sounds
- Frequency (previously the 'frequency_crawler.py' retrieved information about frequency from the National Corpus of Russian, however, after the corpus's website was updated it is not working any more, and I didn't have time to fix it yet)

Semantic
- Lexical features (obsene, jargon, regional etc.)

Sadly, we were not able to replicate the semantic analysis undertaken by Westbury & Hollis (2019) as the measures the are using were not developped for Russian. 

**How to predict funniness?**

To create the target variable 'funny' we used the mean funniness score (mean of all binary assessments made about this word), setting the threshold at 0.5. As a result the relation of funny and unfunny words is 9/91. 
    
Westbury & Hollis (2019) show in their paper that when predicting the funniness of words linear models perform just as well as more complicated instruments. We tried using logistic regressions, Support Vector Machines (SVM), and random forests to predict the funniness score. All instruments yield results of similar quality.
