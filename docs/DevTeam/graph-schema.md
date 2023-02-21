# Graph Schema

## Introduction

The graph schema is designed specifically to fit the needs of a crowd-sourcing application. It can be used to persist any kind of data structure, while allow the structure to be mutated in any way by any one. Any form of the structure can be recovered for any use case. 

For example, you could have a document that you want everyone to be able to edit in any way they see fit. You also want to be able to view the document using only the edits of certain users during a certain timeframe. You'd also like to let users vote on different edits to the document, while showing the document using the most voted on edits, while also giving the option to show the edits from the votes of a specific group of users.

## Tables

Database [schema](https://github.com/etenlab/database-api/blob/main/src/core/sql/schema/v1.schema.sql)

1. `node_types`
1. `node_property_keys`
1. `node_property_values`
1. `relationship_types`
1. `relationships`
1. `relationship_property_keys`
1. `relationship_property_values`

## Legend

![Graph Legend](./img/legend.png)

## Word

![word](./img/word.png)

## Word Sequence

![word-sequence](./img/word-sequence.png)

## Word to Article Link

![word-to-article-link](./img/word-to-article-link.png)

## Verse

![verse](./img/verse.png)

## Chapter

![chapter](./img/chapter.png)

## Book

![book](./img/book.png)

## Bible

![bible](./img/bible.png)

## Sentence

![sentence](./img/sentence.png)

## Article

![article](./img/article.png)

## Paragraph

![paragraph](./img/paragraph.png)

## Section

![section](./img/section.png)

## Section of Sections

![section-of-sections](./img/section-of-sections.png)

## Strong's Entry

![strongs-entry](./img/strongs-entry.png)

## Strong's Word Link

![strongs-word-link](./img/strongs-word-link.png)

## External Content

![external-content](./img/external-content.png)

## Voting

![voting](./img/voting.png)
