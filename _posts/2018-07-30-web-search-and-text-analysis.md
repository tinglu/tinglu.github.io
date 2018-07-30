---
layout: post
title: Web Search and Text Analysis Revision
date: 2018-07-30 15:53 +1000
tags: notes nlp
---

Some dot points of my study of COMP90042 Web Search and Text Analysis (SM1 2018).
The notes below also contain lecture materials (&copy; the University of Melbourne).

## Overview

I. Introduction to text processing

    Text classification, word meaning and document representations

II. Structure learning

    Sequence tagging, n-gram language modelling, parsing & translation

III. Larger tasks in Text Analysis

    Information extraction, question answering

IV. Information Retrieval

    Vector space model, efficient indexing, query expansion and using the web as a graph

## I. Introduction and Preprocessing

## 1. Preprocessing

### Definition

- Words: Sequence of characters with a meaning and/or function
- Sentences: "The student is enrolled at the University of Melbourne."
- Word token: each instance of "the" in the sentence above.
- Word type: the distinct word "the".
- Lexicon: a group of word types.
- Document: one or more sentences.
- Corpus: a collection of documents.

### Text normalisation

- Remove unwanted formatting (e.g. HTML)
- Segment structure (e.g. sentences)
- Tokenise words
- Normalise words
  - Lower casing (Australia -> australia)
  - Removing morphology

    - `Inflectional morphology` 曲折语素 creates grammatical variants.

      `Lemmatisation` means removing any inflection to reach the uninflected form, the lemma 词条 e.g. speaking → speak

    - `Derivational morphology` 派生语素 creates distinct words.
    English derivational suffixes often change the lexical category.
    English derivational prefixes often change the meaning without changing the lexical category.

      `Stemming` strips off all suffixes, leaving a stem.
      Even less lexical sparsity than lemmatisation; Popular in information retrieval.

      `Porter Stemmer` is the most popular stemmer for English:
      - First strip inflectional suffixes, e.g. -ies → -i
      - Then derivational suffixes, from right to left e.g -isation → -ise; -ise →

  - Correcting spelling
  - Expanding abbreviations

- Remove unwanted words

  `Stop Words` a list of words to be removed from the document.
  Typical in bag-of-word (BOW) representations;
  Not appropriate when sequence is important.

  - All closed-class or function words e.g. the, a, of, for, he, ...
  - Any high frequency words

## 2. Text classification

### Major text classification tasks

Tasks | Motivation | Classes | Features | Examples of Corpora
---------|----------|---------|---------|---------
Topic classification | library science, information retrieval | Topic categories, e.g. "jobs", "anxiety disorders" | Unigram bag of words (BOW), with stop-words removed;<br> Longer n-grams (bigrams, trigrams) for phrases | Reuters news corpus (RCV1, see NLTK sample);<br> Pubmed abstracts;<br> Tweets with hashtags
Sentiment analysis | opinion mining, business analytics| Positive/Negative/(Neutral) | N-grams; Polarity lexicons | Polarity movie review dataset (in NLTK);<br> SEMEVAL Twitter polarity datasets
Authorship attribution | forensic linguistics, plagiarism detection | Authors (e.g. Shakespeare) | Frequency of function words;<br> Character n-grams;<br> Discourse structure | Project Gutenberg corpus (see NLTK sample);<br> Livejournal blog corpus
Native-language identification |forensic linguistics, educational applications | first language of author (e.g. Chinese) | Word N-grams;<br> Syntactic patterns (POS, parse trees);<br> Phonological features | TOEFL/IELTS essay corpora;<br> Lang-8 language learner website
Automatic fact-checking | social media, journalism (fake news) | True/False/(Neutral) | N-grams;<br> Non-text metadata;<br> ??? (very recent task) | Emergent, LIAR: political statements

### Building a text classifier

1. Identify a task of interest
2. Collect an appropriate corpus
3. Carry out annotation
4. Select features
5. Choose a machine learning algorithm
6. Tune hyperparameters using held-out development data
7. Repeat earlier steps as needed
8. Train final model
9. Evaluate model on held-out test data

### Choosing a classification algorithm

- Bias vs. Variance
- Feature independence
- Feature scaling
- Complexity
- Speed

Classification Algorithm | Details | Pros | Cons
---------|----------|----------|---------
 Naive Bayes | assumes features are independent | Fast to "train" and classify; robust, low-variance; good for low data situations; optimal classifier if independence assumption is correct; extremely simple to implement. | Independence assumption rarely holds; low accuracy compared to similar methods in most situations; smoothing required for unseen class/feature combinations
 Logistic Regression | A linear model, but uses softmax "squashing" to get valid probability; Training maximizes probability of training data subject to regularization which encourages low or sparse weights | A simple yet low-bias classifier; unlike Naïve Bayes not confounded by diverse, correlated features | Slow to train; some feature scaling issues; often needs a lot of data to work well; choosing regularisation a nuisance but important since overfitting is a big problem
 Support Vector Machines | Finds hyperplane which separates the training data with maximum margin; _Allows for some misclassification_; Weight vector is a sum of `support vectors` (examples on the margin) | fast and accurate linear classifier; can do non- linearity with kernel trick; works well with huge feature sets | Multiclass classification awkward; feature scaling can be tricky; deals poorly with class imbalances; uninterpretable
 K-Nearest Neighbour | Classify based on majority class of k-nearest training examples in feature space | Simple, effective; no training required; inherently multiclass; optimal with infinite data | Have to select k; issues with unbalanced classes; often slow (need to find those k-neighbours); features must be selected carefully
 Decision Tree | Construct a tree where nodes correspond to tests on individual features; Leaves are final class decision; Based on greedy maximization of mutual information | in theory, very interpretable; fast to build and test; feature representation/scaling irrelevant; good for small feature sets, handles non-linearly-separable problems | In practice, often not that interpretable; highly redundant sub-trees; not competitive for large feature sets
 Random Forests | An ensemble 全套 classifier; Consists of decision trees trained on different subsets of the training and feature space; Final class decision is majority vote of sub-classifiers | Usually more accurate and more robust than decision trees, a great classifier for small- to moderate- sized feature sets; training easily parallelised | Same negatives as decision trees: too slow with large feature sets
 Neural Networks | An interconnected set of nodes typically arranged in layers; **`Input layer`** (features), **`output layer`** (class probabilities), and one or more **hidden layers**; Each node performs a linear weighting of its inputs from previous layer, passes result through activation function to nodes in next layer | Extremely powerful, state-of-the-art accuracy on many tasks in natural language processing and vision | Not an off-the-shelf classifier, very difficult to choose good parameters; slow to train; prone to overfitting

