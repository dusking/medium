![alt text](https://github.com/dusking/WordDifficulty/blob/main/cover_image_transcript_ifficulty.png?raw=true)


# Unlocking Transcript Difficulty Of Movies: A Linguistic Analysis

In our previous [article](https://medium.com/@omerduskin/estimating-word-difficulty-in-english-using-python-a-practical-guide-8f6812de5122), 
we explored a practical statistical method for evaluating the difficulty of individual words in the English language. 
Now, let's take that idea a step further and apply it to something we all enjoy - 
evaluating the complexity of the language used in our favorite movies and TV shows.

Have you ever wondered about the words and phrases that make up the dialogue in the films and series you love? 
Whether you're a movie enthusiast, a language lover, or just someone curious about the words we hear on screen, 
this journey into transcript analysis is for you.

## Getting Started

We'll use [OpenSubtitles.com](https://opensubtitles.com/) to retrieve the feature subtitles. 
To make this process seamless via code, we'll employ the [OpenSubtitles.com Python wrapper](https://github.com/dusking/opensubtitles-com). 
This wrapper enables us to search and download subtitles using the OpenSubtitles API. 
If you haven't installed it yet, you can easily do so with pip:

```python
pip install opensubtitlescom
```

Please note that you'll need OpenSubtitles.com API credentials to use this wrapper effectively. 
For API credentials and further details about the OpenSubtitles API, refer to the [OpenSubtitles API documentation](https://api.opensubtitles.com/).

Additionally, ensure you have the Pandas library installed:

```python
pip install pandas
```

We'll leverage the power of Pandas' DataFrame to streamline the process of summarizing and analyzing the words in the subtitles.

## TL;DR: How to Use the Code - A Quick Guide

For easy access to the complete source code of the class, you can find it [here](https://github.com/dusking/WordDifficulty/blob/main/movie_vocabulary_analysis.py).

Using this code is a piece of cake:

1. Start by initializing the `MovieVocabularyAnalysis` class.
2. Execute the `assess_difficulty` method, providing details about the movie or TV show episode you want to analyze.
3. Get the result, which gives you a dictionary with the combined word evaluations.

Here's a simple Python example:

```python
# Initialize the MovieVocabularyAnalysis class
movie_vocabulary_analysis = MovieVocabularyAnalysis()

# Evaluate feature transcript difficulty
movie_vocabulary_analysis.assess_difficulty("Casino Royale", year=2006)
{
    'difficult': '141 (2.1%)',
    'easy': '6140 (92.1%)',
    'moderate': '308 (4.6%)',
    'unclassified': '78 (1.2%)'
}

movie_vocabulary_analysis.assess_difficulty("Friends", season_number=1, episode_number=1)
{
    'difficult': '65 (2.2%)',
    'easy': '2724 (91.9%)',
    'moderate': '155 (5.2%)',
    'unclassified': '20 (0.7%)'
}

movie_vocabulary_analysis.assess_difficulty("spongebob squarepants", season_number=1, episode_number=1)
{
    'difficult': '77 (2.4%)',
    'easy': '2923 (90.6%)',
    'moderate': '152 (4.7%)',
    'unclassified': '76 (2.4%)'
}

movie_vocabulary_analysis.assess_difficulty("The Living Daylights", year=1987)
{
    'difficult': '246 (4.4%)',
    'easy': '4956 (87.8%)',
    'moderate': '296 (5.2%)',
    'unclassified': '149 (2.6%)'
}

movie_vocabulary_analysis.assess_difficulty("Agent Carter", season_number=1, episode_number=2)
{
    'difficult': '223 (4.7%)',
    'easy': '4127 (86.1%)',
    'moderate': '365 (7.6%)',
    'unclassified': '78 (1.6%)'
}
```

This brief guide offers a quick overview of using the MovieVocabularyAnalysis class to evaluate word difficulty 
in movie and TV show transcripts. Interestingly, it appears that older features tend to have a more complex vocabulary.


## Get to Know the MovieVocabularyAnalysis Class

The MovieVocabularyAnalysis class is like your movie language expert. 
It helps us understand how complex the words used in our favorite films and TV shows are.

You know when you're watching a movie, and some words sound really fancy or difficult, while others are simple and easy to understand? 
The MovieVocabularyAnalysis class is like the detective who investigates and tells us which words fall into these categories.

It does this by analyzing the transcripts of movies and TV shows.
It counts the words, checks their complexity, and then gives us a summary. 
It's like a word complexity calculator for movies and TV series!

### Downloading Subtitles: Your Gateway to Word Analysis

The first step in delving into the complexity of words in movies and TV shows is to get the subtitles.
The MovieVocabularyAnalysis class makes this process a breeze. 
It uses a Python wrapper for OpenSubtitles.com, and here's how it works:

You provide the class with the details of the movie or TV show you're interested in, like the title and year for a movie, 
or the name, season number, and episode number for a TV show. Then, 
the class fetches the subtitles from the vast OpenSubtitles database using the [Python wrapper](https://github.com/dusking/opensubtitles-com).

Here's a closer look at how it all happens in Python:

```python
def download_subtitles(self, title, year=None, season_number=None, episode_number=None, index=None) -> str:  
    index = index or 0
    suffix = f"_{index}.srt" if index else ".srt"
    episode_str = f"_s{season_number}_e{episode_number}" if season_number else ""
    filename = title.lower().replace(" ", "_") + episode_str + suffix
    filepath = self.srt_folder / filename
    if not filepath.exists():
        print(f"Retrieving {title} subtitles")
        response = self.opensubtitles.search(query=title, year=year, season_number=season_number,
                                            episode_number=episode_number, languages="en")
        if not response.data:
            print(f"Missing subtitles for {title}")
            return ""
        self.opensubtitles.download_and_save(response.data[index], filename=filepath)
    return str(filepath)
```

Now, armed with the downloaded subtitles, 
we're all set to start evaluating the difficulty of the words in your favorite feature.

## Exploring Word Difficulty

With the subtitles in our hands, the MovieVocabularyAnalysis class begins a linguistic adventure.
It carefully extracts every word from the subtitles and thoroughly assesses their difficulty.
Each word is then categorized as "easy," "moderate," or "difficult."
This in-depth analysis provides valuable insights into the language used in the movie or TV show.

```python
 def analyze_words_from_srt_file(self, filename) -> pd.DataFrame:
    with open(filename) as f:
        srt = f.read()
    subtitles = self.opensubtitles.parse_srt(srt)
    words = Counter()
    for line in subtitles:
        for word in self.extract_words_from_line(line.content):
            words[word] += 1
    df = pd.DataFrame(list(words.items()), columns=['Word', 'Count'])

    # Add a column for word difficulty classification.
    df['Classification'] = df['Word'].apply(self.word_difficulty.evaluate_word_difficulty)

    return df
```

This comprehensive analysis offers a unique window into the complexity of words used in your favorite feature.

## Summarizing the Findings

In the realm of linguistic analysis, statistics play a pivotal role. 
The MovieVocabularyAnalysis class meticulously compiles word counts based on their respective classifications. 
It meticulously calculates the percentage of "easy," "moderate," and "difficult" words found within the subtitles. 
These statistical insights provide a window into the linguistic landscape of the movie.

```python
def summarize_words_classification(self, df) -> dict:
    grouped_df = df.groupby('Classification')["Count"].sum()
    stat = grouped_df.to_dict()
    total = sum(stat.values())
    for key, value in stat.items():
        stat[key] = f"{value} ({round((value / total) * 100, 1)}%)"
    return stat
```

This method offers valuable insights into the linguistic composition of the movie, 
shedding light on the distribution of word difficulties in the subtitles.


## Validating the Subtitles File

Some subtitle files can be corrupted, which may be evident in misspellings or word substitutions, 
such as multiple replacements of the letter "l" with an "i." 
To determine the quality of the subtitles file, we perform a check for unclassified words. 
If the percentage of unclassified words exceeds a predefined threshold, 
we flag the subtitles file as problematic and attempt to download an alternative file.

```python
def is_valid_subtitles_file(self, stat) -> bool:        
    def extract_percentage(stat_value):
        return float(re.search(r'\((.*?)\)', stat_value).group(1).rstrip('%'))

    valid_subtitles_threshold = 5
    unclassified_words_percentage = extract_percentage(stat["unclassified"])
    if unclassified_words_percentage < valid_subtitles_threshold:
        return True
    print(f"Potentially problematic subtitles file, unclassified words: {unclassified_words_percentage}%")
    return False
```

This method is crucial for ensuring the integrity of the subtitles used for linguistic analysis, 
allowing us to identify and replace potentially problematic files.


## Bringing it All Together

Now that we've covered the individual components, let's put it all together in the main function:

```python
def assess_difficulty(self, title, year=None, season_number=None, episode_number=None, attempts=3) -> dict:
    index = 0
    while index < attempts:
        filename = self.download_subtitles(title, year, season_number, episode_number, index)
        if not filename:
            return {}
        df = self.analyze_words_from_srt_file(filename)
        stat = self.summarize_words_classification(df)
        if self.is_valid_subtitles_file(stat):
            return stat
        index += 1
    return {}
```

This main function pulls together all the pieces to evaluate the word difficulty of a movie or TV show transcript, 
ensuring the integrity of the subtitles and providing valuable statistics on word complexity.

## Real-World Applications
The MovieVocabularyAnalysis class offers practical applications beyond linguistic analysis:

- **Language Learning and Education**: For educators and language enthusiasts, 
this tool proves invaluable in creating language learning materials tailored to the audience's proficiency. 
Educational software, language apps, and tutoring materials can adapt to the user's skill level, enhancing the learning experience.

- **Content Recommendation**: Content recommendation systems can leverage word difficulty analysis to provide 
more personalized recommendations. By matching movie scripts with the viewer's language proficiency, 
these systems can suggest movies and shows that align with the viewer's language skills, enhancing the viewing experience.

- **Writing Enhancement**: Writers and content creators can elevate their craft by identifying complex words 
and offering alternative suggestions. This leads to the creation of clearer and more accessible content, 
broadening its appeal to a wider audience.


## Conclusion

Exploring the world of movie transcript analysis has been quite an adventure, 
and the MovieVocabularyAnalysis class has been your trusted companion in this linguistic journey. 
With Python as your tool and a curiosity for unraveling the secrets of movie language, you've dived deep into the heart of language analysis.

As we wrap up this expedition into the captivating world of movie transcript analysis, 
it's worth thinking about all the exciting possibilities this tool unlocks. 
By understanding the complexities of words in movies, you gain a unique insight into the art of storytelling. 
What's on the horizon? The future is filled with intriguing prospects, 
from even more advanced language analysis to a deeper appreciation of the magic of cinema. 
The next time you watch your favorite film, you'll do so with a newfound understanding of the words that shape the story.

Eager to dive into the linguistic world of your beloved movies? Give the MovieVocabularyAnalysis 
class a try and unlock the hidden gems within the transcript of your cherished films.

Happy exploring, fellow linguistic adventurers!
