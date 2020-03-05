---
layout: post
title: Using GroupBy and Aggregate on Python Dataframes
---

Let's say we have a dataset containing data with movie reviews as shown below. This is stored as a pandas dataframe 'df'.
![groupby]({{ site.baseurl }}/images/reviewsdf.PNG "Data Frame")

To find the critics with the most number of reviews:
```python
#Critics with the most No. of Reviews 
df.groupby(["critic"]).size().reset_index(name='count').sort_values('count',ascending= False).head(10)
```

Output:  
![groupby]({{ site.baseurl }}/images/groupbyone.PNG "Critics with the most No. of Reviews")  

If you want to apply functions on more than one column, we can use the aggregate method as follows:
To get the Movie titles sorted by No. of Reviews and the Mean Freshness (Mean of all the freshness ratings for a movie from all reviews).
```python
result = df.groupby(['title']).agg({'review':'count','fresh':'mean'}).sort_values(['review','fresh'],ascending=False)  
result.columns = ['numReviews','meanFreshness']
result
```  

Output:  
![groupby]({{ site.baseurl }}/images/groupbytwo.PNG "Number of Reviews and Mean Freshness Rating")  

  

