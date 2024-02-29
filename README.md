# -Data-Extraction-and-NLP-Assignment
# Data Extraction and NLP Assignment using Web Scrapping 
!pip install requests
!pip install beautifulsoup4
import requests
from bs4 import BeautifulSoup
import nltk
from nltk.corpus import stopwords
from nltk.tokenize import sent_tokenize, word_tokenize
import re  


web= requests.get("https://insights.blackcoffer.com/rising-it-cities-and-its-impact-on-the-economy-environment-infrastructure-and-city-life-by-the-year-2040-2/")

print(web)

web.content

web.url

soup = BeautifulSoup(web.content,"html.parser")

print(soup.prettify())

soup.title

lines=soup.find_all("p")

for l in lines:
    print(l.text)

nltk.download('stopwords')
nltk.download('punkt')

allstopwords=stopwords.words("English")

 # Assuming lines is a 'ResultSet' object containing text data
# Extract text from 'lines'
text_data = [line.text for line in lines]

# Tokenize each line of text
tokenized_lines = [word_tokenize(line) for line in text_data]

# Flatten the list of tokenized lines if needed
all_tokens = [token for tokens in tokenized_lines for token in tokens]

# Filter out stop words
filtered_tokens = [token for token in all_tokens if token.lower() not in allstopwords]


print(len(stopwords.words('English')))

 
file_paths = ['StopWords_Auditor.txt', 'StopWords_Currencies.txt', 'StopWords_DatesandNumbers.txt','StopWords_Generic.txt','StopWords_GenericLong.txt','StopWords_Geographic.txt','StopWords_Names.txt']

 
all_stop_words = []

 
for file_path in file_paths:
 
        with open(file_path, 'r') as file:
            stop_words_text = file.read()
            # Split the text into individual stop words
            stop_words_list = stop_words_text.split('\n')
            # Extend the list of stop words
            all_stop_words.extend(stop_words_list)
            print(f"Stop words from '{file_path}' successfully loaded.")

    

# Remove duplicates from the combined list (optional)
all_stop_words = list(set(all_stop_words))

# Display the combined list of stop words
print("Combined list of stop words:")
print(all_stop_words)   


 # Assuming lines is a 'ResultSet' object containing text data
# Extract text from 'lines'
text_data = [line.text for line in lines]

# Tokenize each line of text
tokenized_lines = [word_tokenize(line) for line in text_data]

# Flatten the list of tokenized lines if needed
all_tokens = [token for tokens in tokenized_lines for token in tokens]

# Filter out stop words
filtered_tokens = [token for token in all_tokens if token.lower() not in all_stop_words]


filtered_tokens

filtered_text = ' '.join(filtered_tokens)

print(filtered_text)

tokens = word_tokenize(filtered_text)

positive_words_path = 'positive-words.txt'
negative_words_path = 'negative-Words.txt'

positive_words = set()
negative_words = set()

# Load positive words
with open(positive_words_path, 'r') as file:
    for line in file:
        positive_words.add(line.strip())

# Load negative words
with open(negative_words_path, 'r') as file:
    for line in file:
        negative_words.add(line.strip())

positive_words

filtered_positive_words = [word for word in positive_words if word not in all_stop_words]
filtered_negative_words = [word for word in negative_words if word not in all_stop_words]


print("Filtered Positive Words:")
print(filtered_positive_words[:10])  

print("\nFiltered Negative Words:")
print(filtered_negative_words[:10])  

# Calculate Positive Score
positive_score = sum(1 for token in tokens if token.lower() in positive_words)

# Calculate Negative Score
negative_score = -sum(1 for token in tokens if token.lower() in negative_words)

# Calculate Polarity Score
polarity_score = (positive_score - negative_score) / (positive_score + negative_score + 0.000001)

# Calculate Subjectivity Score
total_words = len(tokens)
subjectivity_score = (positive_score + abs(negative_score)) / (total_words + 0.000001)

# Print the derived variables
print("Positive Score:", positive_score)
print("Negative Score:", negative_score)
print("Polarity Score:", polarity_score)
print("Subjectivity Score:", subjectivity_score)

nltk.download('cmudict')

 def calculate_average_sentence_length(filtered_text):
    sentences = sent_tokenize(filtered_text)
    words = word_tokenize(filtered_text)
    num_sentences = len(sentences)
    num_words = len(words)
    if num_sentences > 0:
        return num_words / num_sentences
    else:
        return 0
average_sentence_length = calculate_average_sentence_length(filtered_text)
print("Average Sentence Length:", average_sentence_length)

 # Load the set of complex words from the CMU Pronouncing Dictionary
complex_words_set = set(word.lower() for word, pronunciations in nltk.corpus.cmudict.entries() if len(pronunciations) > 2)

# Define a function to calculate the percentage of complex words
def calculate_percentage_complex_words(filtered_text):
    words = word_tokenize(filtered_text)
    num_complex_words = sum(1 for word in words if word.lower() in complex_words_set)
    num_words = len(words)
    if num_words > 0:
        return num_complex_words / num_words
    else:
        return 0

 # Calculate the percentage of complex words
percentage_complex_words = calculate_percentage_complex_words(filtered_text)
print("Percentage of Complex Words:", percentage_complex_words)


 complex_words_set = set(word for word, pronunciations in nltk.corpus.cmudict.entries() if len(pronunciations) > 2)

