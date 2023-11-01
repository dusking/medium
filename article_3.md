# How to use TF-IDF to retrieve Most Significant Words of a file - A Practical Python Guide

![alt text](https://github.com/dusking/medium_src/blob/main/img/cover_most_significant_words.png?raw=true)

## What is TF-IDF?

TF-IDF, which stands for Term Frequency-Inverse Document Frequency, is a powerful natural language processing technique 
that plays a pivotal role in extracting the most significant words from a document. This method evaluates the importance 
of a word within a specific document by considering two crucial factors: term frequency (TF) and inverse document 
frequency (IDF). The term frequency quantifies how often a word appears within the document, while the inverse document 
frequency assesses how unique or rare the word is across a larger collection of documents. By combining these two 
metrics, TF-IDF effectively measures the significance of a word in a specific context. It's a key tool in text analysis, 
information retrieval, and content summarization, and understanding how to use it can unlock a world of insights from 
textual data.

## Applying TF-IDF to Movie and TV Show Transcripts

In this practical Python guide, we'll take the concept of TF-IDF and apply it to a real-world scenario: the extraction
of the most significant words from movie and TV show transcripts. By doing so, we'll uncover the essence of these
stories, identifying the words and phrases that define the narrative and characters. This process simplifies content
analysis, enables the creation of content summaries, and provides valuable insights for various applications, from
content creators looking to understand the key themes of a story to data scientists conducting in-depth text analysis.
Our journey begins with the introduction of a Python class, `MostSignificantWords`, which streamlines the application of
TF-IDF to textual data and makes it accessible to a wide range of professionals and enthusiasts.

## Getting Started

We'll use [OpenSubtitles.com](https://opensubtitles.com/) to retrieve the feature subtitles.
To make this process seamless via code, we'll employ
the [OpenSubtitles.com Python wrapper](https://github.com/dusking/opensubtitles-com).
This wrapper enables us to search and download subtitles using the OpenSubtitles API.
If you haven't installed it yet, you can easily do so with pip:

```bash
pip install opensubtitlescom
```

Please be aware that effective use of this wrapper necessitates OpenSubtitles.com API credentials. To obtain your API
credentials and find further information about the OpenSubtitles API, consult
the [OpenSubtitles API documentation](https://api.opensubtitles.com/).

Furthermore, ensure that you have the necessary libraries installed by running the following commands:

```bash
pip install nltk
pip install scikit-learn
```

These libraries, nltk and scikit-learn, are essential for various natural language processing tasks and TF-IDF
vectorization, which we'll use in our analysis.

## TL;DR: Unlocking the Power of TF-IDF with MostSignificantWords

For those eager to dive into the code, the complete source code of the `MostSignificantWords` class is
available [here](https://github.com/dusking/medium_src/blob/main/src/03_most_significant_words.py).

Using this code is a breeze:

1. Begin by initializing an instance of the `MostSignificantWords` class.
2. Employ the `get_tfidf_significant_words` method, supplying details about the list of movies or TV show episodes you
   wish to analyze.
3. Retrieve the results, which provide you with a list of dictionaries, each containing the most significant words for a
   specific feature.

Here's a straightforward Python example to get you started:

```python
most_significant_words = MostSignificantWords()
features = [
    dict(title="fight club"),
    dict(title="goldeneye"),
    dict(title="Casino Royale", year=2006),
    dict(title="Friends", season_number=1, episode_number=1),
    dict(title="Friends", season_number=1, episode_number=2),
    dict(title="Agent Carter", season_number=1, episode_number=1),
    dict(title="Agent Carter", season_number=1, episode_number=2)
]
most_significant_words.get_tfidf_significant_words(features)
[
    {
        'film': 'fight club',
        'words': 'tyler, fuck, ha, ok, go, fight, marla, know, bob, get, yeah, ohh, want, hey, one, like, sir, come, '
                 'right, look, got, durden, club, oh, let, could, paulson, robert, think, would'
    },
    {
        'film': 'goldeneye',
        'words': 'jame, bond, bori, one, goldeney, severnaya, natalya, know, ye, janu, ourumov, good, satellit, go, '
                 'kill, alec, helicopt, set, code, minist, two, minut, defens, england, moneypenni, come, like, '
                 'right, let, three'
    },
    {
        'film': 'Casino Royale',
        'words': 'bond, chiffr, le, go, million, know, monsieur, one, would, mathi, good, well, bet, money, blind, '
                 'think, mr, get, sir, hundr, call, check, jame, fold, man, need, sorri, thank, ye, game'
    },
    {
        'film': 'Friends_s1_e1',
        'words': 'paul, go, know, want, get, like, date, la, guy, okay, got, can, not, look, joey, wine, everybodi, '
                 'coffe, oh, right, think, good, love, one, barri, monica, chachi, thank, yeah, someth'
    },
    {
        'film': 'Friends_s1_e2',
        'words': 'go, know, carol, oh, hi, geller, helen, susan, look, mindi, ross, okay, thank, great, ring, get, got,'
                 'thing, think, well, barri, comedian, lasagna, pregnant, willick, want, yeah, like, lesbian, julia'
    },
    {
        'film': 'Agent Carter_s1_e1',
        'words': 'stark, know, carter, go, got, like, oh, one, right, want, would, howard, peg, well, get, time, '
                 'jarvi, can, not, agent, say, thing, call, miss, mr, think, raymond, find, work, sell'
    },
    {
        'film': 'Agent Carter_s1_e2',
        'words': 'truck, stark, hum, mr, captain, carter, well, america, jarvi, got, would, sigh, know, branni, '
                 'howard, find, go, need, oh, mcfee, music, devic, throat, vita, clear, right, use, ring, bum, chuckl'
    }
]
```

This code effortlessly reveals the most significant words from your chosen movie or TV show features, offering valuable
insights into the textual content.

## Downloading Subtitles: Your Gateway to Transcripts Analysis

To kickstart the process of analyzing transcripts, the initial step is to obtain the subtitles, and we've made this task
a breeze using the [OpenSubtitles.com Python wrapper](https://github.com/dusking/opensubtitles-com).

To reduce API calls and enhance efficiency, we've implemented a local caching system that saves each downloaded srt file
for future use. This means you won't need to repeatedly fetch the same subtitles from the API, saving time and
resources.

Let's take a closer look at how it all unfolds in Python:

```python
 def get_subtitles(self, title, year=None, season_number=None, episode_number=None) -> list:
    suffix = ".srt"
    episode_str = f"_s{season_number}_e{episode_number}" if season_number else ""
    filename = title.lower().replace(" ", "_") + episode_str + suffix
    filepath = self.srt_folder / filename
    if not filepath.exists():
        print(f"Retrieving {title} subtitles")
        response = self.opensubtitles.search(query=title, year=year, season_number=season_number,
                                             episode_number=episode_number, languages="en")
        if not response.data:
            print(f"Missing subtitles for {title}")
            return []
        self.opensubtitles.download_and_save(response.data[0], filename=filepath)
    with open(str(filepath)) as f:
        srt = f.read()
    return self.opensubtitles.parse_srt(srt)
```

This Python code snippet exemplifies how to seamlessly retrieve subtitles, while the local caching system ensures that
downloaded srt files are stored for future reference, optimizing your transcript analysis process and minimizing API
calls.

## Clean the transcript

Before diving into the analysis, a critical pre-processing phase involves filtering out stopwords and expanding
contractions like "I'm" into "I am". This process results in a clean transcript, optimized for meaningful analysis.

Here's the code that accomplishes this:

```python
 def get_cleaned_transcript(self, title, year=None, season_number=None, episode_number=None) -> str:
    transcript = ""
    subtitles = self.get_subtitles(title, year, season_number, episode_number)
    for line in subtitles:
        words = []
        for word in line.words():
            for word_i in contractions_dict.get(word, word).split():
                if word_i in self.stopwords:
                    continue
                words.append(word_i)
        transcript += " ".join(words) + " "
    return transcript
```

This Python code snippet effectively cleans the transcript by eliminating stopwords and resolving contractions,
resulting in a transcript that's well-prepared for in-depth analysis.

## Main function

The primary function, `get_tfidf_significant_words`, takes a list of feature dictionaries as input. It then proceeds to
retrieve and clean the transcripts associated with these features. Once the transcripts are prepared, the function
leverages the `evaluate_most_important_words` method to calculate and return the desired results.

Here's a breakdown of the code:

```python
 def get_tfidf_significant_words(self, features) -> list:
    transcripts = []
    for feature in features:
        title = feature["title"]
        if "season_number" in feature:
            title += f"_s{feature['season_number']}_e{feature['episode_number']}"
        transcript = self.get_cleaned_transcript(**feature)
        transcripts.append({"title": title, "transcript": transcript})
    return self.evaluate_most_important_words(transcripts)
```

This Python function efficiently manages the entire process, from gathering and cleaning the transcripts to calculating
the most significant words. In the following section, we'll delve deeper into the workings of the
`evaluate_most_important_words` method.

## Exploring the evaluate_most_important_words Method

The heart of the analysis lies within the evaluate_most_important_words method. This critical process determines the
most important words in a collection of movie transcripts through the utilization of TF-IDF (Term Frequency-Inverse
Document Frequency). Let's delve into each step:

### 1. Tokenize the movies transcripts

To start, we tokenize the movie transcripts, which involves breaking down the text into individual words or tokens.
Tokenization is a fundamental Natural Language Processing (NLP) task that converts continuous text into discrete units,
facilitating analysis and processing.

We employ the `nltk.word_tokenize()` function from the NLTK library for tokenization. This function splits the text into
a
list of words or tokens based on rules such as whitespace and punctuation marks. For example, the sentence "Once upon a
time in a galaxy far, far away..." becomes the list of
tokens: ["Once", "upon", "a", "time", "in", "a", "galaxy", "far", ",", "far", "away", ...].

Tokenization serves as a crucial initial step for NLP tasks, enabling analysis of individual words, frequency
assessments, and the application of techniques like TF-IDF analysis.

### 2. Create a TF-IDF Vectorizer

To perform TF-IDF analysis effectively, we initialize and configure a TF-IDF (Term Frequency-Inverse Document Frequency)
vectorization tool. The `TfidfVectorizer` class is designed to convert a collection of text documents into a numerical
matrix of TF-IDF features, where each feature represents the importance of a term within the context of the document and
the entire corpus.

The TF-IDF features serve as a numerical representation of words or terms, emphasizing their significance within each
document while considering their relevance across the entire collection. These features are essential for a wide range
of text analysis tasks, from keyword extraction to document similarity measurement.

The TF-IDF vectorizer is initialized with various parameters that control tokenization, stop-word removal, and more.
Additionally, we employ text normalization by using a custom tokenizer function called `custom_tokenizer`.

#### Custom Tokenization: A Closer Look

Tokenization is the process of breaking down text into individual words or tokens, making it more manageable for
analysis. Our custom tokenizer function, `custom_tokenizer`, not only tokenizes the text but also applies stemming, a
text
normalization technique that reduces words to their root or base form.

Here's the code for the `custom_tokenizer` function:

```python
def custom_tokenizer(text):
 words = nltk.word_tokenize(text)

 # Apply stemming
 stemmer = PorterStemmer()
 base_form_stemmed_words = []
 for word in words:
     word = stemmer.stem(word)  # handle steamid words like "he's"
     if "'" in word:
         # skip words like I'm - since they will be break into I and 'm
         continue
     base_form_stemmed_words.append(word)
 return base_form_stemmed_words
```

This custom tokenization process enhances text data preparation for analysis and ensures that words are represented in
their normalized form, allowing the TF-IDF analysis to capture the core meaning of each term.

With the TF-IDF vectorizer configured and the custom tokenizer in place, we're equipped to analyze the movie transcripts
effectively, quantifying the importance of words relative to the entire corpus.

### 3. Fit and transform the transcript

With the TF-IDF vectorizer in place, we can use the fit_transform method to convert the tokenized text (tokens) into a
TF-IDF matrix. Each row represents a movie transcript, and each column represents a unique word or term, with the cell
values indicating the TF-IDF scores for each word in each transcript.

### 4. Get feature names (words) and their corresponding TF-IDF scores

We retrieve the feature names (words) and their associated TF-IDF scores. This information is essential for
understanding the importance of each term within the movie transcripts.

### 5. Evaluate the most important words per film

In this phase, we assemble the result list named `important_words_per_film`. We sort the words by their TF-IDF scores
and
select the top 30 words for each film, as an example.

The result is a list of dictionaries, each containing the film name and its most important words, providing valuable
insights into the textual content.

Here's the complete source code for the function:

```python
 def evaluate_most_important_words(self, movie_transcripts) -> list:
    def custom_tokenizer(text):
        words = nltk.word_tokenize(text)

        # Apply stemming
        stemmer = PorterStemmer()
        base_form_stemmed_words = []
        for word in words:
            word = stemmer.stem(word)  # handle steamid words like "he's"
            if "'" in word:
                # skip words like I'm - since they will be break into I and 'm
                continue
            base_form_stemmed_words.append(word)
        return base_form_stemmed_words

    # 1. Tokenize the movies transcripts
    tokens_vec = []
    for movie_transcript in movie_transcripts:
        tokens = nltk.word_tokenize(movie_transcript["transcript"])
        tokens_vec.append(" ".join(tokens))

    # 2. Create a TF-IDF vectorizer
    tfidf_vectorizer = TfidfVectorizer(tokenizer=custom_tokenizer)

    # 3. Fit and transform the transcript
    tfidf_matrix = tfidf_vectorizer.fit_transform(tokens_vec)

    # 4. Get feature names (words) and their corresponding TF-IDF scores
    feature_names = tfidf_vectorizer.get_feature_names_out()
    tfidf_scores = tfidf_matrix.toarray()

    # 5. Evaluate the most important words per film
    important_words_per_film = []
    for i, score in enumerate(tfidf_scores):
        # Sort words by TF-IDF scores and select the top 30
        important_words = [word for word, score in
                           sorted(zip(feature_names, score), key=lambda x: x[1], reverse=True)[:30]]
        important_words_str = ", ".join(important_words)
        film_name = movie_transcripts[i]["title"]
        important_words_per_film.append({"film": film_name, "words": important_words_str})

    return important_words_per_film
```

This method encapsulates the core of the TF-IDF analysis, enabling you to uncover the most significant words in your
movie transcripts for in-depth exploration and understanding.

## Conclusion

n this down-to-earth guide, we've explored how to dig out the most important words from movie transcripts using a cool
Python tool called `MostSignificantWords`. It's like having a superpower for understanding what really matters in a
bunch
of text.

We learned how to grab subtitles, tidy up the text, and use this TF-IDF thing to spot the words that carry the most
weight in each movie's story. It's like finding the hidden gems of a film's dialogue and theme.

Why is all of this important? Well, it's like having a secret decoder for text. Whether you're a student, a writer, or
just someone who loves movies, understanding what words pop up the most can reveal the heart of a story, the key info in
research, or even boost your search skills.

This guide is just the beginning of a grand adventure. You can take what you've learned here and apply it to all sorts
of projects. The world of words is vast, and there's always more to uncover. So, go ahead, dive into text analysis, and
discover the treasures hidden within words.

Armed with the knowledge from this guide, you're ready to embark on your own journey of text exploration. Whether you're
analyzing your favorite films, studying documents, or just having fun with words, the power of text analysis is now in
your hands. Enjoy the ride!
