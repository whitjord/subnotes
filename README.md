
# **subnotes** (working draft)

An Open Standard for Notes, Flashcards, and Spaced Repetition  

[![License: CC BY-SA 4.0](https://licensebuttons.net/l/by-sa/4.0/80x15.png)](http://creativecommons.org/licenses/by-sa/4.0/)

---

## Why Notes, Flashcards, and Spaced Repetition

To keep what you learn. 

We spend a lot of time and effort teaching and learning new information, but very little effort on retaining that information, which is too often forgotten or lost.

Notes and Flashcards each have strengths that complement each other. 

* **Notes** are good for capturing and organizing important pieces of information.
* **Flashcards**, when used with **Spaced Repetition**, are great for getting those important pieces of information into long term memory.  
  (If you would like to learn more about this you may want to look at the following topics on Wikipedia.) 
    * [Spaced Repetition](https://en.wikipedia.org/wiki/Spaced_repetition)
    * [Flashcard](https://en.wikipedia.org/wiki/Flashcard)
    * [Forgetting Curve](https://en.wikipedia.org/wiki/Forgetting_curve)
    * [Testing Effect](https://en.wikipedia.org/wiki/Testing_effect) 
    * [Spacing Effect](https://en.wikipedia.org/wiki/Spacing_effect)
    * [Active Recall](https://en.wikipedia.org/wiki/Active_recall)
    
## Why an Open Standard for Notes, Flashcards, and Spaced Repetition

* Existing open source projects focus on either *notes* or *flashcards*, but not both.
* Existing open source projects focus mainly on a single end user application, but not a standard that could be integrated into multiple applications and libraries.
* Existing open source Spaced Repetition software limit what "scheduling" algorithms a person can use.
* Existing proprietary solutions have the same issues as the open source ones and are, well, proprietary. 


### Specifications Used by **subnotes**

* [JSON](http://www.json.org/) 
* [JSON Schema](http://json-schema.org/)
* [JSON Pointers](https://tools.ietf.org/html/rfc6901)
* [CommonMark](http://spec.commonmark.org/)

---

## Core Specification

### Topics, Notes, and Flashcards

Subnotes use a tree structure to organize notes and flashcards by topic. There are three types of nodes: *topic*, *note*, and *flashcard*. The root node is always a *topic* node. It is the only required node and is also called a *notebook*.

#### *flashcard*

> A card with a question on one side and its answer on the other; used as an aid for memorization.

A *flashcard* node must contain a **question** and an **answer**. It is a leaf node, meaning it contains no other nodes. A *flashcard*'s parent node can be a *note* or a *topic* node.

You can think of it like a notecard with a question on one side and an answer on the other.

*flashcard schema*

```json
{
  "flashcard": {
    "type": "object",
    "properties": {
      "question": {
        "description": "A question. To be parsed as Markdown",
        "type": "string"
      },
      "answer": {
        "description": "An answer. To be parsed as Markdown",
        "type": "string"
      }
    },
    "required": [
      "question",
      "answer"
    ]
  }
}
```

*Example flashcard (simplest form)*

```json
{
  "question": "What is the capital of Argentina?",
  "answer": "Buenos Aires"
}
```

#### *note*

> A brief record of something written down for future reference.

A *note* node must contain a **note**. It may include a **topic**. It may contain **flashcards**, a list of zero or more *flashcard* nodes. A *note*'s parent node is always a *topic* node.

You can think of it like an notecard containing a concise piece of information about a topic, but also a folder that can contain flashcards.

*note schema*

```json
{
  "note": {
    "type": "object",
    "properties": {
      "note": {
        "description": "A note. To be parsed as Markdown",
        "type": "string"
      },
      "topic": {
        "description": "The topic of the note. To be parsed as plain text",
        "type": "string"
      },
      "flashcards" : {
        "description": "A list of zero or more flashcards",
        "type": "array",
        "items": {
          "$ref": "#/flashcard"
        }
      }
    },
    "required": [
      "note"
    ]
  }
}
```

*Example note (simplest form)*

```json
{
  "note": "* Capital: Nairobi\n* Population: 48 million\n* Continent: Africa\n"
}
```

#### *topic*

> The subject or theme of a text.

A *topic* node must contain a **topic**. It may include a **note**. It may contain **flashcards**, a list of zero or more *flashcard* nodes. It may contain **subnotes**, a list of zero or more *note* nodes. It may contain **subtopics**, and a list of zero or more *topic* nodes. A *topic*'s parent node, if it has one, is always another *topic* node.

You can think of it like a folder that can contain flashcards, notecards, and other folders. 

*topic schema* 

```json
{
  "topic": {
    "type": "object",
    "properties": {
      "topic": {
        "description": "A note related to the topic. To be parsed as Markdown",
        "type": "string"
      },
      "flashcards": {
        "description": "A list of zero or more flashcards",
        "type": "array",
        "items": {
          "$ref": "#/flashcard"
        }
      },
      "subnotes": {
        "description": "A list of zero or more notes",
        "type": "array",
        "items": {
          "$ref": "#/note"
        }
      },
      "subtopics": {
        "description": "A list of zero or more topics",
        "type": "array",
        "items": {
          "$ref": "#/topic"
        }
      }
    },
    "required": [
      "topic"
    ]
  }
}
```

*Example topic (simplest form)*

```json
{
  "topic": "Europe"
}
```

### Libraries and Notebooks

#### *notebook*

A *notebook* is just a root level *topic* node. It typically contains related, **flashcards**, **subnotes**, and **subtopics**.

#### *library*

A *library* is just a collection (list) of *notebooks*

### Markdown

Notes, questions, and answers are written in Markdown as defined in the [CommonMark Specification](http://spec.commonmark.org/). Markdown has the advantage over HTML of being easily human readable and writeable. Plenty of open source libraries are available for converting markdown to HTML and other document formats. There are also open source WYSIWYG Markdown editors that can be used so end users don't even have to use Markdown directly.  

The use of some Markdown elements in subnotes are discouraged, but not prohibited. The follow is a partial list of such elements and the reasons they should be avoided. *(Still not sure if they should be prohibited, not required to be supported, or just discouraged, TBD)*

* Headers: This is because the *topics* should act as headers.
* Horizontal rules: There should be no need to separate sections of a note, one should usually just create a new note or topic.
* Raw HTML: Could create unexpected results when used by different software and create and inconsistent results

### Spaced Repetition

JSON that stores history of flashcard reviews, next review date of flashcard, a means to determine priority if overdue for review, and name of algorithm used to determine next review date. More coming soon...

### Todo

* Spaced Repetition section: schema, examples
* Add section for conventions used in this doc
* Add more examples for flashcards, notes, and topics
* Add possible HTML outputs for example flashcards, notes, topics, notebook
* Allow pointers (symbolic links) for *note* nodes inside **subnotes** (and maybe the same *topic* nodes and **subtopics** too). Use JSON pointers.
* Create a complete JSON Schema (Still not sure if JSON Schema will do everything I need it to)
* If notes and flashcards allow pictures, how should they be "embedded"? Archive file that contains JSON and images? 
* Alternate versions for flashcards. This is different than a list of flashcards. Spaced Repetition software would randomly pick a version when displaying a flashcard.

--- 

## Extensions

Subnotes core spec should stay simple and comply with the other specs it is based on, but it should also be extensible. Extensions will probably come in three types, listed below with examples.
 
1. Extensions for Nodes and Libraries  
   (Additional metadata, name/value pairs, maybe use JSON Schema, but not sure yet how that would integrate with core spec JSON schema. Some of these could make their way into core spec.)
   * Reference/citations for notebooks and notes
   * regular expression answers for flashcards to facilitate automated testing
   * Different "types" for *note* nodes. A "definition" *note* could facilitate dynamic generation of *flashcards* and glossaries using the *note*'s **topic** and **note** fields  
2. Extensions for Markdown  
   (Should be existing markdown extensions that are commonly used and well supported by markdown libraries.)
   * LaTex/Tex for displaying math
   * Tables
3. Extensions for Spaced Repetition  
   (Most likely just different types of scheduling algorithms.)
   * SM-2 (SuperMemo 2)
 
### Todo
* Expand, refine
* Define process for standardizing extensions (what form should they be in, where are they stored, how are they submitted, accepted, etc.)

---

Author: Jordan White
