
# **subnotes** (working draft)

An Open Standard for Notes, Flashcards, and Spaced Repetition  

[![License: CC BY-SA 4.0](https://licensebuttons.net/l/by-sa/4.0/80x15.png)](http://creativecommons.org/licenses/by-sa/4.0/)

---

## Why Notes, Flashcards, and Spaced Repetition

To keep what you learn. 

We spend a lot of time and effort teaching and learning new information, but very little effort on retaining that information, which is often forgotten or lost.

Notes and Flashcards each have strengths that complement each other. 

* **Notes** are good for capturing and organizing important pieces of information.
* **Flashcards**, when used with **Spaced Repetition**, are great for getting those important pieces of information into long term memory.  
  (If you would like to learn more about this you may want to look at the following topics on Wikipedia.) 
    * [Spaced Repetition](https://en.wikipedia.org/wiki/Spaced_repetition)
    * [Flashcard](https://en.wikipedia.org/wiki/Flashcard)
    * [Forgetting Curve](https://en.wikipedia.org/wiki/Forgetting_curve)
    * [Testing Effect](https://en.wikipedia.org/wiki/Testing_effect) 
    * [Spacing Effect](https://en.wikipedia.org/wiki/Spacing_effect)
    
## Why an Open Standard for Notes, Flashcards, and Spaced Repetition

* Existing open source projects focus on either *notes* or *flashcards*, but not both.
* Existing open source projects focus mainly on a single end user application, but not a standard that could be integrated into multiple applications and libraries.
* Existing open source Spaced Repetition software limit what "scheduling" algorithms a person can use.
* Proprietary solutions have the same issues as the open source ones and are, well, proprietary. 

## Two Specifications

* **subnotes** - A specification for notes and flashcards organized by subtopics.
* **reps** - A specification for implementing spaced repetition using subnotes flashcards.  

### Specifications Used by **subnotes**

* [JSON](http://www.json.org/) (or maybe [YAML](http://yaml.org/))
* [CommonMark](http://spec.commonmark.org/) (or maybe [HTML](https://www.w3.org/TR/html/))
* (Maybe [JSON Pointers](https://tools.ietf.org/html/rfc6901))
* (Maybe [JSON Schema](http://json-schema.org/))

---

## **subnotes** Specification

### Subtopics, Notecards, and Flashcards

Subnotes use a tree structure to organize notes and flashcard by subtopics. There are three types of nodes, *subtopics*, *notecards*, and *flashcards*. The root node is always a *subtopic*. It is the only required node and is also called a *notebook*.

*NOTE: I'm considering using [JSON Schema](http://json-schema.org/) to define the data structure(s)*

#### *flashcard*

A question and answer pair.

*name / value pairs*
```yaml
- name: question
  value: string (Markdown)
  required: yes
- name: answer
  value: string (Markdown)
  required: yes
```

*Example flashcard (YAML)*
```yaml
question: What is the capital of Argentina?
answer: Buenos Aires 
```

*Example flashcard (JSON)*
```json
{
  "question": "What is the capital of Argentina?",
  "answer": "Buenos Aires"
}
```

#### *notecard*

A note, which may include a topic (subtopic/subtitle), and may contain a list of one or more flashcards.

*name / value pairs*
```yaml
- name: note
  value: string (Markdown)
  required: yes
- name: topic
  value: string (plain text)
  required: no
- name: flashcards
  value: array (of flashcards)
  required: no
```

*Example notecard (YAML)*
```yaml
topic: Kenya
note: |
  * Capital: Nairobi
  * Population: 48 million
  * Continent: Africa
```

*Example notecard (JSON)*
```json
{
  "topic": "Kenya",
  "note": "* Capital: Nairobi\n* Population: 48 million\n* Continent: Africa\n"
}
```

#### *subtopic*

A topic (subtopic/title/subtitle), which may include a note, and may contain a list of one or more flashcards, notecards, or even other subtopics. You can think of it like a folder, directory, or section header in an outline. 

*name / value pairs*
```yaml
- name: topic
  value: string (plain text)
  required: yes
- name: note
  value: string (Markdown)
  required: no
- name: flashcards
  value: array (of flashcards)
  required: no
- name: notecards
  value: array (of notecards)
  required: no
- name: subtopics
  value: array (of subtopics)
  required: no
```

*Example subtopic/notebook that contains a notecard, flashcard, and several subtopics (YAML)*
```yaml
topic: Geography
note: This is my notebook for storing info on Geography.
subtopics:
  - topic: Countries
    subtopics:
      - topic: France
        note: A country in Europe
        notecards:
          - topic: basic facts
            note: |
              * Capital: Paris
              * Population: 66 million
              * Continent: Europe
            flashcards:
              - question: What is the capital of France?
                answer: Paris
      - topic: Vietnam
      - topic: New Zealand
```
*Example subtopic/notebook that contains a notecard, flashcard, and several subtopics (JSON)*
```json
{
  "topic": "Geography",
  "note": "This is my notebook for storing info on Geography.",
  "subtopics": [
    {
      "topic": "Countries",
      "subtopics": [
        {
          "topic": "France",
          "note": "A country in Europe",
          "notecards": [
            {
              "topic": "basic facts",
              "note": "* Capital: Paris\n* Population: 66 million\n* Continent: Europe\n",
              "flashcards": [
                {
                  "question": "What is the capital of France?",
                  "answer": "Paris"
                }
              ]
            }
          ]
        },
        {
          "topic": "Vietnam"
        },
        {
          "topic": "New Zealand"
        }
      ]
    }
  ]
}
```

### Libraries and Notebooks

#### *notebook*

A *notebook* is just a root level *subtopic*. It typically contains notecards, flashcards, and other subtopics that share a common Theme.

#### *library*

A *library* is just a collection (array) of *notebooks*

### Markdown

Notes, questions, and answers are written in a subset of [CommonMark](http://commonmark.org/), with a few widely adopted extensions. Markdown has the advantage over HTML of being easily human readable and writeable. There are plenty of open source libraries for converting markdown to HTML and other document formats and even open source WYSIWYG editors.  

**Included in supported subset of Markdown (partial list)**

* Lists
  1. Unordered
  2. Ordered
  3. Sublist
  4. Mixed
* `Code, inline and block.`
* Strong (**bold**), emphasis (*italic*), and combination of the two (***both***).

**Included Markdown extensions**

* TeX/LaTex (for displaying math).
* Simple tables (Github Flavored Markdown style).

**Not included in supported subset of Markdown (or at least discouraged, partial list)**

* Headers, this is because the *subtopics'* topic should act as headers.
* Horizontal rules, there should be no need to separate sections of a *note*, you would just create a new *notecard*.

---

## **reps** Specification

*Coming soon...*

--- 

## To Be Determined

* JSON or YAML. YAML is easier to read, but if we use it we might want to make sure we don't use any features that would prevent converting it to JSON (YAML is a superset of JSON). We could just go with JSON and someone could always output it as YAML if they really wanted to.
* Markdown or HTML. Leaning towards Markdown.
* Should there be a max depth of *subtopics* in a *notebook*. If trying to match HTML/Markdown header levels the deepest level should be 6, including the *notebook* (root level *subtopic*). 

## Possible/Probable Additional Features

* Image support in *notecards* and *flashcards*. Might require using some kind of archival file format, like zip, to bundle images and JSON.
* Special types of *notecards* that could facilitate dynamic creations. For example, a "definition" *notecard* could facilitate creating two *flashcards* (A regular "Define <*notecard:title*>" question and a Jeopardy style question, "What word does <*notecard:note*> describe?"). A "definition" *notecard* would also create the ability generate a glossary for a *notebook* dynamically. 
* *Notebook* meta data (Author, date, version, references/citations, canonical/perma link, etc.).
* *Notecard* meta data (reference/citation, type, etc.).
* Alternate versions for *flashcards* (Example: [question: 3 + x = 9], [question-alt1: 4 + y = 7], [question-alt2: z + 2 = 5]). Spaced Repetition programs could randomly pick one. Note: This is not the same as having multiple *flashcards* in a single *notecard* or *subtopic*.
* Option for Regular Expression answers in *flashcards* to support automated testing (and possibly other pattern matching or parsing methods too). 
* *Notecard* and *subtopic* links (like symbolic links in Unix or shortcuts in Windows) as long as they don't create a recursive/infinite loop. Would probably use [JSON Pointers](https://tools.ietf.org/html/rfc6901) to implement.
* Markdown to *notebook*. A standard for converting a Markdown document into a *notebook*/*notecards*. It would probably not support flashcards. This could be used by someone taking notes in a text file using markdown and an editor of their choice. Headers would be used for *subtopics* and blank lines used to separate *notecards*. (this maybe shouldn't be in the standard, but could be an interesting side project)

## Features Intentionally Not Included

* Direct support for multiple choice questions & answers. Spaced Repetition is more effective if you retrieve the answer from your mind, not a provided list.  

## Author

Jordan White
