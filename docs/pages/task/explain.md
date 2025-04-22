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
- program\
  - output
    - sum_output.md
    - api_output.md
    - how_output.md
  - metadata.txt

Note: Input and Context folder are not required for Procedure or subroutine.

### Input

The input section stores the complete RPGLE source code, including H-specs, file and variable declarations, and full program logic with subroutines and business rules. This section provides all the necessary code that the program uses to perform its operations.

The `program.pgm.rpgle` contains the code to be explained.  

```rpgleforILERPG
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
The context section provides a comprehensive overview of all external files and resources that the RPGLE program relies on. It includes various files and resources that the program interacts with, such as database files, display files, printer files, and copybooks. Additionally, if any physical file is using a field reference file for its record format, that reference file should also be mentioned in this section.

### Components

1. `Physical Files (.pf)`: Store data in a structured format, defined outside the program.
2. `Logical Files (.lf)`: Provide different views or access paths to physical file data.
3. `Display Files (.dspf)`: Define screens for user interaction.
4. `Printer Files (.prtf)`: Define printed report layouts.
5. `Copybooks (.rpgleinc)`: Contain reusable code snippets.
6. `SQL DDL Files (.table, .view)`: Define database tables and views.

This example indicates that the `program.pgm.rpgle` relies on the `accpf.pf `file. If there were additional files like display files or copybooks, they would be included in the context section as well.

`context` has the content for `program.pgm.rpgle`:

```text
 - context
    - accpf.pf 
```

### Output

The Output Section is designed to provide a comprehensive explanation of the RPGLE program's behavior, structure, and logic. It is divided into three main parts: `api_output`, `how_output`, and `sum_output`. Each part serves a specific purpose and includes detailed information to ensure clarity and consistency.

### api_output 
The `api_output` part provides a high-level summary of the program's purpose and behavior, detailing the parameters passed to and from the program, expected inputs and outputs, dependencies, side effects or limitations, and optional usage examples.

1. `Purpose`: Provide a short insight into the purpose of the code for the program or procedure. This should focus on the business logic or functional role of the code.

2. `Inputs`: Provide all input parameters or external inputs used by the program or procedure. Keep it short and make sure to list the field names, if any. This section can be divided into subsections
  - `Entry parameters`: Entry parameters. Mention the length if it is referring to a keyword.
  - `Input files`: List the files that are input for the program or procedure.
  - `Input procedures`: Mention any procedures that are called within the program.

3. `Outputs`: Provide all the return values and output fields such as display fields, host variables, and overall outputs of the procedure execution. Make sure to list the field names, if any. This section can also be divided into subsections
  - `Return values`: Describe the return values from the procedure, including data types and meanings.
  - `Output fields`: List the fields that are output from the program or procedure.
  - `Output files`: List the files that are output for the program or procedure.

4. `Dependencies`: This section outlines all components that the program depends on in order to compile and run successfully. Dependencies include physical and logical files used for data access, copybooks, external procedures for business logic, and display files for user interface interactions.

5. `Side Effects`: Mention any side effects of the code. 
  - Example: This includes any global variables whose values are changed within the procedure.
  - Example: Mention any side effects related to error handling

6. `Limitations`: Note any constraints or limitations of the code.
  - Example: The load all subfile can load only up to 9999 records. 
  - Example: The caller cannot update records it is only input. This means the procedure or program is designed to read data but not modify it.
      
7. `Outcomes`: List possible major outcomes or scenarios resulting from the code execution. This section can include an optional usage example to illustrate the expected outcome.

`api_output.md` has the content for `program.pgm.rpgle`:

```text
### 1. Purpose

The RPGLE program is designed to retrieve the account number (`AccNo`) for a given user ID (`P_UserId`) from the `AccPf` file. It reads through the records in the `AccPf` file and searches for a match with the provided user ID. Once a match is found, it returns the corresponding account number.

### 2. Inputs

