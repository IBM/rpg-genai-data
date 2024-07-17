# Ask questions about RPG

We hope to have knowledge from RPG manuals and books available by asking natural language questions from the AI.
To do this we will need to train with question and answer pairs about RPG and CL programming.

## Format

* chat
  * {unique contributor id}
    * attribution.txt
    * {unique test id}
      * input.txt - Ask any question about RPG
      * output.txt - Provide good answer to the question
      * metadata.md - difficulty

The `input.txt` will have the question you want answered.
The `output.txt` will have the expected answer to this question.
Finally the `metadata.md` documents how difficult you think this question is on scale from 1 to 5.