# Define the function to calculate the percentage of complex words
def calculate_percentage_complex_words(filtered_text):
    words = word_tokenize(filtered_text.lower())  # Convert to lowercase for case-insensitive matching
    num_complex_words = sum(1 for word in words if word in complex_words_set)
    num_words = len(words)
    if num_words > 0:
        return num_complex_words / num_words
    else:
        return 0
 # Calculate the percentage of complex words
percentage_complex_words = calculate_percentage_complex_words(filtered_text)
print("Percentage of Complex Words:", percentage_complex_words)

 # Create a set of words with more than two syllables from the CMU Pronouncing Dictionary
complex_words_set = set(word for word, pronunciations in nltk.corpus.cmudict.entries() if len(pronunciations) > 2)

# Define the function to count complex words
def count_complex_words(filtered_text):
    words = word_tokenize(filtered_text.lower())  # Convert to lowercase for case-insensitive matching
    num_complex_words = sum(1 for word in words if word in complex_words_set)
    return num_complex_words
 

# Count the number of complex words
complex_word_count = count_complex_words(filtered_text)
print("Complex Word Count:", complex_word_count)


 # Define a function to count syllables in a word
def count_syllables(word):
    vowels = 'aeiouy'
    count = 0
    prev_char_is_vowel = False
    for char in word.lower():
        if char in vowels:
            if not prev_char_is_vowel:
                count += 1
            prev_char_is_vowel = True
        else:
            prev_char_is_vowel = False
    if word.endswith('e'):
        count -= 1
    if count == 0:
        count = 1
    return count

# Define a function to calculate syllable count per word
def calculate_syllable_count_per_word(filtered_text):
    words = word_tokenize(filtered_text)
    total_syllables = sum(count_syllables(word) for word in words)
    total_words = len(words)
    if total_words > 0:
        return total_syllables / total_words
    else:
        return 0

# Calculate syllable count per word
syllable_count_per_word = calculate_syllable_count_per_word(filtered_text)
print("Syllable Count Per Word:", syllable_count_per_word)

# Count the total number of words
word_count = len(word_tokenize(filtered_text))
print("Word Count:", word_count)


def count_personal_pronouns(filtered_text):
    pronouns = re.findall(r'\b(I|we|my|ours|us)\b', filtered_text, flags=re.IGNORECASE)
    return len(pronouns)
personal_pronoun_count = count_personal_pronouns(filtered_text)
print("Personal Pronoun Count:", personal_pronoun_count)


def calculate_average_word_length(filtered_text):
    words = word_tokenize(filtered_text)
    total_characters = sum(len(word) for word in words)
    num_words = len(words)
    if num_words > 0:
        return total_characters / num_words
    else:
        return 0
average_word_length = calculate_average_word_length(filtered_text)
print("Average Word Length:", average_word_length)

def count_total_words(filtered_text):
    words = word_tokenize(filtered_text)
    return len(words)

def count_syllables(word):
    vowels = 'aeiou'
    syllables = 0
    prev_char_was_vowel = False
    for char in word:
        if char.lower() in vowels:
            if not prev_char_was_vowel:
                syllables += 1
            prev_char_was_vowel = True
        else:
            prev_char_was_vowel = False
    if word.endswith('e'):
        syllables -= 1
    if syllables == 0:
        syllables = 1
    return syllables

fog_index = 0.4 * (average_sentence_length + percentage_complex_words)
average_words_per_sentence = count_total_words(filtered_text) / len(sent_tokenize(filtered_text))

print("Fog Index:", fog_index)
print("Average Number of Words Per Sentence:", average_words_per_sentence)

# Print the results
print("Average Sentence Length:", average_sentence_length)
print("Percentage of Complex Words:", percentage_complex_words)
print("Fog Index:", fog_index)
print("Average Number of Words Per Sentence:", average_words_per_sentence)
print("Complex Word Count:", complex_word_count)
print("Word Count:", word_count)
print("Syllable Count Per Word:", syllable_count_per_word)
print("Personal Pronoun Count:", personal_pronoun_count)
print("Average Word Length:", average_word_length)

import pandas as pd

file_path = "Output Data Structure.xlsx"
df = pd.read_excel("Output Data Structure.xlsx")

df.head(5)

for index, row in df.iterrows():
 
    df.at[0, 'POSITIVE SCORE'] = positive_score
    df.at[0, 'NEGATIVE SCORE'] = negative_score
    df.at[0, 'POLARITY SCORE'] = polarity_score
    df.at[0, 'SUBJECTIVITY SCORE'] = subjectivity_score
    df.at[0, 'AVG SENTENCE LENGTH '] = average_sentence_length
    df.at[0, 'PERCENTAGE OF COMPLEX WORDS '] = percentage_complex_words
    df.at[0, 'FOG INDEX SCORE'] = fog_index
    df.at[0, 'AVG NUMBER OF WORDS PER SENTENCE'] =  average_words_per_sentence
    df.at[0, ' COMPLEX WORD COUNT'] =  complex_word_count
    df.at[0, ' WORD COUNT'] =  word_count
    df.at[0, 'SYLLABLE PER WORD'] = syllable_count_per_word
    df.at[0, ' PERSONAL PRONOUNS'] = personal_pronoun_count
    df.at[0, ' AVG WORD LENGTH'] = average_word_length
    
    
# Write the updated DataFrame back to the Excel file
df.to_excel(file_path, index=False)

df.head(1)

df.to_csv("Final result submitted by Suraj", index=False)
