---
layout: default
---

Who does not want to have the secret of creating YouTube videos that will be viewed by thousands or millions people around the globe ? Is there a secret recipe ? Is YouTube's algorithm breakable ? Let us try to answer these questions by bringing you through this webiste.

# What is a viral video ?

Our ultimate goal being to find outlier videos that made more views than others, we first have to define what makes a video viral. This question definitely has many possible answers as many factors come into play. Here are a few of them:

1.  The video has more than a certain number of views (ex. one million)
1.  The video has more views than the channel's past month's average.
1.  The video has a better like/views ratio than other ones.
1.  The video has a better views/subscribers ratio than other ones.

We went with the fourth definition in our work because it would pick videos mostly seen by non-subscribers. This would be explained by the fact that they were either recommended by the YouTube's algorithm or were manually searched for, which we thought was a good description of viral videos.

More specifically, our decision function is as follows:

$$\left(1 - \exp \left(-\frac{\log(views + 1)}{\log(subcribers + 1)} \right) + 0.055*\log_{10}(subscribers + 1) - 0.05 \right)^{2.1} \geq threshold $$

The details of how and why our function looks like this will be omitted for clarity purposes. Videos that satisfy this inequality will be tagged as viral. The value for $threshold$ was chosen so that we had a satisfiable rate of viral videos compared to non-viral ones. In our case, we chose $threshold$ as the 95%-quantile. Thus, 5% of the videos in the dataset are viral, the others are not.
Note that due to limited computational resources, we had to work only on a subset of the total videos. 

# How does the data look like ?

Features: duration, category, title, description, etc
(A lot of them have virality less than one)

We first had a look at the titles, as it is one of the first elements that we notice when seeing a video on Youtube. After some data cleaning (ponctuation, links, tokenisation, stopwords...) we have obtained for the non viral sample the following word cloud:

![Wordcloud1](Figures/title_wordcloud3.png)

and for the viral sample, we got:

![Wordcloud2](Figures/title_wordcloud_viral3.png)

Hum they look very similar... This is because some words like 'video','part', 'new' or 'episode' are very often used in youtube title and are not very specific. To circumvent this issue, we have repeated the same process but without the intersection of the top 200 words from the viral sample and the non viral sample. This way we get for the non viral sample:

![Wordcloud3](Figures/title_wordcloud2.png)

and for the viral sample:

![Wordcloud4](Figures/title_wordcloud_viral2.png)

Fron this, we can see that frequent words specific to the viral sample include trendy topics such as 'prank', 'asmr', 'slime' or 'roblox'. It is not surprising to see words as 'lyrics', 'hit' or 'rhymes' which are related to music videos subject to be watched many times by the same viewers. 
# What makes a video viral ?

This is the question we are all waiting for. Long story short: we do not really know, at least with the features in our dataset.

Like we saw previously, we do not have access to major aspects of the videos, such as its thumbnail, the political context at the time of its upload date, and most importantly its actual content. It is of course very hard to say if a video will get viral if we do not know the content of it ! Viral videos often need to connect people, to be emotionally strong or funny ; but we do not have access to these details. Thus, we can only tell if a video will get viral based on:
 - Its category (Music, Gaming, Politics, Sports,...)
 - Its channel's number of subscribers
 - Its duration
 - Its title's and description's lengths
 - Its number of tags
 - If the title is rather positive or negative

These features were the ones we selected to build our model. After having tried a few ones, the most promising one was a XGBoost model. The results can be summarized in the following confusion matrix:

[Insert confusion matrix]

- Accuracy : 95%
- Precision : 65%
- Recall : 5%
- F1-Score : 9%

Having a precision of 65% means that you have a 65% chance of your video getting viral if our model says it does.  
Having a recall of 5% means that our model will recognize 5% of viral videos as such.  
In this context, we prefer having a more conservative model that will prioritize precision over recall. When our model predicts a video as viral, we want that our video actually gets viral with the highest probability. This led our model to tell that our video gets viral only if it is REALLY viral.

But wait, don't we have an accuracy of more than 95% ? Isn't that a super performance ? This metric measures how many correct samples we recognized in total. Although it may seem high, accuracy is actually not a really good metric when classes are skewed, which is our case. Since we have about 95% of negative elements and 5% of positive ones, it performs hardly better than a dumb classifier only outputting "false".

We could not get better results than this one for the reasons we described above: we miss some very important factors of the video and cannot predict a video's virality only based on a few indicators.

# What should you do ?

You learnt something while reading this datastory: that there is not a universal way to make your video go viral. Videos' virality depends on too many factors that we do not have in our datasets. Being able to make a viral video without content, just with a precise duration, category and title would disprove the Youtube's algorithm's efficiency. It is unfortunately not that simple to break it.


```
This data story was brought to you by Corentin Tissier, Wissam Pheng, Sander Miesen and Pierugo Pace.
```