#### 2.1 Parameters:

- `P_UserId`
  - Its 10 character variable
  - Represents the user ID provided as input to the program.
  - Used to search for the corresponding account number in the `AccPf` file.

#### 2.2 Files
- The program receives inputs from the following files

  #### Physical File `AccPf`
  - Type: Physical File (PF)
  - Purpose: Contains records with user IDs and account numbers.

### 3. Outputs

#### 3.1 Return Variable:

- `PAccNo`
  - Its 10 character variable
  - Represents the account number retrieved from the `AccPf` file.
  - Returned as the output of the program.

### 4. Dependencies
- `Physical Files` – `AccPf` is a physical file accessed in input mode with keyed access to retrieve account details based on `CustId`.

### 5. Side Effects
- The program does not check whether `P_UserId` is blank. If `P_UserId` is blank, the program will still attempt to find a matching record and may return an unintended account number.

### 6. Limitations
- The program is designed to read data and return the account number but does not provide functionality to update records in the `AccPf` file.

### 7. Outcomes

- Successful Retrieval: 
  If a record exists in the `AccPf` file where `CustId` matches the input `P_UserId`, the program successfully retrieves and returns the corresponding `AccNo` in `PAccNo`.

- Blank or Invalid User ID: 
  If `P_UserId` is blank or does not match any `CustId` in the file:
  - On the first run, `PAccNo` is not assigned and will return as blank.
  - If the same program runs again without resetting `PAccNo`, it's retain the previous value, leading to incorrect or unintended output. 

#### 7.1 Usage Example

- Input:  
  `P_UserId = '1234567890'`
- If a matching record is found:  
  The program returns `PAccNo = '9876543210'` (example).
- If no match is found: 
  - On the first run: `PAccNo` remains blank and is returned as such.  
  - If called again without reinitializing `PAccNo`, it return the previously retained value (e.g., `'9876543210'`). 
```

### how_output
The how_output part explains how the program works internally, covering the full execution flow and logic used at each step. It includes details on control specifications, file declarations, global variables, constants, indicators, field mapping, subroutines, error handling, and the main program logic.

1. `Control Specifications`: Any key compiler options and runtime control settings included in the program. Examples include `DftActGrp`, `ActGrp`, `BNDDIR`, and other relevant control specifications. Explain these settings and any keywords used.

2. `File Declarations`: List all declared files from the F-specs with relevant keywords. This includes display files, physical,logical files, printer files, and special files. Explain how each file is declared and any keywords used.

3. `Prototype & Interface Definition`: Describe internal procedure prototypes and their usage. This includes procedure names, input/output parameters, data types, and return types.

4. `Prototype for External Program Call`: Mention prototypes for external RPG/CL/API calls. This includes program name, parameter interface, data types, and expected results.

5.`Data Structures and Arrays` – Describe all declared data structures and arrays used in the program, including their purpose and how they are utilized.
  - `File Information Data Structure (INFDS)` – Mention any INFDS declared in the program and what file-level metadata they capture (e.g., file status, error codes, record numbers).
  - `Indicator Data Structure (INDDS)` – Explain the usage of indicator-based data structures, if any, especially those mapped to display files or used for control logic.
  - `Arrays` – Describe any arrays used, whether single or multi-dimensional, compile-time or run-time, and their role.
  - `General Data Structures` – Include details of data structures. Mention key attributes or keywords applied (e.g., `inz`, `overlay`, `dim`, `based`, etc.).

6. `Variables`: Explain the variables used in the program. This includes
  - `Global Variables`: List all global variables used in the program and explain their purpose.
  - `Constants`: List constants declared using `DCL-C`, including status flags, limits, messages, and system values.
  - `Keywords`: Explain any keywords used in variable declarations, such as `LIKE`, `INZ`, `N` data type, etc.

7. `Field Mapping`: Database to Display File: Create a clear table or list to show how physical/logical file fields are mapped to display fields. This includes field names and display fields.

8. `Main Execution Flow`: Describe high-level program logic in ordered steps. This includes initialization, opening files, loading initial data, displaying subfile, handling user actions (Add/Edit/Delete/Search), validating and updating database, printing report (if required), and exiting the program.

9. `Error Handling`: Mention all error-handling techniques used. This includes message subfile, custom messages from MSGF, file status checks, return codes from APIs/procedures, and logging errors to a file or job log.

10. `Possible Problems with this code`: Identify potential issues that could arise during the execution of the program or procedure. Mention any problems that should be noted when discussing the final statement that ends the procedure.

11. `Possible improvements to this code`: Identify areas where the code could be enhanced for better performance, readability, or functionality. This may include suggestions for error handling, optimization techniques, or alternative approaches to achieve the same result.
 
`how_output.md` has the content for `program.pgm.rpgle`:

```text
### 1. High-Level Purpose of the Program
- The RPGLE program is designed to retrieve the account number (`AccNo`) for a given user ID (`P_UserId`) from the `AccPf` file. It reads through the records in the `AccPf` file and searches for a match with the provided user ID. Once a match is found, it returns the corresponding account number.