### Evaluation

`Accuracy` = correct classifications/total classifications

`Precision` = correct classifications of B (tp) /total classifications as B (tp + fp)

`Recall` = correct classifications of B (tp)/total instances of B (tp + fn)

`F1` = 2 precision*recall/(precision + recall)

Defined relative to a specific positive class;
But can be used as a general multiclass metric:

- Macroaverage: Average F-score across classes
- Microaverage: Calculate F-score using sum of counts

## 3. Lexical semantics

> How the meanings of words connect to one another in our minds.
>
> Manually constructed resources: lexicons, thesauri, ontologies, etc.

### Basic lexical relations

- Synonyms (same) and antonyms (opposite/complementary)
- Hypernyms (generic), hyponyms (specific)
- Holoynms (whole) and meronyms (part)

### WordNet

A database of lexical relations.

The nodes of WordNet are not words, but meanings; they are represented by sets of synonyms, or `synsets`.

#### Word similarity with paths

Given WordNet, find similarity based on path length in hypernym/hyponym tree:

$simpath(c_1, c_2) = 1/pathlen(c_1, c_2)$

#### Beyond path length

Problem: edges vary widely in actual semantic distance

Solution 1: **include depth information** (Wu & Palmer):

- Use path to find lowest common subsumer (LCS)
- Compare using depths

$simwup(c_1, c_2) = \frac{2*depth(LCS(c_1,c2))}{depth(c_1) + depth(c_2)}$

#### Information content

Problem: But count of edges is still poor semantic distance metric

Solution 2: **include statistics from corpus** (Resnik; Lin)

- P(c): probability that word in corpus is instance of concept c

$P(c) = \displaystyle\frac{\sum_{w \in words(c)} count(w)}{N}$

- information content (IC)

$IC(c)=-logP(c)$

- Lin distance

$simlin(c_1, c_2) = \frac{2*IC(LCS(c_1, c_2))}{IC(c_1)+IC(c_2)}$

#### Word sense disambiguation (WSD)

- Supervised WSD: Apply standard machine classifiers; Requires sense-tagged corpora
- Less supervised approaches:
  - Lesk;
  - Yarowsky (Bootstrap method)
- Graph methods in WordNet

### Other lexical databases

- FrameNet
- General Inquirer lexicon
- Linguistic Inquiry and Word Count (LIWC) lexicons
- Other useful lexicons in nltk
  - Names: List of male and female names
  - Gazetteer List: lists of cities and countries
  - Comprehensive lists of locations at www.geonames.org
  - WordList: lists of words for various languages
  - Stopwords: list of stopwords for various languages
  - Cmudict: a pronounciation dictionary
- Multiword lexicons

    Both WordNet and FrameNet contain multiword expressions (MWEs)

## 4. Distributional semantics

> How words relate to each other in the text.
>
> Automatically created resources from corpora.

Two approaches:

### Count-based (Vector Space Models - VSM) 对文本中词出现的频次做计数

One matrix, two viewpoints:

- Documents represented by their words (**web search**)
- Words represented by their documents (**text analysis**)

TF-IDF:

- Standard weighting scheme for information retrieval
- Also discounts common words

`Singular value decomposition (SVD)`: $A = U\sum V^T$ **?????**

Trucating - `Latent semantic analysis (LSA)` **?????**

For two events x and y, `pointwise mutual information (PMI)` comparison between the actual joint probability of the two events (as seen in the data) with the expected probability under the assumption of independence:

$PMI(x,y) = log_2\frac{p(x,y)}{p(x)p(y)}$

- PMI does a better job of capturing interesting semantics e.g. heaven and hell
- But it is obviously biased towards rare words
- doesn’t handle zeros well

PMI TRICKS：

- Zero all negative values (PPMI)
  - Avoid –inf and unreliable negative values
- Counter bias towards rare events
  - Artificially increase marginal probabilities
  - Smooth probabilities

### Prediction-based (Embeddings from prediction) 通过上下文预测中心词，或通过中心词预测上下文

- Neural network inspired approaches seek to learn vector representations of words and their contexts
- Key idea
  - Word embeddings should be similar to embeddings of
  neighbouring words
  - And dissimilar to other words that don’t occur nearby
- Using vector dot product for vector ‘comparison’
  - $u \cdot v = \Sigma_j u_j v_j$
- As part of a ‘classifier’ over a word and its immediate context

#### Skip Gram Model

Predict words in local context surrounding given word

#### CBOW (continuous-bag-of-words)

Predict word in centre, given words in the local surrounding context

#### Properties

- Skip-gram and CBOW both perform fairly well
  - No clear reason to prefer one over another, choice is task dependent
- Very fast to train using negative sampling approximation
- In fact Skip-gram with negative sampling related to LSA
  - Can be viewed as factorisation of the PMI matrix over words and their contexts

## II. Structural learning

## 5. Part of Speech (词类) Tagging

### POS Open Classes 开放性词类

- Nouns
  - Proper 专有名词 (Australia) versus common 普通名词 (wombat)
  - Mass (rice) versus count (bowls)

- Verbs
  - Rich inflection (go/goes/going/gone/went)
  - Auxiliary verbs (be, have, and do in English) 助动词
  - Transitivity (wait versus hit versus give) 及物动词

- Adjectives
  - Gradable (happy) versus non-gradable (computational)

- Adverbs
  - Manner (slowly) 情态副词
  - Locative (here)
  - Degree (really)
  - Temporal (yesterday) 时间副词

### POS Closed Classes 封闭性词类

- Prepositions 介词 (in, on, with, for, of, over,…)
  - Regular (transitive; e.g. on the table)
  - Particles (intransitive; e.g. turn it on)
