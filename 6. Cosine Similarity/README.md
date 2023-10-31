---
title: Cosine Similarity
updated: 2023-11-01 19:48:47Z
created: 2023-11-01 17:40:20Z

---

# What is Cosine Similarity?
Cosine similarity is a metric used to measure the similarity of two vectors. Specifically, it measures the similarity in the direction or orientation of the vectors ignoring differences in their magnitude or scale. Both vectors need to be part of the same inner product space, meaning they must produce a scalar through inner product multiplication. The similarity of two vectors is measured by the cosine of the angle between them.

# How to calculate Cosine Similarity
We define cosine similarity mathematically as the dot product of the vectors divided by their magnitude. For example, if we have two vectors, A and B, the similarity between them is calculated as
![3b34b27382a490588ec7cd3969a0b39c.png](_resources/3b34b27382a490588ec7cd3969a0b39c.png)

The similarity can take values between -1 and +1. Smaller angles between vectors produce larger cosine values, indicating greater cosine similarity. For example:

- When two vectors have the same orientation, the angle between them is 0, and the cosine similarity is 1.
- Perpendicular vectors have a 90-degree angle between them and a cosine similarity of 0.
- Opposite vectors have an angle of 180 degrees between them and a cosine similarity of -1.

![1b2d9450c46cfee19a3e19d5a445ef79.png](_resources/1b2d9450c46cfee19a3e19d5a445ef79.png)

# Applications
Cosine similarity is beneficial for applications that utilize sparse data, such as word documents, transactions in market data, and recommendation systems because cosine similarity ignores 0-0 matches. Counting 0-0 matches in sparse data would inflate similarity scores. Another commonly used metric that ignores 0-0 matches is Jaccard Similarity.

Cosine Similarity is widely used in Data Science and Machine Learning applications. Examples include measuring the similarity of:
- Documents in natural language processing
- Movies, books, videos, or users in recommendation systems
- Images in computer vision

# Example
Suppose that our goal is to calculate the cosine similarity of the two documents given below.
- Document 1 = 'the best data science course'
- Document 2 = 'data science is popular'

After creating a word table from the documents, the documents can be represented by the following vectors:
![720e4799b9536e45f80814a4d29efe9e.png](_resources/720e4799b9536e45f80814a4d29efe9e.png)
![40df45fec269ebc9842d05090f7fdfa5.png](_resources/40df45fec269ebc9842d05090f7fdfa5.png)

Using these two vectors we can calculate cosine similarity. First, we calculate the dot product of the vectors:
![3f3a43630cf9c8e323923df5c19244ef.png](_resources/3f3a43630cf9c8e323923df5c19244ef.png)
![dc552a2f6516073c4cfa15834df4b7be.png](_resources/dc552a2f6516073c4cfa15834df4b7be.png)