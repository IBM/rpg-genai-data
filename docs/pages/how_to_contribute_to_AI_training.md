# How to contribute to training the new RPG Generative AI

## Training Data

It is extremely useful to contribute question and answer pairs.  This can be used both to fine tune the AI and to evaluate if it is doing a good job.

This is done by creating questions and answers in [this format](pages/expanded_qna_format.md)

This data is then contributed to the IBM repository by following [these instructions](pull_request.md)

## Use cases and examples

The scenarios we are initially focused on are

1. [Explaining RPG in natural language](pages/task/explain.md)
2. [Prototyping new RPG code from a natural language description](pages/task/prototype.md)
3. [Generating RPG unit tests](pages/task/unittest.md)
4. [Asking questions about RPG in natural language](pages/task/chat.md)
5. [Converting Fixed form RPG to free format](pages/task/fixed_to_free.md)
The latter is not something we expect to use the AI for as there are excellent deterministic tools to do so.  But we feel that this is a very effective way to train the AI.
6. [Modernize old RPG to new RPG](pages/task/modernize.md)
In addition we are very interested in modernizing RPG.  While this is not going to be our first deliverable, we are eager to start collecting data to train the AI on this high value transformation.