- Determiners 限定词
  - Articles (a, an, the)
  - Demonstratives (this, that, these, those)
  - Quantifiers (each, every, some, two,…)
- Pronouns 代词
  - Personal (I, me, she,…)
  - Possessive (my, our,…)
  - Interrogative or Wh (who, what, …) 疑问词
- Conjunctions 连词
  - Coordinating (and, or, but) 并列连词
  - Subordinating (if, although, that, …) 从属连词
- Modals 情态动词
  - Ability (can, could)
  - Permission (can, may)
  - Possibility (may, might, could, will)
  - Necessity (must)
- And some more…

### Tagsets

A compact representation of POS information

#### Major Penn Treebank Tags

- NN noun
- VB verb
- JJ adjective
- RB adverb
- DT determiner
- CD cardinal number
- IN preposition
- PRP personal pronoun
- MD modal
- CC coordinating conjunction
- RP particle
- WH wh-pronoun
- TO to

### Automatic taggers

- Rule-based taggers
  - Hand-coded
  - Transformation-based (Brill): accurate and very fast

- Statistical taggers
  - Unigram tagger: "model" is just a look-up table; a baseline
  - Classifier-based taggers: use standard discriminative classifier (e.g. logistic regression); error propagation (error bias/exposure bias)
  - N-gram taggers: sparsity; must tag words one at a time, left to right
  - Hidden Markov Model (HMM) taggers: a basic sequntial/structured model

#### Unknown words

- Huge problem in morphologically rich languages (e.g. Turkish)
- Can use hapax legomena (things we’ve seen only once) to best guess for things we’ve never seen before
- Can use morphology (look for common affixes)

## 6. Supervised Hidden Markov Models

### Probabilisitic Sequence Modelling

POS tagging local classifier prone to **error propagation**; Exponentially many combinations: $|Tags|^M$, where M is the length.

Solution - **sequence labelling/structured prediction**:

- Define a model that decompose a tagging sentence into individual words
- But that takes into account the whole sequence when learning and predicting (no error propagation)

### Hidden Markov Model

Why "Markov"?
> Because it assumes the sequence follows a Markov chain:
probability of an event (tag) depends only on the previous
one (previous tag)

Why "Hidden"?
> Because the events (tags) are not seen: goal is to find the
best sequence

$\hat{t}=argmax_tP(w|t)P(t)$

$P(w|t)=\Pi_{i=1}^{n}P(w_i|t_i)$ - emission probabilities

$P(t)=\Pi_{i=1}^{n}P(t_i|t_{i-1})$ - transition probabilities

Training uses **Maximum Likelihood Estimation (MLE)**.

### The Viterbi Algorithm

**Complexity**:

- $O(T^2N)$, where T is the size of the tagset and N is the length of the sequence
- _T * N matrix, each cell performs T operations._

**Good practice**:

- work with log probabilities to prevent underflow (multiplications become sums)
- Vectorisation (use matrix-vector operations)

State-of-the-art use tag **trigrams**: $P(t)=\Pi_{i=1}^{n}P(t_i|t_{i-1},t_{i-2})$Viterbi now O(T^3N)

### Other taggers

**Generative models:**

- HMM: P(t, w), 'creates' the input allows for unsupervised HMMs: learn model without any tagged data!

**Discriminative models:**

- modelling P(t | w) directly
- supports richer feature set, generally better accuracy when trained over large supervised datasets
- E.g., Maximum Entropy Markov Model (MEMM), Conditional random field (CRF), Connectionist Temporal Classification (CTC)
- Most deep learning models of sequences are discriminative (e.g. encoder-decoders for translation), similar to an MEMM

### HMM summary

- HMMs are a simple, yet effective way to perform sequence labelling.
- Can still be competitive, and fast. Natural baseline for other sequence labelling tasks.
- Main drawback: not very flexible in terms of feature representation, compared to MEMMs and CRFs.

## 7. Unsupervised Hidden Markov Models

### Hard EM (Expectation Maximisation)

- can perform well depending on the setting
- still too naïve, as it does not take into account distributions on each word and each tag transition

Better approach - to incorporate such distributions by:

- Obtaining marginal emission and transition distributions
- Using weighted (expected) counts to train via MLE

### The Forward Algorithm

- Exactly like Viterbi, but _summing_ scores instead of taking the max.
- Also _no backpointers_ since the goal is not prediction.

### The Backward Algorithm

??

## EM - Final Algorithm

??

- Initialise emission and transition matrices
- E-step: Run forward-backward, obtaining α’s and β’s
- M-step: Update emission and transition matrices using the expected counts.

## 8. Context-Free Grammars

**CFGs (and regexs)** used to describe a set of strings, aka a "language"

**CFGs**:

- can describe hierarchical groupings e.g., matching brackets ($a^nb^n$) & many recursive tree structures found in language
- requires push-down automata to parse

**Regular grammars**:

- describe a smaller class of languages, e.g., a*b*c*
- can be parsed using finite state machine
- HMMs are a model for a (weighted) regular grammar

### CFG trees

- Non-terminals are internal nodes
- Terminals are leaves

### Parsing ambiguity

Often more than on tree can describe a string

### Parsing strategies

- Top down:
  - Start with S, work down towards words
  - Early parsing
- Bottom up:
  - Start with words, word up towards S
  - CYK parsing

### CYK parsing algorithm

1. Convert grammar to **Chomsky Normal Form (CNF)** - $A \to B C$ or $A \to \alpha$
2. Fill in a parse table
3. Use table to derive parse
4. Convert result back to original grammar

## 9. Probablisitic Parsing

### Probabilistic context-free grammars (PCFGs)

#### Stochastic generation with PCFGs

Almost the same as for CFG:

1. Start with S, the sentence symbol
2. Choose a rule with S as the LHS
    - Randomly select a RHS according to Pr(RHS | LHS) e.g., S → VP
    - Apply this rule, e.g., substitute VP for S
3. Repeat step 2 for each non-terminal in the string (here, VP)
4. Stop when no non-terminals remain

Gives us a tree, as before, with a sentence as the yield

### Parsing using dynamic programming

