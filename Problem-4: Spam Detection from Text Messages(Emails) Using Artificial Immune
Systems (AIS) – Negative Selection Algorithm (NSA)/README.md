# Spam Detection from Text Messages/Emails Using Artificial Immune Systems (AIS) – Negative Selection Algorithm (NSA)

## Project Description

The "NSA.ipynb" notebook implements the Negative Selection Algorithm (NSA) to detect spam in SMS messages. NSA is an Artificial Immune System (AIS) algorithm.

The system learns how to detect spam messages, by constructing a set of detectors, which identify non-self patterns (spam), using solely self patterns (ham, non-spam messages).

Message Encodings: The 100 most common words in spam messages were identified and chosen as features. The messages are encoded in binary arrays of size 100, in which "1" shows the presence of a feature, and "0" shows the absence of a feature. 

Hamming Rule: Hamming distances are used to generate detectors. A detector is valid if the Hamming distances between the detector and all training self (ham) instances exceed a threshold. 

Hamming distance measures the difference between two strings, by counting the number of positions at which the two strings differ. 

## Dataset

Location: "Data" folder. 

Dataset Source: https://archive.ics.uci.edu/dataset/228/sms+spam+collection

Datafile Format: Each line contains a label ("spam" or "ham") followed by the message text.

## Notebook Overview

The notebook contains the following sections:
1.  Data Exploration: 
   - Read data into a Pandas DataFrame.
   - Explore the data, remove duplicated lines.

2. Data Preprocessing:
   - Decode HTML entities like "&lt;" into "<" and "&gt;" into ">" 
   - Remove HTML tags "<>"
   - Remove extra spaces
   - Convert text to lowercase
   - Replace patterns with tokens:
      - <date>
      - <time>
      - <email>
      - <url>
      - <emoticon>
   - Remove all punctuation except "!";"£", "$", "<>" 
   - Replace patterns with tokens:
      - <phone> (phone numbers)
      - <money> ("$£" symbols)
      - <num> (other numbers)
   - Normalize unicode to ASCII (e.g ü to u)
   - Remove English stop words (e.g. "the", "and")
   - Lemmatization: Reduce every word to its lemma (dictionary/ base form) 

3. Training, testing and validation data
   - Count the 100 most common words in spam messages
   - Encode all messages in dataset into binary arrays
      - 1 if common word present
      - 0 if common word not present
   - Data splitting:
      - Ham messages: 80% training, 10% validation, 10% testing
      - Spam messages: 0% training, 50% validation, 50% testing

4. NSA Algorithm:
   - Build detectors:
      - Attempt generating a number of detectors for a maximum number of attempts
         - Generate a random candidate detector
         - Measure Hamming distance between candidate detector and all training self instances
         - If all distances are higher than a threshold, add candidate to list of detectors
   - Classify message:
      - If the distance between a message and all detectors is lower or equal to the threshold, then the message is classified as "spam". Otherwise, "ham".
   - Classify dataset:
      - Classify each message in a dataset.
 
 5. Training 
   - Parameters:
      - Number of detectors: [500, 1000, 2000]
      - Threshold: [0.3, 0.4, 0.5]
      - Random seeds: 20 runs for robustness

   For each run:
      - Use the training data to generate detectors 
      - Use the validation data to collect the metrics:
         - Primary: F1 score (spam)
         - Secondary: Precision (spam), Recall (spam), False Positive Rate (ham, not-spam)
   
   Display summary statistics for the runs, and select the threshold and detectors corresponding to the best run: highest F1 score (primary objective).

   6. Evaluate Best Detectors on Test Data
   Display the metrics:
      - F1 score (spam)
      - Precision (spam)
      - Recall (spam)
      - False positive rate (not-spam, ham)
      - Display confusion matrix
      - Display ROC curve
      - Display Precision-Recall curve
      - Display recall vs number of detectors coverage

# How to Run
1. Open the "NSA.ipynb" notebook.
2. Select Run All to execute all cells.

**Python version:** 3.12.4

**Libraries used:**
- \_\_future\_\_
- numpy
- random
- pandas
- matplotlib
- multiprocessing
- re
- string
- unicodedata
- html
- scikit learn 
- nltk
- collections
- 

**How to install the libraries:**
- pip install numpy pandas matplotlib scikit-learn nltk


## References
T. Almeida and J. Hidalgo. "SMS Spam Collection," UCI Machine Learning Repository, 2011. [Online]. Available: https://doi.org/10.24432/C5CC84.