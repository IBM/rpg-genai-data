# Explanation format for an RPG program

The following explanation applies to both modern linear-main programs as well as legacy cycle-main programs.
The [`scope`](/pages/metadata#scope) in `metadata.txt` will be `program-linear` or `program-cycle` in this case.

## api_output

The `api_output` part provides information to the caller of a program about how to call the program and what the effects of calling it would be. It includes
a high-level summary of the program's purpose and behavior, detailing the parameters passed to and from the program, dependencies, side effects or limitations, and optional usage examples.

1. `Purpose`: Provide a short insight into the purpose of the code for the program. This should focus on the business logic or functional role of the code.

2. `Parameters`: List all entry parameters accepted by the program, specifying whether each one is an input, output, or both.

3. `Inputs/Outputs` : This section describes how the program reads from and writes to both data and device files. It also explains how the program interacts with display files for user input and output, and outlines the function keys and indicators used to manage screen behavior and control program flow.

4. `Side Effects`: Mention any indirect or non-obvious effects such as data area updates, logs, or changes to global state. 
  - Example: Programs might update data areas, which can affect other programs or procedures that rely on the same data areas

5. `Limitations & Assumptions`: Mention any assumptions the code makes, or scenarios where it may fail or behave incorrectly.
  - Example: The load all subfile can load only up to 9999 records.

6. `Usage Example`: Define the expected outcomes of the program execution.

### how_output

The how_output part explains how the program works internally, covering the full execution flow and logic used at each step. It includes details on control specifications, file declarations, global variables, constants, indicators, field mapping, subroutines, error handling, and the main program logic.

1. `Control Specifications`: Any key compiler options and runtime control settings included in the program. Examples include `DftActGrp`, `ActGrp`, `BNDDIR`, and other relevant control specifications. Explain these settings and any keywords used.

2. `File Declarations`: List all global files from the F-specs with relevant keywords. This includes display files, physical,logical files, printer files, and special files. Explain how each file is declared and any keywords used.

3. `Global Components`: The Global Variables section includes all variables, data structures, arrays, constants, and keywords used in the variable declaration (example`LIKE`, `INZ`, `N` data type).

4. `Field Mapping`: Database to Display File or Printer file: Create a clear table or list to show how physical/logical file fields are mapped to display fields or Printer file fields. This includes field names and display fields.

5. `Indicators` : Create a clear table or list to show how error indicators are mapped. This includes indicator names, descriptions and Purpose.

6. `Main Execution Flow`: Describe high-level program logic in ordered steps. This includes initialization, opening files, loading initial data, displaying subfile, handling user actions (Add/Edit/Delete/Search), validating and updating database, printing report (if required), and exiting the program.

7. `Possible Problems with this code`: Identify potential issues that could arise during the execution of the program. Mention any problems that should be noted when discussing the final statement that ends the program.

8. `Possible improvements to this code`: Identify areas where the code could be enhanced for better performance, readability, or functionality. This may include suggestions for error handling, optimization techniques, or alternative approaches to achieve the same result.

### sum_output

The sum_output part offers a business summary in a couple of sentences, explaining the overall purpose of the program.

## metadata.txt

The `metadata.txt` file describes important attributes of the training data.  Its structure looks like:

```yaml
difficulty: <rating>
language: <language>
scope: <scope>
use: <usage>
```

Full description of all the metadata variants can be found [here](/pages/metadata.md).

The sample explanation program can be found [here](/pages/task/sample_program.md).