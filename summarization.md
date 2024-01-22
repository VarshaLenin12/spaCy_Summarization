# Install spaCy
pip install spacy

import spacy
from collections import Counter
from heapq import nlargest

# Load spaCy English model
nlp = spacy.load("en_core_web_sm")

def preprocess_text(text):
# Tokenize and lemmatize the text using spaCy
    doc = nlp(text)
    lemmatized_tokens = [token.lemma_ for token in doc if token.is_alpha and not token.is_stop]
    return lemmatized_tokens

def extractive_summarize(text, num_sentences=3):
# Tokenize and preprocess the input text
    tokens = preprocess_text(text)

# Calculate word frequency using Counter
    word_freq = Counter(tokens)

# Calculate sentence scores based on word frequency
    sentence_scores = {i: sum(word_freq[word] for word in preprocess_text(sentence)) for i, sentence in enumerate(text.split('\n'))}

# Get the indices of the top-ranked sentences
    top_sentences = nlargest(num_sentences, sentence_scores, key=sentence_scores.get)

# Sort the selected sentences based on their indices
    selected_sentences = sorted(top_sentences)

# Sort the selected sentences in the original order
    summary = '\n'.join(sentence for i, sentence in enumerate(text.split('\n')) if i in selected_sentences)

    return summary

# Example usage
input_text = """
In shadows deep, where dreams reside,
A quiet echo, whispers inside.
Inside the night, a silent song,
Song of stars, dancing along.

Along the velvet sky, they gleam,
Gleam with hope, a timeless theme.
Theme of love, a tender rhyme,
Rhyme of forever, beyond time.
"""

num_sentences = 5

summary = extractive_summarize(input_text, num_sentences=num_sentences)
print("Original Text:\n", input_text)
print("\nExtractive Summarized Text:\n", summary)
