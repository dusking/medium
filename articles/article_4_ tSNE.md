# Using t-SNE for Movie Transcript Analysis: A Practical Guide using Python

In this article, we'll explore a down-to-earth example of how to use t-SNE in Python to understand movie transcripts
better. There are plenty of technical resources out there that explain t-SNE, but this article is all about hands-on
learning for those who want to apply this cool technique. If you're up for a deep dive, you can check
out this comprehensive
guide: [t-SNE Clearly Explained](https://towardsdatascience.com/t-sne-clearly-explained-d84c537f53a).

## What is t-SNE?

When you're dealing with data in the world of numbers and algorithms, it can sometimes feel like trying to navigate a
complex, mountainous terrain. Each data point is like a peak or a valley, and it's hard to see the big picture when
you're surrounded by so much detail.

That's where t-SNE, short for "t-Distributed Stochastic Neighbor Embedding" comes to the rescue. Think of t-SNE as a
pair of magical glasses for your data. It helps you uncover hidden patterns and structures in your data, even when
you're drowning in numbers and information.

This nifty technique was developed by Laurens van der Maaten and Geoffrey Hinton back in 2008, and it's like a tour
guide for your data. It's all about giving you a broader view of your data without losing sight of the important little
details.

## Why Does It Matter?

So, you might wonder, why does t-SNE matter in our everyday lives? Well, it turns out, it's pretty handy in many
real-world situations:

1. **Seeing the Hidden:** One of the coolest things about t-SNE is its superpower to reveal hidden patterns in your
   data. t-SNE helps uncover the things that might
   not be obvious at first glance.

2. **Grouping Things Together:** Imagine you have a big box of puzzle pieces, and you want to group similar pieces
   together. t-SNE is like your sorting assistant, helping you find pieces that fit together, making it perfect for
   tasks like figuring out what customers have in common or classifying documents and images.

3. **Spotting the Oddballs:** Sometimes, you want to find the odd one out. It's not always easy when you have lots of
   choices. t-SNE shines here by showing you where the unusual stuff is hiding in your data.

## Unveiling Clusters and Textual Similarity in Movies and TV Shows

Imagine you have a vast collection of movies and TV shows, each with its unique storyline, characters, and themes.
You're on a quest to discover hidden patterns and group similar content together. This is where t-SNE steps in as your
trusty guide.

**Clustering Movies and TV Shows:** With t-SNE, you can embark on a journey through your movie and TV show dataset to
uncover clusters of similar content. It's like having a magical magnifying glass that reveals which movies or episodes
share common themes, genres, or textual characteristics.

**The Magic of Textual Similarity:** t-SNE truly shines when you're dealing with textual data. It helps you grasp how
similar the textual content of movies or episodes is. Are there movies with closely related plots or dialogues? t-SNE
provides answers by placing similar items close to each other in the visualization.

## Real-World Impact

The ability to find these clusters and visualize textual similarity among movies and TV shows extends far beyond mere
entertainment. Here's how it can make a real impact:

1. **Enhanced Recommendations:** By identifying movies or episodes with similar themes or dialogue, t-SNE enriches
   recommendation systems, making the experience more insightful and enjoyable.

2. **Deeper Genre and Theme Insights:** For content creators and researchers, t-SNE uncovers the intricate structure of
   your movie or TV show dataset, offering valuable insights for genre analysis, recurring themes, and trend-spotting.

3. **Data Organization:** t-SNE enhances data by revealing clusters and similarities in textual content, making it
   easier to manage, search, and retrieve specific content in your media collection.

4. **Engaging Content Exploration:** Visual exploration of the content space enhances viewer engagement and
   understanding, creating a more enjoyable entertainment experience.

With t-SNE as your ally, you're not just uncovering hidden patterns; you're revolutionizing the way you engage with
movies and TV shows, making it an enriching and insightful experience.

## How to Run t-SNE on a Transcript

Running t-SNE on a transcript involves a series of steps, and the output is a lower-dimensional representation of the
text data that can help you visualize the relationships between different transcripts. Here's a high-level overview of
how to run t-SNE on a transcript:

1. **Data Preprocessing:**
    - Tokenize the transcript into words or phrases.
    - Remove stop words (common words like "the," "and," "is") and perform stemming or lemmatization to reduce words to
      their base form.

2. **Feature Extraction:**
    - Convert the text data into a numerical format. You can use techniques like TF-IDF (Term Frequency-Inverse Document
      Frequency) or word embeddings (e.g., Word2Vec, GloVe) to represent the text.

3. **t-SNE Computation:**
    - Apply the t-SNE algorithm to the feature vectors created in the previous step. You can use libraries like
      scikit-learn in Python to do this.
    - Configure hyperparameters like the number of dimensions in the lower-dimensional space (usually 2 or 3) and the
      perplexity, which controls the balance between preserving local and global relationships.

4. **Visualization:**
    - Plot the t-SNE results in a scatter plot in the lower-dimensional space. Each point represents a transcript, and
      their positions reflect their similarity based on the extracted features.

5. **Interpretation:**
    - Analyze the t-SNE plot to gain insights. Transcripts that are close to each other in the plot share similarities
      in the original text data. You can identify clusters of related transcripts and explore their common themes or
      characteristics.

What t-SNE gives you is a visual representation of your text data in a lower-dimensional space where similar transcripts
are positioned closer together. It helps you explore the structure and relationships within your dataset. You can use
this visualization for tasks like identifying themes, spotting outliers, or understanding patterns in the transcripts,
making it a valuable tool for text data analysis and visualization.

## Let's Put t-SNE into Action

Now, let's get hands-on with t-SNE and see how this dimensionality reduction technique can uncover fascinating insights.

For this practical demonstration, we've gathered a collection of episodes from various TV series and a selection of
movies with similar contexts, such as James Bond films and Star Wars movies. Our goal is to apply t-SNE to the
transcripts of these features and observe the results.

To make it happen, we'll follow the same steps we discussed earlier:

1. **Data Preprocessing:** We'll start by obtaining the subtitle features and tokenizing the transcripts into words.
   Additionally, we'll remove common stop words and transform words into their base forms to ensure consistency.

2. **Feature Extraction:** The next step involves converting the textual data into a numerical format, and we'll
   leverage techniques like TF-IDF.

The first two steps have already been taken care of by the `MostSignificantWords` class, as mentioned in our previous
article: [How to use TF-IDF to retrieve Most Significant Words of a file](https://medium.com/python-in-plain-english/how-to-use-tf-idf-to-retrieve-most-significant-words-of-a-file-a-practical-python-guide-1c0426ba9567)
.

Here is the code for that:

```python
def run(self):
    def name(**feature):
        season = feature.get("season_number", "")
        episode = f" S{season}" if season else ""
        return feature.get("title").title() + episode

    features = [
        dict(title="Black Mirror", season_number=1, episode_number=1, color="black"),
        dict(title="Black Mirror", season_number=1, episode_number=2, color="black"),
        dict(title="Black Mirror", season_number=1, episode_number=3, color="black"),
        dict(title="Black Mirror", season_number=2, episode_number=1, color="black"),
        dict(title="Black Mirror", season_number=2, episode_number=2, color="black"),
        dict(title="Black Mirror", season_number=2, episode_number=3, color="black"),

        dict(title="Two and a Half Men", season_number=1, episode_number=1, color="red"),
        dict(title="Two and a Half Men", season_number=1, episode_number=2, color="red"),
        dict(title="Two and a Half Men", season_number=1, episode_number=3, color="red"),
        dict(title="Two and a Half Men", season_number=2, episode_number=1, color="red"),
        dict(title="Two and a Half Men", season_number=2, episode_number=2, color="red"),
        dict(title="Two and a Half Men", season_number=2, episode_number=3, color="red"),

        dict(title="How I Met Your Mother", season_number=1, episode_number=1, color="pink"),
        dict(title="How I Met Your Mother", season_number=1, episode_number=2, color="pink"),
        dict(title="How I Met Your Mother", season_number=1, episode_number=3, color="pink"),
        dict(title="How I Met Your Mother", season_number=2, episode_number=1, color="pink"),
        dict(title="How I Met Your Mother", season_number=2, episode_number=2, color="pink"),
        dict(title="How I Met Your Mother", season_number=2, episode_number=3, color="pink"),

        # James Bond
        dict(title="Licence To Kill", year=1989, color="green"),
        dict(title="GoldenEye", year=1995, color="green"),
        dict(title="Tomorrow Never Dies", year=1997, color="green"),
        dict(title="The World Is Not Enough", year=1999, color="green"),
        dict(title="Die Another Day", year=2002, color="green"),
        dict(title="Casino Royale", year=2006, color="green"),

        # Start Wars
        dict(title="The Clone Wars", year=2008, color="blue"),
        dict(title="The Phantom Menace", year=1999, color="blue"),
        dict(title="Attack of the Clones", year=2002, color="blue"),
        dict(title="Revenge of the Sith", year=2005, color="blue"),
        dict(title="The Force Awakens", year=2015, color="blue"),

        dict(title="Friends", season_number=1, episode_number=1, color="purple"),
        dict(title="Friends", season_number=1, episode_number=2, color="purple"),
        dict(title="Friends", season_number=1, episode_number=3, color="purple"),
        dict(title="Friends", season_number=1, episode_number=4, color="purple"),
        dict(title="Friends", season_number=2, episode_number=1, color="purple"),
        dict(title="Friends", season_number=2, episode_number=2, color="purple"),
        dict(title="Friends", season_number=2, episode_number=3, color="purple"),
        dict(title="Friends", season_number=2, episode_number=4, color="purple"),

        dict(title="Agent Carter", season_number=1, episode_number=1, color="orange"),
        dict(title="Agent Carter", season_number=1, episode_number=2, color="orange"),
        dict(title="Agent Carter", season_number=1, episode_number=3, color="orange"),
        dict(title="Agent Carter", season_number=1, episode_number=4, color="orange"),
        dict(title="Agent Carter", season_number=2, episode_number=1, color="orange"),
        dict(title="Agent Carter", season_number=2, episode_number=2, color="orange"),
        dict(title="Agent Carter", season_number=2, episode_number=3, color="orange"),
        dict(title="Agent Carter", season_number=2, episode_number=4, color="orange"),
    ]

    names = [name(**item) for item in features]
    colors = [item["color"] for item in features]
    tfidf = MostSignificantWords()
    tfidf_matrix = tfidf.get_tfidf_matrix_for_features(features)
    return self.visualize_tfidf_with_tsne_clusters(tfidf_matrix, names, colors)
```

Now, let's focus on the implementation of steps 3 and 4 with the following Python code:

```python
 def visualize_tfidf_with_tsne_clusters(self, tfidf_matrix, movie_names, colors, n_components=2, perplexity=10):
    # Run t-SNE on the TF-IDF features
    tsne = TSNE(n_components=n_components, perplexity=perplexity, random_state=42)
    tsne_results = tsne.fit_transform(tfidf_matrix.toarray())

    # Create a scatter plot with data points
    plt.figure(figsize=(10, 8))
    plt.scatter(tsne_results[:, 0], tsne_results[:, 1], c=colors)

    # List of cluster labels for each data point.
    color_to_label = {color: i for i, color in enumerate(set(colors)}
    cluster_labels = [color_to_label[color] for color in colors]

    # Add colored circles for clusters
    cluster_ids = np.unique(cluster_labels)
    for cluster_id in cluster_ids:
        cluster_indices = np.where(cluster_labels == cluster_id)[0]
    cluster_center = np.mean(tsne_results[cluster_indices], axis=0)
    cluster_color = colors[cluster_indices[0]]
    cluster_size = len(cluster_indices)
    circle = mpatches.Circle(cluster_center, radius=cluster_size, color=cluster_color, alpha=0.3)
    plt.gca().add_patch(circle)

    # Label the points with movie names
    for i, movie in enumerate(movie_names):
        plt.annotate(movie, (tsne_results[i, 0], tsne_results[i, 1]))

    plt.title("t-SNE Visualization of Movie Transcripts")
    plt.show()
```

In this code, we employ t-SNE to reduce the dimensionality of our TF-IDF features, creating a scatter plot where each
point corresponds to a transcript. We also use colorful circles to highlight clusters of related transcripts and label
the points with the respective movie names.

What you'll witness in this visual journey is the magic of t-SNE bringing together related transcripts and forming
clusters, making it easier to spot common themes and patterns. It's like revealing the hidden connections within your
movie and TV show data, and the possibilities for analysis and exploration are boundless.

## Decoding the t-SNE Plot

Let's dive into the t-SNE plot and decode the fascinating insights it unveils:

![t-SNE Plot](https://github.com/dusking/medium/blob/main/img/tsne_of_movies_transcript.png?raw=true)

It's clear at first glance that features sharing the same context tend to stick together. TV show episodes form
tight-knit clusters, and even within those, the episodes of "Agent Carter" gracefully align by season. The James Bond
films huddle in their own clique, as do the Star Wars movies and Black Mirror episodes. The power of t-SNE is evident as
it seamlessly organizes films based on their textual context.

## Revealing Content Similarity

As we take a closer look at these groups, we discover a goldmine of information by observing how close movies are to
each other on the t-SNE plot. It's like a map that guides us to see the connections and shared characteristics in your
dataset. Here's what this can reveal:

1. **Content Similarity:** When movies are neighbors in the t-SNE plot, they're likely to share content similarities.
   These similarities encompass themes, genres, plots, or dialogues, signifying a close relationship in terms of textual
   content.

2. **Genre or Theme Groups:** The clusters in the t-SNE space can naturally group movies based on their genre or themes.
   For instance, if a cluster predominantly features comedy films, it signals commonalities in their action-packed
   content.

3. **Recommendation Opportunities:** The proximity of movies opens the door to content-based recommendations. If a
   viewer enjoys one movie in a cluster, it's a cue to recommend other cluster members since they share content
   similarities.

4. **Semantic Connections:** Closeness on the t-SNE plot can also reveal semantic connections. Movies with similar word
   choices or dialogues tend to gravitate towards each other, indicating semantic relationships.

It's essential to recognize that t-SNE is a visualization and dimensionality reduction tool. While it provides valuable
insights into your data's structure, it doesn't label clusters explicitly. Interpreting the clusters and proximity often
demands domain knowledge or further analysis to understand why movies are close and what it signifies for your
particular dataset.

## Conclusion: Navigating Movies and TV Shows with t-SNE

In the world of data and technology, understanding complex information can sometimes feel like exploring a vast
landscape. But with t-SNE, we have a pair of special glasses that help us see the hidden connections within our data.

In this article, we've shown you how t-SNE can make sense of movie and TV show transcripts. It's like a map that brings
similar content together, making it easier to spot common themes and relationships.

Imagine you're a fan of movies or TV shows, and you want to discover those hidden connections. t-SNE is like your trusty
guide, helping you find similar episodes or films. It can group them based on their stories, characters, and dialogues.

What's even more exciting is that this isn't just about entertainment. It has real-world benefits too:

1. It makes recommendations smarter, suggesting content you might love.
2. It helps content creators and researchers understand genres and themes better.
3. It makes it easier to organize and search through your media collection.
4. It enhances your viewing experience, making it more enjoyable.

5. So, next time you have a bunch of transcripts to explore, put on your t-SNE glasses and uncover the fascinating
connections hidden within your favorite movies and TV shows. It's like having a magical tool that turns data into
stories you can see and understand. Happy exploring!