### Limitations of 'context-free' assumption and some solutions

- **poor independence assumptions**: rewrite decisions made independently, whereas inter-dependence is often needed to capture global structure. E.g., NP → PRP used often as subject (first NP), much less often as object (second NP)
- **lack of lexical conditioning**: non-terminals representation behaviour of the actual words, but are much too coarse. Problems with:
  - preposition attachment ambiguity;
  - subcategorisation ([forgot NP] vs [forgot S]);
  - coordinate structure ambiguities (dogs in houses and cats)

#### Solutions

- parent annotation
- head lexicalisation

## 10. Dependency Grammar & Parsing

**Phrase-structure grammars** assume a constituency tree which identifies the phrases in a sentence; based on idea that these phrases are interchangable (e.g., swap an NP for another NP) and maintain grammaticality

**Dependency grammar** offers a simpler approach describe **binary relations** between pairs of words, namely, between **heads** (中心词) and **dependents**

### What is a dependency

- Links between a head word and its dependent words in the sentence: either syntactic roles or modifier (修饰语) relations
- argument of a predicate, e.g., ate(rat, cheese): rat is the subject of verb ate (thing doing the eating), cheese is the direct object of verb ate (thing being eaten)
- head may determine type of relation, lexical form of dependent etc
- Various other types of dependencies exist:
  - a _modifier_ which is typically optional (aka adjunct), e.g. (with) me modifies the act of (the rat) eating
  - _specifiers_, e.g., the rat, the cheese, with me

### Dependency Parsing

#### (1). Transition-based parsing

Treats problem as _incremental sequence of decisions over next action in a state machine_.

Maintain two **data structures**:

- buffer = input words yet to be processed
- stack = head words currently being processed

Two types of **transitions**

- shift = move word from buffer on to top of stack
- arc = add arc (left/right) between top two items on stack (and remove dependent from stack)

Always results in a projective tree.

#### (2). Graph-based parsing

Uses chart over possible parses, and dynamic programming to solve for the maximum.

_Dependency parsing using dynamic programming_.

## 11. Ngram language models

> Assign a probability to a sequence of words.
>
> Framed as "sliding a window" over the sentence, predicting each word from finite context to left.
>
> N-gram language models are a structure-neutral way to capture the predictability of language.

Useful for:

- Speech recognition
- Spelling correction
- Machine translation
- ...

### Smoothing (or Discounting)

Basic idea: give events that are never seen before some probabilities.

#### Laplacian (Add-One) smoothing

Simple idea: pretend we’ve seen each n-gram once more than we did.

Add-k smoothing:

$$P_{addk}(w_i|w_{i-1}, w_{i-2})=\dfrac{C(w_{i-2},w_{i-1},w_i)+k}{C(w_{i-2},w_{i-1})+k|V|}$$

- Works for text classification (and to some extent, POS tagging) because the number of classes is small.
- but the number of "classes" is huge (n-grams) and the frequency can vary a lot.

#### Kneser-ney smoothing

State-of-the-art method for n-gram language models.

- Backoff

$$
\begin{aligned}
P_{BO}(w_i|w_{i-2}, w_{i-1}) = \\
P(w_i|w_{i-2}, w_{i-1}) \quad if C(w_{i-2},w_{i-1}, w_i) > 0 \\
\alpha(w_{i-2}, w_{i-1}) * P_{BO}(w_i|w_{i-1}) \quad otherwise
\end{aligned}
$$

- Interpolation

$$P_{interp}(w_i|w_{i-2}, w_{i-1}) = \lambda(w_{i-2}, w_{i-1})P(w_i|w_{i-2}, w_{i-1}) + (1 - \lambda(w_{i-2}, w_{i-1}))P_{interp}(w_i|w_{i-1})$$

- Absolute discounting???
- Continuation count???

Most used LMs use 5-grams as the max order but higher order sometimes can be used if large amounts of data are available.

### Evaluation

- Extrinsic: e.g. Spelling correction, machine translation
- Intrinsic: perplexity

$$PP(w_i, w_2, .. w_m) = \sqrt[m]{\dfrac{1}{P(w_i, w_2, .. w_m)}} $$

## 12. Neural language models

### Log-bilinear LM

- Parameters: embedding matrix (or 2 matrices, for input & output) of size V x d; weights now d x d
- d chosen as parameter typically in [100, 1000]
- closely related to word2vec

### "Feed-forward" neural language models (FFNNLM)

**NN** = Neural Network

> a.k.a. artificial NN, deep learning, multilayer perceptron

**NN Units** - Each unit is a function:

$$h = tanh\Big(\Sigma_jw_jx_j + b\Big)$$

- given input x, computes real-value (scalar) h
- scales input (with weights, w) and adds offset (bias, b)
- applies non-linear function, such as **logistic sigmoid**, **hyperbolic sigmoid (tanh)**, or **rectified linear unit**

Typically have several hidden units:

$$\vec{h} = tanh\Big(W\vec{x} + \vec{b}\Big)$$
$$\vec{y} = softmax\Big(U\vec{h}\Big)$$

- where W is a matrix comprising the unit weight vectors, and b is a vector of all the bias terms
- and tanh applied element-wise to a vector

> Maximise the probability => minimise negative log

**loss function**: $-log P$

### Recurrent neural language models (RNN)

Used widely as sentence encodings, translation, summarisation, generation, text classification, and more.

### Ngram VS. Neural networks

- Ngram LMs
  - cheap to train (_just compute counts_)
  - but _too many parameters_, problems with _sparsity_ and _scaling_ to larger contexts
  - don’t adequately capture _properties_ of words (_grammatical_ and _semantic_ similarity), e.g., film vs movie
- NNLMs more robust
  - force words through _low-dimensional embeddings_
  - _automatically_ capture word _properties_, leading to more robust estimates

NNet Pros:

- Robust to word variation, typos, etc
- Excellent generalization, especially RNNs
- Flexible — forms the basis for many other models (translation, summarization, generation, tagging, etc)

Cons:

