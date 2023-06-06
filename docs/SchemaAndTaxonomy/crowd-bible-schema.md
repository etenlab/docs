---
sidebar_position: 3
---

# crowd.Bible Sub-Schema

## Introduction

The crowd.Bible sub-schema is designed specifically to fit the needs of a crowd-sourcing application. It can be used to persist any kind of data structure, while allowing the structure to be mutated in any way by any one. Any form of the structure can be recovered for any use case.

For example, you could have a document that you want everyone to be able to edit in any way they see fit. You also want to be able to view the document using only the edits of certain users during a certain timeframe. You'd also like to let users vote on different edits to the document, while showing the document using the most voted on edits, while also giving the option to show the edits from the votes of a specific group of users.

## Crowd Sourcing Schema

By adding 4 additional abstract types to the 6 abstract types shown [here](./abstract-graph-schema.md), we can give users the ability to create elections and place votes on any data in the abstract graph.

![crowd-sourcing-schema](./img/crowd-sourcing-schema.png)

This structure allows us to reference any row in the abstract graph to enable it to become an election or candidate that users can vote on:

![crowd-sourcing-abstract-schema](./img/voting.png)