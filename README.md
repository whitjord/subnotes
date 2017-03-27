# An Open Standard for Notes, Flashcards, and Spaced Repetition (Rough Draft)

[![License: CC BY-SA 4.0](https://licensebuttons.net/l/by-sa/4.0/80x15.png)](http://creativecommons.org/licenses/by-sa/4.0/)

## Why Notes, Flashcards, and Spaced Repetition

To keep what you learn. 

We can spend a lot of time and effort teaching and learning new information, but very little on retaining that information, which is too often forgoten or lost.

Notes and Flashcards have strengths and weaknesses. Fortunately each of their weaknesses is overcome by the strength of the other.

* __Notes__ are ideal for capturing and organizing important facts.
* __Flashcards__, along with __Spaced Repetition__, are ideal for getting important facts into a your long term memory. If you would like to learn more about this you may want to look at the following topics on Wikipedia. 
    * [Spaced Repetition](https://en.wikipedia.org/wiki/Spaced_repetition)
    * [Flashcard](https://en.wikipedia.org/wiki/Flashcard)
    * [Forgeting Curve](https://en.wikipedia.org/wiki/Forgetting_curve)
    * [Testing Effect](https://en.wikipedia.org/wiki/Testing_effect) 
    * [Spacing Effect](https://en.wikipedia.org/wiki/Spacing_effect)
    
## Why An Open Standard for Notes, Flashcards, and Spaced Repetition

* Existing open source projects focus on either *notes* or *flashcards*, but not both.
* Existing open source projects focus mainly on a single end user application, but not a standard that could be intergrated into multiple applications.
* Existing open source Spaced Repetition software limits what "scheduling" algorithms a person can use.
* Propriatary solutions have the same issues as the open source ones and are, well, propriatary. 

## Definition of Terms

* __Notecard__ - A *notecard* consists of a *subtitle* (plain text) and a *note* (to be determined markup language). It may also act as a container for other *notecards* (*sub-notecards*) or *flashcards*.
* __Flashcard__ - A study *question* and *answer* associted with a *notecard* (to be determined markup language).
* __Notebook__ - An organized collection of *notecards* and *flashcards* (Technically the root or top level *notecard*).
* __Library__ - A person or organization's collection of *notebooks*.

## Existing Specifications Used by This Standard

* [JSON](http://www.json.org/) (or maybe [YAML](http://yaml.org/))
* [CommonMark](http://spec.commonmark.org/) (or maybe [HTML](https://www.w3.org/TR/html/))

**NOTE:** Examples are shown first in JSON and then in YAML. All Examples use Markdown for the markup language, not HTML.

## Notecards

A *notecard* consists of a *subtitle* (plain text) and a *note* (markup language). 
It may also contain *flashcards* and even other *notecards* (*sub-notecards*). 

*json*
```json
{
  "subtitle": "France",
  "note": "* Capital: Paris\n* Population: 66 million\n* Continent: Europe\n",
  "sub-notecards": null,
  "flashcards": null
}
```

*yaml*
```yaml
---
subtitle: France
note: |
  * Capital: Paris
  * Population: 66 million
  * Continent: Europe
sub-notecards:
flashcards:
```

### Branch Notecards

* Contain an ordered list of other *sub-notecards* (You can think of them like a directory or folder for other *notecards* (*sub-notecards*), along with optionaly containing a *note* themselves).
* The *subtitle* value **cannot** be empty.
* The *note* value **can** be empty.
* Can contain zero or more *flashcards*.
* TBD if *sub-notecards* are treated differently if they are *branch* or *leaf* *notecards*. For example, should *sub-leaf notecards* always be listed before *sub-branch notecards*.

#### Notebook

A *notebook* contains *notecards* and *flashcards* that are normally related to a single topic. A *notebook* is just the top (or root) level *branch notecard*. The one exception is if it is the only notecard. 

### Leaf Notecards

* Do **not** contain any other *notecards* (*sub-notecards*).
* The *subtitle* value **can** be empty.
* The *note* value **cannot** be empty.
* Can contain zero or more *flashcards*.

### Example Branch and Leaf Notecards

In this example, "Kenya" is the subtitle of the *branch notecard* and "Border Countries" is the *subtitle* of the *sub-leaf notecard*

*json*
```json
{
  "subtitle": "Kenya",
  "note": "* Capital: Nairobi\n* Population: 48 million\n* Continent: Africa\n",
  "subnotes": [
    {
      "subtitle": "Border Countries",
      "note": "* Tanzania\n* Uganda\n* South Sudan\n* Ethiopia\n* Somalia\n"
    }
  ],
  "flashcards": null
}
```

*yaml*
```yaml
---
subtitle: Kenya
note: |
  * Capital: Nairobi
  * Population: 48 million
  * Continent: Africa
subnotes:
- subtitle: Border Countries
  note: |
    * Tanzania
    * Uganda
    * South Sudan
    * Ethiopia
    * Somalia
flashcards:
```

## Flashcards

An example *notecard* that contains two *flashcards*

*json*
```json
{
  "subtitle": "Argentina",
  "note": "* Capital: Buenos Aires\n* Population: 43 million\n* Continent: South America\n",
  "sub-notecards": null,
  "flashcards": [
    {
      "question": "What is the capital of Argentina?",
      "answer": "Buenos Aires"
    },
    {
      "question": "What is the population of Argentina?",
      "answer": "43 Million"
    }
  ]
}
```

*yaml*
```yaml
---
subtitle: Argentina
note: |
  * Capital: Buenos Aires
  * Population: 43 million
  * Continent: South America
sub-notecards:
flashcards:
- question: What is the capital of Argentina?
  answer: Buenos Aires
- question: What is the population of Argentina?
  answer: 43 Million
```

## Markup for Notes and Flashcards

Markup language is to be determined, but will probably be a subset of Markdown, specifically [CommonMark](http://commonmark.org/), with some posible extensions for math and simple tables. Markdown has the advantage over HTML of being easily human readable and writeble. There are plenty of open source libraries for converting markdown to HTML and other document formats and even open source WYSIWYG editors.  

**Included in Supported Subset of Markdown (partial list)**

* lists (unordered, ordered, sublists, mixed lists).
* code, `inline and block`.
* strong (**bold**), emphasis (*italic*), and combination of the two (***both***).

**Included Markdown Extensions**

* TeX/LaTex (for displaying math).
* Simple tables (Github Flavored Markdown style).

**Not Included in Subset of Markdown (partial list)**

* Headers, this is because the *notecard*'s subtitles should act as headers.
* Horizontal rules, there should be no need to seperate sections of a *note*, you would just create a new *notecard*.

## Spaced Repetition

## Posible Additional Features

* Image support in *notes* and *flashcards*.
* Special types of *notecards* that could facilite dynamic creations. For example, a "definition" *notecard* could facilitate creating two *flashcards* (A regular "What is <*subtitle*>?" question and a Jepordy style question, "What word does <*note*> describe?"). A "definition" *notecard* would also create the ability generate a glossary for a *notebook* dynamicaly. 
* *Notebook* meta data (Author, date, version, refrences/citations, canonical/perma link, etc.).
* *Notecard* meta data (reference/citation, type, etc).
* Alternate versions for *flashcards* (Example: Q0: 3 + x = 9, Q1: 4 + y = 7, Q2: z + 2 + 5). Spaced Repetition programs could randomly pick one. Note: This is not the same as having multiple *flashcards* for a single *notecard*.
* Regular Expression answers in *flashcards* for automated testing (and posibly other pattern matching or parsing methods). 
* *Notecard* links (like symbolic links in Unix or shortcuts in Windows) as long as they don't create a recursive/infinite loop. Would probably use [JSON Pointers](https://tools.ietf.org/html/rfc6901) to implement.
* Markdown to *notebook*. A standard for converting a Markdown document into a *notebook*/*notecards*. It would probably not support flashcards. This could be used by someone taking notes in a text file using markdown and an editor of their choice. Headers would be used for *subtitles*, the header level to determine if it is a *sub-notecard*, and blank lines used to seperate *notecards* 

## Features Intentionally Not Included

* Multiple choice questions & answers. Spaced Repetition is more effective if you retrieve the answer from your mind, not a provided list.  

## Author

Jordan White