### 2. Control Specifications

Control specifications in RPGLE are used to define the overall behavior and environment settings for the program. 
- `ctl-opt dftactgrp(*no) actgrp(*new);`
  - `dftactgrp(*no)`: Indicates that the program should not run in the default activation group.
  - `actgrp(*new)`: Specifies that the program should run in a new activation group.

### 3. File Specifications

These file specifications define how each file will be accessed and used within the program, ensuring that the necessary operations (input, update, add) can be performed on the files as required.

- `FAccPf IF E K Disk`
  - File Type: Physical file `AccPf` declared.
  - Attributes:
    - `I`: Input file, meaning the file can be read.
    - `F` : It defines a fully procedural file, meaning all I/O operations are manually controlled in the program.
    - `E`: Externally described file, meaning the file's structure is defined outside the program.
    - `K`: Keyed access, meaning the file is accessed using a key field.
    - `Disk`: Indicates that this is a disk file used for storing data on disk.

### 4. Stand Alone Variables

Stand alone variables in RPGLE are used to store data that is not directly associated with a file or data structure. These variables can be used for various purposes, such as holding intermediate values, parameters, and flags.

- `D P_UserId S 10A`
  - `P_UserId`: A character variable with a length of 10.
  - Attributes:
    - `S`: Indicates that this is a stand-alone variable.
    - `10A`: Specifies that the variable is alphanumeric with a length of 10 characters.
  - Purpose: It is used to store the user ID provided as input to the program.

- `D PAccNo S 10A`
  - `PAccNo`: A character variable with a length of 10.
  - Attributes:
    - `S`: Indicates that this is a stand-alone variable.
    - `10A`: Specifies that the variable is alphanumeric with a length of 10 characters.
  - Purpose: It is used to store the account number retrieved from the `AccPf` file.

### 5. Procedure Interface

The procedure interface in RPGLE defines the parameters that are passed to a procedure when it is called.

- `dcl-pi *n`
  - `*n`: Indicates that the procedure has no name (anonymous procedure).
  - `PI`: Stands for Procedure Interface, indicating that this block defines the parameters for the procedure.

- `P_UserId char(10) const`
  - `P_UserId`: The name of the parameter, which represents the user ID.
  - `char(10)`: Specifies that the parameter `P_UserId` is a character variable with a length of 10.
  - `const`: Indicates that the parameter is a constant, meaning its value cannot be changed within the procedure.

### 6. Main Line Code

