# Explain existing RPG in Natural Language

A common problem for IBM i customers is coming across old code that is not easy to understand. The goal of this task is to train the AI to be able to produce a natural language description of some RPG.
In this scenario the question is in format of RPG code and the output is an explanation of that code.

Since the task is to "explain" some RPG, all of the following data is found in the `data/explain` directory.
For this example we will have a training pair submitted by IBM, so the data will be in the `data/explain/IBM` directory.

The simple directory format for groud truth structure would look like 

Complete compilable ILE RPG source file for programs or modules or copybooks

- program\
  - context
    - physical.pf
    - logical.lf
    - displyfile.dspf
    - copybook.rpgleinc
    - <SQL DDL>.table
    - <SQL DDL>.view
  - input
    - program.pgm.rpgle
  - output
    - sum_output.md
    - api_output.md
    - how_output.md
  - metadata.txt

Stracture for Procedure or subroutine will be 

  - output
    - sum_output.md
    - api_output.md
    - how_output.md
  - metadata.txt

Note: Input and Context folder are not required for Procedure or subroutine.

### Input

The input section stores the complete RPGLE source code, including H-specs, file and variable declarations, and full program logic with subroutines and business rules. This section provides all the necessary code that the program uses to perform its operations.

The `program.pgm.rpgle` contains the code to be explained.  

```
  **free
  ctl-opt dftactgrp(*no) actgrp(*new);

  dcl-f AccPf if e k disk;

  dcl-s P_UserId char(10);
  dcl-s PAccNo char(10);

  dcl-pi *n;
    P_UserId char(10) const;
  end-pi;

  setll *loval AccPf;
  read AccPf;
  dow not %eof(AccPf);
    if CustId = P_UserId;
      PAccNo = AccNo;
      leave;
    endif;
    read AccPf;
  enddo;

  return PAccNo;
```

### Context 

The context section provides a comprehensive overview of all external files and resources that the RPGLE program relies on. It includes various files and resources that the program interacts with, such as database files, display files, printer files, and copybooks.

### Components

1. `Physical Files (.pf)`: Store data in a structured format, defined outside the program.
2. `Logical Files (.lf)`: Provide different views or access paths to physical file data.
3. `Display Files (.dspf)`: Define screens for user interaction.
4. `Printer Files (.prtf)`: Define printed report layouts.
5. `Copybooks (.rpgleinc)`: Contain reusable code snippets.
6. `SQL DDL Files (.table, .view)`: Define database tables and views.

This example indicates that the program.pgm.rpgle relies on the accpf.pf file. If there were additional files like display files or copybooks, they would be included in the context section as well.

`context` has the content for `program.pgm.rpgle`:

```text
 - context
    - accpf.pf 
```

### Output

The Output Section is designed to provide a comprehensive explanation of the RPGLE program's behavior, structure, and logic. It is divided into three main parts: api_output, how_output, and sum_output. Each part serves a specific purpose and includes detailed information to ensure clarity and consistency

### api_output 
The api_output part provides a high-level summary of the program's purpose and behavior, detailing the parameters passed to and from the program, expected inputs and outputs, dependencies, side effects or limitations, and optional usage examples.

1. `Purpose`: Provide a short insight into the purpose of the code for the program or procedure. This should focus on the business logic or functional role of the code.

2. `Inputs`: Provide all input parameters or external inputs used by the program or procedure. Keep it short and make sure to list the field names, if any. This section can be divided into subsections:
  - `parameters`: Entry parameters. Mention the length if it is referring to a keyword.
  - `Input files`: List the files that are input for the program or procedure.
  - `Input procedures`: Mention any procedures that are called within the program.

3. `Outputs`: Provide all the return values and output fields such as display fields, host variables, and overall outputs of the procedure execution. Make sure to list the field names, if any. This section can also be divided into subsections:
  - `Output fields`: List the fields that are output from the program or procedure.
  - `Return values`: Describe the return values from the procedure, including data types and meanings.

4. `Side Effects`: Mention any side effects of the code. 
  - Example: This includes any global variables whose values are changed within the procedure.
  - Example: Mention any side effects related to error handling

5. `Limitations`: Note any constraints or limitations of the code.
  - Example: The load all subfile can load only up to 9999 records. 
  - Example: The caller cannot update records it is only input. This means the procedure or program is designed to read data but not modify it.
      
6. Outcomes: List possible major outcomes or scenarios resulting from the code execution. This section can include an optional usage example to illustrate the expected outcome.


`api_output.md` has the content for `program.pgm.rpgle`:

```text
The fully free-form ILE RPG program displays the message 'hello world' to the 5250 workstation that invoked the interactive program. 
```

### how_output
The how_output part explains how the program works internally, covering the full execution flow and logic used at each step. It includes details on control specifications, file declarations, global variables, constants, indicators, field mapping, subroutines, error handling, and the main program logic.

1. `Control Specifications`: Any key compiler options and runtime control settings included in the program. Examples include `DftActGrp`, `ActGrp`, `BNDDIR`, and other relevant control specifications. Explain these settings and any keywords used.

2. `File Declarations`: List all declared files from the F-specs with relevant keywords. This includes display files, physical/logical files, printer files, and special files. Explain how each file is declared and any keywords used.

3. `Prototype & Interface Definition`: Describe internal procedure prototypes and their usage. This includes procedure names, input/output parameters, data types, and return types.

4. `Prototype for External Program Call`: Mention prototypes for external RPG/CL/API calls. This includes program name, parameter interface, data types, and expected results.

