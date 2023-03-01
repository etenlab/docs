# Graph Schema

## Introduction

The graph schema is designed specifically to fit the needs of a crowd-sourcing application. It can be used to persist any kind of data structure, while allow the structure to be mutated in any way by any one. Any form of the structure can be recovered for any use case. 

For example, you could have a document that you want everyone to be able to edit in any way they see fit. You also want to be able to view the document using only the edits of certain users during a certain timeframe. You'd also like to let users vote on different edits to the document, while showing the document using the most voted on edits, while also giving the option to show the edits from the votes of a specific group of users.

## Tables

Database [schema](https://github.com/etenlab/database-api/blob/main/src/core/sql/schema/v1.schema.sql)

- `node_types`
- `node_property_keys`
- `node_property_values`
- `relationship_types`
- `relationships`
- `relationship_property_keys`
- `relationship_property_values`

## Graph API Layer 1

Layer 1 is the only layer that interacts with the type models.

### Node Type
- createNodeType(type_name: string): string
- listNodeTypes(): String[]
- listAllNodesByType(type_name: string): Node[]

### Nodes
- listAllNodesByType(type_name: string): Node[]
- createNode(type_name: string): uuid
- createNodePropertyKey(node_id: uuid, key_name: string): uuid
- createNodePropertyValue(key_id: uuid, key_value: any): uuid
- readNode(node_id: uuid) Node

### Relationship Type
- createRelationshipType(type_name: string): uuid
- listRelationshipsTypes(): String[]
- listAllRelationshipsByType(type_name: string): Relationships[]

### Relationships
- createRelationship(node_1: uuid, node_2: uuid, type_name: string): uuid
- createRelationshipPropertyKey(rel_id: uuid, key_name: string): uuid
- createRelationshipPropertyValue(key_id: uuid, property_value: any): uuid
- readRelationship(rel_id: uuid): Relationship
- listRelatedNodes(node_id: uuid): Array<{relationship: [Relationship Object], node: [Node Object]}>

### Elections
- createElection():uuid
- addBallotEntry(election_id: uuid, node_id: uuid): uuid
- addVote(ballot_entry_id: uuid, vote: boolean): uuid
- addVote(ballot_entry_id: uuid): boolean
- readElection(election_id: uuid): Election

### Discussion
- createDiscussion(): uuid
- createPost(discussion_id: uuid, content: string): uuid
- updatePost(post_id: uuid): boolean
- deletePost(post_id: uuid): boolean
- readDiscussion(discussion_id: uuid): Discussion

## Graph API Layer 2

Layer 2 only calls layer 1 functions.

### Node/Relationship CREATE Convenience Wrappers

These functions will always create a new node or relationship. The root keys of the object will be used as the unique keys of the new node/relationship. The values of those keys will be the child value/object of the root keys passed in.

- createNodeFromObject(type_name: string, obj: {}): Node
- createRelationshipFromObject(type_name: string, obj: {}, from_node: uuid, to_node: uuid): Relationship
- createRelatedToNodeFromObject(node_uuid: uuid, rel_type_name: string, type_name: string, obj: {}): {relationship: [Relationship Object], node: [Node Object]}
- createRelatedFromNodeFromObject(type_name: string, obj: {}, rel_type_name: string, node_uuid: uuid): {relationship: [Relationship Object], node: [Node Object]}

### Upsert Node/Relationship Convenience Wrappers

These operations use a previously created node/relationship and are idempotent with key creation. They will first search for a key before inserting.

- upsertNodeObject(node_uuid: uuid, obj: {}): Node
- upsertRelationshipObject(rel_uuid: uuid, obj: {}): Relationship

## Graph API Layer 3

Layer 3 only calls layer 2 functions.

### Legend

![Graph Legend](./img/legend.png)

### Table Data

![Table Data](./img/table-data.png)

- createTable(name: string): Table
- addTableData(table_name: string, column_name: string, row_id: string, cell_data: any): Cell
- getTable(name: string): Table

### Key Terms

![Key Terms](./img/key-terms.png)

### Word

![word](./img/word.png)

### Word Sequence

![word-sequence](./img/word-sequence.png)

### Word to Article Link

![word-to-article-link](./img/word-to-article-link.png)

### Verse

![verse](./img/verse.png)

### Chapter

![chapter](./img/chapter.png)

### Book

![book](./img/book.png)

### Bible

![bible](./img/bible.png)

### Sentence

![sentence](./img/sentence.png)

### Article

![article](./img/article.png)

### Paragraph

![paragraph](./img/paragraph.png)

### Section

![section](./img/section.png)

### Section of Sections

![section-of-sections](./img/section-of-sections.png)

### Strong's Entry

![strongs-entry](./img/strongs-entry.png)

### Strong's Word Link

![strongs-word-link](./img/strongs-word-link.png)

### External Content

![external-content](./img/external-content.png)

### Voting

![voting](./img/voting.png)
