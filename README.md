# Insurance-Project-SQL-PowerBI-part-2
NLTK for insurance feedback: tokenising the feedback, removing stop words, lemmatising tokens to complete sentimental analysis of the feedback (0 for negative and 1 for positive).

## Steps followed:

- Step 1: Importing libraries - pandas for handling the dataset, nltk for natural language processing, SentimentIntensityAnalyzer for sentiment analysis:
```
import pandas as pd
import nltk
from nltk.sentiment.vader import SentimentIntensityAnalyzer
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
from nltk.stem import WordNetLemmatizer
```
- Step 2: Loading the feedback dataset in xlsx:
```sql
df = pd.read_excel('Insurance+Customer+Feedback.xlsx')
```
- Step 3: Previewing the dataset.
- Step 4: Creating a function to tokenise the text, remove stop words and lemmatise the tokens:
```
def preprocess_text(text):
    tokens = word_tokenize(text.lower())
    filtered_tokens = [token for token in tokens if token not in stopwords.words('english')]
    lemmatizer = WordNetLemmatizer()
    lemmatized_tokens = [lemmatizer.lemmatize(token) for token in filtered_tokens]
    processed_text = ' '.join(lemmatized_tokens)
    return processed_text
```
- Step 5: Applying it onto the "Feedback" column of the file:
```
df['Feedback'] = df['Feedback'].apply(preprocess_text)
df
```
- Step 6: Creating a function calls the polarity scores method of the sentiment intensity analyser object to get a dictionary of sentiment scores. Text with positive score will get 1, and with 0 get a negative one:
```
analyser = SentimentIntensityAnalyzer()
def get_sentiment(text):
    scores = analyser.polarity_scores(text)
    sentiment = 1 if scores['pos'] > 0 else 0
    return sentiment
```
- Step 7: Applying it onto the "Feedback column":
```
df['sentiment'] = df['Feedback'].apply(get_sentiment)
df
```