- Much slower than counts… but hardware acceleration
- Need to limit vocabulary, not so good with rare words (~10,000 vocabulary; any word outside of that vocabulary is treated as rare words => hackish)
- Not as good at memorizing fixed sequences
- Data hungry, not so good on tiny data sets

## III. Larger tasks in Text Analysis

## 13. Information Extraction (IE)

### Machine learning in IE

**Named Entity Recognition (NER)** 命名实体识别: (usually) sequence models such as HMMs, MEMMs or CRFs.

### Typical entity tags

- PER: people, characters
- ORG: companies, sports teams
- LOC: regions, mountains, seas
- GPE: countries, states, provinces (sometimes conflated with LOC)
- FAC: bridges, buildings, airports
- VEH: planes, trains, cars

Tagset is application-dependent: some domains deal with specific entities such as proteins, genes or works of art.

### NER as sequence labelling task

- NE tags can be **ambiguous**: "Washington" can be either a person, a location or a political entity.
- We faced a similar problem when doing POS tagging - Solution: incorporate **context** by treating NER as sequence labelling.
- Can we use an out-of-the-box HMM for this? - Not really: entities can span multiple tokens; Solution: **adapt the tag set**.

#### IO tagging

I-ORG represents a token that is inside an entity (ORG in this case). All tokens which are not entities get the O token (for outside).

Can not differentiate between a single entity with multiple tokens or multiple entities with single tokens.

#### IOB tagging

B-ORG represents the beginning of an ORG entity. If the entity has more than one token, subsequent tags are represented as I-ORG.

#### Features

- character & word shapes
- prefix / suffix
- POS tags / syntactic chunks

In real world applications, **pipeline** approaches are more common.

### Relation extraction

1. If have access to a fixed relation database:

**Rule-based**

- Lexico-syntactic patterns: high precision, low recall, manual effort required.

**Supervised:**

- Assume a corpus with annotated relations.
- Two steps. First, find if an entity pair is related or not (binary classification).
- Second, for pairs predicted as positive, use a multi-class classifier to obtain the relation.

**Semi-supervised:**

- Assume we have a set of seed tuples. These can be get from annotated corpora.
- Mine the web for text containing the tuples

**Distant supervision:**

- Semi-supervised methods assume the existence of seed tuples.
- Distant supervision obtain new tuples from a range of sources:
  - DBpedia
  - Freebase
- Generate very large training sets, enabling the use of richer features
- Still rely on a fixed set of relations.

2.If no restrictions on relations:

**Unsupervised**: - Sometimes referred as "**OpenIE**":

- If there is no relation database or the goal is to find new relations, unsupervised approaches must be used.
- Relations become substrings, usually containing a verb.

### Evaluation

- NER: **F1-measure** at the **entity** level.
- Relation Extraction with _known_ relation set: F1-measure
- Relation Extraction with _unknown_ relations: much harder to evaluate
  - ually need some human evaluation
  - massive datasets used in these settings are impractical to evaluate manually: use a small sample
  - can only obtain (approximate) precision, not recall.

#### Temporal expressions

Normalisation: mapping expressions to canonical forms.

Mostly **rule-based** approaches. _False positives_ are a problem: e.g. U2’s classic _Sunday_ Bloody _Sunday_

#### Event extraction

- Very similar to relation extraction, including annotation and learning methods.
- **Event ordering**: detect how a set of events happened in a timeline.
  - Involves both event extraction and temporal extraction/normalisation.
  - Useful for rumour detection.

#### Template filling

- Some events can be represented as `templates`.
  - A "fare raise" event has an airline, an amount and a date when it occurred, among other possible **slots**.
- Goal is to fill these slots given a text. Models can take the template information into account to ease the learning and extraction process.
- Need to determine if a piece of text contain the information asked in the template (binary classification).

### IE Summary

- Information Extraction is a vast field with many different tasks and applications
- Named Entity Recognition + Relation Extraction
- Events can be tracked by combining event and temporal expression extraction
- Template filling can help learning algorithms
- Machine learning methods involve classifiers and sequence labelling models.
- Domain is key for good performance.

## 14. Question Answering

`Question answering (QA)` is the task of automatically determining the answer (set) for a natural language question.

**Primary approaches:**

`Natural Language Interface to Database (NLIB)` = automatically construct a query, and answer question relative to fixed KB
`Direct text matching` = answer question via string/text passage in a document (collection)

### Knowledge-rich restricted-domain QA (1950-1980)

The first phase of QA research focused on NL interface to knowledge base from a particular domain (e.g. blocks world, baseball statistics, or lunar rock samples) with idiosyncratic "data semantics"

`QA system` = means of translating NL query into formal representation that
can be used to query knowledge base directly

#### Limitations

- specic to a particular domain, and doesn't generalise across domains
- scalability - inherently limited in scale by the size of the knowledge base
- The NL parsers used for knowledge-rich restricted-domain QA tended to adopt a **depth first** approach to resource development (= attempt to parse particular input types particularly well, _at expense of generality/coverage_)
- Note that these are limitations of these early approaches, and that there have been successful applications of general-purpose parsers to knowledge-rich restricted-domain QA

### QA as IR (1999-)

The focus of the IR community has always been the resolution of **user information needs** (what information [type] is the user after in issuing a given query?), e.g.:

- **Informational**: want to learn about something (40%); _Australian submarine contract_
- **Navigational**: want to go to a particular page (25%);_ Australian Taxation Office_
- **Transactional**: want to do something (web-mediated) (35%); _Access a service (e.g. Melbourne weather); Shop (e.g. Samsung Galaxy S7)_
- **Mixed-mode** information needs; _Find a good hub site (e.g. Tokyo hotels); Exploratory search_

As such, it is natural to treat QA as an IR problem.

Examples: TREC QA, (Pure IR) PRICS

### QA as IE (2005-)

The TREC QA tasks emerged out of the IR community, but were quickly picked up on as relevant by the information extraction (IE) community (at a time when IE was languishing slightly ...)

**IE components** of a TREC-style system:

- question classication

  Task = predict the entity type of the answer based on the wording of the question

- named entity recognition

#### QA as semantic parsing:

`Semantic parsing` = automatically translate a natural language text into a formal meaning representation.

### QA as Deep Learning (2014-)

