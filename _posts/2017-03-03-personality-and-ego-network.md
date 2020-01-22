---
layout: post
title:  "Personality Traits and Ego-network Dynamics"
date:   2017-03-03 00:00:00
subtitle: Our latest work on how personality traits affect the way we manage our social relationships published on the PLOS ONE journal!
categories: research
img:  # Add image post (optional)
tags: [research, social signatures, personality, big five] # add tag
---

Me and my advisor **Bruno Lepri**, in collaboration with professors **Jari Saramäki** from Aalto University, Finland, and **Eduardo López** from George Mason University, US, have recently published a new paper on the PLOS ONE journal called **"Personality Traits and Ego-network Dynamics"**. [[link]][plos-pers].

In this work we were interested in understanding whether and how personality traits affect the way we manage our social relationships. As we know, we interact with a wide network of people on a daily basis, and these social relationships play an important role in our lives. The problem is that there are costs in their maintenance. 

But how individuals' personality traits affect this picture? In order to find an answer, we combined detailed mobile phone call records with personality traits scores collected by means of surveys, from participants of the **Mobile Territorial Lab (MTL)** study[^1].
The MTL was a living laboratory that took place in Trento, Italy, in collaboration with **Fondazione Bruno Kessler (FBK)**, **MIT**, **TIM** and **Telefónica**, and observed the lives of 142 parents through multiple channels for more than two years. The data was collected from smartphones, questionnaires and experience sampling probes. In particular, they tracked social interactions (e.g. call and SMS communications), mobility routines and spending patterns. The data was than used to create a multi-layered view of the lives of the study participants.

We modeled the patterns of mobile phone interactions of individuals as **social signatures** defined by Saramäki et al.[^2] as
> "*[...] the way that egos divide their communication efforts among alters and how persistent the observed patterns are over time.*"

Possible explanations on how persistent these communication patterns are over time, and thus how we allocate our efforts in maintaining our social relationships, could be explained by different factors such as
* Time constraints
* Cognitive constraints
* **Differences in personality**

All the individuals' personality has been measured by the Big Five model, a model that describes the personality of an individual with five dimensions defined as Openness to Experience, Conscientiousness, Extraversion, Agreeableness, and Neuroticism, also known as OCEAN.  
(If interested, there is a nice [TED talk][ted-little] by professor Brian Little about the OCEAN personality types, where he focus on the differences between introverts and extroverts). 

During all the analysis, we followed the assumption that individuals in the extreme of the scale for a given trait would exhibit largest differences in communication patterns. Therefore, we identify for each of the Big Five personality traits people falling in the 25th percentile (low personality scores) and the 75th percentile (high personality scores). Thus, for example, for the Extraversion trait we found the most extroverted and the most introverted individuals.

So, to recap, we specifically focused on understanding whether and how personality traits affect the 
* **persistence of social signatures**, namely the similarity of the social signature shape of an individual measured in different time intervals;
* **turnover in egocentric networks**, that is, differences in the set of alters present at two consecutive temporal intervals;
* **rank dynamics** defined as the variation of alter rankings in egocentric networks in consecutive intervals.

### Persistence
Here, we tried to understand whether a particular personality disposition influences the stability of an individual signature over time. We found that the **social signatures of individuals that are more extroverted are slightly less persistent with respect the signatures of individuals that tend to be more introverted**.


### Turnover
The network turnover inside the egocentric networks of the individuals tends to be characterized by the Openness to
Experience trait, where **people that are willing to try new experiences exhibit a higher network turnover with respect to people who are more closed to experience** (Fig 1.). Network turnover seems to be characterized by the Agreeableness personality trait as well, where **generally more likable people have a lower network turnover as compared to disagreeable individuals**.

{% include image.html
   img="assets/images/post/2017_03_03_openness_turnover.png"
   caption="Fig 1. Openness to Experience and network turnover. Individuals who are more open to experience show a higher turnover as compared to the lowest-scoring 25%. Left:  the estimated probability density functions of the distributions of network turnover in egocentric networks.  Right:  violin plots of the same distributions."
%}

### Rank Dynamics
Here, we take a detailed look at what happens inside the network of a focal ego by focusing at the alters
rank dynamics and subsequently we analyze the effect of personality traits on such  dynamics. In order to do this, for two consecutive temporal intervals of each ego, we build a transition matrix as  follows:  if  there  is  a  transition  of  an alter from rank *i* to rank *j* in consecutive intervals then we assign the value 1 to that particular transition.
We then used the transition matrices of egos to represent the alter rank variations of entire subgroups. We simply sum the matrices of all egos in the subgroup and normalize them by the number of egos in that particular subgroup, in order to have probabilities on both rows and columns.
The resulting matrix now contains the alters rank dynamics represented as probabilities of moving up and down rank positions. 


{% include image.html
   img="assets/images/post/2017_03_03_pers_population.png"
   caption="Fig 2. The normalized transition matrix for the entire population. Looking at the diagonal of the transition matrix, it is possible to notice that the top positions are more stable with respect to low-ranked positions"
   width=500
   height="auto"
%}

Fig 2. shows the transition matrix of the entire population under study. We can clearly see that for the top ranks, the probability mass is clearly concentrated on the diagonal, meaning that the top ranks are more stable.  This is expected, since people in the top positions of the network are the people that a particular ego contacts
more frequently, such as for example family members, and these relationships are expected to be more close and stable. 

Adding the personality traits dimension, it seems that **people that show a higher disposition to curiosity and willingness to experiment new things tend to be less stable** regarding the set of alters that they communicate with, as compared to their counterpart. We have similar results with the Agreeableness personality trait, where **agreeable people tend to have a higher spread, namely larger rank dynamics with respect their counterpart**.


{% include image.html
   img="assets/images/post/2017_03_03_openess_rank.png"
   caption="Fig 3. Top row: the transition matrices for the subgroups of individuals with high and low scores in the Openness to Experience personality trait. It is possible to observe that the subgroup of people that display higher scores shows a higher spread with respect to the opposite subgroup, where the “heat” is more concentrated around the diagonal. Bottom  row:  The  2-dimensional  kernel density estimation plots emphasize the fact the rank variations inside the group of people with high scores in the Openness to Experience trait are larger with respect to the opposite subgroup."
%}


We have seen that some traits have effects on the stability of the social signatures as well as network turnover and rank dynamics. On broader terms, our study shows that personality traits affect the ways in which individuals maintain their personal network.

In this post I have briefly presented some of the results of our study, omitting a lot of details and the technical parts. If you are interested, you can find all the details and the results in the paper, freely accessible [[here]][plos-pers].

[^1]: *Centellegher, S., De Nadai, M., Caraviello, M., Leonardi, C., Vescovi, M., ... & Lepri, B.* **"The Mobile Territorial Lab: a multilayered and dynamic view on parents’ daily lives."** EPJ Data Science 5.1 (2016): 3.

[^2]: *Saramäki, J., Leicht, E. A., López, E., Roberts, S. G., Reed-Tsochas, F., & Dunbar, R. I.* **"Persistence of social signatures in human communication."** Proceedings of the National Academy of Sciences 111.3 (2014): 942-947.





[plos-pers]: http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0173110
[ted-little]: https://www.ted.com/talks/brian_little_who_are_you_really_the_puzzle_of_personality