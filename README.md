# Description

**DATA-X Tinger Slide Deck**
Link: https://docs.google.com/presentation/d/1nPaO_1IeuwvIt6jt_X70j6D0_XfHt5FNj1OgXjfdKL4/edit?usp=sharing

**Mission**\
Tinger’s mission is to provide these cultural adventurers with a simple, yet highly effective means to explore and learn about another culture: through pop music. Tinger recommends English songs to Mandarin music lovers and Mandarin songs to English music lovers.

**How it works**\
The user will be able to input a song, either in English or mandarin and Tinger will extract data and run four different models independently, including the siamese neural network, BPM detector and chord progression on the audio data and K-nn on the metadata. These four different models will each output a similarity score or difference distance. We then calculate a weighted average of these four values to arrive at a final similarity score between the target song and the comparison songs. Next, we filter out all the songs with romance labels different from the target song and finally output the top 5 most similar songs of the opposite language.

# Data

In total, we collected over 450 songs for the database we are using to run our tinger tool.\
Audio files are collected by running a script that downloads MP3 files from YouTube playlists.\
Other English & Mandarin song information - collected the lyrics, chord progression, and other metadata (i.e., year published, gender of artist, solo/group, etc.) manually.


# Models

**1. Metadata: K-Nearest-Neighbors -**\
We used a K-nn model to determine similarity between the songs in terms of their metadata. We focused on 3 different features: the gender of the artist, the year the song was published and a dummy variable for whether the artist is solo or group. We calculated the euclidean distance between the target song and all the songs in our database of the opposite language and ranked their similarity.

**2. Lyrics: Bag-of-Words and Random Forest -**\
For lyrics, we chose a Random forest classifier model that predicted whether a song belongs in the romance category. To train this model, we manually labeled 20% of our database. The model achieved 68% accuracy on the test data and identified the following 10 key works that are most important in differentiating between romance and non romance songs. We then went ahead and ran this model on the rest of the database to create romance labels for all the songs.

**3. Audio: Siamese Neural Networks -**\
Created a SNN via keras. Wrote custom functions to: (1) create training pairs, (2) fetch batches during training, and (3) calculate accuracy using 50-way, 100-shot learning. Afterwards, we wrote functions to calculate similarity scores using the SNN, as well to create necessary graphics for further demonstration.

**4. Audio: Chord Progression -**\
We analyzed and compared the intervals of the 8 main chords in the chorus of each song in the databases. First, we converted those chords to numbers using the Nashville number system. Then, we found the difference between each chord (thus all the lists went from having 8 elements to 7) and made a column in our database with all these differences - these are the intervals that we used for each song. Finally, we ran a SequenceMatcher function comparing the input to every single interval list in the database; we were then able to extract the top 10 most similar songs and use those as our recommendations.

**5. Audio: BPM -**\
BPM and Danceability measures were extracted from raw audio files using the Essentia library developed by the Music Technology Group of Universitat Pompeu Fabra Barcelona. The actual algorithms used to extract the information include TempoTapDegara and Detrended Fluctuation Analysis.


# Usage

**Offline**\
The following are sample lines of code to run our Tinger full stack recommendation model directly.
```
print(tinger('Perfect', eng=True))
print('\n')
print(tinger('雪花落下', eng=False))
print('\n')
```

**Online**\
Tinger’s recommendation model can be accessed via our web app: https://a4fm7jk7kidqp2jf.anvil.app/GE2BAFIVA7CBWAFVOZUKIVQ5 . Our front-end deployment is powered by Anvil, and our “TingerIntegration” Colab notebook acts as our back-end server. As long as the Colab notebook is running, the web app is accessible, and users are able to query functions directly into our Colab notebook through Anvil Cloud Services.


# Notes

**Support**\
Please contact Carin Chang at 21carinc@berkeley.edu if you have any questions or concerns.

**Authors and acknowledgment**\
Ryan Park, Andrew Cheng, Carin Chang, Tianhao Wu, Matilda Ju, Wenjia Tong

**Project status**\
Our Data-X prototype currently focuses on solely English-Mandarin cross recommendations, but our team envisions a future with wider coverage for languages across the world.


# License

MIT License

Copyright (c) [2021] [Tinger]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

