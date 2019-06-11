## Detecting artist identity from rap lyrics using NLP

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


![Eminem songs show similar but distinct clustering to those of Kendrick Lamar](https://github.com/seanmcmanus13/Portfolio/blob/master/Using%20Doc2Vec,%20TSNE,%20BOW%20and%20PCA%20to%20predict%20artist%20identity%20in%20rap%20songs/Images/emken.png?raw=true)

![A comparison between Busta Rhymes and Eminem shows even more distinct clustering](https://github.com/seanmcmanus13/Portfolio/blob/master/Using%20Doc2Vec,%20TSNE,%20BOW%20and%20PCA%20to%20predict%20artist%20identity%20in%20rap%20songs/Images/bustaem.png?raw=true)
