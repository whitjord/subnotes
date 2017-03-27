# An Open Standard for Notes, Flashcards, and Spaced Repetition (Rough Draft)

[![License: CC BY-SA 4.0](https://licensebuttons.net/l/by-sa/4.0/80x15.png)](http://creativecommons.org/licenses/by-sa/4.0/)

## Why Notes, Flashcards, and Spaced Repetition

To keep what you learn. We can spend a lot of time and effort teaching and learning new information, but very little on retaining that information, which is too often forgoten.

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
* Propriatary solutions are, well, propriatary. 

## Definition of Terms

* __Notecard__ - A *notecard* consists of a *subtitle* (plain text) and a *note* (to be determined markup language). 
  It may also act as a container for other *notecards* or *flashcards*.
* __Flashcard__ - A study *question* and *answer* associted with a *notecard* (to be determined markup language).
* __Notebook__ - An organized collection of *notecards* and *flashcards* (Technically the root or top level *notecard*).
* __Library__ - A person or organization's collection of *notebooks*.

## Existing Specifications Used by This Standard

* [JSON](http://www.json.org/) (or maybe [YAML](http://yaml.org/))
* [CommonMark](http://spec.commonmark.org/) (or maybe [HTML](https://www.w3.org/TR/html/))

**NOTE:** Examples are shown first in JSON and then in YAML. All Examples use Markdown for the markup language, not HTML.

## Notecards

A *notecard* consists of a *subtitle* (plain text) and a *note* (markup language). 
It may also contain *flashcards* and even other *notecards* (sub-*notecards*). 

```json
{
  "subtitle": "France",
  "note": "* Capital: Paris * Population: 66 million * Continent: Europe\n",
  "subnotes": null,
  "flashcards": null
}
```

```yaml
---
subtitle: France
note: >
 * Capital: Paris
 * Population: 66 million
 * Continent: Europe
subnotes:
flashcards:

```
### Branch Notecards

* Contain an ordered list of other *sub-notecards* (You can think of them like a directory or folder for other *notecards* (*sub-notecards*), along with optionaly containing a *note* themselves).
* The *subtitle* value **cannot** be empty.
* The *note* value **can** be empty.
* Can optionally contain one or more *flashcards*.
* TBD if *sub-notecards* are treated differently if they are *branch* or *leaf* *notecards*. For example, should *sub-leaf notecards* always be listed before *sub-branch notecards*.

```json

```

#### Notebook

A *notebook* contains *notecards* and *flashcards* that are normally related to a single topic. A *notebook* is just the top (or root) level *branch notecard*. 

### Leaf Notecards

* Does **not** contain any other *notecards*.
* The *subtitle* value **can** be empty.
* The *note* value **cannot** be empty.
* Can optionally contain one or more *flashcards*.

## Flashcards

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
* Headers, this is because the Notecard's subtitles should act as headers .
* Horizontal rules, there should be no need to seperate sections of a note, you would just create a new note.

## Spaced Repition

## Posible Aditional Features

* Image support in *notes* and *flashcards*.
* Notebook meta data (Author, date, version, refrences/citations, canonical/perma link, etc.).
* Notecard meta data (reference/citation).
* Alternate versions for *flashcards* (Example: Q0: 3 + x = 9, Q1: 4 + y = 7, Q2: z + 2 + 5). Spaced Repition programs could randomly pick one. Note: This is not the same as having multiple *flashcards* for a single *notecard*. 
* Notecard links (like symbolic links in Unix or shortcuts in Windows) as long as they don't create a recursive/ininite loop. 
  Would probably use [JSON Pointers](https://tools.ietf.org/html/rfc6901) to implement.

## Author(s)

Jordan White
