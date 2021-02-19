# How to Design a Trending Algorithm for Twitter

From the very beginning of Twitter, [trending topics](https://twitter.com/search-home) has become one of the core features of this popular product. From Twitter trends, you can easily get what’s popular now. There’s no wonder that many companies like to ask candidates to design a trending algorithm in system design interviews.

We’ve covered this topic a briefly in a previous post – [How to Design Twitter (Part 2)](http://blog.gainlo.co/index.php/2016/02/24/system-design-interview-question-how-to-design-twitter-part-2/). Since this question is so popular that I decided to write an in-depth article about it.

Again, none of us at [Gainlo](http://www.gainlo.co/?utm_source=blog&utm_medium=How to Design a Trending Algorithm for Twitter&utm_campaign=blog) has worked on Twitter trends and different interviewers have their own styles of conducting system design interviews. Therefore, the point of this article is definitely not to give you a standard answer to this question, but give you more ideas about the topic.

 

## Question

Since Twitter is so popular, I will only clarify the question briefly here.

As to design a trending algorithm for Twitter, the system should be able to provide a list of topics that are popular at the current moment. I like to make the problem general and a little bit vague since I want to see how a candidate can approach this kind of open-ended question. For instance, there’s no clear definition of what popular means here.

 

## General Ideas

If you haven’t thought about this problem before, I would recommend you spend at least 15min thinking about it. It’s always better to have your own solution and then compare with others.

Alright, here we go.

A general idea is that let’s use a term (or a word) to represent a topic and it can be a hashtag (like #gainlo) or just a phrase (like Donald Trump). If a term has a huge volume in recent tweets compared to the past, the term should be identified as popular. For example, if millions of people are talking about #gainlo today but in the past only hundreds of people talked about it, #gainlo should definitely be a hot topic at the current moment.

The reason we should compare to past volume is that for some common terms like “Monday” or “weather”, they have a huge volume at any time and shouldn’t be selected as trending in most cases.

To sum up, the basic idea is that for each term, if the ratio of term volume within last several hours and term volume of last X days is high, it will be regarded as a trending topic.

 

## Infrastructure

Of course the trending topics should be displayed instantly, which means we can ask users to wait for an hour so that the system can calculate and rank all the terms. So what the underline infrastructure looks like?

Obviously, the calculation can be costly given the huge amount of tweets everyday. **In this case, we can consider using offline pipelines.**

More specifically, we can keep several pipelines running in the offline that calculates the ratio of each term and output the results to some storage system. The pipelines may refresh every several hours assuming there’s no big difference between a short period of time. So when a user checks the trending topics from the front end, we can just this user with pre-computed results.

The basic approach is really naive, do you have any ideas to improve it? It can be from any perspective, like improve the system cost, or improve the data quality etc.. I’ll illustrate few ideas in the following part of the post.

 

## Absolute term volume

If you simply calculate the ratio as explained above, I’m pretty sure there will be some very weird terms selected. Think about the following scenario, suppose there are only 300 people who tweeted about a weird topic “#gailo-mock-interview” and in the past no one has ever talked about it. The ratio (volume within last few hours / volume within last X days) is 1, which can rank at the top of the list.

Apparently, this is not something we want to show to users. I believe that you’ve already identified the problem here. If the absolute volume is not big enough, some unpopular terms may get picked. You can calculate the ratio like *volume within last few hours / (volume within last X days + 10000)* so that small volume gets diluted. Or you can use a separate signal as absolute term volume score to combine with the ratio.

 

## Influencers

Another idea is that if some topics are discussed by high profile people, they might be more likely to be interesting and popular.

There are many ways to design this algorithm. One approach is to first identify who are high profile users. We can simply use follower number (although there are lots of fake influencers who bought followers). If a topic was tweeted by any influencer, we can count this topic tweeted by multiple times. So just multiply the tweet counts with a parameter based on the popularity of the influencer.

One may argue that we should not give influencers more weight since if a topic is trending, there must be a huge number of normal users talking about it. That might be true. The point here is that you will never know the result until you give it a try. So I’m definitely not saying that this is the right thing to do, but it may be worth to have an experiment.

 

## Personalization

Different people have different taste and interests. We can adjust the trends list according different users. This can be quite complicated since there are so many things you can do to make it personalized.

For instance, you can calculate a relevance score between each topic and the user based on signals including his previous tweets, who he has followed and what tweets he has favorited etc.. Then the relevance score can be used together with the trending ratio.

In addition, location should also be a valuable signal. We may even calculate trending topics for each location (maybe city level).

 

## Summary

There are many more interesting topics I haven’t covered yet. For example, a lot of topics just mean the same thing, how does the system dedup them? Are there other signals like users search history can be integrated?

Trending algorithm is absolutely fun and useful. Although we use Twitter as an example, many things we’ve discussed can also be applied to Facebook or other platforms.