In line with some of the more ambitious goals of deep learning, there has also been recent work which has attempted to perform **string-string QA** in an **end-to-end deep learning architecture**, e.g.

- "quiz bowl QA, mapping paragraph-length quiz-style questions to factoid answers
- "episodic" QA, where questions can be asked of the current state of dierent entities in a monologue

While deep learning can be highly effective at ranking answer sets or matching questions to answers, it is not currently scalable as a full-on IR solution, so _most deep learning research focuses on answering questions relative to text passages which contain the answer_, _answer set ranking_ (assuming pre-retried answer set), etc.

## 15. Topic Models

**Topics**:

- A topic can have a specific semantic interpretation but does not have a label.
- Additionally, documents can have multiple topics.
- This concept enables us to perform open-ended, **unsupervised** learning on documents.
- The standard algorithm for this is called **Latent Dirichlet Allocation (LDA)**.

### Topic modelling

Topic models aim at _making sense of large collections of documents_, combining two ideas:

- Unsupervised learning (clustering)
- Documents can have multiple topics

Two key simplifications:

- Emission probabilities are shared across documents
- Topic of word does not depend on previous words' topcs; ONLY depends on the **document**

Parameters:

- $\beta$ (one per **topic**): the distribution of words given a topic
- $\theta$ (one per **document**): the distribution of topics given a document

Can use **EM** to train this, given a **topic initialisation**:

- $\beta$ count (expected): **word-topic frequencies** (in the whole corpus) and normalise
- $\theta$ count (expected): **topic-document frequencies** and normalise

_E-step_:

$$P(t=1 | w=gene, d=7, \beta_1, \theta_7) = \dfrac{P(w=gene, t=1 | d=7, \beta_1, \theta_7)}{\Sigma_kP(w=gene, t=k | d=7, \beta_k, \theta_7)}$$

_M-step_:

