# Estimating Word Difficulty In English Using Machine Learning (ML) - a practical python guide

## Introduction

Some time ago, I wrote an article about "Estimating Word Difficulty in English Using Python". The main idea was pretty
simple: if a word is commonly used and shows up a lot in corpora, it's considered easy.

While this worked to some extent, there were issues. One problem was deciding when a word was easy or hard based on
specific frequency cutoffs. The challenge was that it wasn't clear if these cutoffs were really right.

Also, assuming that words frequency mean the same across different types of corpora just doesn't make sense.
For example, words that are common in web chats might be easy, suggesting that frequently used words in chats
are generally simple. But this idea falls apart when you look at words used in U.S. presidential speeches, where common
words might be more complicated and not necessarily easy.

Realizing these challenges made it clear that a new way of doing things was needed. In this article, I'll introduce a
machine learning (ML) approach to tackle these issues. I'll talk about the `WordDifficultyML` class, which uses a
trained
ML model to estimate word difficulty, providing a more detailed and flexible solution.

## TL;DR: How to Utilize the Code - A Simple Guide

For quick access to the full source code of the class, you can find it here: [WordDifficultyML Source Code](https://github.com/dusking/medium_src/blob/main/src/a05_word_difficulty_ml.py).

Using this code is straightforward:

1. Initialization: Begin by initializing the WordDifficultyML class.
2. Model Setup: Execute the set_model method, which not only sets up the machine learning model but also trains it.
3. Evaluate Word Difficulty: Employ the eval_word method with a given word, and it will furnish you with the assessed difficulty score based on the trained model.

Here's a quick example in Python:

```python
word_difficulty = WordDifficultyML()
word_difficulty.set_model() 

# Evaluate word difficulty
print(word_difficulty.eval_word("Love"))  # Example 1
print(word_difficulty.eval_word("Ratcheted"))  # Example 2
```

Feel free to explore and adapt the code for your specific needs!

## Benefits of Employing Machine Learning in Word Difficulty Estimation

Switching to a machine learning (ML) approach for understanding word difficulty brings real-world advantages that make a
noticeable difference:

- Adaptability to Context:
  Machine learning models, unlike static threshold-based methods, can adapt to different contexts. They learn patterns
  from data, allowing the model to discern nuances in word difficulty across diverse corpora. This adaptability proves
  invaluable when dealing with varied types of texts, such as casual chats or formal speeches.

- Reduced Manual Intervention:
  Traditional methods often require manual setting of thresholds and rules, introducing subjectivity and potential
  inaccuracies. ML automates the learning process, minimizing the need for manual intervention. This not only saves time
  but also enhances the objectivity of the difficulty assessment.

- Enhanced Generalization:
  ML models are designed to generalize well to unseen data. Once trained on a diverse dataset, the WordDifficultyML
  class can effectively evaluate the difficulty of words in new contexts. This generalization ability ensures the
  model's utility across a broad spectrum of language usage scenarios.

- Improved Precision:
  ML models can capture intricate relationships between various features contributing to word difficulty. This allows
  for a more precise estimation compared to simplistic frequency-based rules. The model considers a multitude of
  factors, providing a comprehensive analysis that goes beyond mere word occurrence.

- Scalability:
  ML models can scale effortlessly to handle large datasets and a wide range of words. This scalability ensures that the
  WordDifficultyML class remains efficient and applicable even when dealing with extensive text corpora or expanding
  vocabularies.

In conclusion, the incorporation of machine learning in word difficulty estimation offers a forward-thinking and
adaptive approach. The WordDifficultyML class, leveraging ML techniques, overcomes the limitations of traditional
methods, providing a robust and versatile tool for understanding the complexity of words in different linguistic
contexts.

### WordDifficultyML Initialization

The `WordDifficultyML` class seamlessly integrates various functionalities designed to assess word difficulty. During
initialization, instances of `WordDifficulty` and `WordNetLemmatizer` are created to enhance the class's capabilities.

- **WordDifficulty Integration:**
    - The `WordDifficulty` class, introduced in a previous article, furnishes the `eval_word` function. This function
      evaluates a word's frequency across various corpora, providing valuable insights into its usage.

- **WordNetLemmatizer Usage:**
    - The `WordNetLemmatizer` is employed to transform words into their base form. This step is crucial when the
      original word form proves challenging for accurate assessment, especially in different tenses.

- **Model and Scaler Initialization:**
    - The class initializes placeholders for the ML model (`self.model`) and the feature scaler (`self.scaler`). Both
      these components are pivotal for the machine learning model's functionality.

- **Standardization with StandardScaler:**
    - The `StandardScaler` is used for standardizing features, a crucial step in many machine learning models.
      Standardization ensures that all features contribute uniformly to the model training process, preventing the
      dominance of certain variables.

- **Dynamic Assignment in `set_model` Function:**
    - The actual model and scaler instances will be assigned within the `set_model` function. This dynamic assignment
      allows flexibility and adaptability based on the chosen machine learning model.

Here's the code snippet capturing the initialization:

```
def __init__(self):
  self.word_difficulty = WordDifficulty()
  self.lemmatizer = WordNetLemmatizer()
  self.model = None
  self.scaler = None
```

This initialization sets the stage for a comprehensive and flexible framework within the WordDifficultyML class,
combining linguistic analysis, lemmatization, and machine learning functionalities.

## The set_model Function

The set_model function is the cornerstone of our machine learning pipeline, responsible for training and evaluating the
model. Here's a breakdown of its key steps:

1. **Create an Empty DataFrame:**
    - Prepare a DataFrame to store features extracted from both easy and difficult words. This forms the foundation for
      our model training.

2. **Populate the DataFrame:**
    - Utilize a predefined list of easy and difficult words to extract features. These features include various
      linguistic and contextual properties, enhancing the dataset for classification.

3. **Set Classification Columns:**
    - Designate the classification column as a category, specifying whether a word is categorized as easy or difficult.
      This ensures the model understands the target variable.

4. **Separate Features and Target Variable:**
    - Divide the dataset into features (X) and the target variable (y), which is the word difficulty classification.
      This step prepares the data for training and testing.

5. **Split Data into Training and Testing Sets:**
    - Allocate a portion of the dataset for testing, enabling the assessment of the model's performance on unseen data,
      promoting generalization.

6. **Standardize Features:**
    - Use the `StandardScaler` from scikit-learn to standardize features, ensuring uniform contribution to the machine
      learning model and enhancing its performance.

7. **Create and Train the Model:**
    - Choose a model (Logistic Regression, in this case) and train it on the standardized training data. Training
      involves teaching the model to recognize patterns and relationships within the data.

8. **Make Predictions on the Test Set:**
    - Apply the trained model to the test set to generate predictions. This step is crucial for validating the model's
      effectiveness on previously unseen data.

9. **Evaluate the Model:**
    - Assess the model's performance by calculating accuracy and generating a detailed classification report. This step
      provides insights into how well the model generalizes to new, unseen data and identifies its strengths and
      weaknesses.

Here is the corresponding code:

```python
def set_model(self):
    # 1. Create an empty DataFrame
    df = pd.DataFrame(index=difficult_words,
                      columns=['words', 'wordnet_words', 'movie_reviews', 'reuters', 'brown', 'gutenberg',
                               'webtext', 'nps_chat', 'inaugural', "syllable_count", "classification"])

    # 2. Populate the DataFrame using your function
    for word in easy_words:
        for column, value in self.word_difficulty.eval_word(word).items():
            # Update the DataFrame using the at method
            df.at[word, column] = value
        df.at[word, "syllable_count"] = self.get_syllable_count(word)
        df.at[word, "classification"] = 1
    for word in difficult_words:
        for column, value in self.word_difficulty.eval_word(word).items():
            # Update the DataFrame using the at method
            df.at[word, column] = value
        df.at[word, "syllable_count"] = self.get_syllable_count(word)
        df.at[word, "classification"] = 2

    # 3. set the classification columns as category
    df['classification'] = df['classification'].astype('category')

    # 4. Separate features and target variable
    X = df[['words', 'wordnet_words', 'movie_reviews', 'reuters', 'brown', 'gutenberg', 'webtext', 'nps_chat',
            'inaugural', 'syllable_count']]
    y = df['classification']

    # 5. Split the data into training and testing sets
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

    # 6. Standardize the features
    self.scaler = StandardScaler()
    X_train_scaled = self.scaler.fit_transform(X_train)
    X_test_scaled = self.scaler.transform(X_test)

    # 7. Create a model (you can choose a different model if needed)
    self.model = LogisticRegression(random_state=42)

    # 8. Train the model
    self.model.fit(X_train_scaled, y_train)

    # 9. Make predictions on the test set
    y_pred = self.model.predict(X_test_scaled)

    # 10. Evaluate the model
    accuracy = accuracy_score(y_test, y_pred)
    print(f'Accuracy: {accuracy:.2f}')

    # Print classification report for more detailed evaluation
    print(classification_report(y_test, y_pred))
```

This function encapsulates a comprehensive workflow for training, testing, and evaluating a machine learning model
designed to classify words based on their difficulty.

## Optional models as example

As highlighted earlier, while Logistic Regression serves as our primary machine learning model, it's important to note
that alternative models can be employed based on specific requirements. The syntax for integrating other models is quite
similar, providing flexibility and the opportunity to fine-tune results according to unique needs.

- RandomForestClassifier

The RandomForestClassifier is an ensemble learning method that excels in diverse data scenarios. It's particularly
effective for tasks involving decision-making based on multiple features.

```python
from sklearn.ensemble import RandomForestClassifier

self.model = RandomForestClassifier(random_state=42)
```

- Logistic Regression

Logistic Regression stands out as a straightforward yet effective model, especially well-suited for binary
classification problems. It serves as a reliable baseline model.

```python
from sklearn.linear_model import LogisticRegression

self.model = LogisticRegression(random_state=42)
```

- Support Vector Machines (SVM)

Support Vector Machines (SVMs) are robust models for binary classification tasks, demonstrating proficiency in
high-dimensional spaces. Their versatility makes them suitable for various applications.

```python
from sklearn.svm import SVC

self.model = SVC(probability=True, random_state=42)
```

- Gradient Boosting

Gradient Boosting models are known for their high performance and ability to capture intricate relationships within the
data. They are particularly effective in scenarios where complex patterns need to be identified.

```python
from sklearn.ensemble import GradientBoostingClassifier

self.model = GradientBoostingClassifier(random_state=42)
```

These optional models offer diverse capabilities, allowing you to select the one that aligns best with your specific
objectives and the nature of your dataset. Feel free to explore and experiment with different models to optimize
performance for your particular use case.

## The random_state

The random_state parameter plays a crucial role in machine learning algorithms by initializing the random number
generator. This setting ensures reproducibility, meaning that running the same code with the same random seed should
yield consistent results.

In the following code snippet:

```python
RandomForestClassifier(random_state=42)
```

The number 42 is an arbitrary choice as the seed for the random number generator. You could opt for any integer value,
and as long as you use the same seed, the results should remain consistent across multiple runs. This feature is
particularly valuable for debugging and code sharing, enabling others to replicate your results.

Fun Fact - It's noteworthy that the value 42 itself holds no inherent significance; rather, it has become a humorous
tradition in the programming community. This tradition is partly attributed to its connection with "The Hitchhiker's
Guide to the Galaxy" by Douglas Adams, where the number 42 is humorously identified as the "Answer to the Ultimate
Question of Life, the Universe, and Everything."

## eval_word Function

Once the model is trained, we can use it to evaluate the difficulty of a given word. The process involves three main
steps:

1. Feature Extraction: Extract word features using the word_difficulty.eval_word method.
2. Standardization: Standardize the features using the same scaler applied during training.
3. Prediction: Predict probability scores using the trained model.

If a word appears exceptionally difficult, it might not be a valid word. To address this, a function checks the validity
of the word. If it's not valid, an attempt is made to obtain the base form of the word, and the evaluation process is
rerun.

Here is the refined code:

```python
def is_valid_word(self, word):
    word = word.strip().lower()
    if not word.isalpha():
        return False
    if word in self.word_difficulty.stopwords:
        return True
    if word in self.word_difficulty.wordnet_words:
        return True
    if word in self.word_difficulty.names:
        return False
    return False


def to_base_form(self, word):
    return self.lemmatizer.lemmatize(word)  # reduce words to their base or root form


def eval_word(self, word):
    new_word_features = self.word_difficulty.eval_word(word)
    new_word_features["syllable_count"] = self.get_syllable_count(word)
    print(new_word_features)

    # Convert the dictionary to a DataFrame with a single row
    new_word_df = pd.DataFrame([new_word_features])

    # Standardize the features using the same scaler used during training
    new_word_features_scaled = self.scaler.transform(new_word_df)

    # Predict the probability scores using the trained model
    probability_scores = self.model.predict_proba(new_word_features_scaled)[0]
    difficult_probability = round(probability_scores[1], 2)
    predicted_class = "difficult" if difficult_probability > 0.55 else "easy"

    if difficult_probability >= 0.9 and not self.is_valid_word(word):
        print(f"It seems that '{word}` is not a word (got {difficult_probability})")

        word_base = self.to_base_form(word)
        if word_base != word:
            return self.eval_word(word_base)
        return -1

    print(f"The difficulty-probability score for `{word}` is: {difficult_probability} ({predicted_class})")
    return difficult_probability
```

## In a Nutshell: Unveiling WordDifficultyML's Language Magic

In our journey through word complexity, we've explored the mix of language intricacies and machine learning smarts.
Enter WordDifficultyML, a language-savvy companion that makes sense of word difficulty in various real-world scenarios.

WordDifficultyML goes beyond traditional word counting. It understands that a word's complexity isn't a fixed rule but
varies across different conversations—whether it's a casual chat or a formal speech.

Why split the data into training and testing sets? It's like training a friend to recognize new words. You want them to
get the hang of it without just memorizing specific examples. This helps WordDifficultyML generalize and make sense of
new words it hasn't seen before.

Standardizing features? Think of it as putting all the information on the same scale, so no one detail overshadows the
others. It ensures a fair and balanced judgment of word difficulty.

For language enthusiasts, students, or anyone intrigued by words, WordDifficultyML is your language sidekick. Whether
you're diving into language studies, chatting online, or simply exploring the richness of words, WordDifficultyML is
there to make sense of the diverse world of language, one word at a time. It's the language buddy you never knew you
needed!
