# Explain compilable RPG source file

When a complete source file is being explained then everything that would be required to compile this source file into the corresponding module or program should be stored in the corresponding directory in the following format.

- sourcename\
  - context
    - physical.pf
    - logical.lf
    - displyfile.dspf
    - copybook.rpgleinc
    - sqlobject.table
    - sqlobject.view
    - sqlobject.index
  - input
    - source.rpgle
  - output
    - sum_output.md
    - api_output.md
    - how_output.md
  - metadata.txt

As a matter of fact, it is a good idea to test that the source compiles using something like [BOB](https://ibm.github.io/ibmi-bob/#/).

## Input

The input section stores the complete RPGLE source file as it appears in the final source member or stream file that gets compiled.

## Context

The context section provides a comprehensive overview of all external files and resources that the RPGLE program relies on. It includes various files and resources that the program interacts with, such as database files, display files, printer files, and copybooks. Additionally, if any physical file is using a field reference file for its record format, that reference file should also be mentioned in this section.  A good test to see if the context is complete is if you can compile this source file using only what is in this directory.

### Components

1. `Physical Files (.pf)`: Store data in a structured format, defined outside the program.
2. `Logical Files (.lf)`: Provide different views or access paths to physical file data.
3. `Display Files (.dspf)`: Define screens for user interaction.
4. `Printer Files (.prtf)`: Define printed report layouts.
5. `Copybooks (.rpgleinc)`: Contain reusable code snippets.
6. `SQL DDL Files (.table, .view, .index)`: Define database tables, views and indices.

## Output

The Output directory contains the explanations that the LLM is expected to generate.  Three different depths of explanation are supported.

- `sum` contains a high level non-technical summary of the purpose without reference to implementation details.
- `api` provides information to the caller of the RPG code about how to call it
and what the effects of calling it would be.
- `how` provides implementation details of exactly how the RPG code accomplishes this purpose

All three depths of explanation are stored in markdown format in the `output` directory as `sum_ouput.md`, `api_output.md` and `how_output.md` respectively.

An RPG source file can be compiled into a module, program or be included into another source file.  The explanation format will vary depending on which type of source it is. For details of how the explanation should be structured see the respective links for:

- [program](/pages/task/explain_program.md)
- [module](/pages/task/explain_module.md)
- [copybook](/pages/task/explain_copybook.md)

 is designed to provide a comprehensive explanation of the RPGLE program's behavior, structure, and logic. It is divided into three main parts: `api_output`, `how_output`, and `sum_output`. Each part serves a specific purpose and includes detailed information to ensure clarity and consistency.  The `sum` depth of explana

## metadata.txt

The `metadata.txt` file describes important attributes of the training data.  Its structure looks like:

```yaml
difficulty: <rating>
language: <language>
scope: <scope>
use: <usage>
```

Full description of all the metadata variants can be found [here](/pages/metadata.md).


## jsonl training data

The entire contents of this directory will be summarized into a single line in a `jsonl` file in a parent directory in a format that the LLM can use.
In this example the data is `use: eval` so it will  be chosen to evaluate the model and it will be put into `eval_rpgle_to_text.jsonl`.  A separate line will be generated for each explanation in the output directory.

```json
{"id":"getaccno_sum","code":"source from getaccno.pgm.rpgle","context":"all referenced source from context dir","explanation":"from sum_output.md","metadata": {"provenance":"https://github.com/AIforIBMi/rpg-genai-data/tree/main/data/explain/IBM/program/sum_output.md","difficulty":0,"language":"rpg4ff","scope":"file","depth":"sum"}}
{"id":"getaccno_api","code":"source from getaccno.pgm.rpgle","context":"all referenced source from context dir","explanation":"from api_output.md","metadata": {"provenance":"https://github.com/AIforIBMi/rpg-genai-data/tree/main/data/explain/IBM/program/api_output.md","difficulty":0,"language":"rpg4ff","scope":"file","depth":"api"}}
{"id":"getaccno_how","code":"source from getaccno.pgm..rpgle","context":"all referenced source from context dir","explanation":"from how_output.md","metadata": {"provenance":"https://github.com/AIforIBMi/rpg-genai-data/tree/main/data/explain/IBM/program/how_output.md","difficulty":0,"language":"rpg4ff","scope":"file","depth":"how"}}
```
