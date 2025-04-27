# Working with Files and Tokenization in Python

## Introduction

This tutorial introduces you to two essential text processing techniques in digital humanities: **working with files** and **tokenization**.  

This lesson assumes you have basic Python knowledge — variables, loops, and simple data types — but no prior experience with reading files or tokenization.

You will learn:

- How to open and read a text file using Python
- How to split text into tokens (words)
- How to normalize tokens for analysis (e.g., lowercasing)
- How to remove common "stop words" to focus on meaningful tokens
- How to clean and filter your tokens to prepare for further analysis
- How to use regular expressions for better tokenization

## Getting Started

You will need:

- Python 3 installed on your computer
- A plain text file to work with (we will use a sample file in this lesson)

You can download a sample text file [here](https://github.com/lnguyen2693/TuftsIntroDH-ProgrammingHistorianLesson/blob/main/book_9.txt) if you don't already have one.

You can follow along using a Jupyter notebook.

## Working with Files

Let's start by learning how to open and read the contents of a file.

```python
# Open the file and read its contents
with open('book_9.txt', 'r') as file:
    text = file.read()

# Print the first 500 characters
print(text[:500])
```

**What's happening here?**

- `open('filename', 'r')`: opens the file in **read** mode.
- `with` automatically closes the file when we are done.
- `.read()` reads the whole file into a single string.
- We print a sample to check what the text looks like.


## Basic Tokenization

Tokenization is breaking a text into units. The simplest way to tokenize is to split on spaces.

```python
# Split the text into tokens
tokens = text.split()

# See the first 20 tokens
print(tokens[:20])
```

**Important**: Splitting by spaces is easy but very naive — punctuation will stick to words ("day," instead of "day").

### Exercise

Print how many tokens you have:

```python
print(len(tokens))
```

How many words do you get?

## Lowercasing Tokens

One problem with raw tokenization is that "Summer" and "summer" are treated differently.  
A common step is to **normalize** the case.

```python
# Lowercase all tokens
lower_tokens = [token.lower() for token in tokens]

print(lower_tokens[:20])
```

Using **list comprehension** makes this transformation fast and clean.

### Exercise

Why might it be important to lowercase all tokens before counting them?

## Removing Stop Words

"Stop words" are very common words (like "the", "and", "of") that often add little value to analysis.

Let's remove them!

First, define a set of stopwords:

```python
# Common stopwords
stopwords = {'the', 'and', 'of', 'a', 'an', 'to', 'in', 'is', 'with', 'that', 'for', 'on', 'as', 'are', 'it', 'at'}

# Filter tokens
filtered_tokens = [token for token in lower_tokens if token not in stopwords]

print(filtered_tokens[:20])
```

**What's happening here?**

- We create a Python **set** of stop words (fast for lookup).
- We use list comprehension to keep only tokens *not* in the stopword list.

### Exercise

What additional stop words might you want to remove depending on your text?

## Cleaning Punctuation

You may have noticed that even after removing stop words, some tokens still look messy ("Fear," or "breast;").  
Let's clean that up by removing punctuation from tokens.

One way is to use the `string` module:

```python
import string

# Remove punctuation
cleaned_tokens = [token.strip(string.punctuation) for token in filtered_tokens]

# Remove empty strings
cleaned_tokens = [token for token in cleaned_tokens if token]

print(cleaned_tokens[:20])
```

**How this works**:

- `string.punctuation` is a built-in list of common punctuation characters.
- `strip()` removes punctuation from both ends of the token.

### Exercise

Some punctuation like apostrophes (e.g., "it's", "don't") are important. How might you modify this cleaning step if you want to keep apostrophes?

## Tokenization Using Regular Expressions

While splitting by spaces (`split()`) is quick, it often leaves punctuation attached to words.  
A more robust way to tokenize is by using **regular expressions** (`re` module) to match only actual words.

```python
import re

# Use a regular expression to find all words
regex_tokens = re.findall(r'\b\w+\b', text.lower())

print(regex_tokens[:20])
```

**What's happening here?**

- `re.findall(pattern, text)`: returns all substrings that match the pattern.
- `\b\w+\b` means:
  - `\b` = word boundary (start/end of a word)
  - `\w+` = one or more word characters (letters, numbers, underscore)
- This pattern finds *only* clean words — no need to manually strip punctuation later!

**Notice**:  
We use `text.lower()` directly inside `findall()` so that the tokens are already normalized to lowercase.

### Exercise

Try changing the regex to allow apostrophes inside words (like “it’s” and “don’t”) by modifying the pattern.  
Hint: you can use something like:

```python
r"\b[\w']+\b"
```

## When to Use Regex Tokenization?

- **Simple `.split()`**: quick for rough work, but messy.
- **Manual cleaning + stopwords**: better if you want full control over each step.
- **Regex tokenization**: ideal for fast, clean word extraction without needing extra cleanup.

## Summary

In this lesson, you've learned how to:

- Open and read text files in Python
- Tokenize text by splitting it into words
- Normalize case to make analysis consistent
- Remove common stopwords
- Clean punctuation from tokens
- Use regular expressions for better tokenization

These steps are the foundation of almost any text analysis project, whether you're counting words, finding patterns, or building more complex models like topic modeling or sentiment analysis.

## Next Steps

Now that you have a cleaned list of tokens, you could:

- Count the frequency of each token
- Visualize word frequencies using a word cloud
- Analyze the text structure (e.g., sentences, paragraphs)

Continue to [Counting Frequencies](https://programminghistorian.org/en/lessons/counting-frequencies) to learn how!

## Sample Code Recap

Here's the complete pipeline using regex tokenization:

```python
import re

# Read the file
with open('book_9.txt', 'r') as file:
    text = file.read()

# Tokenize with regex
regex_tokens = re.findall(r'\b\w+\b', text.lower())

# Remove stop words
stopwords = {'the', 'and', 'of', 'a', 'an', 'to', 'in', 'is', 'with', 'that', 'for', 'on', 'as', 'are', 'it', 'at'}
filtered_tokens = [token for token in regex_tokens if token not in stopwords]

# Final tokens
print(filtered_tokens)
```

