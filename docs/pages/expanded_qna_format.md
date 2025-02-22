# Training Data Format

The format used to train the AI is JSONL.  This is the file format where each line is a separate JSON with the format:

```json
{"id":"CityFurnJulianDatePgm_how","input_data":"source from i1_julian.pgm.rpgle","context":"","output":"from how_output.md","task":"explain","metadata": {"provenance":"https://github.com/AIforIBMi/rpg-genai-data/blob/4bf9140019e237/data/explain/IBM/helloworld/how_output.txt","difficulty":1,"language":"rpg4fx","scope":"file","depth":"how"}}

```

Where

- **ID** =
unique alphanumeric string with no blanks
- **INPUT_DATA** =
the raw data of the question being posed to the LLM
- **CONTEXT** =
files and definitions that the question is dependent on
- **OUTPUT** =
the response that the LLM *should* provide in markdown format
- **TASK_TYPE** =
What type of task is being attempted: explain/prototype/unittest/fixed-to-free
- **METADATA** =
Additional metadata that document the difficulty of this question and relevant attributes that will determin the right prompt

These JSONL files will be found in the [AIforIBMi repo](https://github.com/AIforIBMi/rpg-genai-data) under
```/data/<task_name>/<your_directory>```.
Please add to this directory a file [`attribution.txt`](attribution.md) with content of the form:

```yaml
title : ILE RPG Compiler Test Cases
link : https://github.com/edmundreinhardt/rpg-genai-data/tree/main/src/001compiler_tests/
contact : Barbara Morris
email : bmorris@ca.ibm.com
license : Apache 2.0
organization : IBM
version : https://github.com/edmundreinhardt/rpg-genai-data/commit/d41c5d45a58653d7d12958be6c2b739cb5d7e902
```

Since RPG source in `jsonl` will be difficult to read as it all has to be on one line, we have an easier format to submit your training data.  Use the following data structure:

- data/
  - *task_type*/
    - *your_org*/
      - any_id1
        - i1_file1.rpgle
        - c1_MYDSPF.DSPF
        - c2_MYTABLE.TABLE
        - output.md
        - o1_somefile.rpgle
        - metadata.txt

The above directory structure will automatically be transformed by IBM into a `jsonl` line of the form:

```json
{"id": "any_id1", "input_data":”input.txt contents\n\nfile1.rpgle\n i1_file1.rpgle contents”, "context”: "MYDSPF.DSPF\n\n c1_MYDSPF.DSPF contents\n\nMYTABLE.TABLE\n i3_MYTABLE.TABLE contents", ”output”: ”output.txt contents\n\nsomefile.rpgle\no1_somefile.rpgle contents”, "task":"task_name", "difficulty":0}
```

Note that the `i<number>_` prefix is used to impose an order in which the input files show up in the question.  The `c<number>_` prefix similarly orders the files that are needed to give the context for the question. Finally the `o<number>_` prefix similarly orders the output files that are embedded in the answer.  The `output.md` could also be `output.rpgle` if the output only contains code.
The `metadata.txt` contains important attributes as described [here](/pages/metadata.md).

```yaml
 difficulty: 0
 language: rpg4ff
 scope: file
 use: train
```
