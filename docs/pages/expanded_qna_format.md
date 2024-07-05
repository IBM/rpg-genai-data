# Training Data Format

The format used to train the AI is JSONL.  This is the file format where each line is a separate JSON with the format:

```json
{"id": "<UNIQUE_ID>", "input":”<QUESTION>”, "context":"<CONTEXT>", ”output”:”<ANSWER>”, "task":"<TASK>", "difficulty" : "<DIFFICULTY>"}
```

Where

- **UNIQUE_ID** =
unique alphanumeric string with no blanks
- **QUESTION** =
the request being made to the LLM
- **CONTEXT** = 
files and definitions that the question is dependent on
- **ANSWER** =
the response that the LLM *should* provide
- **TASK_TYPE** =
What type of task is being attempted: explain/prototype/unittest/fixed-to-free
- **DIFFICULTY** =
Difficulty of question on a scale from 0 to 5 where 5 is the most difficult and 0 is trival

These JSONL files will be found in the [AIforIBMi repo](https://github.com/AIforIBMi/rpg-genai-data) under
```/data/<task_name>/<your_directory>```.
Please add to this directory a file `attribution.txt` with content of the form:

```yaml
title : ILE RPG Compiler Test Cases
link : https://github.com/edmundreinhardt/rpg-genai-data/tree/main/src/001compiler_tests/
creator : Barbara Morris
license : Apache 2.0
organization : IBM
version : https://github.com/edmundreinhardt/rpg-genai-data/commit/d41c5d45a58653d7d12958be6c2b739cb5d7e902
```

Since RPG source in `jsonl` will be difficult to read as it all has to be on one line, we have an easier format to submit your training data.  Use the following data structure:

- data/
  - <task_name>/
    - <your_org>/
      - any_id1
        - input.txt
        - i1_file1.rpgle
        - c1_MYDSPF.DSPF
        - c2_MYTABLE.TABLE
        - output.txt
        - o1_somefile.rpgle
        - metadata.txt

The above directory structure will automatically be transformed by IBM into a `jsonl` line of the form:

```json
{"id": "any_id1", "input":”<input.txt contents>\n\nfile1.rpgle\n<i1_file1.rpgle contents>”, "context”: "MYDSPF.DSPF<c1_MYDSPF.DSPF contents>\n\nMYTABLE.TABLE\n<i3_MYTABLE.TABLE contents>", ”output”: ”<output.txt contents>\n\nsomefile.rpgle\no1_somefile.rpgle contents”, "task":"<task_name>", "difficulty":0}
```

Note that the `i<number>_` prefix is used to impose an order in which the input files show up in the question.  The `c<number>_` prefix similarly orders the files that are needed to give the context for the question. Finally the `o<number>_` prefix similarly orders the output files that are embedded in the answer.  The `output.txt` could also be `output.rpgle` if the output only contains code.  The `metadata.txt` in this case contains the difficulty level.

```yaml
 difficulty: 0
```