- `setll *loval AccPf;`: Sets the lower limit for the file `AccPf` to the lowest possible value.
- `read AccPf;`: Reads the next record from the file `AccPf`.
- `dow not %eof(AccPf);`: Starts a loop that continues until the end of the file `AccPf` is reached.
- `if CustId = P_UserId;`: Checks if the `CustId` field in the current record matches the input parameter `P_UserId`.
- `PAccNo = AccNo;`: Assigns the `AccNo` field from the current record to the variable `PAccNo`.
- `leave;`: Exits the loop if the condition is met.
- `read AccPf;`: Reads the next record from the file `AccPf`.
- `enddo;`: Ends the loop.
- `return PAccNo;`: Returns the value of `PAccNo` as the result of the procedure.

### 7. Possible Problems with this Code
- If the same program runs again without resetting `PAccNo`, it retain the previous value, leading to incorrect or unintended output. This can happen in interactive or looped calls if the variable is not reinitialized.
- The program does not check whether `P_UserId` is blank. If `P_UserId` is blank, the program will still attempt to find a matching record and may return an unintended account number.
- The program reads through the entire `AccPf` file until it finds a match or reaches the end of the file. This can be inefficient for large files.

### 8. Possible Improvements to this Code

- Ensure `PAccNo` is initialized to an empty value at the start of the procedure to avoid retaining previous values. This prevents unintended outputs in subsequent runs of the program.
- Add validation to check if `P_UserId` is blank before processing the file. This ensures that the program does not attempt to find a match for an empty user ID, which could lead to unintended results.
- Implement error handling to manage cases where no matching record is found. This could involve returning a specific value or message indicating that no match was found, improving the program's robustness and user feedback.
- Use a keyed read operation instead of reading through the entire file to improve performance. This approach directly accesses the record with the matching key, reducing the time complexity from linear to constant time for the lookup. 
``` 

### sum_output
The sum_output part offers a business summary in a couple of sentences, explaining the overall purpose of the program

`sum_output.md` has the content for `program.pgm.rpgle`:

```text
### Summary 
The program is designed to retrieve the account number (`PAccNo`) associated with a given user ID (`P_UserId`) from a file (`AccPf`). It iterates through the records in the file, checking each record to find a match for the user ID. Once a match is found, the corresponding account number is returned
```

### Notes

- Use `rpgleforILERPG` code blocks and `rpgforOPMRPG` code blocks.
- Use `###` for first-level headings and `####` for sub-level headings.
- Avoid using `**` for bold text use backticks ` ` to highlight keywords or important terms.
- Use the term `fully-free` instead of `full free format`.
- Use `ILE RPG` instead of `RPGLE`.
- Do not expand `RPG` as `Report Program Generator`. Always refer to it as `RPG`. 

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
difficulty: 2
language: rpg4ff
scope: program-cycle
use: eval

``` 

Full description of all the metadata variants can be found [here](/pages/metadata.md).

The main file used to do the training is `train_rpgle_to_text.jsonl` or it your example might be chosen to evaluate the model `eval_rpgle_to_text.jsonl`.  This whole directory will be summarized in single line similar to the following.

```json
{"id":"program_sum","code":"source from program.pgm.rpgle","context":"","explanation":"from sum_output.md","metadata": {"provenance":"https://github.com/AIforIBMi/rpg-genai-data/tree/main/data/explain/IBM/program/sum_output.md","difficulty":0,"language":"rpg4ff","scope":"file","depth":"sum"}}
{"id":"program_api","code":"source from program.pgm.rpgle","context":"","explanation":"from sum_output.md","metadata": {"provenance":"https://github.com/AIforIBMi/rpg-genai-data/tree/main/data/explain/IBM/program/api_output.md","difficulty":0,"language":"rpg4ff","scope":"file","depth":"api"}}
{"id":"program_how","code":"source from program.pgm..rpgle","context":"","explanation":"from sum_output.md","metadata": {"provenance":"https://github.com/AIforIBMi/rpg-genai-data/tree/main/data/explain/IBM/program/how_output.md","difficulty":0,"language":"rpg4ff","scope":"file","depth":"how"}}
```