$$\theta_7^1 = \dfrac{ \Sigma_{i\in{d}} P(t=1 | w_i, \beta_1, \theta_7) }{ \Sigma_k\Sigma_{i\in{d}} P(t=k | w_k, \beta_k, \theta_7) } \approx \dfrac{ \textit{\# tokens in d with topic 1} }{ \textit{ \# tokens in d } } $$

$$\beta_1^w = \Sigma_d\in{D} \dfrac{ \Sigma_{i:w_{i}=w} P(t=1 | w_i=w, \beta_1, \theta_d) }{ \Sigma_{v\in{V}}\Sigma_{i:wi=v} P(t=k |w_{i}=v, \beta_k, \theta_d) } \approx \dfrac{ \textit{\# tokens with type w and with topic     } }{ \textit{ \# tokens in with topic 1 } } $$

**_Add-k smoothing_**:

_E-step_: is the same

_M-step_:

$$\theta_7^1 = \dfrac{ \alpha + \Sigma_{i\in{d}} P(t=1 | w_i, \beta_1, \theta_7) }{ \Sigma_k\Sigma_{i\in{d}} P(t=k | w_k, \beta_k, \theta_7) } $$

$$\beta_1^w = \Sigma_d\in{D} \dfrac{ \eta + \Sigma_{i:w_i=w} P(t=1 | w_i=w, \beta_1, \theta_d) }{ \Sigma_{v\in{V}} (\eta + \Sigma_{i:wi=v} P(t=k |w_{i}=v, \beta_k, \theta_d)) } $$

Add-k smoothing: can be interpreted as having a **prior distribution** over the parameters $\theta$ and $\beta$

$P(\theta|\alpha) = Dirichlet(\alpha+1)$

$P(\beta|\eta) = Dirichlet(\eta+1)$

_The above model is essentially **LDA with symmetric Dirichlet** over topics and words._

### The Dirichlet Distribution

The Dirichlet is a distribution over a probability simplex.

- $\alpha$ defines the **spread**
- **Symmetric**: $\alpha$ is a **scalar**
- **Asymmetric**: one $\alpha$ per topic

In practice:

- Use asymmetric Dirichlet
- Instead of finding the maximum value for $\theta$ and $\beta$, estimate the **posterior distribution** over the parameters

### Evaluating topic models

- Intrinsic: perplexity
- Extrinsic: harder
  - If the topic model is used as a tool for an end task (information retrieval, machine translation, etc.), calculate the end task metric.
  - Otherwise some human intervention is required. **Interpretability** is key.

Many other **visualisation** options available:

- Word clouds
- Word graphs

**Labelling techniques:**

- Distant supervision (Wikipedia, knowledge bases, etc.)
- Article names, pictures.
- Combination of all above

**Goal:** enhance **interpretability**

> Perplexity can give evidence of performance but ultimate goal in clustering is interpretability

### Application

- Information retrieval: **useful for query expansion**.
- Analysis of historical documents (newspapers, books).
- Making sense of scientific publications: emergence of new fields and multidisciplinary ones.
- Literary analysis: stylometry, comparative literature.
- Computational social science: text on social media (Twitter), stance detection.
- Translation: multilingual topic models for mining parallel data.

## IV. Information Retrieval

## 16. Information retrieval: vector space model

### Evaluation on test collections

IR research often use reusable test collections constructed for IR evaluation, e.g., for **TREC** competitions; comprising:

- **corpus** of documents
- **set of queries** (**"topics"**), often including long-form elaboration of information need
- **relevance judgements** (**qrels**) for each document and query, a human judgement of whether the document is relevant to the information need in the given query.

### Document representation

`Term-document matrix` - allow efficient access, e.g. to find a specific word

#### Speeding up cosine distance

`Vector space model` - use `Inverted index` - exploits sparsity, allow efficient storage and querying

`Inverted index` -> `postings`

### BM25: Parameterised Scoring Method

$$
\begin{aligned}
W_t = \bigg[ \log \frac{ N - f_t + 0.5 }{f_t + 0.5} \bigg] \quad\quad \textit{(idf)} \\
\times \frac{ (k_1 + 1)f_{d, t} }{ k_1 \bigg( 1-b + b \frac{L_d}{L_ave} \bigg) + f_{d, t} } \quad\quad \textit{(tf and doc length)} \\
\times \frac{ (k_3 + 1) f_{q, t} }{ k_3 + f_{q, t} } \quad\quad \textit{(query tf)} \\
\end{aligned}
$$

Default: $k_1 = 1.5, b = 0.5, K_3 = 0$

$$
W_t = \bigg[ \log \frac{ N - f_t + 0.5 }{f_t + 0.5} \bigg]
\times \frac{ (k_1 + 1)f_{d, t} }{ k_1 \bigg( 1-b + b \frac{L_d}{L_ave} \bigg) + f_{d, t} }
$$

## 17. Index compression & Efficient query processing

### Index compression

**Benefits:**

- reduce storage requirements
- keep larger parts of the index in memory
- faster query processing

**Compression principles:**

Compressibility is bounded by the information content of a data set

**Posting list compression:**

- minimize storage costs
- fast sequential access
- support GEQ(x) operation

#### Variable byte compression

#### OptForDelta compression

### Efficient query processing

#### BM25 for one document

$$
S_{Q, d}^{BM25} = \frac{ (k_1 + 1)f_{d, q} }{ k_1 \bigg( 1-b + b \frac{n_d}{n_ave} \bigg) + f_{d, q} } \cdot f_{Q, q} \cdot \ln \bigg( \frac{ N - f_{D, q} + 0.5 }{f_{D, q} + 0.5} \bigg)
$$

**Inefficient Evaluation:**

- For each q in Q compute $w_{Q,q}$ in $O(|Q|)$ time.
- For each d in the document collection containing any q in Q evaluate $w_{d,q}$. (Potentially $O(N)$ time!)
- Return the top-k highest scoring documents.

#### Top-k - The WAND algorithm

`WAND` - Weak AND

The Maximum contribution of a term q as the largest score any
document in the collection can have for the query Q only consisting
of q.

- Depends on the similarity measure.
- Can be computed at construction time of the index.
- Only requires storing a single floating point number for each list.
- Can be used to overestimate the score of a document in a multi term query.

Discussion:

- Use max contribution of query term to overestimate score of a document.
- Do not score document if it can not enter the top-k heap.
- Utilize GEQ function of compressed representation to skip over large parts of the postings lists.
- Similarity metric fixed at index construction time.
- Works very well in practice.

## 18. Query completion/expansion

### Query completion

**Goals:**

1. Assist users to formulate search requests.
2. Reduce number of keystrokes required to enter query.
3. Help with spelling query terms.
4. Guide user towards what a good query might be.
5. Cache results! Reduce server load.

**Strategy:**

1. Generate list of completions based on partial query.
2. Refine suggestions as more keys are pressed.
3. Stop once users selects candidate or completion fails.
4. Why not a Language Model? Might not return results!

#### Completion targets

**Where does the set S of possible completions come from:**

1. Most popular queries (websearch)
2. Items listed on website (ecommerce)
3. Past queries by the user (email search)

**Properties:**

1. Static (e.g. completion for "twi")
2. Dynamic (e.g. time-sensitive, "world cup")
3. Massive or small (email search vs websearch)

#### Completion Types / Completion Modes

Modes:

1. Prefix match.
2. Substring match.
3. Multi-term prefix match.
4. Relaxed match.

#### Prefix match - Trie + RMQ based index

`RMQ` - Range Maximum Query

Step 1: Preprocess data by sorting query log in lexicographical
order and counting frequency of unique queries

Step 2: Insert all unique queries and their frequencies into a trie
(also called a prefix tree).

`Trie`:

- A tree representing a set of strings.
- Edges of the tree are labeled.
- Children of nodes are ordered.
- Root to node path represents prefix of all strings in the subtree starting at that node.

Prefix search using a trie: Insert queries into trie. For a pattern P, find node in trie
representing the subtree prefixed by P in $O(|P|)$ time.

Observation: The subtree prefixed by P corresponds to a continuous range.

### Query expansion

- User and documents may refer to a concept using different words (poison - toxin, danger - hazard, postings list - inverted list)
- Vocabulary mismatch can have impact on **recall**
- Users often attempt to fix this problem manually (query reformulation)
- Adding these synonyms should improve query performance (query expansion)

#### Psuedorelevance feedback

- Take top-K results of original query
- Determine important/informative terms/topics (topic modelling!) shared by those documents
- Expand query by those terms
- No explicit user feedbackneeded (also called `blind relevance feedback`)

#### Indiret relevance feedback

- For a query look at what users click on in the result page
- Use clicks as signal of relevance
- Learning-2-Rank uses neural models to rerank result pages

#### Query expansion summary

- Helps with vocabulary **mismatch**
- Can improve **recall**
- Global expansion
- User, pseudo or indirect relevance feedback

## 19. Index constructin & Advanced queries

### Inverted index construction

#### Static construction

**Invert one batch:**

1. Process documents in batch
2. Use global vocabulary to map terms to term ids
3. Create inverted index for all docs in batch
4. Can already compress postings lists (vbyte)

**Merge batches:**

1. Open all n files containing batches on disk
2. Read one term (or a few) from each file
3. Perform n-way merge, merging terms with same ids
4. Merge equivalent to appending bytes as document ids are increasing
5. Read the next terms

#### Incremental, logarithmic indexing

1. Use a logarithmic number ($\log N$) of indexes. At each level i, store index of size $2^i \times n$
2. Query all log N indexes at the same time and merge results
3. Construction cost: $N \log(N/n)$ ???

#### Index construction summary

- Block based processing of large collections
- Merge blocks to larger indexes
- Logarithmic merging reduces I/O costs
- Query multiple indexes at once
- When dealing with large amounts of data, careful engineering of construction algorithms is required to make things "work"

### Phrase searching

`Phrase queries`:

1. Seek to identify documents that contain a specific phrase "the who"
2. Combination of individual known terms that occur sequentially in documents
3. Two or more terms possible - "the president of the united states"

**Approaches:**

1. Inverted Index based
2. String matching indexes (Suffix Arrays)

#### Positional Inverted Index

- Intersect document ids of phrase terms
- For the documents containing all terms, intersect position lists
- Perform ranking on result set

**Problems:**

- Slow when phrase terms occur in many documents in the collection ("the who")
- Slow when terms occur often in documents but not as phrase ("the president of the united states")

#### Suffix arrays

- Alternative search index which does not require tokenization
- Widely used in bioinformatics for exact string searches
- Gives you more runtime guarantees than an inverted index

#### Phrase search summary

- Inverted indexes with positional information and intersection
- Use substantially more space than regular inverted index
- Suffix array is an alternative index structure to inverted indexes
- Complex queries require specialized indexes

## 20. IR evaluation， Re-ranking, L2R

### Relevance measures

Mainly use **precision oriented** metrics:

- `precision@k`
- average precision (`AP`): rank sensitive
- Mean Average Precision (`MAP`): AP averaged across multiple queries

**Utility based** relevance metrics:

- Rank-Biased Precision (`RBP`)

$$ RBP = (1 - P) \times \Sigma_{i=1}^{d} r_i \times p^{i-1} $$

### Multi stage retrieval

- Use a cheap, fast, simple similarity metric (such as BM25) to retrieve an initial set of relevant documents (top-k retrieval)
- For those k documents, apply a Machine Learning algorithm which uses more features to re-rank the initial set of k documents
- Why not apply Machine Learning to rank all documents? Expensive!

### Learning to rank

#### Point-wise objective

Given a query q, a document $d_i$, and a user u, find a function $f(q,d_i,u)$ that predicts $r_i$ for document $d_i$.

Point-wise algorithm sketch:

- Train classifier that can predict $r_i$
- Train model that can compute:

$$ P(r_i = relevant | x_i) $$

- Sort documents by the probability of being relevant
- Multiple classes: Assign classes a value and compute expectation (e.g. -2 highly non relevant, 2 highly relevant)

### Summary

- Evaluation using **relevance judgements**
- Precision@k, (M)AP, RBP evaluation metrics
- Use **BM25** as a _first step_ in multi-stage retrieval system
- Use complex trained ranking models to **re-rank** the original BM25 ranking
- Many _features and training methods_ exists

#### Pair-wise objective

#### List-wise objective

## 21. Machine Translation: word based models

### Noisy channel

**Use Baye's inversion:**

$P(e|f) = \frac{ P(e)P(f|e) }{P(f)}$

**Decoder seeks to maximise:**

$\hat{e} = argmax_e P(e)P(f|e)$

$P(e)$ - `Language Model (LM)`

$P(f|e)$ - `Translation Model (TM)`

Responsible for:

- $P(f|e)$ rewards good translations, but permissive of disfluent e
- $P(e)$ rewards e which look like fluent English, and helps put words in the correct order

_Why not just one TM to model $P(e|f)$ directly?_

- If we do so, we can't be sure that the translation will follow proper english rules  (fluency) . Is this the main reason?...
- Yes, this is largely the answer.
- The thing is, any TM you learn will be pretty basic, and won't be able to produce very well formed sentences. To correct for this problem, the noisy-channel setup allows you to pair a TM with a LM, which helps to address this problem as the LM can help to ensure well formedness.
- This argument holds less true with neural seq2seq models, which directly model p(e|f) without using the noisy channel setting. (Although some research has shown benefits from using neural models in a noisy channel.)

**Learning**:

- LM: based on text frequencies in large monolingual corpora
- TM: based on word co-occurrences in parallel texts

`Parallel texts` / `Bitexts`:

- one text in multiple languages
- Produced by human translation; readily available on web: news, legal transcripts, literature, subtitles, bible, …

### IBM Model 1

Formulate probabilistic model of translation:

$P(F, A|E) = \frac{\epsilon}{(I + 1)^J} \Pi_{j=1}^J t(f_j | e_{a_j})$

where $t(f|e)$ are translation probabilities; alignments $a_j$ indexes the translation of word j

#### EM for IBM1

1. make initial guess of **t** parameters, e.g. uniform
2. initialise counts **c** of translation pairs to 0
3. for each sentence pairs **(E, F)**, for each position **j** and value of $a_j \in {1,2,3, ..., I}$,

    - compute $P(a_j|E,F)$ i.e. $P( a_j|E,F ) = \frac{ t(f_j | e_{a_j}) } { \Sigma_{a_j} t(f_j|e_{a_j}) }$
    - update fractional counts $c(e_j, f_{a_j}) \gets c(e_j, f_{a_j}) + P(a_j | E, F)$

4. update t with normalised counts $t(f|e) = c(e, f) / c(e)$

#### Modelling limitations

- simple model and quite naive - ignores the positions of words in both strings
- limited to word0based phenonema
- asymmetric, can't handle 1:many or many:many
- learning from sparse data (solution: using large corpora)

### HMMS for alignment

- IBM 2 & 3 include an explicit term for modelling typical alignment values using table of condition probabilities $P_r(a_j = i|j, l, m)$
- suffers for long sentence pairs, where there too little data to estimate
- HMM provides a better solution: each alignment $a_j$ depends on the previous alignment $a_{j-1}$

???

## 22. Machine translation: phrase based translation & Neural encoder-decoder

### Phrase based MT

Treats n-grams as translation units.

- Start with sentence-aligned parallel text
1. learn word alignments
2. extract phrase-pairs from word alignments & normalise counts
3. learn a language model
- Now decode test sentences using beam-search (where 2 & 3 above form part of scoring function)

### Neural machine translation

So-called `sequence2sequence` models combine:

- `encoder` which represents the source sentence as a vector or matrix of real values; akin to word2vec’s method for learning word vectors
- `decoder` which predicts the word sequence in the target; framed as a language model, albeit conditioned on the encoder representation

### MT evaluation: BLEU

BLEU measures **closeness** of translation to one or more references:

$BLEU = bp \times prec_{1-gram} \times prec_{2-gram} \times prec_{3-gram} \times prec_{4-gram}$

- **weighted average** of 1, 2, 3 & 4-gram precisions: $precn-gram = num n-grams correct / num n-grams$ predicted in output; numerator clipped to #occurences of ngram in the reference
- **brevity penality** hedge against short outputs: $bp = min ( 1, output length / reference length )$
- Correlates with human judgements of fluency & adequacy