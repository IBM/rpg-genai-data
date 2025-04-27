# Explanation format for an RPG module

The following explanation applies to RPG source files modules that will be bound into programs or service programs.
The [`scope`](/pages/metadata#scope) in `metadata.txt` will be `module` in this case.

## sum_output

The sum_output part offers a business summary in a couple of sentences, explaining the overall purpose of the module

## api_output

The `api_output` part provides information to the caller about how to call the exported procedures in this module
and what the effects of calling it would be.
It includes a high-level summary of the module's purpose and then for each exported procedure, an api-level description of it.  Note that for modules the caller will not care about any side effects within the module, but only on the external objects such as databases.

## how_output

The how_output part explains how the module works internally, covering the full execution flow and logic used at each step. It includes details on control specifications, file declarations, global variables, constants, indicators, field mapping, subroutines, error handling, and the logic of each procedure.

1. `Control Specifications`: Any key compiler options and runtime control settings included in the module. Examples include `DftActGrp`, `ActGrp`, `BNDDIR`, and other relevant control specifications. Explain these settings and any keywords used.

2. `File Declarations`: List all declared files from the F-specs with relevant keywords. This includes display files, physical,logical files, printer files, and special files. Explain how each file is declared and any keywords used.

3. `Prototype & Interface Definition`: Describe internal procedure prototypes and their usage. This includes procedure names, input/output parameters, data types, and return types.

4. `Prototype for External Program Call`: Mention prototypes for external RPG/CL/API calls. This includes program name, parameter interface, data types, and expected results.

5.`Data Structures and Arrays` – Describe all declared data structures and arrays used in the module, including their purpose and how they are utilized.
  - `File Information Data Structure (INFDS)` – Mention any INFDS declared in the module and what file-level metadata they capture (e.g., file status, error codes, record numbers).
  - `Indicator Data Structure (INDDS)` – Explain the usage of indicator-based data structures, if any, especially those mapped to display files or used for control logic.
  - `Arrays` – Describe any arrays used, whether single or multi-dimensional, compile-time or run-time, and their role.
  - `General Data Structures` – Include details of data structures. Mention key attributes or keywords applied (e.g., `inz`, `overlay`, `dim`, `based`, etc.).

6. `Variables`: Explain the variables used in the module. This includes
  - `Global Variables`: List all global variables used in the module and explain their purpose.
  - `Constants`: List constants declared using `DCL-C`, including status flags, limits, messages, and system values.
  - `Keywords`: Explain any keywords used in variable declarations, such as `LIKE`, `INZ`, `N` data type, etc.

7. `Field Mapping`: Database to Display File: Create a clear table or list to show how physical/logical file fields are mapped to display fields. This includes field names and display fields.

8. `Procedures`: Describe high-level module logic for each procedure in ordered steps. This includes initialization, opening files, loading initial data, displaying subfile, handling user actions (Add/Edit/Delete/Search), validating and updating database, printing report (if required), and returning control to the caller.

9. `Error Handling`: Mention all error-handling techniques used. This includes message subfile, custom messages from MSGF, file status checks, return codes from APIs/procedures, and logging errors to a file or job log.

10. `Possible Problems with this code`: Identify potential issues that could arise during the execution of the procedure.

11. `Possible improvements to this code`: Identify areas where the code could be enhanced for better performance, readability, or functionality. This may include suggestions for error handling, optimization techniques, or alternative approaches to achieve the same result.
