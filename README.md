![title](in.jpg)
# Innoplexus Online Hiring Hackathon: Saving lives with AI
Innoplexus Online Hiring Hackathon : Saving Lives with AI hosted by Analytics Vidya

## Problem Statement

Clinical studies often require detailed patients’ information documented in clinical narratives. **Named Entity Recognition (NER) is a fundamental Natural Language Processing (NLP) task to extract entities of interest (e.g., disease names, medication names and lab tests) from clinical narratives, thus to support clinical and translational research.** Clinical notes have been analyzed in greater detail to harness important information for clinical research and other healthcare operations, as they depict rich, detailed medical information.


In this challenge, hackers are invited to extract all disease names from a given set of 20000 paragraphs/documents in the test set provided the labelled entities (diseases) for 30000 documents in the train set.

For example, here is a sentence from a clinical report:

*We compared the inter-day reproducibility of post-occlusive **reactive hyperemia** (PORH) assessed by single-point laser Doppler flowmetry (LDF) and laser speckle contrast analysis (LSCI).*


In the sentence given, **reactive hyperemia (in bold)** is the named entity with the type disease/indication.

 

## Data Description
The train file has the following structure:
 
|Variable | Definition|
|---|---|
|id|	Unique ID for a token/word|
|Doc_ID	|Unique ID for a Document/Paragraph|
|Sent_ID|	Unique ID for a Sentence|
|Word	|Exact word/token|
|tag	(Target)| Named Entity Tag  |

The target 'tag' follows the **Inside-outside-beginning (IOB)** tagging format. The IOB format (short for inside, outside, beginning) is a common tagging format for tagging tokens in named-entity recognition.

**The B-indications (beginning) tag indicates that the token is the beginning of a disease entity (disease name in this case)
An I-indications (inside) tag indicates that the token is inside an entity
An O (outside) tag indicates that a token is outside a disease entity**
 
**Example**
For more clarity, let's look at the same sample in the given tabular format, each row here corresponds to a word/token:

The disease **'reactive hyperemia'** is labelled using **'B-indications'** for the word **'reactive'** and **'I-indications'** for the word **'hypermia'**. All the other words that are outside **'reactive hyperemia'** are labelled with **'O'.**


## Evaluation Metric

The evaluation for this contest is based on modified F1-Score as explained below:
Suppose the ground truth has the following entities (mentioned in square brackets) for the given sentence

**[Malaria] and [Yellow Fever] remain more deadly than [Hepatitis B] today**

This has 3 entities.
Supposing the actual prediction has the following

**[Malaria] [and] [Yellow] Fever remain more deadly than Hepatitis B [today]**

We have an exact match for Malaria, false positives for and and today, a false negative for Hepatitis B and a substring match for Yellow. We compute precision and recall by first defining matching criteria. We are also trying to reward partial match here and not just exact entity match.

Here, True positives are of 2 types - Exact match and partial match and we are giving a weight of 1 to Exact Match and 0.5 to partial match. The computations are as follows:

Exact Match = 1 (Malaria) and Partial Match = 1 ( Yellow which overlaps Yellow Fever), False Positives =2 (and, and today), False Negatives = 1 (Hepatitis B)

**Precision** = (Exact Match + 0.5 * Partial Match) / (Exact Match + Partial Match + False Positives) = (1 + 0.5)/(1+1+2) = 0.375

**Recall** = (Exact Match + 0.5 * Partial Match) / (Exact Match + Partial Match + False Negatives) = (1 + 0.5)/(1+1+1) = 0.50

**F1 Score** = (2 * Precision * Recall)/(Precision + Recall) = 0.428


The counts of exact match, partial match, false positives and false negatives is summed across all sentences in the test set and overall F1 Score is the leaderboard score.

Please find the script for the evaluation metric implemented in Python at this [link](https://gist.github.com/frenzy2106/3a12b7fefeb33941edea45d881d6f81a) 

# Approach

* Tried NB with simple TF-ID features which gave ~58.XX
* Used sklearn crf_suite and F1 score increased to ~75 and tried to build some more features which resulted in melt down, finally optimized parameters
