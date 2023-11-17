![alt text](https://github.com/dusking/medium_src/blob/main/img/cover_image_1.jpg?raw=true)


# Estimating Word Difficulty In English Using Python: A Practical Guide

In the world of language analysis and text processing, figuring out how hard or easy words are can be pretty handy.
Whether you're creating educational software, building systems to recommend content, or just aiming to write better,
understanding the complexity of English words is essential.

But what exactly makes a word "difficult"? In the context of language analysis, a difficult word is one that poses
challenges due to its uncommon usage, intricate structure, or limited familiarity among the general population.
It's not solely about the length of a word but also about how easily it can be understood and comprehended by a
diverse audience.

In this article, we're going to explore a Python library that helps us with exactly that. But let's be honest,
judging word difficulty isn't always crystal clear. Some words are obviously super tough, like "floccinaucinihilipilification"
– it's long and hardly ever used. On the flip side, you have words like "love," which are short and extremely common,
making them a breeze.

But what about all those words in the middle? That's where things get interesting. We'll use an approach that leans
on word frequency to determine word difficulty. It's not perfect, but it's a practical way to tackle the challenge.
Let's jump in and get a handle on this intriguing topic.


## Getting Started

Before we delve into the code, we need to ensure you have the necessary resources and libraries installed. 
To work with the WordDifficulty class, you'll need to have NLTK and TextBlob installed. 
If you haven't already, you can easily install them using pip:

```python
pip install nltk
pip install textblob
```

These libraries are the foundation for our word difficulty estimation journey.


## TL;DR: How to Use the Code - A Simple Guide

For quick access to the full source code of the class, you can find it here: [WordDifficulty Source Code](https://github.com/dusking/medium_src/blob/main/src/01_word_difficulty.py).

Using this code is straightforward:

1. Initialize the `WordDifficulty` class.
2. Run the `evaluate_word_difficulty` method, providing the word you want to evaluate.
3. Obtain the result, which categorizes the word as "easy," "moderate," or "difficult."

Here's a quick example in Python:

```python
# Initialize the WordDifficulty class
word_difficulty = WordDifficulty()
word_difficulty.download_corpora()  # For the first time

# Evaluate word difficulty
print(word_difficulty.evaluate_word_difficulty("Love"))  # Example 1
print(word_difficulty.evaluate_word_difficulty("Ratcheted"))  # Example 2
```

This concise guide provides a quick overview of how to utilize the `WordDifficulty` class for word difficulty assessment.


## The WordDifficulty Class

The WordDifficulty class provides methods to estimate the difficulty level of an English word.
It's based on word usage frequency in various corpora and linguistic transformations. Leveraging
the Natural Language Toolkit (NLTK) library, renowned for its resources in text analysis.


## Initialization

To get started with the WordDifficulty class, you must initialize it. 
This process involves downloading essential NLTK resources and loading word frequency distributions from various corpora.
These frequency distributions are crucial for estimating word difficulty.

Here's a brief background on the corpora used for direct word checks, providing context for the word difficulty assessment:

- **stopwords:** Contains words commonly used as stopwords, such as "and." 
These words are often filtered out in text processing to focus on more meaningful content. 
We'll classify them as "Easy" words.
- **words:** This corpus comprises a collection of general English words, serving as a reference for common vocabulary,
to verify if a given word is a valid English word.
- **names:** General names dataset, useful for checking if a word is a common name.

We'll also rely on several corpora as sources of text for word frequency checks:

- **movie_reviews:** This corpus contains movie reviews and is useful for assessing the difficulty of words encountered in the context of film-related content.
- **reuters:** Comprising news articles from the Reuters newswire, this corpus helps in evaluating words commonly used in news reports.
- **brown:** A general corpus of American English text, ideal for assessing the difficulty of words in everyday language.
- **gutenberg:** This corpus consists of public domain books, including classic literature and other high-level content, making it valuable for words encountered in literature.
- **nps_chat:** A collection of instant messaging chat sessions, offering insights into words used in informal digital communication.

In the Python code below, you'll find the initialization process, including the setup of word frequency data and the use of the NLTK library's WordNetLemmatizer:

```python
def __init__(self):
    # Set required NLTK resources
    self.stopwords = set(nltk.corpus.stopwords.words('english'))
    self.words = set(nltk.corpus.words.words())
    self.names = set(nltk.corpus.names.words())
    # Initializes word frequency data from various corpora
    self.word_freq = {
        "movie_reviews": FreqDist(nltk.corpus.movie_reviews.words()),
        "reuters": FreqDist(nltk.corpus.reuters.words()),
        "brown": FreqDist(nltk.corpus.brown.words()),
        "gutenberg": FreqDist(nltk.corpus.gutenberg.words()),
        "webtext": FreqDist(nltk.corpus.webtext.words()),
        "nps_chat": FreqDist(nltk.corpus.nps_chat.words()),
    }
    self.lemmatizer = WordNetLemmatizer()
```

This initialization process lays the foundation for our word difficulty assessment, 
ensuring that you have the necessary tools and data in place to perform accurate evaluations.


## Assessing Word Difficulty

The `evaluate_word_difficulty` method serves as the cornerstone of estimating the difficulty of a given word. 
It categorizes words into one of three classes: "easy," "moderate," or "difficult."

The evaluation process unfolds with the creation of multiple word variations:
- **Base Form:** This step involves transforming the word into its base form, 
which may include converting plurals to singulars and verbs to present tense forms.
- **Slang to Normal Form:** For words with slang or non-standard variations, 
we convert them into their standard vocabulary form.

Each word variation is then examined to ensure precise word classification. 
If a variation differs from the original word, its word score is calculated, and its evaluation is considered.

Ultimately, the word receives an overall evaluation score, 
which is then benchmarked against predefined thresholds to determine its classification.

This method provides a nuanced understanding of word complexity, 
considering variations and linguistic transformations to offer a well-informed assessment.

```python
def evaluate_word_difficulty(self, word):    
    word = word.lower()    
    base_form = self.to_base_form(word)
    not_slang_form = self.slang_to_normal(word)

    value = self.score_word_difficulty(word)
    sum_eval = value
    if base_form != word:
        value_base = self.score_word_difficulty(base_form)
        if value_base:
            sum_eval = max(sum_eval, value_base) if sum_eval else value_base
    if not_slang_form != word:
        value_base = self.score_word_difficulty(not_slang_form)
        if value_base:
            sum_eval = max(sum_eval, value_base) if sum_eval else value_base
    if sum_eval is None:
        return "unclassified"
    if sum_eval < 20:
        return "difficult"
    if sum_eval < 110:
        return "moderate"
    return "easy"
```

## Evaluating Word Difficulty

The `eval_word` method plays a critical role in determining the difficulty of a given word. 
It calculates the frequency of the word within various corpora, contributing to the word's overall assessment.

The `score_word_difficulty` method calculates a difficulty score by summing the frequencies across corpora. 
It also performs a quick check to determine if the word is a stop word, which is categorized as "easy," 
or if it's not a recognized word at all. If the word is not found, the method returns None.

In the Python code below, you'll see these methods in action, 
effectively evaluating and scoring words for their level of complexity:

```python
def score_word_difficulty(self, word):
    word = word.lower()
    if word in self.stopwords:
        return 200  # super easy
    eval_result = self.eval_word(word)
    if not self.is_a_word(word, eval_result):
        return
    sum_eval = sum(eval_result.values())
    return sum_eval

def eval_word(self, word):
    word = word.lower()
    return {corpus: freq[word] for corpus, freq in self.word_freq.items()}
```

These methods combine to provide a comprehensive and data-driven approach to word difficulty estimation.

## Validating English Words

The `is_a_word` method offers a straightforward way to determine the validity of a given word as an English word to be evaluated. 
It helps distinguish between valid English words and names or other non-standard text that should not be evaluated.

It's important to note that comparing a word to the words corpus alone may not be sufficient, as it does not include all words. 
Similarly, comparing a word to the names corpus is not enough, as some names may also be valid English words, 
and some names may exist in movie reviews. This method accounts for these considerations when determining word validity.

The method is implemented as follows:

```python
def is_a_word(self, word, eval_result=None):
    if any(value in self.stopwords for value in [word, word.title()]):
        return True
    if any(value in self.words for value in [word, word.title()]):
        return True
    if not eval_result:
        eval_result = self.eval_word(word)
    sum_eval = sum(eval_result.values())
    if sum_eval == 0:
        return False
    non_movie_reviews_eval = sum(value for key, value in eval_result.items() if key not in ["movie_reviews"])
    if non_movie_reviews_eval == 0 and word.title() in self.names:
        return False
    return True
```

This method is a valuable component of the WordDifficulty class, 
ensuring that only valid English words are subject to evaluation, enhancing the precision of word difficulty assessment.

## Practical Applications

The `WordDifficulty` class can be a valuable addition to various applications:

- **Educational Tools:** You can use it to create educational software or games that adapt their content difficulty to the user's level.
- **Content Recommendations:** Improve content recommendations by matching articles or books with a reader's language proficiency.
- **Writing Enhancement:** Enhance your writing by identifying complex words and suggesting alternatives.

## Conclusion

Estimating the difficulty of English words is not a deterministic problem; 
a word can be difficult for one person and easy for another. 
The goal of this article was to present a technique for scoring word difficulty using the NLTK library and corpora.

The `WordDifficulty` class makes this task accessible and straightforward. 
For the full source code of the class, you can find it here: [WordDifficulty Source Code](https://github.com/dusking/medium_src/blob/main/src/01_word_difficulty.py).

In the next article, we will explore how to use the WordDifficulty class to analyze different movies and TV series 
to determine which ones contain more difficult words or easy words. 
This practical application will demonstrate the versatility and power of this approach.
