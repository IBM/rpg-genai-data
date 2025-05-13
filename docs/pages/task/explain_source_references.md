# Explain subset of RPG source file

To simulate selection procedure or subroutine from a source file, the following syntax is used to avoid duplicating the entire program source and context.
This corresponds to the metatdata.txt [`scope`](/pages/metadata.txt###scope) of  `proc` or `subr`.

The directory structure for procedure or subroutine look like:

- procedure\
  - output
    - sum_output.md
    - api_output.md
    - how_output.md
  - metadata.txt

Note: Input and Context folder are not required for procedure or subroutine.  This is because the `source`, `start` and `end` attributes will define exactly which source and context is referenced.

## Output

The Output Section is designed to provide a comprehensive explanation of the RPGLE procedure oe subroutine behavior, structure, and logic. It is divided into three main parts: `api_output`, `how_output`, and `sum_output`. Each part serves a specific purpose and includes detailed information to ensure clarity and consistency.

## api_output

The `api_output` part provides a high-level summary of the procedure or subroutine purpose and behavior, detailing the parameters passed to and from the procedure, expected inputs and outputs, dependencies, side effects or limitations, and optional usage examples.

1. `Purpose`: Provide a short insight into the purpose of the code for the procedure or subroutine. This should focus on the business logic or functional role of the code.

2. `Parameters`: List all entry parameters accepted by the code, specifying whether each one is an input.

3. `Return Values` : List the return value accepted by the procedure, specifying whether it is an output

4. `Dependencies`: This section outlines all components that the procedure or subroutine depends on in order to run successfully. Dependencies include physical and logical files used for data access, external procedures for business logic, and display files for user interface interactions.

5. `Side Effects`: Mention any indirect or non-obvious effects such as data area updates, logs, or changes to global state. 
  - Example: This includes any global variables whose values are changed within the procedure.

6. `Limitations & Assumptions`: Mention any assumptions the code makes, or scenarios where it may fail or behave incorrectly.
  - Example: The caller cannot update records it is only input. This means the procedure or subroutine is designed to read data but not modify it.
      
7. `Outcomes`: List possible major outcomes or scenarios resulting from the code execution. This section can include an optional usage example to illustrate the expected outcome.

## how_output
The `how_output` section explains how the specific procedure or subroutine works internally, covering the full execution flow and logic used at each step. It includes details on variables, constants, indicators, field mappings, subroutine calls, and error handling related to that unit.

1. `Global Components`: The Global Variables section includes all variables, prototypes for external program calls, data structures, arrays, constants, and keywords used in the module. Detailed explanations will be provided later in the logic sections
  - `Prototype & Interface Definition`: Describe internal procedure prototypes and their usage. This includes procedure names, input/output parameters, data types, and return types.
  - `Prototype for External Program Call`: Mention prototypes for external RPG/CL/API calls. This includes program name, parameter interface, data types, and expected results.
  - `Data Structures and Arrays` – Describe all declared data structures and arrays used in the program, including their purpose and how they are utilized.
    - `File Information Data Structure (INFDS)` – Mention any INFDS declared in the program and what file-level metadata they capture (e.g., file status, error codes, record numbers).
    - `Indicator Data Structure (INDDS)` – Explain the usage of indicator-based data structures, if any, especially those mapped to display files or used for control logic.
    - `Arrays` – Describe any arrays used, whether single or multi-dimensional, compile-time or run-time, and their role.
    - `General Data Structures` – Include details of data structures. Mention key attributes or keywords applied (e.g., `inz`, `overlay`, `dim`, `based`, etc.).
  - `Variables`: Explain the variables used in the program. This includes
    - `Global Variables`: List all global variables used in the program and explain their purpose.
    - `Constants`: List constants declared using `DCL-C`, including status flags, limits, messages, and system values.
    - `Keywords`: Explain any keywords used in variable declarations, such as `LIKE`, `INZ`, `N` data type, etc

2. `Field Mapping`: If the procedure or subroutine relates to any display file or printer file, create a clear table or list to show how physical/logical file fields are mapped to display fields or printer file. This includes field names and display fields.

3. `Indicators`: If the procedure or subroutine uses indicators, explain their purpose and how they are set or cleared during execution in table format.

4. `Main Execution Flow`: Describe high-level procedure or subroutine logic in ordered steps. This includes initialization, opening files, loading initial data, validating and updating the database, error handling, and exiting the procedure.
    `Subroutine`: If the procedure contains subroutines, list and explain each subroutine and its purpose. 

6. `Possible Problems with this Code`: Identify potential issues that could arise during the execution of the procedure or subroutine. Mention any problems that should be noted when discussing the final statement that ends the procedure or subroutine.

7. `Possible Improvements to this Code`: Identify areas where the code could be enhanced for better performance, readability, or functionality. This may include suggestions for error handling, optimization techniques, or alternative approaches to achieve the same result.

## sum_output

The sum_output part offers a business summary in a couple of sentences, explaining the overall purpose of the procedure

## metadata.txt

The `metadata.txt` file describes important attributes of the training data.

Structure look like:

```yaml
source: <source_member_name>
start: <start_line_number>
end: <end_line_number>
difficulty: <rating>
language: <language>
scope: <scope>
use: <usage>
```

Full description of all the metadata variants can be found [here](/pages/metadata.md).

The sample explanation source reference can be found [here](/pages/task/sample_source_references.md).