# Insurance Customer Feedback Sentiment Analysis | Python and NLTK

This project uses Python and the Natural Language Toolkit (NLTK) to preprocess insurance customer feedback and perform sentiment analysis using VADER. The workflow includes tokenisation, stop-word removal, and lemmatisation to prepare the text for analysis.
The objective is to classify customer feedback into two sentiment categories: 0 for negative and 1 for positive.

## Steps followed:

- Step 1: Imported the required libraries: pandas for data manipulation, nltk for natural language processing, SentimentIntensityAnalyzer forfor sentiment scoring, stopwords for removing common English words, word_tokenize for tokenisation, and WordNetLemmatizer for lemmatisation.:
```
import pandas as pd
import nltk
from nltk.sentiment.vader import SentimentIntensityAnalyzer
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
from nltk.stem import WordNetLemmatizer
```
- Step 2: Loaded the insurance customer feedback dataset from an Excel file using pandas:
```sql
df = pd.read_excel('Insurance+Customer+Feedback.xlsx')
```
- Step 3: nspected the dataset to review its structure and the available customer feedback.
- Step 4: CCreated a function to convert feedback to lowercase, tokenise the text, remove English stop words, and lemmatise the remaining tokens. The processed tokens are then combined into a single string.
```
def preprocess_text(text):
    tokens = word_tokenize(text.lower())
    filtered_tokens = [token for token in tokens if token not in stopwords.words('english')]
    lemmatizer = WordNetLemmatizer()
    lemmatized_tokens = [lemmatizer.lemmatize(token) for token in filtered_tokens]
    processed_text = ' '.join(lemmatized_tokens)
    return processed_text
```
- Step 5: Applied the preprocessing function to the Feedback column so that the text could be used for sentiment analysis.
```
df['Feedback'] = df['Feedback'].apply(preprocess_text)
df
```
- Step 6: Initialised SentimentIntensityAnalyzer and created a function that calculates sentiment scores for each feedback entry. The current approach assigns 1 if the positive sentiment score is greater than zero and 0 otherwise.
```
analyser = SentimentIntensityAnalyzer()
def get_sentiment(text):
    scores = analyser.polarity_scores(text)
    sentiment = 1 if scores['pos'] > 0 else 0
    return sentiment
```
- Step 7: Applied the classification function to the processed feedback and stored the resulting binary labels in a new column named sentiment.
```
df['sentiment'] = df['Feedback'].apply(get_sentiment)
df
```

## Limitations and potential improvements

- The current method assigns feedback a positive label if its positive score is greater than zero. All other entries are labelled negative, including feedback that may be neutral.
- Removing stop words may remove words such as "not", which can change the meaning of a sentence and affect its sentiment classification.

## Potential business application

Sentiment analysis can help insurance companies explore customer feedback, identify potential dissatisfaction, and investigate customer experience patterns.
