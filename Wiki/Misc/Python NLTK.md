A **collocation** is a sequence of words that occur together unusually often. Thus _red wine_ is a collocation, whereas _the wine_ is not. A characteristic of collocations is that they are resistant to substitution with words that have similar senses; for example, _maroon wine_ sounds definitely odd.

To get a handle on collocations, we start off by extracting from a text a list of word pairs, also known as **bigrams**. This is easily accomplished with the function bigrams()

**Lexicon**

-   A lexicon, or lexical resource, is a collection of words and/or phrases along with associated information such as part of speech and sense definitions. Lexical resources are secondary to texts, and are usually created and enriched with the help of texts.
-   A **lexical entry** consists of a **headword** (also known as a **lemma**) along with additional information such as the part of speech and the sense definition. Two distinct words having the same spelling are called **homonyms**.
-   The simplest kind of lexicon is nothing more than a sorted list of words. Sophisticated lexicons include complex structure within and across the individual entries. In this section we'll look at some lexical resources included with NLTK.
-   There is also a corpus of **stopwords**, that is, high-frequency words like _the_, _to_ and _also_ that we sometimes want to filter out of a document before further processing. Stopwords usually have little lexical content, and their presence in a text fails to distinguish it from other texts.

## A Pronouncing Dictionary

-   A slightly richer kind of lexical resource is a table (or spreadsheet), containing a word plus some properties in each row. NLTK includes the CMU Pronouncing Dictionary for US English, which was designed for use by speech synthesizers.
    
-   For each word, this lexicon provides a list of phonetic codes — distinct labels for each contrastive sound — known as _phones_. Observe that _fire_ has two pronunciations (in US English): the one-syllable F AY1 R, and the two-syllable F AY1 ER0. The symbols in the CMU Pronouncing Dictionary are from the _Arpabet_, described in more detail at [http://en.wikipedia.org/wiki/Arpabet](http://en.wikipedia.org/wiki/Arpabet)
    
    [Untitled](https://www.notion.so/e2bfdb83cf0e4aa39a61c90e16782f62)
    

**WordNet :**

-   WordNet is a semantically-oriented dictionary of English, similar to a traditional thesaurus but with a richer structure. NLTK includes the English WordNet, with 155,287 words and 117,659 synonym sets
-   Hypernyms and hyponyms are called **lexical relations** because they relate one synset to another. These two relations navigate up and down the "is-a" hierarchy. Another important way to navigate the WordNet network is from items to their components (**meronyms**) or to the things they are contained in (**holonyms**). For example, the parts of a _tree_ are its _trunk_, _crown_, and so on; thepart\_meronyms(). The _substance_ a tree is made of includes _heartwood_ and _sapwood_; the substance\_meronyms(). A collection of trees forms a _forest_

Universal Part-of-Speech Tagset

[Untitled](https://www.notion.so/6c5fd35ee2664838bd6f3505467342b3)

**Nouns**

Nouns generally refer to people, places, things, or concepts, e.g.: _woman, Scotland, book, intelligence_. Nouns can appear after determiners and adjectives, and can be the subject or object of the verb, as shown below

Syntactic Patterns involving some Nouns

[Untitled](https://www.notion.so/22007e4b59e149fda38a2111ae278d66)