# Latent Soundscape: Discovering Song Clusters for Content-Based Music Recommendation

## Overview
This project explores whether songs can be grouped using intrinsic musical and lyrical characteristics rather than relying on traditional genre labels. Using Spotify audio features and song lyrics, we built an unsupervised learning pipeline to discover latent song clusters and demonstrate how those clusters can support a simple content-based recommendation system.

Rather than treating genre as fixed ground truth, this project asks whether songs that sound similar or convey similar lyrical themes can be grouped in a more flexible and scalable way.

## Business Motivation
Genre labels are widely used in music recommendation systems, but they are often subjective, noisy, and incomplete. Manual labeling does not scale well, and many songs—especially niche or newly released tracks—may have unreliable or missing genre metadata.

A clustering-based recommendation system offers a useful alternative by grouping songs according to what they actually sound like and what they express lyrically. This can help surface musically similar tracks across traditional genre boundaries and improve cold-start recommendations.

## Project Objective
The project was designed around five key questions:

1. Do natural clusters of songs emerge from audio and lyrical features without genre labels?
2. How do different unsupervised methods compare in capturing latent song structure?
3. How can discovered clusters support a simple content-based recommendation logic?
4. What engineering choices are required to scale the pipeline to a near-million-track corpus?
5. Can the same feature space also support a lightweight supervised genre-prediction benchmark on a labeled subset?

## Data Sources
This project uses two public Spotify-related datasets:

- A large corpus of Spotify tracks with audio attributes and lyrics
- A secondary SpotifyFeatures dataset containing genre and popularity labels on an overlapping subset

The main corpus contains 955,320 tracks and serves as the foundation for the unsupervised clustering pipeline. The labeled overlap subset is used for external validation and a supervised benchmark.

## Methodology

### 1. Data Cleaning
- Removed duplicate track IDs
- Standardized audio and text fields
- Parsed artist fields
- Cleaned lyrics through normalization and whitespace cleanup

### 2. Feature Engineering
The final feature space combines:

**Audio features**
- danceability
- energy
- loudness
- speechiness
- acousticness
- instrumentalness
- liveness
- valence
- tempo
- duration
- key and mode

**Derived text features**
- words per second
- sentiment score

**Lyrics representation**
- TF-IDF vectorization
- Truncated SVD for dimensionality reduction

These feature blocks were standardized, concatenated, reweighted, and L2-normalized before clustering.

### 3. Modeling
Several unsupervised approaches were evaluated:
- K-Means
- MiniBatchKMeans
- DBSCAN
- Hierarchical Clustering

Although hierarchical clustering and DBSCAN produced stronger internal separation on smaller reduced samples, MiniBatchKMeans was selected as the final deployment model because it scales efficiently to the full dataset and supports practical out-of-sample assignment.

### 4. Recommendation Logic
Given a query song, the system retrieves nearest neighbors in the learned feature space using cosine similarity. Recommendations can also be constrained within the same cluster to improve coherence.

## Key Results
- Processed a corpus of **955,320 songs**
- Selected **k = 8** as the final number of clusters
- Built a scalable full-dataset clustering pipeline using **MiniBatchKMeans**
- Demonstrated interpretable clusters using:
  - numeric profile heatmaps
  - distinctive lyric terms
  - representative songs
- Built a simple content-based recommendation demo using nearest neighbors
- On the labeled subset, a multinomial logistic regression benchmark achieved:
  - **59.1% accuracy**
  - **85.3% top-3 accuracy**

These results suggest that audio and lyrical features contain meaningful structure that can support music discovery and recommendation, even without relying directly on genre labels.

## Example Cluster Themes
Some discovered clusters included patterns such as:
- speech-heavy / fast-lyrics songs
- acoustic / low-energy songs
- upbeat dance-oriented songs
- darker high-energy instrumental regions

This suggests that the learned feature space captures musically and lyrically meaningful distinctions.