5. `Data Structures`: Describe the data structures used in the program. This includes:
  - `File Information Data Structure (INFDS)`: List any INFDS used and what file-level information they capture, such as file status, error codes, and relative record number.
  - `Indicator Data Structure (INDDS)`: Describe indicators used via DS, including display control, conditional formatting, and record formats.
  - `Arrays and Data Structures`: Describe declared arrays and data structures, including single/multi-dimension arrays, compile-time or run-time arrays, and data structures for record buffers, subfile row handling, and screen mapping. Explain any keywords used.

6. `Variables`: Explain the variables used in the program. This includes:
  - `Global Variables`: List all global variables used in the program and explain their purpose.
  - `Constants`: List constants declared using `DCL-C`, including status flags, limits, messages, and system values.
  - `Keywords`: Explain any keywords used in variable declarations, such as `LIKE`, `INZ`, `N` data type, etc.

7. `Field Mapping`: Database to Display File: Create a clear table or list to show how physical/logical file fields are mapped to display fields. This includes field names and display fields.

8. `Main Execution Flow`: Describe high-level program logic in ordered steps. This includes initialization, opening files, loading initial data, displaying subfile, handling user actions (Add/Edit/Delete/Search), validating and updating database, printing report (if required), and exiting the program.

9. `Error Handling`: Mention all error-handling techniques used. This includes message subfile, custom messages from MSGF, file status checks, return codes from APIs/procedures, and logging errors to a file or job log.

10. `Possible improvements to this code`: Identify areas where the code could be enhanced for better performance, readability, or functionality. This may include suggestions for error handling, optimization techniques, or alternative approaches to achieve the same result.
  - Example : Any proceduure returns normally even if it does not find the required record. Perhaps it could signal an error (SND-MSG *ESCAPE) in that case.Consider using SQL instead of native I/O for the interaction with the XYZ file.

11. `Possible Problems with this code`: Identify potential issues that could arise during the execution of the program or procedure. Mention any problems that should be noted when discussing the final statement that ends the procedure.

`how_output.md` has the content for `program.pgm.rpgle`:

```text
The fully free-form ILE RPG program displays the message 'hello world' to the 5250 workstation that invoked the interactive program. 
``` 

### sum_output
The sum_output part offers a business summary in a couple of sentences, explaining the overall purpose of the program

`sum_output.md` has the content for `program.pgm.rpgle`:

```text
The fully free-form ILE RPG program displays the message 'hello world'.
```

### metadata.txt

Complete compilable ILE RPG source file for programs or modules or copybooks

```yaml
difficulty: <rating>
language: <language>
scope: <scope>
use: <usage>
```

Structure for Procedure or Subroutine

```yaml
source: <source_member_name>
start: <start_line_number>
end: <end_line_number>
difficulty: <rating>
language: <language>
scope: <scope>
use: <usage>
```
- `difficulty`: The difficulty of the explanation as rated up to 5. This helps to understand the complexity of the code being documented. For example, a trivial explanation might be rated as 0.
- `language`: The language of the snippet of code being explained. For example, `rpg4ff` indicates RPG IV fully free format. If the source file contains both fixed format and free format RPGLE, it should be marked as `rpg4fc`.
- `scope`: The scope of the language being explained. Use more specific scopes according to how the compiler views the source:
- `module`: A source file with the NOMAIN keyword is a module and should only talk about the exported procedures. The top-level summary should just summarize what the procedures do in general (unless there's only one).
- `program-linear`: A source file with the MAIN keyword should only talk about the main procedure. For the "usage", it could talk about how to call it within RPG code (using the prototype) and also about how to call it from a CL program or from the command line if it's simple enough.
- `program-cycle`: A source file with only a cycle-main procedure (no MAIN or NOMAIN keyword) should assume it will be a program-entry-procedure and talk about it as though it is creating a program. For the usage, show how to call it from other RPG modules using a prototyped call and also how to call it from the command line. If there are other exported procedures, describe them as well in the api.
- `copybook`: Some files are only intended to be used as copy files and would need a completely different style of explanation. The file only contains prototypes and definitions or subroutines. This could be handled similar to a NOMAIN module.
- `use`: Indicates whether the data is used for training (`train`) or evaluation (`eval`). This helps to understand the purpose of the data in the context of training or evaluating the LLM.

`metadata.txt` has the content fro `program.pgm.rpgle`:

```text
The fully free-form ILE RPG program displays the message 'hello world' to the 5250 workstation that invoked the interactive program. 
``` 

Full description of all the metadata variants can be found [here](/pages/metadata.md).

The main file used to do the training is `train_rpgle_to_text.jsonl` or it your example might be chosen to evaluate the model `eval_rpgle_to_text.jsonl`.  This whole directory will be summarized in single line similar to the following.

```json
{"id":"program_sum","code":"source from program.pgm.rpgle","context":"","explanation":"from sum_output.md","metadata": {"provenance":"https://github.com/AIforIBMi/rpg-genai-data/tree/main/data/explain/IBM/program/sum_output.md","difficulty":0,"language":"rpg4ff","scope":"file","depth":"sum"}}
{"id":"program_api","code":"source from program.pgm.rpgle","context":"","explanation":"from sum_output.md","metadata": {"provenance":"https://github.com/AIforIBMi/rpg-genai-data/tree/main/data/explain/IBM/program/api_output.md","difficulty":0,"language":"rpg4ff","scope":"file","depth":"api"}}
{"id":"program_how","code":"source from program.pgm..rpgle","context":"","explanation":"from sum_output.md","metadata": {"provenance":"https://github.com/AIforIBMi/rpg-genai-data/tree/main/data/explain/IBM/program/how_output.md","difficulty":0,"language":"rpg4ff","scope":"file","depth":"how"}}
```
