## Detecting artist identity in rap lyrics using NLP

This project aims to explore whether the identity of popular rap artists can be predicted by employing supervised and unsupervised learning techniques.

**Text and data mining:**
Initially, Paul Lamere’s [Spotipy](https://github.com/plamere/spotipy) package was used to interface with the Spotify API and pull the discography of a subset* of popular Hip Hop artists. Along with song title, artist name and artist, this package pulls a vast array of song level data including acoustic properties (liveness, loudness, key, valence, tempo, time signature, etc). Crucially, Spotify’s API does not provide lyric data. Accordingly, John Miller’s [Lyric Genius](https://github.com/johnwmillr/LyricsGenius) package was utilized to pull lyrics for each of the song’s pulled by the Spotipy package.

*It should be noted that broader interpretation of the results presented here are limited by the sampling of artists withing this analysis.*

## **Cleaning:**
The rap lyrics scraped using genius.com’s API show a number of inconsistencies. The lyrics are transcribed by various contributors and there is not a consistent style format. Accordingly, issues regarding comparability of the lyrics were presented. Efforts to standardize the lyric format are detailed in the Data Cleaning notebook. The approach utilized included removing punctuation, removing non-song lyrics (descriptions of other sounds, headings for Chorus/Hook, etc). Further to these issues, it should be noted that some of the artist’s music contains a significant number of non-lyric adlibs, shoutouts that may have been included in the lyrics pulled from genius.com. These words were omitted from the BOW generation and where possible, were removed from the data completely.

## Feature generation:

**Bag of Words:**

A Bag of Words (BOW) approach was utilized that represents each song as a multiset of the 300 most common words across the corpus. Further to this, to ensure the words gave a whole corpus picture, any words with a document frequency less than 0.15 were excluded from the analysis. This approach generated 170 words whose frequency were counted across all songs.

As the depth of features generated would present dimensionality issues, a method for reducing dimensionality was required. It has been previously suggested that [PCA is the optimal approach for dimension reduction with PCA](http://cs229.stanford.edu/proj2017/final-reports/5163902.pdf). Accordingly PCA was applied to to BOW results.

PCA was not as effective as hoped in condensing explained variability into the seperate Principal Components. However, due to dimensionality concerns and the strong possibility of overfitting to the BOW results, the first 4 Principal Components were utilized in our models and clusters. These first 4 Principal Components accounted for 12% of the explained variance in the 170 feature deep BOW data.

![pcaimage.png](https://github.com/seanmcmanus13/Portfolio/blob/master/Using%20Doc2Vec,%20TSNE,%20BOW%20and%20PCA%20to%20predict%20artist%20identity%20in%20rap%20songs/Images/pcaimage.png?raw=true)

Of note, is that the some meaning is lost in BOW models due to the fact that they disregards grammar and word order. Accordingly, a variation of the Word2Vec approach called Doc2Vec was used at the song level in order to establish a numeric measure of the themes and ideas at the song level.

**Doc2Vec:**

BOW features have two major weaknesses: [they lose the ordering of the words and they also ignore semantics of the words](https://cs.stanford.edu/~quocle/paragraph_vector.pdf). It should come as no surprise that methods that take these factors into account have been shown to perform better as features in classification tasks.

In order to account for these factors, a Doc2Vec model was utilized. The Doc2Vec logarithm generates a 100th dimension vector representation of the meaning of a piece of text. In short, Doc2Vec generates these vectors by a combination of Word2Vec and gradient descent algorithms. The downside to this approach is that the nature of our text will lead to some noise in the 100th dimensional vector space. As our documents contain on average over 200 words even after removing stop words, the effect of abberantly spelled words should not weigh heavily on our data.

![Our model seems appropriately trained to the corpus](https://github.com/seanmcmanus13/Portfolio/blob/master/Using%20Doc2Vec,%20TSNE,%20BOW%20and%20PCA%20to%20predict%20artist%20identity%20in%20rap%20songs/Images/wordvectors.png?raw=true)

**T-Distributed Stochiastic Neighbor Embedding:**

Finally, T-Distributed Stochiastic Neighbor Embedding (TSNE) was conducted on the DOC2VEC vectors to reduce the 100 dimensional space to a 2 dimensional space.

T-SNE is a non-linear dimensionality reduction technique. One of it's main utility is reducing of high dimensionality vectors to a 2 or 3 dimensional vector space for visualization purposes. In theory, this method should map more similar data points to be closer in the reduced vector space.

## Results:
**Clustering:**


![Eminem songs show similar but distinct clustering to those of Kendrick Lamar](https://github.com/seanmcmanus13/Portfolio/blob/master/Using%20Doc2Vec,%20TSNE,%20BOW%20and%20PCA%20to%20predict%20artist%20identity%20in%20rap%20songs/Images/emken.png?raw=true)

![A comparison between Busta Rhymes and Eminem shows even more distinct clustering](https://github.com/seanmcmanus13/Portfolio/blob/master/Using%20Doc2Vec,%20TSNE,%20BOW%20and%20PCA%20to%20predict%20artist%20identity%20in%20rap%20songs/Images/bustaem.png?raw=true)

Clustering results using DOC2VEC and TSNE showed that songs for clustered together depending on the artist but there was significant overlap. 

Accordingly, TSNE results were plotted by artist level centroids. The graph below presents these results with the size of each datapoint representing the standard deviation along the X-axis (which closely approximates the standard deviation along the Y-axis).

![enter image description here](https://github.com/seanmcmanus13/Portfolio/blob/master/Using%20Doc2Vec,%20TSNE,%20BOW%20and%20PCA%20to%20predict%20artist%20identity%20in%20rap%20songs/Images/Doc2Vec%20TSNE%20centroids%20plotted%20by%20artist%20(size%20corresponds%20to%20standard%20deviation%20along%20x-axis).png?raw=true?raw=true)


The TSNE plot above suggests that there are artist level differences in DOC2VEC results, but these differences aren't sufficient to form clearly resolved clusters at the artist level.

Vader was also used to explore if TSNE results for DOC2Vec were representative of song sentiment or any intrinsic meaning. 

![enter image description here](https://github.com/seanmcmanus13/Portfolio/blob/master/Using%20Doc2Vec,%20TSNE,%20BOW%20and%20PCA%20to%20predict%20artist%20identity%20in%20rap%20songs/Images/Doc2Vec%20TSNE1%20plotted%20against%20Vader%20compound%20sentiment.png?raw=true?raw=true)

![enter image description here](https://github.com/seanmcmanus13/Portfolio/blob/master/Using%20Doc2Vec,%20TSNE,%20BOW%20and%20PCA%20to%20predict%20artist%20identity%20in%20rap%20songs/Images/Doc2Vec%20TSNE2%20plotted%20against%20Vader%20compound%20sentiment.png?raw=true?raw=true)

TSNE1 was weakly positively correlated with Vader sentiment scores and TSNE2 was weakly negatively correlated. As [previously suggested in the literature](https://distill.pub/2016/misread-tsne/),  the distance between TSNE results are often not reflective of meaning. These results are supportive of this observation.

A Gaussian Mixture Clustering was also applied which did not lead to clear clustering at the artist level:

![enter image description here](https://github.com/seanmcmanus13/Portfolio/blob/master/Using%20Doc2Vec,%20TSNE,%20BOW%20and%20PCA%20to%20predict%20artist%20identity%20in%20rap%20songs/Images/gmversusartists.png?raw=true)

Clustering results matrix:

|                      |2Pac|Big Daddy Kane|Busta Rhymes|Common|Dilated Peoples|E-40|Eminem|J.Cole|Jeezy|Kanye West|Kendrick Lamar|Mac Miller|Nas|OutKast|Snoop Dogg|T.I.|The Notorious B.I.G.|All|
|----------------------|----|--------------|------------|------|---------------|----|------|------|-----|----------|--------------|----------|---|-------|----------|----|--------------------|---|
|2Pac                  |0   |0             |2           |0     |46             |3   |1     |1     |1    |4         |1             |0         |0  |1      |2         |1   |1                   |64 |
|Big Daddy Kane        |0   |10            |0           |1     |7              |0   |0     |0     |2    |20        |2             |0         |0  |0      |0         |0   |0                   |42 |
|Busta Rhymes          |1   |0             |0           |0     |41             |0   |1     |2     |5    |7         |1             |0         |7  |1      |0         |1   |1                   |68 |
|Common                |0   |7             |0           |0     |5              |1   |3     |1     |5    |14        |2             |4         |1  |0      |1         |0   |0                   |44 |
|Dilated Peoples       |1   |10            |0           |0     |4              |0   |0     |7     |4    |6         |1             |0         |1  |1      |1         |4   |0                   |40 |
|E-40                  |0   |0             |0           |0     |14             |0   |3     |1     |5    |9         |0             |1         |0  |2      |1         |3   |0                   |39 |
|Eminem                |2   |7             |2           |2     |39             |2   |1     |0     |0    |2         |3             |1         |2  |1      |0         |0   |1                   |65 |
|J.  Cole              |3   |1             |1           |0     |17             |0   |1     |2     |1    |12        |0             |0         |0  |1      |0         |0   |0                   |39 |
|Jeezy                 |1   |1             |0           |0     |22             |1   |3     |6     |3    |10        |0             |0         |0  |2      |1         |1   |1                   |52 |
|Kanye West            |0   |7             |0           |1     |19             |1   |1     |3     |6    |18        |3             |3         |0  |3      |1         |4   |0                   |70 |
|Kendrick Lamar        |1   |3             |0           |2     |16             |2   |3     |1     |2    |7         |0             |0         |0  |1      |0         |1   |0                   |39 |
|Mac Miller            |0   |6             |0           |0     |14             |0   |0     |3     |2    |21        |2             |1         |1  |2      |0         |4   |1                   |57 |
|Nas                   |1   |3             |0           |0     |44             |1   |2     |3     |3    |15        |2             |3         |2  |2      |1         |2   |1                   |85 |
|OutKast               |0   |2             |0           |0     |14             |0   |3     |2     |8    |11        |1             |1         |1  |1      |1         |0   |1                   |46 |
|Snoop Dogg            |2   |3             |0           |0     |41             |0   |3     |0     |5    |4         |0             |0         |2  |4      |1         |1   |1                   |67 |
|T.I.                  |0   |2             |1           |0     |21             |2   |2     |3     |0    |13        |1             |0         |1  |2      |0         |0   |2                   |50 |
|The Notorious B.I.G.  |0   |0             |3           |0     |15             |1   |0     |0     |2    |2         |0             |0         |1  |0      |1         |1   |0                   |26 |
|All                   |12  |62            |9           |6     |379            |14  |27    |35    |54   |175       |19            |14        |19 |24     |11        |23  |10                  |893|


**Modelling:**
Random Forest and a simple Multi Layer Perceptron (MLP) model were utilized to investigate whether these models could accurately predict artist identity using Doc2Vec, BOW/PCA and song sentiment as inputs.

Data was split into a 75% training group and a 25% test group.

As previously noted in the literature, Random Forest and BOW led to extreme overfitting with scores of 100% for the training group and 40% for the test group. These results are presented in the *Feature engineering, clustering and modelling* notebook in this repository.

A simple MLP model lead to a lower training score of 48% and a score on the test holdout group of 41%. These scores represent a 700% improvement over a random guess and as there is some level of consistency between the test and training scores, this indicates that this model may be somewhat generalized and may be suited to this task than the Random Forest model. Weaknesses of this model seem to be that it does a better job of picking out certain artists. For example, it seems to struggle with identifying lyrics of Kanye West, Kendrick Lamar and J.Cole. This may be an artifact of class imbalance in training data or it could be reflective of contributions of feature artists  or writers in their songs. The results for the test data are presented below:

|FIELD1              |2Pac|Big Daddy Kane|Busta Rhymes|Common|Dilated Peoples|E-40|Eminem|J. Cole|Jeezy|Kanye West|Kendrick Lamar|Mac Miller|Nas|OutKast|Snoop Dogg|T.I.|The Notorious B.I.G.|All|
|--------------------|----|--------------|------------|------|---------------|----|------|-------|-----|----------|--------------|----------|---|-------|----------|----|--------------------|---|
|2Pac                |8   |1             |0           |0     |0              |0   |0     |0      |0    |0         |1             |0         |3  |0      |0         |0   |0                   |13 |
|Big Daddy Kane      |0   |5             |0           |2     |0              |0   |1     |0      |0    |2         |0             |0         |0  |1      |0         |0   |0                   |11 |
|Busta Rhymes        |1   |2             |15          |0     |0              |0   |1     |0      |0    |0         |1             |1         |1  |1      |1         |0   |2                   |26 |
|Common              |0   |0             |0           |5     |0              |0   |0     |0      |0    |0         |0             |0         |1  |1      |0         |0   |0                   |7  |
|Dilated Peoples     |1   |0             |0           |0     |7              |0   |0     |0      |0    |1         |0             |0         |3  |0      |0         |1   |0                   |13 |
|E-40                |0   |0             |0           |1     |0              |1   |1     |0      |2    |1         |0             |3         |0  |0      |3         |1   |1                   |14 |
|Eminem              |1   |1             |0           |1     |0              |0   |17    |0      |2    |1         |1             |0         |0  |0      |0         |0   |1                   |25 |
|J. Cole             |0   |0             |2           |0     |0              |0   |0     |3      |5    |4         |1             |0         |1  |0      |0         |2   |0                   |18 |
|Jeezy               |0   |0             |0           |0     |0              |2   |0     |2      |2    |2         |1             |3         |1  |0      |0         |0   |0                   |13 |
|Kanye West          |0   |1             |0           |2     |0              |1   |1     |2      |0    |3         |0             |6         |1  |0      |0         |1   |0                   |18 |
|Kendrick Lamar      |1   |0             |1           |0     |1              |0   |1     |0      |2    |2         |1             |3         |1  |0      |1         |2   |0                   |16 |
|Mac Miller          |0   |1             |0           |1     |1              |1   |0     |1      |1    |0         |0             |3         |2  |0      |0         |0   |0                   |11 |
|Nas                 |0   |1             |2           |4     |2              |1   |1     |1      |1    |3         |1             |0         |10 |3      |0         |2   |1                   |33 |
|OutKast             |0   |0             |0           |1     |5              |1   |1     |2      |1    |1         |0             |0         |1  |3      |1         |1   |0                   |18 |
|Snoop Dogg          |2   |0             |2           |0     |1              |0   |1     |0      |2    |0         |0             |1         |1  |1      |10        |3   |1                   |25 |
|T.I.                |0   |1             |0           |0     |0              |2   |1     |2      |6    |2         |0             |4         |3  |0      |2         |4   |1                   |28 |
|The Notorious B.I.G.|1   |0             |1           |0     |0              |0   |1     |0      |1    |1         |0             |0         |1  |1      |1         |0   |1                   |9  |
|All                 |15  |13            |23          |17    |17             |9   |27    |13     |25   |23        |7             |24        |30 |11     |19        |17  |8                   |298|

## Future work:

Future iterations of this project would be well advised to consider the following:

 - Containerize and full automate the song data/lyric functions.
- Increase corpus sample size from 1250.
- Consider reducing dimensionality of Doc2Vec vector space before applying T-SNE. Furthermore, outputting T-SNE results in 3 dimensions and graphing in 3 dimensions has shown potential for better resolution of clusters (work not presented here). This should be further investigated.
- Optimization of dimension reduction of BOW results need further investigation. 
- Consider utilization of auditory properties as features in predictive models.

